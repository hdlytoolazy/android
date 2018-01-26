公司最近有打包自动化的要求，将代码的编译、打包以及发布到svn（也可以是git）这些费时费力的操作放到服务器上，这样撸代码的方便，测试的同事也方便，

所以就开始搞起。


一，Jenkins+Gradle实现android开发持续集成、打包

Jenkins的安装配置、以及Gradle的基础配置这里就不再过多的叙述了，链接如下：

http://blog.csdn.net/xiongmc/article/details/26515577


二，打包后发送到指定svn目录下

这里有两种情况：

1，打包后的apk要放的svn目录就是代码所在的svn目录

这种情况的同学恭喜你，跟着这个链接走就可以搞定：

https://www.cnblogs.com/EasonJim/p/6828364.html

2，打包后的apk要放到一个专门的svn路径中（别问我为什么会这样，xx说的要规范...）

① 先将代码checkout到服务器的某个路径下（注意这不是废话，你需要把这个路径记下来）

② 更改服务器中项目的Gradle文件，目的是将打包后的文件直接输出到①的路径中，大概如下：

android{

	applicationVariants.all { variant ->
	
        variant.outputs.each { output ->
		
            def today = new Date().format('MMdd-HHmm');//自定义添加日期
			
            def outputFile = output.outputFile;//def意为定义，其实你可以把它当做String outputFile = output.outputFile这样来理解
			
            def newFilePath = "/data/jenkinsPack/version3.x.x_test";
			
            if (outputFile != null && outputFile.name.endsWith('.apk')) {
			
                def oldFileName = outputFile.name.replace(".apk","-" + today + "-" + defaultConfig.versionName + ".apk");//replace方法，将原本的xxx-release.apk更名为xxx-release-0802-1212-v1.0.0.apk这样的apk命名
				
                File newFile = new File(newFilePath);//指定移动到的目标目录
				
                output.outputFile = new File(newFile,oldFileName );//创建并生成目标目录
				
//               outputFile.delete()

            }else {
			
            }
			
        }
		
    }
	
}


注意：newFilePath的路径一定要对应本地svn的路径，注意路径的正确性以及是否有读写权限；这段代码是放在android{}里的。

③ 第一种情况是使用SVN Publisher这个插件，直接将代码在构建后更新到svn。但现在就只能写shell脚本了，大概如下图：

![Alt text](https://raw.githubusercontent.com/hdlytoolazy/android/master/raw/Screenshots/20180126215830.png)

--no-auth-cache  不缓存验证信息，否则会出现类似如下图的错误：

![Alt text](https://raw.githubusercontent.com/hdlytoolazy/android/master/raw/Screenshots/20180126221714.png)

svn st | grep '^\?' | tr '^\?' ' ' | sed 's/[ ]*//' | sed 's/[ ]/\\ /g' | xargs svn add  直接add所有未添加过的文件or文件夹

注意：这里使用临时用户直接提交可能不是很好，后期可以优化一下脚本，使用已经在列表中的用户进行操作。


到这里，开开心心的去Jenkins上构建吧！
