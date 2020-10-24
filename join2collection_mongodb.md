# Công cụ chuyển dữ liệu real time từ mongodb sang elastic search

Mục đích xây dựng công cụ này sử dụng để nhận biết sự thay đổi từ mongodb là chuyển dữ liệu sang elacticsearch để phục vụ cho mục đích tìm kiếm và làm báo cáo.
Ngoài ra còn sử dụng để trigger dữ liệu, lưu vết dữ liệu hoặc cảnh báo khi mongodb gặp sự cố.

# Công cụ sử dụng!

- Visual studio 2019
- Robo 3T
- Mongodb
- Docker
- .Net core 3.1

# Hướng dẫn cài đặt

Ở đây mình hướng dẫn các bạn cài đặt mongodb trên docker. Cài đặt bộ elk trên docker. Để tránh loãng phần hướng dẫn thì mình ko nói phần cài đặt các công cụ như visual studio, docker, robo3T. .Net core 3.1 bạn có thể lấy trên trang chủ hoặc có thể download từ Docker

- Download mongodb từ docker hub
  `sh $ docker pull mongo `
  Tạo Replica Set cho Mongodb trên Docker
- Tạo network riêng để chạy
  ```sh
  $  docker network create mongo-cluster-dev
  ```
- Tạo mongo containers
  ```sh
    $  docker run -d --net mongo-cluster-dev -p 27017:27017 --name mongoset1 mongo mongod --replSet mongodb-replicaset --port 27017
    $  docker run -d --net mongo-cluster-dev -p 27018:27018 --name mongoset2 mongo mongod --replSet mongodb-replicaset --port 27018
    $  docker run -d --net mongo-cluster-dev -p 27019:27019 --name mongoset3 mongo mongod --replSet mongodb-replicaset --port 27019
  ```
  -Vào 1 replica set để khởi tạo
  ![Alt text](https://github.com/nguyenduchanh/Waiter/blob/master/Waiter/Images/Open%20mongo%20set%20command.png?raw=true "replica set 1 open command")
- Gõ lệnh
  ```sh
  $  mongo
  ```
- Paste đoạn sau vào shell
  ```sh
  > db = (new Mongo('localhost:27017')).getDB('test')
   config={"_id":"mongodb-replicaset","members":[{"_id":0,"host":"mongoset1:27017"},{"_id":1,"host":"mongoset2:27018"},{"_id":2,"host":"mongoset3:27019"}]}
   rs.initiate(config)
  ```
- Dưới đây là kết quả thực hiện
  ![Alt text](https://github.com/nguyenduchanh/Waiter/blob/master/Waiter/Images/replica%20set%201%20run.PNG?raw=true "Result")

Dùng Robo 3T để kết nối đến với 3 replica vừa tạo

Cài đăt elasticsearch
Mình sử dụng repo này để cài đặt bộ Elastic stack : https://github.com/euclid1990/elk

# Change stream Mongodb

- Hàm nhận sự thay đổi trên tất cả các database
  ```sh
  public static void GetChangeStreamOnAllDatabase(string connectionString)
  {
      try
      {
          var client = new MongoClient(connectionString);
          using (var cursor = client.Watch())
          {
              foreach (var change in cursor.ToEnumerable())
              {
                  // nhận giá trị thay đổi
                  var next = cursor.Current.First();
                  // do something
              }
          }
      }
      catch (Exception ex)
      {
          Console.WriteLine("Something wrong!!!");
      }
  }
  ```
- Hàm nhận sự thay đổi trên 1 database
  ```sh
    public static void GetChangeStreamOnSingleDatabase(string connectionString, string databaseName)
     {
         try
         {
             var client = new MongoClient(connectionString);
             var database = client.GetDatabase(databaseName);
             using (var cursor = database.Watch())
             {
                 foreach (var change in cursor.ToEnumerable())
                 {
                     // nhận giá trị thay đổi
                       var next = cursor.Current.First();
                     // do something
                 }
             }
         }
         catch (Exception ex)
         {
             Console.WriteLine("Something wrong!!!");
         }
     }
  ```
- Hàm nhận sự thay đổi trên 1 collection, chỉ nhận khi có sự kiện insert
  ```sh
  public static void GetChangeStreamOnsingleCollection(string connectionString, string databaseName, string collectionName)
  {
    try
    {
        var client = new MongoClient(connectionString);
        var database = client.GetDatabase(databaseName);
        var pipeline = new EmptyPipelineDefinition<ChangeStreamDocument<BsonDocument>>()
            .Match(change =>
                change.OperationType == ChangeStreamOperationType.Insert
                );
        var coll = database.GetCollection<BsonDocument>(collectionName);
        using (var cursor = coll.Watch(pipeline))
        {

            while (cursor.MoveNext() && cursor.Current.Count() == 0)
            { }
            // nhận giá trị thay đổi
            var next = cursor.Current.First();
            // do something
        }
    }
    catch (Exception ex)
    {
        Console.WriteLine("Something wrong!!!");
    }
  }
  ```
