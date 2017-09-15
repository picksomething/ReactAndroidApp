# 简介

原生Android项目接入RN的一个Hello World Demo

# Demo创建过程

* 新建一个ReactAndroidApp项目文件夹

* 文件夹里面新建一个android目录，可以把之前的原生Android项目拷贝进来，或者在android目录下用Android Studio新建一个Demo项目

* 在项目根目录下创建package.json文件，文件内容见demo

* 然后在根目录下执行npm install，执行完毕之后会在项目根目录生成一个/node_modules目录，里面都是一些项目编译需要的js依赖文件 

# 运行

* Debug模式

	* 下载代码:
	
	 ```git clone git@github.com:picksomething/ReactAndroidApp.git```

	* 在项目根目录执行: 

		```npm install``` (确保RN Android开发环境已经配置好，配置开发环境可参考[RN环境配置](https://reactnative.cn/docs/0.48/getting-started.html#content))
	
	* 执行: ```react-native run-android``` (也可以换成

		第三步也可以换成如下两步：
		* 启动本地Packager服务器 `npm start`
		* 运行Android项目 `./gradlew installDebug`或者`Ctrl+R`

* Release模式

	* 在Android项目中创建存放index.android.bundle的assets目录:
	
 	```mkdir android/app/src/main/assets```
 	
	* 打包index.android.js生成index.android.bundle到指定目录: 
	
	```
	react-native bundle --platform android --dev false --entry-	file index.android.js --bundle-output android/app/src/main/	assets/index.android.bundle --assets-dest android/app/src/	main/res
	```
	
	* 运行: ```react-native run-android``` 或者 ```./gradlew 	installRelease```

    > **如果你后面自己修改了index.android.js文件，那么通过release执行方式就要重新执行打包生成index.android.bundle，这样你的修改才会生效**


# 遇到的问题
* 在64位Android手机上运行的时候提示下面错误：

```
java.lang.UnsatisfiedLinkError: dlopen failed: "/data/data/cn.picksomething.reactnativeapp/lib-main/libgnustl_shared.so" is 32-bit instead of 64-bit
```

#### 解决办法(Solution)
在Android项目中的build.gradle文件中的defaultConfig里面加入如下代码：

```
ndk {
        abiFilters "armeabi-v7a", "x86"
    }

    packagingOptions {
        exclude "lib/arm64-v8a/libgnustl_shared.so"
    }
```

* ```unable to load script from assets index.android.bundle```

#### 解决办法(Solution)
使用运行中的Release模式，先在assets目录下生成index.android.bundle文件

* 红屏错误提示

```
undefined is not an object(ecaluating 'ReactInternals.ReactCurrentOwner')
```
#### 解决办法(Solution)

package.json中配置的react版本不对，我之前配置的是:

```
"dependencies": {
    "react": "15.6.0",
    "react-native": "0.48.3"
}
```
改为

```
"dependencies": {
    "react": "16.0.0-alpha.12",
    "react-native": "0.48.3"
}
```

# 其他
Hello World跑起来了，其他还会远吗？