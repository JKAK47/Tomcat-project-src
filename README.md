 
[TOC]
# 构建Tomcat 源码环境 
## 安装JDK，并设置JAVA_HOME环境变量等设置
## 安装Ant1.9.8+ 以上的ant 配置并设置ANT_HOME配置，并将${ANT_HOME}/bin 目录添加到path环境变量里面
## [下载源码](https://tomcat.apache.org/download-80.cgi)
## 在构建tomcat 时候会下载一些构建依赖库，默认在 `${user.home}/tomcat-build-libs`目录下存放下载下来的依赖文件，如果你需要改存放位置设置`base.path`，只需要创建`${tomcat.source}/build.properties`文件重新定义 `${tomcat.source}/build.properties.default`文件 和 `${tomcat.source}/build.xml`文件定义的属性。
## 进入`${tomcat.source}` tomcat 源码目录，执行ant 命令构建tomcat ，一旦构建完成可以看到BUILD SUCCESSFULLY 字样` ${tomcat.source}/output/build`目录下文件结构和 发行版tomcat目录结构一样；就是完整的tomcat 项目，可以直接使用。(如果不调试源码到这里即可，如果需要调试源码)
## Tomcat 转成maven项目并导入到IDEA 进行调试
- 
	- 我的tomcat源码路径是:`D:\Dev\tomcat\apache-tomcat-8.5.37-src-2`
	- 在该源码路径下新建pom.xml文件并填写如下信息
		```
		<?xml version="1.0" encoding="UTF-8"?>
		<project xmlns="http://maven.apache.org/POM/4.0.0"
		         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
		         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">

		    <modelVersion>4.0.0</modelVersion>
		    <groupId>org.apache.tomcat</groupId>
		    <artifactId>Tomcat-project-src</artifactId>
		    <name>Tomcat8.5.37-SrcCode</name>
		    <version>8.5.37</version>

		    <build>
		        <finalName>Tomcat-8.5.37</finalName>
		        <sourceDirectory>java</sourceDirectory>
		        <resources>
		            <resource>
		                <directory>java</directory>
		            </resource>
		        </resources>
		        <plugins>
		            <plugin>
		                <groupId>org.apache.maven.plugins</groupId>
		                <artifactId>maven-compiler-plugin</artifactId>
		                <version>3.5.1</version>
		                <configuration>
		                    <encoding>UTF-8</encoding>
		                    <source>1.8</source>
		                    <target>1.8</target>
		                </configuration>
		            </plugin>
		        </plugins>
		    </build>

		    <dependencies>
		        <dependency>
		            <groupId>junit</groupId>
		            <artifactId>junit</artifactId>
		            <version>4.12</version>
		            <scope>test</scope>
		        </dependency>
		        <dependency>
		            <groupId>org.easymock</groupId>
		            <artifactId>easymock</artifactId>
		            <version>3.4</version>
		            <scope>test</scope>
		        </dependency>
		        <!-- https://mvnrepository.com/artifact/org.apache.ant/ant -->
		        <dependency>
		            <groupId>org.apache.ant</groupId>
		            <artifactId>ant</artifactId>
		            <version>1.10.1</version>
		        </dependency>

		        <!-- <dependency>
		            <groupId>ant</groupId>
		            <artifactId>ant-apache-log4j</artifactId>
		            <version>1.6.5</version>
		        </dependency>
		        <dependency>
		            <groupId>ant</groupId>
		            <artifactId>ant-commons-logging</artifactId>
		            <version>1.6.5</version>
		        </dependency> -->
		        <dependency>
		            <groupId>wsdl4j</groupId>
		            <artifactId>wsdl4j</artifactId>
		            <version>1.6.2</version>
		        </dependency>
		        <!-- https://mvnrepository.com/artifact/javax.xml/jaxrpc-api -->
		      <dependency>
		          <groupId>javax.xml</groupId>
		          <artifactId>jaxrpc-api</artifactId>
		          <version>1.1</version>
		      </dependency>
		        <dependency>
		            <groupId>org.eclipse.jdt.core.compiler</groupId>
		            <artifactId>ecj</artifactId>
		            <version>4.6.1</version>
		        </dependency>
		    </dependencies>
		</project>
		```
# 执行tomcat入口方法` org.apache.catalina.startup.Bootstrap.main` main 方法即可。
如果 第一次启动遇到 类不存在，只需要将`${tomcat-src-directory}/webapps/example`文件夹删除即可。再次启动
```
java.lang.ClassNotFoundException: listeners.ContextListener
	at org.apache.catalina.loader.WebappClassLoaderBase.loadClass(WebappClassLoaderBase.java:1344)
	at org.apache.catalina.loader.WebappClassLoaderBase.loadClass(WebappClassLoaderBase.java:1172)
	at org.apache.catalina.core.DefaultInstanceManager.loadClass(DefaultInstanceManager.java:546)
	at org.apache.catalina.core.DefaultInstanceManager.loadClassMaybePrivileged(DefaultInstanceManager.java:527)
	at org.apache.catalina.core.DefaultInstanceManager.newInstance(DefaultInstanceManager.java:150)
	at org.apache.catalina.core.StandardContext.listenerStart(StandardContext.java:4734)
	at org.apache.catalina.core.StandardContext.startInternal(StandardContext.java:5278)
	at org.apache.catalina.util.LifecycleBase.start(LifecycleBase.java:150)
	at org.apache.catalina.core.ContainerBase.addChildInternal(ContainerBase.java:754)
	at org.apache.catalina.core.ContainerBase.addChild(ContainerBase.java:730)
	at org.apache.catalina.core.StandardHost.addChild(StandardHost.java:734)
	at org.apache.catalina.startup.HostConfig.deployDirectory(HostConfig.java:1140)
	at org.apache.catalina.startup.HostConfig$DeployDirectory.run(HostConfig.java:1875)
	at java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:511)
	at java.util.concurrent.FutureTask.run(FutureTask.java:266)
	at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1142)
	at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:617)
	at java.lang.Thread.run(Thread.java:745)

一月 24, 2019 4:34:39 下午 org.apache.catalina.core.StandardContext listenerStart
严重: Error configuring application listener of class [listeners.SessionListener]
java.lang.ClassNotFoundException: listeners.SessionListener
	at org.apache.catalina.loader.WebappClassLoaderBase.loadClass(WebappClassLoaderBase.java:1344)
	at org.apache.catalina.loader.WebappClassLoaderBase.loadClass(WebappClassLoaderBase.java:1172)
	at org.apache.catalina.core.DefaultInstanceManager.loadClass(DefaultInstanceManager.java:546)
	at org.apache.catalina.core.DefaultInstanceManager.loadClassMaybePrivileged(DefaultInstanceManager.java:527)
	at org.apache.catalina.core.DefaultInstanceManager.newInstance(DefaultInstanceManager.java:150)
	at org.apache.catalina.core.StandardContext.listenerStart(StandardContext.java:4734)
	at org.apache.catalina.core.StandardContext.startInternal(StandardContext.java:5278)
	at org.apache.catalina.util.LifecycleBase.start(LifecycleBase.java:150)
	at org.apache.catalina.core.ContainerBase.addChildInternal(ContainerBase.java:754)
	at org.apache.catalina.core.ContainerBase.addChild(ContainerBase.java:730)
	at org.apache.catalina.core.StandardHost.addChild(StandardHost.java:734)
	at org.apache.catalina.startup.HostConfig.deployDirectory(HostConfig.java:1140)
	at org.apache.catalina.startup.HostConfig$DeployDirectory.run(HostConfig.java:1875)
	at java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:511)
	at java.util.concurrent.FutureTask.run(FutureTask.java:266)
	at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1142)
	at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:617)
	at java.lang.Thread.run(Thread.java:745)

一月 24, 2019 4:34:39 下午 org.apache.catalina.core.StandardContext listenerStart
严重: Error configuring application listener of class [async.AsyncStockContextListener]
java.lang.ClassNotFoundException: async.AsyncStockContextListener
	at org.apache.catalina.loader.WebappClassLoaderBase.loadClass(WebappClassLoaderBase.java:1344)
	at org.apache.catalina.loader.WebappClassLoaderBase.loadClass(WebappClassLoaderBase.java:1172)
	at org.apache.catalina.core.DefaultInstanceManager.loadClass(DefaultInstanceManager.java:546)
	at org.apache.catalina.core.DefaultInstanceManager.loadClassMaybePrivileged(DefaultInstanceManager.java:527)
	at org.apache.catalina.core.DefaultInstanceManager.newInstance(DefaultInstanceManager.java:150)
	at org.apache.catalina.core.StandardContext.listenerStart(StandardContext.java:4734)
	at org.apache.catalina.core.StandardContext.startInternal(StandardContext.java:5278)
	at org.apache.catalina.util.LifecycleBase.start(LifecycleBase.java:150)
	at org.apache.catalina.core.ContainerBase.addChildInternal(ContainerBase.java:754)
	at org.apache.catalina.core.ContainerBase.addChild(ContainerBase.java:730)
	at org.apache.catalina.core.StandardHost.addChild(StandardHost.java:734)
	at org.apache.catalina.startup.HostConfig.deployDirectory(HostConfig.java:1140)
	at org.apache.catalina.startup.HostConfig$DeployDirectory.run(HostConfig.java:1875)
	at java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:511)
	at java.util.concurrent.FutureTask.run(FutureTask.java:266)
	at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1142)
	at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:617)
	at java.lang.Thread.run(Thread.java:745)

一月 24, 2019 4:34:39 下午 org.apache.catalina.core.StandardContext listenerStart
严重: Error configuring application listener of class [websocket.drawboard.DrawboardContextListener]
java.lang.ClassNotFoundException: websocket.drawboard.DrawboardContextListener
	at org.apache.catalina.loader.WebappClassLoaderBase.loadClass(WebappClassLoaderBase.java:1344)
	at org.apache.catalina.loader.WebappClassLoaderBase.loadClass(WebappClassLoaderBase.java:1172)
	at org.apache.catalina.core.DefaultInstanceManager.loadClass(DefaultInstanceManager.java:546)
	at org.apache.catalina.core.DefaultInstanceManager.loadClassMaybePrivileged(DefaultInstanceManager.java:527)
	at org.apache.catalina.core.DefaultInstanceManager.newInstance(DefaultInstanceManager.java:150)
	at org.apache.catalina.core.StandardContext.listenerStart(StandardContext.java:4734)
	at org.apache.catalina.core.StandardContext.startInternal(StandardContext.java:5278)
	at org.apache.catalina.util.LifecycleBase.start(LifecycleBase.java:150)
	at org.apache.catalina.core.ContainerBase.addChildInternal(ContainerBase.java:754)
	at org.apache.catalina.core.ContainerBase.addChild(ContainerBase.java:730)
	at org.apache.catalina.core.StandardHost.addChild(StandardHost.java:734)
	at org.apache.catalina.startup.HostConfig.deployDirectory(HostConfig.java:1140)
	at org.apache.catalina.startup.HostConfig$DeployDirectory.run(HostConfig.java:1875)
	at java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:511)
	at java.util.concurrent.FutureTask.run(FutureTask.java:266)
	at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1142)
	at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:617)
	at java.lang.Thread.run(Thread.java:745)




```
# 再次启动控制台有对应的信息输出即表示导入源码编译成功
```
一月 24, 2019 4:22:31 下午 org.apache.catalina.startup.ClassLoaderFactory validateFile
警告: Problem with directory [D:\Dev\tomcat\apache-tomcat-8.5.37-src-2\lib], exists: [false], isDirectory: [false], canRead: [false]
一月 24, 2019 4:22:31 下午 org.apache.catalina.startup.ClassLoaderFactory validateFile
警告: Problem with directory [D:\Dev\tomcat\apache-tomcat-8.5.37-src-2\lib], exists: [false], isDirectory: [false], canRead: [false]
一月 24, 2019 4:22:31 下午 org.apache.catalina.startup.ClassLoaderFactory validateFile
警告: Problem with directory [D:\Dev\tomcat\apache-tomcat-8.5.37-src-2\lib], exists: [false], isDirectory: [false], canRead: [false]
一月 24, 2019 4:22:31 下午 org.apache.catalina.startup.ClassLoaderFactory validateFile
警告: Problem with directory [D:\Dev\tomcat\apache-tomcat-8.5.37-src-2\lib], exists: [false], isDirectory: [false], canRead: [false]
一月 24, 2019 4:22:32 下午 org.apache.catalina.startup.VersionLoggerListener log
信息: Server version:        Apache Tomcat/@VERSION@
一月 24, 2019 4:22:32 下午 org.apache.catalina.startup.VersionLoggerListener log
信息: Server built:          @VERSION_BUILT@
一月 24, 2019 4:22:32 下午 org.apache.catalina.startup.VersionLoggerListener log
信息: Server number:         @VERSION_NUMBER@
一月 24, 2019 4:22:32 下午 org.apache.catalina.startup.VersionLoggerListener log
信息: OS Name:               Windows 7
一月 24, 2019 4:22:32 下午 org.apache.catalina.startup.VersionLoggerListener log
信息: OS Version:            6.1
一月 24, 2019 4:22:32 下午 org.apache.catalina.startup.VersionLoggerListener log
信息: Architecture:          amd64
一月 24, 2019 4:22:32 下午 org.apache.catalina.startup.VersionLoggerListener log
信息: Java Home:             C:\fastDev\Java\jdk1.8.0_91\jre
一月 24, 2019 4:22:32 下午 org.apache.catalina.startup.VersionLoggerListener log
信息: JVM Version:           1.8.0_91-b14
一月 24, 2019 4:22:32 下午 org.apache.catalina.startup.VersionLoggerListener log
信息: JVM Vendor:            Oracle Corporation
一月 24, 2019 4:22:32 下午 org.apache.catalina.startup.VersionLoggerListener log
信息: CATALINA_BASE:         D:\Dev\tomcat\apache-tomcat-8.5.37-src-2
一月 24, 2019 4:22:32 下午 org.apache.catalina.startup.VersionLoggerListener log
信息: CATALINA_HOME:         D:\Dev\tomcat\apache-tomcat-8.5.37-src-2
一月 24, 2019 4:22:32 下午 org.apache.catalina.startup.VersionLoggerListener log
信息: Command line argument: -javaagent:D:\Dev\JetBrains\IntelliJ IDEA 2017.3.5\lib\idea_rt.jar=51103:D:\Dev\JetBrains\IntelliJ IDEA 2017.3.5\bin
一月 24, 2019 4:22:32 下午 org.apache.catalina.startup.VersionLoggerListener log
信息: Command line argument: -Dfile.encoding=UTF-8
一月 24, 2019 4:22:32 下午 org.apache.catalina.core.AprLifecycleListener lifecycleEvent
信息: The APR based Apache Tomcat Native library which allows optimal performance in production environments was not found on the java.library.path: [C:\fastDev\Java\jdk1.8.0_91\bin;C:\Windows\Sun\Java\bin;C:\Windows\system32;C:\Windows;D:\Dev\SQLDev\Oracle\Oracle-Base\product\11.2.0\dbhome_1\bin;C:\Program Files (x86)\HP\NV\lib\shunra\vcat;C:\fastDev\Python3_5_2\Scripts\;C:\fastDev\Python3_5_2\;C:\ProgramData\Oracle\Java\javapath;D:\Program Files (x86)\CodeBlocks\MinGW\bin;D:\Dev\Android\android-sdk\ndk-bundle;D:\Dev\Android\android-sdk\platform-tools;D:\Dev\Android\android-sdk\tools;C:\fastDev\Mysql-5.7.19-win64\bin;C:\fastDev\Java\jdk1.8.0_91\bin;C:\fastDev\Java\jdk1.8.0_91\jre\bin;C:\Windows\system32;C:\Windows;C:\Windows\System32\Wbem;C:\Windows\System32\WindowsPowerShell\v1.0\;C:\Program Files (x86)\Intel\OpenCL SDK\3.0\bin\x86;C:\Program Files (x86)\Intel\OpenCL SDK\3.0\bin\x64;C:\Program Files (x86)\Windows Kits\8.0\Windows Performance Toolkit\;C:\Program Files\Microsoft SQL Server\110\Tools\Binn\;C:\Program Files (x86)\Microsoft Visual Studio 11.0\Common7\IDE;C:\Program Files (x86)\Microsoft Visual Studio 11.0\VC\bin;D:\Dev\adt-bundle-windows-x86_64-20140702\sdk\tools;D:\Dev\adt-bundle-windows-x86_64-20140702\sdk\platform-tools;D:\Dev\maven\apache-maven-3.5.3\bin;D:\Dev\maven\repo\;D:\Dev\VirtualBox;D:\Dev\Ant\apache-ant-1.10.1/bin;D:\Dev\Aspectj\aspectj1.8.10\bin;C:\Program Files (x86)\Calibre2\;D:\Dev\Android\strawberry-perl\perl\bin;C:\Program Files (x86)\HP\NV\lib\thirdparty\safenet\LDK\7.0;C:\Program Files (x86)\HP\NV\lib\shunra\snv;C:\Program Files (x86)\nodejs\;D:\Program Files (x86)\WinSCP\;D:\Dev\NodeJs\node-v8.9.0-win-x64;D:\Dev\NodeJs\node-v8.9.0-win-x64\node_global;D:\Dev\Git\TortoiseGit\bin;D:\Dev\Gradle\gradle-4.6\bin;D:\Dev\Groovy\Groovy-3.0.0\bin;D:\Dev\Git\Git\cmd;C:\Program Files\Microsoft Network Monitor 3\;C:\Users\PengRong\AppData\Local\GitHubDesktop\bin;C:\Users\PengRong\AppData\Roaming\npm;C:\Users\PengRong\AppData\Local\Programs\Fiddler;C:\Program Files\Mozilla Firefox;C:\Users\PengRong\AppData\Local\Google\Chrome\Application;D:\Dev\Microsoft VS Code\bin;.]
一月 24, 2019 4:22:32 下午 org.apache.coyote.AbstractProtocol init
信息: Initializing ProtocolHandler ["http-nio-8080"]
一月 24, 2019 4:22:32 下午 org.apache.tomcat.util.net.NioSelectorPool getSharedSelector
信息: Using a shared selector for servlet write/read
一月 24, 2019 4:22:32 下午 org.apache.coyote.AbstractProtocol init
信息: Initializing ProtocolHandler ["ajp-nio-8009"]
一月 24, 2019 4:22:32 下午 org.apache.tomcat.util.net.NioSelectorPool getSharedSelector
信息: Using a shared selector for servlet write/read
一月 24, 2019 4:22:32 下午 org.apache.catalina.startup.Catalina load
信息: Initialization processed in 860 ms
一月 24, 2019 4:22:32 下午 org.apache.catalina.core.StandardService startInternal
信息: Starting service [Catalina]
一月 24, 2019 4:22:32 下午 org.apache.catalina.core.StandardEngine startInternal
信息: Starting Servlet Engine: Apache Tomcat/@VERSION@
一月 24, 2019 4:22:32 下午 org.apache.catalina.startup.HostConfig deployDirectory
信息: Deploying web application directory [D:\Dev\tomcat\apache-tomcat-8.5.37-src-2\webapps\docs]
一月 24, 2019 4:22:33 下午 org.apache.catalina.util.SessionIdGeneratorBase createSecureRandom
警告: Creation of SecureRandom instance for session ID generation using [SHA1PRNG] took [564] milliseconds.
一月 24, 2019 4:22:33 下午 org.apache.catalina.startup.HostConfig deployDirectory
信息: Deployment of web application directory [D:\Dev\tomcat\apache-tomcat-8.5.37-src-2\webapps\docs] has finished in [1,089] ms
一月 24, 2019 4:22:33 下午 org.apache.catalina.startup.HostConfig deployDirectory
信息: Deploying web application directory [D:\Dev\tomcat\apache-tomcat-8.5.37-src-2\webapps\host-manager]
一月 24, 2019 4:22:33 下午 org.apache.catalina.startup.HostConfig deployDirectory
信息: Deployment of web application directory [D:\Dev\tomcat\apache-tomcat-8.5.37-src-2\webapps\host-manager] has finished in [89] ms
一月 24, 2019 4:22:33 下午 org.apache.catalina.startup.HostConfig deployDirectory
信息: Deploying web application directory [D:\Dev\tomcat\apache-tomcat-8.5.37-src-2\webapps\manager]
一月 24, 2019 4:22:34 下午 org.apache.catalina.startup.HostConfig deployDirectory
信息: Deployment of web application directory [D:\Dev\tomcat\apache-tomcat-8.5.37-src-2\webapps\manager] has finished in [95] ms
一月 24, 2019 4:22:34 下午 org.apache.catalina.startup.HostConfig deployDirectory
信息: Deploying web application directory [D:\Dev\tomcat\apache-tomcat-8.5.37-src-2\webapps\ROOT]
一月 24, 2019 4:22:34 下午 org.apache.catalina.startup.HostConfig deployDirectory
信息: Deployment of web application directory [D:\Dev\tomcat\apache-tomcat-8.5.37-src-2\webapps\ROOT] has finished in [66] ms
一月 24, 2019 4:22:34 下午 org.apache.coyote.AbstractProtocol start
信息: Starting ProtocolHandler ["http-nio-8080"]
一月 24, 2019 4:22:34 下午 org.apache.coyote.AbstractProtocol start
信息: Starting ProtocolHandler ["ajp-nio-8009"]
一月 24, 2019 4:22:34 下午 org.apache.catalina.startup.Catalina start
信息: Server startup in 1462 ms

```

# 参考
[Tomat启动-源码跟踪解决ClasNotFound问题](https://blog.csdn.net/it_freshman/article/details/81539009)

[Tomcat8源码分析系列-环境搭建](https://blog.csdn.net/Dwade_mia/article/details/79051370)

[tomcat源码系列一（环境搭建）](https://seemoonup.github.io/2018/11/06/tech/tomcat/tomcat%E6%8E%A2%E7%B4%A2%E4%B9%8B%E7%8E%AF%E5%A2%83%E6%90%AD%E5%BB%BA/)

[Apache Tomcat 8](https://tomcat.apache.org/tomcat-8.5-doc/building.html)