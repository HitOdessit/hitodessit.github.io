---
layout: post
title: "[Realm] First look into brand new Mobile persistent storage"
date: 2015-04-30 17:32:55 +0300
comments: true
categories: [Database, Realm, Android, Wear, Mobile]
---

Starting new project we spent some time investigating new approaches for data persistent storage on Android Platform.

One of the newest and the most promising library everybody is talking about is [Realm](http://realm.io).
This is noSQL, non-relational storage written on C++ for Android and iOS platforms which promises very handy and fast object-oriented access to local storage.

What can be better that writing something like:

```Java
realm.beginTransaction();
User user = realm.createObject(User.class); // Create a new object
user.setName("John");
user.setEmail("john@corporation.com");
realm.commitTransaction();
```

and store your business object into storage without any mapping, SQL, and other complicated and potentially slow operations inside?

The first impression was - “wow, it’s awesome!”.
It’s easy to integrate, pretty good documentation available, also samples, videos, community.

But deeper dive showed shocking drawbacks: <!--more-->

- you can store only “absolutely pure realm objects”, which means no inheritance from other class, no hierarchy  of your business classes, even no other methods except getters and setters are allowed. 
- you can’t override methods like `toString()` and `equals()`
- you can access your data fields only by getters and/or setters which makes impossible to serialize data objects to JSON by vell known libraries like [GSON](https://code.google.com/p/google-gson/) cause they uses direct field access for it, instead of getters/setters.
- no built-in mechanism to export/import data using JSON, only import functionality available (http://realm.io/docs/java/latest/#json), but it’s absolutely unclear how to import objects with many-to-many relationship to each other.

It’s too much for us. As we need to export/import data from local storage intensively this solution is unacceptable for us.

We will keep our eyes on Realm project progress, but now we're going to try other solutions.
