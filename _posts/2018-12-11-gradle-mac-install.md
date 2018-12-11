---
layout: post
title: gradle安装配置
share: true
comments: true
imagefeature:
tags: [gradle]
category: gradle
description: "在mac上安装gradle"
---



<!--more-->


从[官网下载](https://gradle.org/releases/)gradle


## 环境变量

```shell
export GRADLE_PATH="/Users/[you]/tools/gradle-x.xx.x"
export GRADLE_USER_HOME="/Users/[you]/.gradle"
```

此处有两段`GRADLE_PATH`为机器默认使用的路径，`GRADLE_USER_HOME`为项目中包装配置的gradle使用的路径，添加后修改PATH追加指向**$GRADLE_PATH/bin**


## 项目包装文件配置

包装配置文件一般位于**project_path/gradle/wrapper/gradle-wrapper.properties**例如：


```properties
distributionBase=GRADLE_USER_HOME
distributionPath=wrapper/dists
zipStoreBase=GRADLE_USER_HOME
zipStorePath=wrapper/dists
distributionUrl=https\://services.gradle.org/distributions/gradle-4.10.2-all.zip

```
gradle使用的版本也是在这里指定的
## 代理修改

### 全局配置


为了加速访问使用全局代理方式，编辑`/Users/[you]/.gradle/init.gradle`

```json

allprojects{
    repositories {
        def ALIYUN_REPOSITORY_URL = 'http://maven.aliyun.com/nexus/content/groups/public'
        def ALIYUN_JCENTER_URL = 'http://maven.aliyun.com/nexus/content/repositories/jcenter'
        all { ArtifactRepository repo ->
            if(repo instanceof MavenArtifactRepository){
                def url = repo.url.toString()
                if (url.startsWith('https://repo1.maven.org/maven2')) {
                    project.logger.lifecycle "Repository ${repo.url} replaced by $ALIYUN_REPOSITORY_URL."
                    remove repo
                }
                if (url.startsWith('https://jcenter.bintray.com/')) {
                    project.logger.lifecycle "Repository ${repo.url} replaced by $ALIYUN_JCENTER_URL."
                    remove repo
                }
            }
        }
        maven {
            url ALIYUN_REPOSITORY_URL
            url ALIYUN_JCENTER_URL
        }
    }
}

```


### 项目级配置

修改**build.gradle**,repositories中添加`maven {url 'http://maven.aliyun.com/nexus/content/groups/public/'}`

apply之前，添加

```json
allprojects {
    repositories {
        maven{ url 'http://maven.aliyun.com/nexus/content/groups/public/'}
        mavenCentral()
    }
}

```


