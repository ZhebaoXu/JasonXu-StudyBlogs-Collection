#### 关于android缓存的数据放在本地的哪个地方的研究

#### **一、问题描述**：

应用程序在运行的过程中如果需要向手机上保存数据，一般是把数据保存到sdcard中的，大部分应用是直接在sdcard根目录下创建一个文件夹，然后把数据保存在该文件夹中。这样就会产生一个问题，那就是当你的应用被卸载之后，这些数据就还会保留在sdcard中，留下了垃圾数据。如果有这么个需求，就是当应用被卸载，与该应用相关的数据也要被清除掉的话，那么，你该怎么办？

#### **二、解决方案：**

##### 首先需要明白两点：

一、通过Context.getExternalFilesDir()方法可以获取到SDCard/Android/data/你的应用的包名/files/ 目录，一般放一些长时间保存的数据；

二、通过Context.getExternalCacheDir()方法可以获取SDCard/Android/data/你的应用包名/cache/目录，一般存放临时缓存数据。

**其次，说明：**

1. 如果按照用上面的方法，当你的应用被用户卸载后，SDCard/Android/data/你的应用的包名/ 这个目录下的所有文件都被删除，不会留下垃圾信息。


2. 上面两个目录分别对应设置—>应用—>应用详情页面的“清除数据”与“清除缓存”选项。

**最后，总结：**

1. Context.getFilesDir()可以获取到"/data/data/<package name>/files" 这个方法获取的目录不是在sdcard上，而是在应用安装的目录；
2. Context.getCacheDir可以获取到"/data/data/<package name>/cache" 这个方法获取的目录不是在sdcard上，而是在应用安装的目录，这个文件里面的数据在设备内存不足的时候，会被系统删除数据。注意：你不能依赖系统帮你删除这些文件，当这个文件夹里面的数据超过1MB的时候，系统会删除这个文件夹里面的数据；
3. 通过Context.getExternalFilesDir()方法可以获取到 SDCard/Android/data/你的应用的包名/files/ 目录，一般放一些长时间保存的数据；
4. 通过Context.getExternalCacheDir()方法可以获取到 SDCard/Android/data/你的应用包名/cache/目录，一般存放临时缓存数据。

**资料：**

<!--以下是android文件目录的结构图，很强很精致-->

<!--网址：[http://www.jianshu.com/p/c88ebf1e0ca7](http://www.jianshu.com/p/c88ebf1e0ca7)

[http://blog.csdn.net/jdsjlzx/article/details/52442113](http://blog.csdn.net/jdsjlzx/article/details/52442113)  -->

```
/**
     * |   ($rootDir)
     * +- /data                    -> Environment.getDataDirectory()
     * |   |
     * |   |   ($appDataDir)
     * |   +- data/$packageName
     * |       |
     * |       |   ($filesDir)
     * |       +- files            -> Context.getFilesDir() / Context.getFileStreamPath("")
     * |       |      |
     * |       |      +- file1     -> Context.getFileStreamPath("file1")
     * |       |
     * |       |   ($cacheDir)
     * |       +- cache            -> Context.getCacheDir()
     * |       |
     * |       +- app_$name        ->(Context.getDir(String name, int mode)
     * |
     * |   ($rootDir)
     * +- /storage/sdcard0         -> Environment.getExternalStorageDirectory()/ Environment.getExternalStoragePublicDirectory("")
     * |                 |
     * |                 +- dir1   -> Environment.getExternalStoragePublicDirectory("dir1")
     * |                 |
     * |                 |   ($appDataDir)
     * |                 +- Andorid/data/$packageName
     * |                                         |
     * |                                         | ($filesDir)
     * |                                         +- files                  -> Context.getExternalFilesDir("")
     * |                                         |    |
     * |                                         |    +- file1             -> Context.getExternalFilesDir("file1")
     * |                                         |    +- Music             -> Context.getExternalFilesDir(Environment.Music);
     * |                                         |    +- Picture           -> Context.getExternalFilesDir(Environment.Picture);
     * |                                         |    +- ...               -> Context.getExternalFilesDir(String type)
     * |                                         |
     * |                                         |  ($cacheDir)
     * |                                         +- cache                  -> Context.getExternalCacheDir()
     * |                                         |
     * |                                         +- ???
     * <p/>
     * <p/>
     * 1.  其中$appDataDir中的数据，在app卸载之后，会被系统删除。
     * <p/>
     * 2.  $appDataDir下的$cacheDir：
     * Context.getCacheDir()：机身内存不足时，文件会被删除
     * Context.getExternalCacheDir()：空间不足时，文件不会实时被删除，可能返回空对象,Context.getExternalFilesDir("")亦同
     * <p/>
     * 3. 内部存储中的$appDataDir是安全的，只有本应用可访问
     * 外部存储中的$appDataDir其他应用也可访问，但是$filesDir中的媒体文件，不会被当做媒体扫描出来，加到媒体库中。
     * <p/>
     * 4. 在内部存储中：通过  Context.getDir(String name, int mode) 可获取和  $filesDir  /  $cacheDir 同级的目录
     * 命名规则：app_ + name，通过Mode控制目录是私有还是共享
     * <p/>
     * <code>
     * Context.getDir("dir1", MODE_PRIVATE):
     * Context.getDir: /data/data/$packageName/app_dir1
     * </code>
     */
```