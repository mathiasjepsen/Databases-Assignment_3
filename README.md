# Databases-Assignment_3
## 1. Twitter Data
### Queries & Output
**How many Twitter users are in the database?**
```mongo
db.tweets.distinct("user").length
```
> 659774
**Which Twitter users link the most to other Twitter users? (Provide the top ten.)**
```mongo
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
>
