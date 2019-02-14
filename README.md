# Databases-Assignment_3

## 1. Twitter Data

### Queries & Output

**How many Twitter users are in the database?**

```
db.tweets.distinct("user").length
```

> 659774

**Which Twitter users link the most to other Twitter users? (Provide the top ten.)**

```
db.tweets.aggregate([
  { $match: 
    { text: 
      { $regex: /@\w+/ }
    }
  }, 
  { $group: 
    { _id: "$user", 
      count: {$sum: 1}
    }
  }, 
  { $sort: 
    { count: -1 }
  }, 
  { $limit: 10 }
])
```
> { "_id" : "lost_dog", "count" : 549 }
> { "_id" : "tweetpet", "count" : 310 }
> { "_id" : "VioletsCRUK", "count" : 251 }
{ "_id" : "what_bugs_u", "count" : 246 }
{ "_id" : "tsarnick", "count" : 245 }
{ "_id" : "SallytheShizzle", "count" : 229 }
{ "_id" : "mcraddictal", "count" : 217 }
{ "_id" : "Karen230683", "count" : 216 }
{ "_id" : "keza34", "count" : 211 }
{ "_id" : "TraceyHewins", "count" : 202 }
