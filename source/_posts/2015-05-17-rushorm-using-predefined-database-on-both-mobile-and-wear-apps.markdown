---
layout: post
title: "[RushORM] Using predefined database on both Mobile and Wear apps"
date: 2015-05-17 16:31:37 +0300
comments: true
categories: [Database, RushORM, Android, Wear, Mobile]
---

Problem statement: 
> Assemble both mobile and wearable apps with the same RushORM database (.db file) with some predefined data in it, which will be absolutely the same for mobile and wear apps.

While developing one of our apps, we were needed to ship both mobile and wear apps with the same set of “predefined” (pre-populated) data. However, user should be able to modify this data, add new or delete existed. 

Using Android’s Framework `res` folder seems like not an option, because we can’t modify it’s content at runtime. Another idea is to upload that “predefined” data to some server, lets say in JSON format, download it during first app launch and store it in RushORM database using `RushCore.getInstance().deserialize(jsonString)` API. This may seems like a nice option, but it introduces few dependencies which we don’t want to have in our apps, like:

- Internet connection is required during first app start.
- Downloading, parsing and storing data into database increase first startup time, therefore worsens user experience.
- Some server is required, however it could be simply Dropbox with publicly shared files.
- Some script is required to upload generated files to remote server, and also to download these files from server to Android device.


We didn’t want to have any of the above dependencies and drawbacks in our app, so we come up with another solution: <!--more--> **generate database** (`.db` file) with all **predefined data** preliminary and assemble both mobile and wearable apps with the same `.db` file. Similar feature existed in [ActiveAndroid ORM](https://github.com/pardom/ActiveAndroid/wiki/Pre-populated-databases) framework and it’s called **Pre-populated databases**. However, [RushORM](http://www.rushorm.com/advanced.html) doesn’t have such feature. So, we decided to implement that feature by ourselves. 

There are 2 parts of the above task:

- Generate correct `.db` file in RushORM format with all data that we need in our apps initially.
- Use that pre-populated `.db` file to initialize RushORM both on Mobile and Wear apps.


To implement first part, we’ve created separate module in our Android project called `db-generator`. It’s a separate mobile app, which is very simple - just initialize empty RushORM database and then run data generation process, which saves generated data (business objects in our case) into DB, synchronously, using methods like `RushCore.getInstance().save(List<? extends Rush> objects)`.
RushORM store files in `/data/data/<yourAppID>/databases/` directory. After database was generated, we need to get it from rooted Android device or Android emulator where you were running `db-generator`. We can pull files using `adb pull` command. To make this process automated, we’ve created simple `Makefile`:

```make
THIS_FILE := $(lastword $(MAKEFILE_LIST))

db:
adb uninstall <yourAppID>
./gradlew db-generator:installDebug
adb shell am start -n <yourAppID>/.DBGeneratorActivity
sleep 5
adb pull /data/data/<yourAppID>/databases/predefined.db ./common/src/main/assets/predefined.db
@echo "\nBuild successful.\n"
```


Here we’re uninstalling our app first (to clear any previsouly stored data), then running standard Gradle task to install debug build of `db-generator` app, starting `DBGeneratorActivity` and waiting few seconds to allow data to be generated and stored into local storage. After that we’re pulling `.db` file using `adb pull` command (Note: this will only work on emulators or rooted devices) and storing it in our `common` module `assets` folder.

To implement second part, we need to copy `.db` file from `assets` folder to app sandbox folder (because files in `assets` folder are read-only) and only after that initialize RushORM. We need to do this only once, during app first launch. Here is the code:

```Java
public static void copyPredefinedDBIfNeeded(Context context) {
    boolean isProcessed = targetDBFile(context, PREDEFINED_DB_FILENAME).exists();
    if (isProcessed) {
        LOG.i("Predefined DB is already processed, skipping");
        return;
    }
    try {
        copyFile(context, PREDEFINED_DB_FILENAME);
        LOG.i("Predefined DB files are copied");
    } catch (IOException e) {
        LOG.e("Can't copy predefined db file", e);
    }
}

private static File targetDBFile(Context context, String name) {
    return context.getDatabasePath(name);
}

private static void copyFile(Context context, String name) throws IOException {
    InputStream in = null;
    File outFile = targetDBFile(context, name);
    try {
        in = context.getAssets().open(name);
        FileUtils.copyInputStreamToFile(in, outFile);
    } finally {
        if (in != null) {
            //noinspection EmptyCatchBlock
            try {
                in.close();
            } catch (IOException e) {
            }
        }
    }
}
```

After that is done, we can initialize RushORM with `predefined.db` file.


PS: Theoretically, it could be possible to run `db-generator` on desktop, but we didn’t try this approach. If you have some information about launching RushORM on desktop, please share it with us in comments.

