---
layout:     post
title:      MongoDB - 003
subtitle:   Advanced CURD & Operator 
date:       2019-2-2
author:     William
header-img: img/post-bg-universe.jpg
catalog: true
tags:
    - MongoDB
    - CURD
    - Operator
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

# Comparison Operator
| Operator   |  Meaning  |
| :-----:  | :----:  |
|   \$eq   |   Matches values that are equal to a specified value.   |
|    \$gt    |  Matches values that are greater than a specified value.  |
|\$gte|Matches values that are greater than or equal to a specified value.|
|\$lt|Matches values that are less than a specified value.|
|\$lte|Matches values that are less than or equal to a specified value.|
|\$ne|Matches all values that are not equal to a specified value.|
|\$in|Matches any of the values specified in an array.|
|\$nin|Matches none of the values specified in an array.|

```js
db.movieDetails.find({runtime: {$gt:90, $lt:120}})

db.movieDetails.find({runtime: {$gt:90},"tomato.meter":{$gte:95}})

db.movieDetails.find({rated: {$ne: "UNRATED"}})

db.movieDetails.find({rated: {$in: ["G","PG"]}})

// Return specific field
db.movieDetails.find({runtime: {$gt:90, $lt:120}},{_id:0, title:1, runtime:1})
```


# Element Operator
| Operator   |  Meaning  |
| :-----:  | :----:  |
|   \$exists   |   Matches documents that have the specified field.   |
|    \$type    |  Selects documents if a field is of the specified type.  |

```js
// Return documents with mpaaRating field
db.movieDetails.find({mpaaRating: {$exists:true}})

db.movies.find({viewerRating:{$type: "int"}})

```

#Logical Operator
| Operator   |  Meaning  |
| :-----:  | :----:  |
|   \$and   | Joins query clauses with a logical AND returns all documents that match the conditions of both clauses.  |
|    \$or   |  Joins query clauses with a logical OR returns all documents that match the conditions of either clause.  |
|\$not|Inverts the effect of a query expression and returns documents that do not match the query expression.|
|\$nor|Joins query clauses with a logical NOR returns all documents that fail to match both clauses.|

```js
db.movieDetails.find({$or: 
[{filter1},
{filter2}])
```

# Array Operator
| Operator   |  Meaning  |
| :-----:  | :----:  |
|   \$all   |   Matches arrays that contain all elements specified in the query.   |
|    \$elemMatch    |  Selects documents if element in the array field matches all the specified $elemMatch conditions.  |
|\$size|Selects documents if the array field is a specified size.|

```js
// The genres doesn't need to be exactly ["Comedy","Crime","Drama"]
// But each of ["Comedy","Crime","Drama"] should occur in genres
db.movieDetails.find({genres: {$all: ["Comedy","Crime","Drama"]}})


boxOffice:[{"country":"USA","revenue":41.3},
{"country":"Australia","revenue":2.9},
{"country":"UK","revenue":10.1},
{"country":"Germany","revenue":4.3},
{"country":"France","revenue":3.5},]

// This statement will return the result, even the revenue in Germany doesn't larger than 17
// Because this statement check all the revenue in boxOffice array
db.movieDetails.find({"boxOffice.country":"Germany","boxOffice.revenue":{$gt: 17}})


// This statement doesn't return anything
// It will need at least one element in boxOffice satisfy the filter
db.movieDetails.find({boxOffice: {$elemMatch: {"country": "Germany", "revenue":{$gt: 17}}}})


// Return the documents with one element in contries array
db.movieDetails.find({countries: {$size:1}})

```

# References
[Query and Projection Operators
](https://docs.mongodb.com/manual/reference/operator/query/)







