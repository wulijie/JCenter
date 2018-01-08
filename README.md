# 如何上传library到jcenter

1. 上传的整个过程大概如下图：
![image1](https://raw.githubusercontent.com/wulijie/JCenter/master/res/1.png)

2. 去[Bintray](https://bintray.com/)注册一个账户（建议使用github登录就好很方便，最好翻墙上，不然很卡）
需要翻墙软件的话，建议你使用这个免费软件[lantern](https://github.com/getlantern/forum)

3. 完成注册之后，登录网站，然后点击maven。
![image1](https://raw.githubusercontent.com/wulijie/JCenter/master/res/2.png)


4. 点击Add New Package，为我们的library创建一个新的package。
![image1](https://raw.githubusercontent.com/wulijie/JCenter/master/res/3.png)

5. 输入所有需要的信息，虽然如何命名包名没有什么限定，但是也有一定规范。所有字母应该为小写，单词之间用－分割，比如，fb-like。当每项都填完之后，点击Create Package。
![image1](https://raw.githubusercontent.com/wulijie/JCenter/master/res/4.png)


6. 网页将引导你到 Package编辑页面。点击 Edit Package文字下的Package名字，进入Package详情界面。到这里你就已经拥有自己的maven库了，下面要做的是如何通过AndroidStudio上传你的lib到你新建的maven仓库里
![image1](https://raw.githubusercontent.com/wulijie/JCenter/master/res/5.png)

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
    bintrayRepo = 'maven'////bintray上的仓库名，我这里取名为maven
    bintrayName = 'Slider'//bintray 仓库内包名

    libraryName = 'Slider'
    libraryVersion = '1.0'
    libraryDescription = 'A Tool for Android Project'

    publishedGroupId = 'com.warpdrive.slider'
    artifact = 'Slider'

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

// 温馨提示：通过from的方式获取我提交在github上封装后的bintray代码（一般为公共常用）
apply from:'https://raw.githubusercontent.com/wulijie/JCenter/v2/bintray.gradle'
apply from:'https://raw.githubusercontent.com/wulijie/JCenter/v2/install.gradle'

```

9. 在local.properties里加上：  

```
bintray.apikey=apiKey(点击Edit可以看到)
bintray.user=Bintray用户名
bintray.userOrg=组织名（如果你的maven库并没有创建在组织下 这个是不需要增加的）

```
user为下图：

apikey为下图：



10. 打开控制台，输入`gradle bintrayupload`然后坐等SUCCESS。  
![terminal](https://raw.githubusercontent.com/Jude95/JCenter/master/image/terminal.png)
如果找不到gralde命令，确定你把gradle加入了你的环境变量。
有时候注释里的一些特殊字符会造成编译失败。提示哪句不对就改一下那部分注释吧。  

11. 成功过后到[Bintray](https://bintray.com/)找到你刚上传的包。点`add to Jcenter`。随便填点评论提交，每天半夜12点半准时审核通过(= = 美国时间上班了)。然后你会收到一条通知。  
然后你的就可以用 `GroupId:ArtifactId:libraryVersion` 来依赖了。以后有更新直接重复第5部即可，会自动同步到jcenter仓库。

如果上传后却依赖不了可以去[http://jcenter.bintray.com](http://jcenter.bintray.com)找到你的group目录看看你到底上传上去没有。  
比如：`com.jude:easyrecyclerview:1.0.2`就是http://jcenter.bintray.com/com/jude/easyrecyclerview   
因为，有时候你上次到了你的meaven仓库，而jcenter并没有同步过去。这种情况很少见，重新上传个新版本即可解决。  

