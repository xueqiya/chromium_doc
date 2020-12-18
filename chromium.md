## chromium

#### ChromeLauncherActivity.
启动`ChromeTabbedActivity`

#### ChromeTabbedActivity
```
ChromeTabbedActivity extends ChromeActivity
ChromeActivity extends AsyncInitializationActivity

AsyncInitializationActivity onCreate()   ---->
ChromeBrowserInitializer.getInstance(this).handlePreNativeStartup(this);

ChromeBrowserInitializer handlePreNativeStartup() ---->

AsyncInitializationActivity setContentViewAndLoadLibrary() --->
mNativeInitializationController.startBackgroundTasks();

NativeInitializationController startBackgroundTasks(), new Thread() ---> 

libraryLoader.ensureInitialized()
catch ProcessInitException, if catch it, throw new RuntimeException(e), browser re-start().

LibraryLoader ensureInitialized() throws ProcessInitException--->

LibraryLoader loadAlreadyLocked() ---> System.load("libchrome_public.so");
```



## webview  vs content shell  vs chromium

|  | 类型 | 性能 | 控件 | 其他 |  |
|  ----  | ----  |  ----  |  ----  |  ----  |  ----  |
| webview | webview控件 | webview8.0之前没有多进程架构，web页面可能会引起整个app的崩溃webview8.0之后android:externalService可以在context中开启渲染器进程，但是还不支持多个渲染器进程 | View |  |  |
| content shell | 基于content API的简单浏览器 | 支持多进程，多线程 | Surfaceview |  |  |
| chromium | 完整浏览器 | 支持多进程，多线程 | Surfaceview | 多了预渲染 同步 翻译 等 |  |
## Content shell

JAVA：ContentShellActivity ShellManger Shell ContentViewReaderView Surfaceview




## 下载编译
```

apt-get install sudo

sudo apt-get install python

git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git

mkdir ~/chromium && cd ~/chromium

fetch --nohooks android

cd src

git fetch --tags 

git checkout -b local_tags_79.0.3945.107 tags/79.0.3945.107

gclient sync --with_branch_heads --nohooks --job 16

echo "target_os = [ 'android' ]" >> ../.gclient

gclient sync

sudo apt-get install lsb-core

build/install-build-deps.sh

build/install-build-deps-android.sh
（build/install-build-deps-android.sh --no-chromeos-fonts）

gclient runhooks

gn args out/Default

*****

sudo add-apt-repository ppa:openjdk-r/ppa

sudo apt-get update

sudo apt-get install openjdk-8-jdk

autoninja -C out/Default chrome_public_apk

autoninja -C out/Default content_shell_apk

build/android/gradle/generate_gradle.py --output-directory out/Default --project-dir ~/Projects/chromium_source/gradle_project
```

编译后出现v8等文件夹

```
拉取服务器已编译代码：
rsync -P -avpr --progress --exclude='*/.git/*' --exclude='*.o' --exclude='*.a' --exclude='*/lib.java/*' --exclude='*/lib.unstripped/*' root@192.168.6.100:/root/chromium/src .
```

## 单独编译模块与使用
```
autoninja -C out/Default net
```

`base`、`boringssl`、`crcrypto`等

## 其他

Bromite浏览器，基于chromium，已开源，ui几乎没变，添加搜索引擎和去广告等



https://chromium.googlesource.com/chromium/src/+/HEAD/content/public/README.md

https://chromium.googlesource.com/chromium/src/+/HEAD/content/README.md



|     | 基于 | chromium版本 | 备注 | apk大小 | so大小 |
|  ----  | :---  |  ----  |  ----  |  ----  |  ----  |
| opera | Content api | 77 |  | 73.1 | 47.5 |
| Mint | 继承webview | 61 |  | 13.6 | 0.8 |
| Uc | 发现chromium webview代码 | 57 |  | 54.8 | 58.1 |
| qq | 发现fromwork webview代码和x5内核 | 57 |  | 31.5 | 6.5+5.8+5.5 |
| Baidu | 发现fromwork webview代码 | 79 |  | 25.3 | 11.3 |
| Edge | chromium | 73 |  | 99.4 | 107.8 |
| Yandex | chromium | 77 |  | 132 | 82.9 |
| Brave | chromium | 79 | 外形几乎没变 | 162.1 | 61.5 |
| 自由 | Chromium | 闪退 | 外形几乎没变 | 106.2 | 80.6 |
| 360极速 | 混淆太严重 | 闪退 |  | 12 | 8.7 |
| 遨游 | 混淆太严重，好像是自己的内核 | 79 |  | 30.4 | 4.5 |

```
public MyWebView(Context context, AttributeSet attrs, int defStyleAttr, boolean privateBrowsing) {
    super(context, attrs, defStyleAttr, privateBrowsing);
}
此方法api17(android4.2)后过时
```