---
layout:     post
title:      MongoDB - 002
subtitle:   Cloud Cluster & CURD 
date:       2019-2-2
author:     William
header-img: img/post-bg-universe.jpg
catalog: true
tags:
    - MongoDB
    - Altas Cluster
---
<script type="text/x-mathjax-config">
  MathJax.Hub.Config({
    tex2jax: { 
      inlineMath: [['$','$'], ['\\(','\\)']],
      processEscapes: true
    }
  });
  </script>
<script type="text/javascript" async
  src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.5/MathJax.js?config=TeX-MML-AM_CHTML">
</script>

# Create Altas Cluster

Follow the [Instruction](https://university.mongodb.com/mercury/M001/2019_January/chapter/Chapter_2_The_MongoDB_Query_Language_Atlas/lesson/595aae2236942e83e9a361f2/tab/595aae2236942e83e9a361f3) to create free Altas Cluster.

# Insert
```js
//Using video database
use video 

show collections

//It will auto-generate unique ObjectId as id. Any json object can be used as parameter.
db.moviesScratch.insertOne({titile: "Hello, World",imbd: "tt212314"})

db.moviesScratch.insertMany([
{titile: "Hello, World",imbd: "tt212314"},
{titile: "Hello, World2",imbd: "tt2123142"}])

// Also we can define the _id as we need
// Below statement will encounter duplicate key error
// So it will stop at 3th record, then the 4th one won't insert into collection
db.moviesScratch.insertMany([
{_id:"tt1" ,titile: "Hello, World",imbd: "tt212314"},
{_id:"tt2" ,titile: "Hello, World2",imbd: "tt2123142"},
{_id:"tt1" ,titile: "Hello, World3",imbd: "tt2123143"},
{_id:"tt3" ,titile: "Hello, World4",imbd: "tt2123144"}])

// Adding second argument to make insert continuing when facing duplicated key error
db.moviesScratch.insertMany([
{_id:"tt1" ,titile: "Hello, World",imbd: "tt212314"},
{_id:"tt2" ,titile: "Hello, World2",imbd: "tt2123142"},
{_id:"tt1" ,titile: "Hello, World3",imbd: "tt2123143"},
{_id:"tt3" ,titile: "Hello, World4",imbd: "tt2123144"}
],
{"ordered": false})

```

# Read

When there are a lot of results to return. MongoDB returns results in batch, we can iterate the cursor to get more results.
## Scalar Field
```js
// It will return the records with mpaaRating equals PG-13 and year euqals 2009
db.movies.find({mpaaRating:"PG-13",year:2009})

// To show the result in a strucutre pretty way
db.movies.find({mpaaRating:"PG-13",year:2009}).pretty()

// To return the number of results
db.movies.find({mpaaRating:"PG-13",year:2009}).count()

// Set constrain on nested object
// In this case, key must be inside of ""
db.movies.find({"wind.direction.angle": 20})

```

## Array Field
```js
// Exactly Match 
db.movies.find({cast:["Jeff", "Tim"]})

// Find the record contains "Jeff" in its cast array
db.movies.find({cast:"Jeff"})

// Asign its exact index in cast array
db.movies.find({cast.0:"Jeff"})

```

# Update
```js
// If there are several mathes, it will only update the first document
db.movies.updateOne({
    title: "The Martian"
},
{
    $set:{
        poster: "Updated Value"
    }
})

// $inc is another updating operator, it will increase the reviwe value by 3
db.movies.updateOne({
    title: "The Martian"
},
{
    $inc:{
        reviews: 3
    }
})

// $push will add item to an array called reviews(nested object)
// If reviews array doesn't exist, it will create one
db.movies.updateOne({
    title: "The Martian"
},
{
    $push:{
        reviews:{
            rating: 4.5,
            data: "2018-9-01"
        }
    }
})

// It will modified all the matched records
db.movies.updateMany()

// fileter was used to select record
// json document is all the contents stored in json object
db.movies.replaceOne(filter, json object)
```

# Delete
```js
db.movies.deleteOne(filter)

db.movies.deleteMany(filter)
```