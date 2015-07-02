---
layout: post
title: "[RushORM] How to enable debug logging"
date: 2015-07-01 19:38:14 +0300
comments: true
categories: [Database, RushORM, Android, Mobile, Logging]
---


Using [RushORM](http://www.rushorm.com) I’ve found that it is very hard to understand root cause of any database-related issues without internal debug logging, I mean at least all SQL queries should be printed into log (RushORM doesn’t enable debug logging by default).

But how to enable debug logging in RushORM? It’s not clear from the documentation, in fact, the documentation doesn’t say a word about this feature. Luckily, this feature exists!

Analyzing source code I found how to enable debug logging in RushORM: you simply need to add following flag to your `AndroidManifest.xml` file, under `<application>` section:

<!--more-->
```xml
<meta-data android:name="Rush_log" android:value="true" />
```

That’s it! With this option enabled, RushORM will print all SQL queries he’s going to execute into Logcat. Very helpful for troubleshooting. 

PS: **@Stu_cams**, you should add this flag into *Setup* section of documentation on RushORM website.
