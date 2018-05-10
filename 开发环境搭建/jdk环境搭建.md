#jdk环境搭建

## win7系统搭建jdk1.8环境

 1. 下载jdk安装包
    在oracle官网'http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html' 下载jdk，点击接受并且选择windows版本  
![jdk](https://raw.githubusercontent.com/s290717997/ItertkImage/master/res/jdkImage/jdk1.8_download.png)  
  下载完成后点击exe安装，其实安装了2个程序，第一个是jdk，第二次安装是jre:
    - JRE： Java Runtime Environment，
        是包含了java虚拟机，java基础类库。是使用java语言编写的程序运行所需要的软件环境，是提供给想运行java程序的用户使用的。
    - JDK： Java Development Kit 
        是java的开发环境，包含编译调试java程序的一整套工具

 2. 设置环境变量
    右键点击计算机->属性->高级系统设置->环境变量 

![jdk](https://raw.githubusercontent.com/s290717997/ItertkImage/master/res/jdkImage/jdk_path.png)  

 - 新建JAVA_HOME变量，值为jdk的安装路径，比如我的安装路径为d:\Program\jdk  
 - 新建CLASSPATH变量，值为D:\Program\jdk\lib;%JAVA_HOME%\lib\tools.jar  
"JAVA_HOME"表示JAVA_HOME的值，前面为绝对路径，本来之前用过"JAVA_HOME"来应用，但是一直不行，就用了绝对路径。如果安装jdk的路径和我的不一样，要将\Program\jdk这部分改成你自己安装的路径
 - 在已有的系统变量中对Path变量新添加D:\Program\jdk\jre\bin;这里也是绝对路径，记得改为自己安装的路径
![jdk](https://raw.githubusercontent.com/s290717997/ItertkImage/master/res/jdkImage/jdk_environment.png)

 3. 测试
    设置完成后，打开cmd控制台输入java -version，看是否和自己配置的版本一值，如果命令不存在则表示配置失败，在配置一下环境变量
 ![jdk](https://raw.githubusercontent.com/s290717997/ItertkImage/master/res/jdkImage/jdk_test.png)

