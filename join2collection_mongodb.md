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
