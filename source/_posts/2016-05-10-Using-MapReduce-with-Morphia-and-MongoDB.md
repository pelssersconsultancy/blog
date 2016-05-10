---
title: Using MapReduce with Morphia and MongoDB
date: 2016-05-10 14:43:33
tags:
 - MongoDB
 - Morphia
 - MapReduce
---

 {% asset_img mapreduce-mongodb.jpg Map Reduce MongoDB %} 

### 1. Precondition

You have a database called **HELLOWORLD** with 1 collection called **Person** containing following documents:

```js
[
{
  "firstName": "John",
  "lastName": "Doe",
  "age": 35,
  "location": {
     "country": "NL",
     "city": "Maastricht"
  }
},
{
  "firstName": "Mark",
  "lastName": "Doe",
  "age": 45,  
  "location": {
     "country": "NL",
     "city": "Eindhoven"
  }
},
{
  "firstName": "Pieter",
  "lastName": "Nelissen",
  "age": 55,  
  "location": {
     "country": "NL",
     "city": "Amsterdam"
  }
},
{
  "firstName": "Edgard",
  "lastName": "Lejeune",
  "age": 25,   
  "location": {
     "country": "FR",
     "city": "Paris"
  }
}
]
```

### 2. Find number of persons per city for country NL

```
robbypelssers1@macbookpelssers:~$ mongo
MongoDB shell version: 3.2.3
connecting to: test
> use HELLOWORLD
switched to db HELLOWORLD
>


var mapFunc = function() { emit(this.location.city, this.age); };

var reduceFunc = function(k, v) { return Array.sum(v); };

db.Person.mapReduce(mapFunc,reduceFunc, {out: { inline: 1 }, query: {"location.country": "NL"}})

```

### 3. Output from shell

```js 
{
	"results" : [
		{
			"_id" : "Amsterdam",
			"value" : 55
		},
		{
			"_id" : "Eindhoven",
			"value" : 45
		},
		{
			"_id" : "Maastricht",
			"value" : 35
		}
	],
	"timeMillis" : 13,
	"counts" : {
		"input" : 3,
		"emit" : 3,
		"reduce" : 0,
		"output" : 3
	},
	"ok" : 1
}
```

### 4. How to do it in Java using Morphia and MongoDB Java driver

```java
Datastore datastore = ... 
DBCollection collection = datastore.getCollection(Person.class);
DBObject query =  new BasicDBObject("location.country", "NL");
String mapFunc = "function() { emit(this.location.city, this.age); }";
String reduceFunc = "function(k, v) { return Array.sum(v); }";

MapReduceCommand mapReduceCommand = new MapReduceCommand(collection, mapFunc, reduceFunc, null,
                MapReduceCommand.OutputType.INLINE, query);

MapReduceOutput output = collection.mapReduce(mapReduceCommand);
        
output.results().forEach(result -> {
      DBObject key = (DBObject) result.get("_id");
      Double value = (Double) result.get("value");
     //do something with this data....
});
```

