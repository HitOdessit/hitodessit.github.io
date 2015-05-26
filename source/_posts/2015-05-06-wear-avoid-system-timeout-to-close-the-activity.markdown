---
layout: post
title: "[Wear] Avoid system timeout to close the activity"
date: 2015-05-06 12:57:34 +0300
comments: true
categories: [Android, Wear, UI]
---

Interestingly, by default Android Wear close the application activity after 30 seconds of idle.
It means that Google wants Wear apps be “lightweight” -  designed to work in short-time interaction with user.

But still Android is Android :) Here you can avoid almost everything.

Just add `android:keepScreenOn="true"` to your activities root view XML and timeout will be dismissed.

Alternatively, you can [set same flag at runtime from Java code](https://developer.android.com/training/scheduling/wakelock.html#screen)
like:

```Java
public class MainActivity extends Activity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        getWindow().addFlags(WindowManager.LayoutParams.FLAG_KEEP_SCREEN_ON);
    }
```

**Do not abuse this option, because it may drain wearable device’s battery!**

