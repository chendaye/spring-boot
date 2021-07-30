# 参考

[地址](https://blog.csdn.net/smart_an/article/details/107219821)

# 背景

要阅读源码那就首先得先搭建源码阅读环境，那么本篇文章就来介绍下Spring Boot的源码环境搭建。

鉴于spring团队已经全面抛弃maven构建工具而选用gradle来构建，而且网上目前看来还没有文章介绍springboot最新版2.3.x的gradle构建（绝大多数都是maven构建），那么本篇文章就是基于gradle来构建最新版springboot2.3.2的源码阅读环境。


# 环境准备

## git

## jdk8

## gradle6.5.1

[下载地址](https://services.gradle.org/distributions/ )

- 解压
- 添加系统变量`GRADLE_HOME`
- 添加系统变量`%GRADLE_HOME%\bin`
- `gradle -v`


# 下载源码

[下载源码](https://github.com/spring-projects/spring-boot)

> Fork 出属于自己的仓库:开始阅读、调试源码，我们可能会写一些注释，有了自己的仓库，可以进行自由的提交
> 本文使用的 Springboot 版本为最新的 2.3.x的分支代码 （2.3.2.BUILD-SNAPSHOT）
> 使用 git 从 Fork 出来的仓库拉取代码，注意这里为什么不拉取master分支呢?master分支依赖的spring版本为spring-5.3.0-M1版本，该版本非稳定版本，而且编译到最后会出现问题，报一些spring模块的5.3.0-M1.jar包不存在或无法下载等一些莫名其妙的错误， 所以我这边拉取的是2.3.x分支，这个分支依赖的spring版本为5.2.7.RELEASE版本。所以我就git clone 2.3.x分支到本地，然后再导入idea中



- 进入目录`spring-boot-2.3.1`
- `git clone -b 2.3.x https://github.com/spring-projects/spring-boot.git`


# 开始构建

- `打开idea后，【File】->【Open…】,打开刚拉取的spring-boot源码，点击ok即可打开,打开之后，gradle会自动构建，开始下载gradle-6.4-bin.zip工具包，idea中还有一些地方需要设置，所以先不构建，点击取消`
- `选择【File】->【project Structure…】,打开后点击左侧Project，然后Project SDK选择java version 1.8，Project language level选择8`
- `设置完毕之后，打开工程下的gradle->wrapper下的gradle-wrapper.properties文件，注释掉.换成本地的gradle-6.5.1-all.zip，这个版本是当前最新版，而且是带源码的。`
- `distributionUrl=file:///d/software/gradle-6.5/gradle-6.5-all.zip`
- `修改工程下的buildSrc下的build.gradle文件，找到如下代码段，添加阿里云镜像（不添加的话几个小时也构建不完）`
  ```
repositories {
//加上阿里云镜像
maven { url 'https://maven.aliyun.com/nexus/content/groups/public/' }
maven { url 'https://maven.aliyun.com/nexus/content/repositories/jcenter' }
maven { url "https://repo.spring.io/plugins-release" }
mavenCentral()
gradlePluginPortal()
maven { url "https://repo.spring.io/release" }
}

  ```
- `继续修改同目录下的settings.gradle文件，这是全局配置文件，也要加上阿里云镜像，找到如下代码块`

```
pluginManagement {
repositories {
//加上阿里云镜像
maven { url 'https://maven.aliyun.com/nexus/content/groups/public/' }
maven { url 'https://maven.aliyun.com/nexus/content/repositories/jcenter' }
maven { url "https://repo.spring.io/plugins-release" }
mavenCentral()
gradlePluginPortal()
}
......
}

```

- `修改工程根目录下的build.gradle文件（前面修改的是buildSrc下的，注意区别），同样是加上阿里云镜像，红框中的代码需要全部加上，且只能加在该文件头部。`

```
buildscript {
repositories {
maven { url 'https://maven.aliyun.com/nexus/content/groups/public/' }
maven { url 'https://maven.aliyun.com/nexus/content/repositories/jcenter' }
maven { url "https://repo.spring.io/plugins-release" }
}
}

```

- `还是这个文件，继续修改，往下找到如下图的代码块`

```
allprojects {
group "org.springframework.boot"

repositories {
//阿里云镜像
maven { url 'https://maven.aliyun.com/nexus/content/groups/public/' }
maven { url 'https://maven.aliyun.com/nexus/content/repositories/jcenter' }
mavenCentral()
......
}
......
}

```

- `继续修改根目录下的全局配置文件settings.gradle，同样是加上阿里云镜像`

```
pluginManagement {
repositories {
//阿里云镜像
maven { url 'https://maven.aliyun.com/nexus/content/groups/public/' }
maven { url 'https://maven.aliyun.com/nexus/content/repositories/jcenter' }
mavenCentral()
......
}
......
}

```

- `ok，到此才可以开始愉快的构建`