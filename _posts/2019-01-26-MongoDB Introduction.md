---
layout:     post
title:      MongoDB - 001
subtitle:   Introduction & GUI Tool
date:       2019-1-26
author:     William
header-img: img/post-bg-universe.jpg
catalog: true
tags:
    - MongoDB
    - Compass
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

#Introduction

MongoDB is nosql databse and is an open-source document database that provides high performance, high availability, and automatic scaling. 

## Key Features
- High Performance
- Rich Query Language
- High Availability
- Horizontal Scalability
- Support for Multiple Storage Engines

## Basic Structure

Databases -> Collections -> Documents

Each databses contains several collections, each record in collections is called as document. Below is the corresponding relationship in relational database.

| Relational Database   |  NoSQL Database  |
| :-----:  | :----:  |
|   Table   |   Collection   |
|    Tuple    |  Document  |

## Document Types 
### Scalar Value Types

Below is part of the list of types.

| Scalar Value Types   |  
| :-----:  | 
|   Double   |   
|    String    |  
|    Boolean    |  
|    Date    |  
|    Int    |  

For full list, please check the [table](https://docs.mongodb.com/manual/reference/bson-types/)

### Array Types
Except scalar value type, the field can also take array as its type. The actors filed in below example is Array type.

![](http://ww1.sinaimg.cn/large/83d6b255ly1fzkfbi4g2bj20fg055glm.jpg)

### Document(Object) Types
Also, field can take aggregated data strucutre as its type, like the imbd field below.

![](http://ww1.sinaimg.cn/large/83d6b255ly1fzkfe6ai3jj203w01owe9.jpg)


# GUI Tool - Compass

Compass can be downloaded from [download center](https://www.mongodb.com/download-center/compass).

## Schema View

The compass will fetch a sample of documents to analyse field type and its range.

![](http://ww1.sinaimg.cn/large/83d6b255ly1fzkewgxxb2j20ri0g4dgi.jpg)


# References
[MongoDB Basics](https://university.mongodb.com/mercury/M001/2019_January/overview)

