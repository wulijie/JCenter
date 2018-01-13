# 如何上传library到jcenter

1. 上传的整个过程大概如下图：
![image1](https://raw.githubusercontent.com/wulijie/JCenter/master/res/1.png)

2. 去[Bintray](https://bintray.com/)注册一个账户（建议使用github登录就好很方便，最好翻墙上，不然很卡）
需要翻墙软件的话，可以用免费软件[lantern](https://github.com/getlantern/forum)

 注册的时候注意：START YOUR FREE TRIAL （这个注册的是公司或者组织的账号，一般审核都要一个月，这一个月期间你没有办法上传你的库到Jcenter，所以最好不要注册这个），点击右侧的 For an Open Source
 Acount 注册个人账号。
![image1](https://raw.githubusercontent.com/wulijie/JCenter/master/res/j10.png)

  注册时建议使用github账号登录，方便好记。
![image1](https://raw.githubusercontent.com/wulijie/JCenter/master/res/j6.png)

3. 完成注册之后，登录网站，然后
   点击 Add New Repository。创建一个Maven仓库，仓库名字随意，我这里给的是maven。
![image1](https://raw.githubusercontent.com/wulijie/JCenter/master/res/j1.png)
![image1](https://raw.githubusercontent.com/wulijie/JCenter/master/res/j13.png)


4. 点击Add New Package，为我们的library创建一个新的package。
![image1](https://raw.githubusercontent.com/wulijie/JCenter/master/res/j3.png)

5. 输入所有需要的信息，当每项都填完之后，点击Create Package。
![image1](https://raw.githubusercontent.com/wulijie/JCenter/master/res/j9.png)


6. 网页将引导你到 Package编辑页面。点击 Edit Package文字下的Package名字，进入Package详情界面。到这里你就已经拥有自己的maven库了
![image1](https://raw.githubusercontent.com/wulijie/JCenter/master/res/j12.png)
下面要做的是如何通过AndroidStudio上传你的lib到你新建的maven仓库里

7. 在你项目的Project的gradle内加上并且给仓库加上mavenCentral()

```
 //如果报错请先把gradle版本升到2.11以上。如果还报错请保证你的gradle与这3个插件全都是最新。
 classpath 'com.jfrog.bintray.gradle:gradle-bintray-plugin:1.7.3'

 classpath 'com.github.dcendents:android-maven-gradle-plugin:1.5'
 
```
例如：

```
buildscript {
            repositories {
                mavenCentral()
                jcenter()
            }
            dependencies {
                classpath 'com.android.tools.build:gradle:2.2.0'
                classpath 'com.jfrog.bintray.gradle:gradle-bintray-plugin:1.7'
                classpath 'com.github.dcendents:android-maven-gradle-plugin:1.5'
            }
        }
        allprojects {
            repositories {
            mavenCentral()
                jcenter()
            }
        }

```

8. 在要上传的moudel里的gradle里最外层加上 

```
ext {

    bintrayRepo = 'maven'////bintray上的仓库名，我给的是maven，你自己可以随意给
    bintrayName = 'Slider'//bintray 你创建的new Package的名字

    libraryName = 'slider'//你开源库的名字
    libraryVersion = '1.0.2'//开源库的版本
    libraryDescription = 'a tool for Android'//开源库的描述

    publishedGroupId = 'com.warpdrive.slider'//这个发布的id 我一般给包名
    artifact = 'slider'//这个实际项目名，我一般给项目的小写名字
    //上面两个名字决定了  你的compile库怎么写 例如：我这个是  compile 'com.warpdrive.slider:slider:1.0'

    siteUrl = 'https://github.com/wulijie/Slider'//你的库在github上的地址
    gitUrl = 'https://github.com/wulijie/Slider.git' //你的库在github上的地址

    //开发者信息
    developerId = 'wulijie'
    developerName = 'wulijie'
    developerEmail = 'cocos2dx@126.com'

    //开源协议
    licenseName = 'The Apache Software License, Version 2.0'
    licenseUrl = 'http://www.apache.org/licenses/LICENSE-2.0.txt'
    allLicenses = 'Apache-2.0'
}
//下面是我抽离出来的两个通用的代码，你也可以打开复制到你的gradle
//或者你自己传到你的git上使用自己的，或者就用我提供的这两个。
//这两个文件的地址就在本仓库内
apply from: 'https://raw.githubusercontent.com/wulijie/JCenter/master/install.gradle'
apply from: 'https://raw.githubusercontent.com/wulijie/JCenter/master/bintray.gradle'

```

9. 在local.properties里加上：   

```
bintray.apikey=apiKey #(点击Edit可以看到)
bintray.user= #Bintray用户名
#只有你的账号是公司或者组织才需要增加这个。
bintray.userOrg=组织名

```
user及apikey为下图这两个位置：
![image1](https://raw.githubusercontent.com/wulijie/JCenter/master/res/j2.png)


10. 以上都配置完毕后，点开项目右侧的Gradle,找到图中指示的按钮
![image1](https://raw.githubusercontent.com/wulijie/JCenter/master/res/j7.png)
在弹出的框里输入：install后点击ok
![image1](https://raw.githubusercontent.com/wulijie/JCenter/master/res/j4.png)
等intall的任务执行完毕以后，继续上面的操作在弹出的输出框里输入：
BintrayUpload,点击ok,坐等提交成功。
![image1](https://raw.githubusercontent.com/wulijie/JCenter/master/res/j11.png)

11. 成功过后到[Bintray](https://bintray.com/)找到你刚上传的包。点`add to Jcenter`。随便填点评论提交，每天半夜12点半准时审核通过(= = 美国时间上班了)。然后你会收到一条通知。 
	 ![image1](https://raw.githubusercontent.com/wulijie/JCenter/master/res/j14.png)
然后你的就可以用 `GroupId:ArtifactId:libraryVersion` 来依赖了。以后有更新直接重复第10步即可，会自动同步到jcenter仓库。
  如果上传后却依赖不了可以去[http://jcenter.bintray.com](http://jcenter.bintray.com)找到你的group目录看看你到底上传上去没有。 如果上传了，但是还是依赖不了，那么就在更新一个版本上去(这个情况极少出现）

