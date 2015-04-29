---
layout: post
title: "Hello, World!"
date: 2015-04-27 19:54:33 +0300
comments: true
tags:
---

This is my first post in this blog and in general my first blogging experience. Please do not judge strictly :)

more content

1. one
2. two
3. three

## h2
### h3
#### h4

> quote 1
>> quote 2

h first

----

h second

****

Now, some Java code:

```Java
private static String serializeRoutine(Routine obj) throws JSONException {
        Map<String, Object> map = new HashMap<>();
        map.put("id", obj.getId());
        map.put("name", obj.getName());
        map.put("synced", obj.isSynced());
        map.put("entries", new JSONArray(obj.getEntries()));
                                            
        return new JSONObject(map).toString();
}
```

And some XML code
```XML
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context=".MainActivity"
    tools:deviceIds="wear_square">
                        
    <ScrollView
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_weight="1"
        android:scrollbars="vertical"
        >
            <TextView
                android:id="@+id/txObjectsLog"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:singleLine="false"
                />
    </ScrollView>
                                                                                                                                            
    <Button
	android:id="@+id/btnPost"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="@string/post_stub_data" />
                                                                                                                    
    <Button
        android:id="@+id/btnStartWorkout"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="@string/start_workout" />
                                                    
</LinearLayout>
```

end.