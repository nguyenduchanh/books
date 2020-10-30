# Map 2 array of object in collection in Mongodb
# Ví dụ 1 
Cho collection "CollectionA" có cấu trúc như sau
 ```sh
  {
    "_id" : ObjectId("5f9b89200186bed4f326ffa2"),
    "product" : [ 
        {
            "productId" : ObjectId("5f9b89200186bed4f326ffa3"),
            "value" : 1
        }, 
        {
            "productId" : ObjectId("5f9b89200186bed4f326ffa4"),
            "value" : 2
        }, 
        {
            "productId" : ObjectId("5f9b89200186bed4f326ffa5"),
            "value" : 3
        }
    ],
    "product2" : [ 
        {
            "productId" : ObjectId("5f9b89200186bed4f326ffa3"),
            "name" : "first",
            "anotherValue" : 1
        }, 
        {
            "productId" : ObjectId("5f9b89200186bed4f326ffa4"),
            "name" : "second",
            "anotherValue" : 2
        }, 
        {
            "productId" : ObjectId("5f9b89200186bed4f326ffa5"),
            "name" : "third",
            "anotherValue" : 3
        }
    ]
}
```
Trong "CollectionA" có 2 array of object. Cần join các object trong array "product" và "product2" có cùng productId.
kết quả là một dữ liệu có cấu trúc như sau

 ```sh
  {
    "_id" : ObjectId("5f9b89200186bed4f326ffa2"),
    "product" : [ 
        {
            "productId" : ObjectId("5f9b89200186bed4f326ffa3"),
            "value" : 1
        }, 
        {
            "productId" : ObjectId("5f9b89200186bed4f326ffa4"),
            "value" : 2
        }, 
        {
            "productId" : ObjectId("5f9b89200186bed4f326ffa5"),
            "value" : 3
        }
    ],
    "product2" : [ 
        {
            "productId" : ObjectId("5f9b89200186bed4f326ffa3"),
            "name" : "first",
            "anotherValue" : 1
        }, 
        {
            "productId" : ObjectId("5f9b89200186bed4f326ffa4"),
            "name" : "second",
            "anotherValue" : 2
        }, 
        {
            "productId" : ObjectId("5f9b89200186bed4f326ffa5"),
            "name" : "third",
            "anotherValue" : 3
        }
    ],
    "fullProduct" : [ 
        {
            "productId" : ObjectId("5f9b89200186bed4f326ffa3"),
            "value" : 1,
            "name" : "first",
            "anotherValue" : 1
        }, 
        {
            "productId" : ObjectId("5f9b89200186bed4f326ffa4"),
            "value" : 2,
            "name" : "second",
            "anotherValue" : 2
        }, 
        {
            "productId" : ObjectId("5f9b89200186bed4f326ffa5"),
            "value" : 3,
            "name" : "third",
            "anotherValue" : 3
        }
    ]
}
```

Dùng Mongo shell thì câu lệnh join sẽ như sau:
 ```sh
db.getCollection('CollectionB').aggregate([ {
    $addFields: {
      fullProduct: {
        $map: {
          input: "$product",
          as: "one",
          in: {
            $mergeObjects: [
              "$$one",
              {
                $arrayElemAt: [
                  {
                    $filter: {
                      input: "$product2",
                      as: "two",
                      cond: { $eq: ["$$two.productId", "$$one.productId"]}
                    }
                  },
                  0
                ]
              }
            ]
          }
        }
      }
    }
  }
])
```

