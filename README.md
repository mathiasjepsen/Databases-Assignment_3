# Databases-Assignment_3

## 1. Twitter Data

### Queries & Output
---

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
---
```
lost_dog        — 549
tweetpet        — 310
VioletsCRUK     — 251
what_bugs_u     — 246
tsarnick        — 245
SallytheShizzle — 229
mcraddictal     — 217
Karen230683     — 216
keza34          — 211
TraceyHewins    — 202
```

**Who are the most mentioned Twitter users? (Provide the top five.)**

```
db.tweets.aggregate([
  { $match: 
    { text: 
      { $regex: /@\w+/ }
    }
  }, 
  { $project: 
    { mentionedName: 
      { $split: ["$text", " "] }
    }
  }, 
  { $unwind: "$mentionedName" }, 
  { $match: 
    { mentionedName: 
      { $regex: /@\w+/} 
    }
  }, 
  { $group: 
    { _id: "$mentionedName", 
      count: { $sum: 1 }
    }
  }, 
  { $sort: 
    { count: -1 }
  }, 
  { $limit: 5 }
])
---
```
@mileycyrus    — 4310
@tommcfly      — 3837
@ddlovato      — 3349
@Jonasbrothers — 1263
@DavidArchie   — 1222
