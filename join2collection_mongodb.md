# Ví dụ về join 2 collection trong Mongodb
# Ví dụ 1 
Cho collection "CollectionA" có cấu trúc như sau
 ```sh
  {
    "_id" : ObjectId("582d43d18ec3f432f3260682"),
    "description" : [ 
        {
            "b_id" : ObjectId("58345e0e996d340bd8126149")
        }, 
        {
            "b_id" : ObjectId("5836bc0b291918eb42966320")
        }, 
        {
            "b_id" : ObjectId("5836bc0b291918eb42966321")
        }
    ]
}
```
Collection "CollectionB" có cấu trúc như sau

 ```sh
  {
    "_id" : ObjectId("58345e0e996d340bd8126149"),
    "name" : "1 object"
  }
```
Trong "CollectionA" có trường "description" là một mảng các object chứa các "b_id" để join với "_id" trong "CollectionB"

Collection sau khi join có cấu trúc như sau:
 ```sh
  {
     "_id" : ObjectId("582d43d18ec3f432f3260682"),
    "description" : {
        "CollectionB" : [ 
            {
                "_id" : ObjectId("58345e0e996d340bd8126149"),
                "name" : "1 object"
            }
        ]
    }
  }
```

Dùng Mongo shell thì câu lệnh join sẽ như sau:
 ```sh
 db.getCollection('CollectionA').aggregate([
    { "$addFields": { 
            "description": { "$ifNull" : [ "$description", [ ] ] }    
        } },
        { "$lookup": {
            "from": "CollectionB",
            "localField": "description.b_id",
            "foreignField": "_id",
            "as": "description.CollectionB"
        } }
    ]
    )
```
Hoặc
 ```sh
    db.getCollection('CollectionA').aggregate([
     {
       $lookup:
         {
           "from": "CollectionB",
          "localField": "description.b_id",
          "foreignField": "_id",
          "as": "description.CollectionB"
         }
    } ]
    )
```
# Ví dụ 2
Cho collection "CollectionC" có cấu trúc như sau
 ```sh
  {
    "_id" : ObjectId("582d43d18ec3f432f3260682"),
    "description" : [ 
        ObjectId("58345e0e996d340bd8126149"), 
        ObjectId("5836bc0b291918eb42966320"), 
        ObjectId("5836bc0b291918eb42966321")
    ]
  }
```
Cho collection "CollectionD" có cấu trúc như sau
 ```sh
  {
    "_id" : ObjectId("58345e0e996d340bd8126149"),
    "name" : "1 object"
}
```
Trong "CollectionC" có trường "description" là một mảng các ObjectId để join với "_id" trong "CollectionD"
Collection sau khi join có cấu trúc như sau:
 ```sh
  {
    "_id" : ObjectId("582d43d18ec3f432f3260682"),
    "description" : [ 
        ObjectId("58345e0e996d340bd8126149"), 
        ObjectId("5836bc0b291918eb42966320"), 
        ObjectId("5836bc0b291918eb42966321")
    ],
    "CollectionD" : [ 
        {
            "_id" : ObjectId("58345e0e996d340bd8126149"),
            "name" : "1 object"
        }
    ]
}
```
Dùng Mongo shell thì câu lệnh join sẽ như sau:
 ```sh
 db.getCollection('CollectionC').aggregate([
   {
      $lookup:
         {
            from: "CollectionD",
            localField: "description",
            foreignField: "_id",
            as: "CollectionD"
        }
   }
])
```
# Join 2 collections with muilti coditions
- Có 2 collection như sau
- Collection "Demo395"
```sh
{
    "_id" : ObjectId("5e5e782317aa3ef9ab8ab207"),
    "Name" : "Chris"
}

{
    "_id" : ObjectId("5e5e782317aa3ef9ab8ab208"),
    "Name" : "David"
}

```
- Collection "Demo396"
```sh
{
    "_id" : ObjectId("5e5e787817aa3ef9ab8ab209"),
    "details" : [ 
        ObjectId("5e5e782317aa3ef9ab8ab207"), 
        ObjectId("5e5e782317aa3ef9ab8ab208")
    ]
}

```
- Để join "Demo396" với "Demo395" theo trường "details" của "Demo396" và "_id" của "Demo395"
thì câu lệnh mongo shell sẽ như sau:
```sh
db.getCollection('Demo396').aggregate([
   {
      $lookup:
         {
            from: "Demo395",
            localField: "details",
            foreignField: "_id",
            as: "Demo395"
        }
   }
])
```
- Kết quả sẽ như sau
```sh
{
    "_id" : ObjectId("5e5e787817aa3ef9ab8ab209"),
    "details" : [ 
        ObjectId("5e5e782317aa3ef9ab8ab207"), 
        ObjectId("5e5e782317aa3ef9ab8ab208")
    ],
    "Demo395" : [ 
        {
            "_id" : ObjectId("5e5e782317aa3ef9ab8ab207"),
            "Name" : "Chris"
        }, 
        {
            "_id" : ObjectId("5e5e782317aa3ef9ab8ab208"),
            "Name" : "David"
        }
    ]
}
```
- code trong c# driver
```sh
    var client = new MongoClient("mongodb://localhost:27017/?connect=direct"); // your connection
    var database = client.GetDatabase("hvadb"); // your database
    var collection = database.GetCollection<Demo396>("Demo396"); //Demo396 laf class chua du lieu cua collection "Demo396"
    var result = collection.Aggregate().
                                    Lookup(
                                            foreignCollectionName: "Demo395",
                                            localField: "details",
                                            foreignField: "_id",
                                            @as: "Demo395"
                                           ).ToList();
```
- Tuy nhiên nếu ta cần thêm điều kiện để lấy dữ liệu ở collectinon "Demo395" như là  "Name" = "Chris". chỉ có ai có tên là "Chris" mới lấy thì query sẽ thay đổi như sau

- Mongo shell
```sh
    db.getCollection('Demo396').aggregate([
    { "$lookup": {
            "from": "Demo395",
            "let": { "details": "$details" },
            "pipeline": [
            { 
                "$match": { 
                    "$expr": {"$and":[
                        { "$in": [ "$_id", "$$details" ] } ,
                        { "$eq": [ "$Name", "Chris" ] },
                                      ]}} 
            }
                        ],
            "as": "output"
        }
    }
])
```
- C# driver
```sh
    var client = new MongoClient("mongodb://localhost:27017/?connect=direct"); // your connection
    var database = client.GetDatabase("hvadb"); // your database
    var collection = database.GetCollection<Demo396>("Demo396");//Demo396 la class chua du lieu cua collection "Demo396"
    //pipline
    BsonArray subpipeline = new BsonArray();
            subpipeline.Add(
                new BsonDocument("$match", new BsonDocument(
                    "$expr", new BsonDocument(
                    "$and", new BsonArray {
                        new BsonDocument("$in", new BsonArray{ "$_id", "$$details" } ),
                        new BsonDocument("$eq", new BsonArray{"$Name", "Chris" } )
                         }
                    )
                ))
            );
    //lookup
    var lookup = new BsonDocument("$lookup",
                 new BsonDocument("from", "Demo395")
                             .Add("let",
                                 new BsonDocument("details", "$details"))
                             .Add("pipeline", subpipeline)
                             .Add("as", "output")); 
    //get result
    var results = collection.Aggregate()
                .AppendStage<BsonDocument>(lookup).ToList();
```
- Kết quả
```sh
{
    "_id" : ObjectId("5e5e787817aa3ef9ab8ab209"),
    "details" : [ 
        ObjectId("5e5e782317aa3ef9ab8ab207"), 
        ObjectId("5e5e782317aa3ef9ab8ab208")
    ],
    "output" : [ 
        {
            "_id" : ObjectId("5e5e782317aa3ef9ab8ab207"),
            "Name" : "Chris"
        }
    ]
}
```