# Databases-Assignment_3

## Queries & Output

**1. How many Twitter users are in the database?**

```
db.tweets.distinct("user").length
```
---
```
659774
```

**2. Which Twitter users link the most to other Twitter users? (Provide the top ten.)**

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

**3. Who are the most mentioned Twitter users? (Provide the top five.)**

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
```
---
```
@mileycyrus    — 4310
@tommcfly      — 3837
@ddlovato      — 3349
@Jonasbrothers — 1263
@DavidArchie   — 1222
```

**4.Who are the most active Twitter users (top ten)?**

```
db.tweets.aggregate([
  { $group: 
    { _id: "$user",
      count: { "$sum": 1 }
    }
  },
  { $sort: { count: -1 }
  },
  { $limit: 10 }
])
```
---
```
lost_dog        — 549
webwoke         — 345
tweetpet        — 310
SallytheShizzle — 281
VioletsCRUK     — 279
mcraddictal     — 276
tsarnick        — 248
what_bugs_u     — 246
Karen230683     — 238
DarkPiano       — 236
```

**5. Who are the five most grumpy (most negative tweets) and the most happy (most positive tweets)? (Provide five users for each group)**

```
db.tweets.aggregate([
  { $facet: 
    { "grumpy": 
      [
        { $match: 
          { polarity: 0 }
        },
        { $group: 
          { _id: "$user",
            count: { $sum: 1 }
          }
        },
        { $sort: 
          { count: -1 }
        },
        { $limit: 5 }
      ],
      "happy": 
        [
          { $match: 
            { "polarity": 4 }
          },
          { "$group": 
            { _id: "$user",
              count: { $sum: 1 }
            }
          },
          { $sort: 
            { count: -1 }
          },
          { $limit: 5 }
        ]
    }
  }
])
```
---
```
grumpy: lost_dog    – 549
        tweetpet    – 310
        webwoke     – 264
        mcraddictal – 210
        wowlew      – 210
            
happy:  what_bugs_u – 246
        DarkPiano   – 231
        VioletsCRUK – 218
        tsarnick    – 212
        keza34      – 211
```

## Modeling

