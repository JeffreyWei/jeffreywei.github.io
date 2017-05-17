---
layout: post
title: Java项目中缓存的使用
share: true
comments: true
imagefeature:
tags: [java,cache]
category: java,cache
description: "Using Spring & Ehcache"
---

在Spring项目中集成Ehcache,缓存数据、缓存页面

<!--more-->

##配置修改

###工程配置

For Maven project :

{% highlight xml %}

<dependency>
	<groupId>net.sf.ehcache</groupId>
	<artifactId>ehcache</artifactId>
	<version>2.9.0</version>
</dependency>

<!-- Optional, to log stuff -->
<dependency>
	<groupId>ch.qos.logback</groupId>
	<artifactId>logback-classic</artifactId>
	<version>1.0.13</version>
</dependency>
 
<!-- Spring caching framework inside this -->
<dependency>
	<groupId>org.springframework</groupId>
	<artifactId>spring-context</artifactId>
	<version>4.1.4.RELEASE</version>
</dependency>

<!-- Support for Ehcache and others -->
<dependency>
	<groupId>org.springframework</groupId>
	<artifactId>spring-context-support</artifactId>
	<version>4.1.4.RELEASE</version>
</dependency>

{%  endhighlight %}

For Gradle project :

{% highlight javascript %}
apply plugin: 'java'
apply plugin: 'eclipse-wtp'
 
version = '1.0'
 
// Uses JDK 7
sourceCompatibility = 1.7
targetCompatibility = 1.7
 
// Get dependencies from Maven central repository
repositories {
	mavenCentral()
}
 
//Project dependencies
dependencies {
	compile 'org.springframework:spring-context:4.1.4.RELEASE'
	compile 'org.springframework:spring-context-support:4.1.4.RELEASE'
	compile 'net.sf.ehcache:ehcache:2.9.0'
	compile 'ch.qos.logback:logback-classic:1.0.13'
}
{%  endhighlight %}


###Spring配置
在spring的`主配置`中添加
{% highlight xml %}
 <!-- 缓存配置 -->
    <!-- 启用缓存注解功能(请将其配置在Spring主配置文件中) -->
    <cache:annotation-driven cache-manager="cacheManager"/>
    <!-- Spring自己的基于java.util.concurrent.ConcurrentHashMap实现的缓存管理器(该功能是从Spring3.1开始提供的) -->
    <!--
    <bean id="cacheManager" class="org.springframework.cache.support.SimpleCacheManager">
        <property name="caches">
            <set>
                <bean name="myCache" class="org.springframework.cache.concurrent.ConcurrentMapCacheFactoryBean"/>
            </set>
        </property>
    </bean>
     -->
    <!-- 若只想使用Spring自身提供的缓存器,则注释掉下面的两个关于Ehcache配置的bean,并启用上面的SimpleCacheManager即可 -->
    <!-- Spring提供的基于的Ehcache实现的缓存管理器 -->
    <bean id="cacheManagerFactory" class="org.springframework.cache.ehcache.EhCacheManagerFactoryBean">
        <property name="configLocation" value="classpath:ehcache.xml"/>
    </bean>
    <bean id="cacheManager" class="org.springframework.cache.ehcache.EhCacheCacheManager">
        <property name="cacheManager" ref="cacheManagerFactory"/>
    </bean>

{%  endhighlight %}

xml文件的schema中添加

		xmlns:cache="http://www.springframework.org/schema/cache"
		http://www.springframework.org/schema/cache
        http://www.springframework.org/schema/cache/spring-cache-3.2.xsd
        
###缓存配置
编辑文件`resources/ehcache.xml`

{% highlight xml %}
<!-- Ehcache2.x的变化(取自https://github.com/springside/springside4/wiki/Ehcache) -->
<!-- 1)最好在ehcache.xml中声明不进行updateCheck -->
<!-- 2)为了配合BigMemory和Size Limit,原来的属性最好改名 -->
<!--   maxElementsInMemory->maxEntriesLocalHeap -->
<!--   maxElementsOnDisk->maxEntriesLocalDisk -->
<ehcache>
    <diskStore path="java.io.tmpdir"/>
    <defaultCache
            maxElementsInMemory="1000"
            eternal="false"
            timeToIdleSeconds="120"
            timeToLiveSeconds="120"
            overflowToDisk="false"/>
    <cache name="myCache"
           maxElementsOnDisk="20000"
           maxElementsInMemory="2000"
           eternal="true"
           overflowToDisk="true"
           diskPersistent="true"/>
</ehcache>
        <!--
        <diskStore>==========当内存缓存中对象数量超过maxElementsInMemory时,将缓存对象写到磁盘缓存中(需对象实现序列化接口)
        <diskStore path="">==用来配置磁盘缓存使用的物理路径,Ehcache磁盘缓存使用的文件后缀名是*.data和*.index
        name=================缓存名称,cache的唯一标识(ehcache会把这个cache放到HashMap里)
        maxElementsOnDisk====磁盘缓存中最多可以存放的元素数量,0表示无穷大
        maxElementsInMemory==内存缓存中最多可以存放的元素数量,若放入Cache中的元素超过这个数值,则有以下两种情况
                             1)若overflowToDisk=true,则会将Cache中多出的元素放入磁盘文件中
                             2)若overflowToDisk=false,则根据memoryStoreEvictionPolicy策略替换Cache中原有的元素
        eternal==============缓存中对象是否永久有效,即是否永驻内存,true时将忽略timeToIdleSeconds和timeToLiveSeconds
        timeToIdleSeconds====缓存数据在失效前的允许闲置时间(单位:秒),仅当eternal=false时使用,默认值是0表示可闲置时间无穷大,此为可选属性
                             即访问这个cache中元素的最大间隔时间,若超过这个时间没有访问此Cache中的某个元素,那么此元素将被从Cache中清除
        timeToLiveSeconds====缓存数据在失效前的允许存活时间(单位:秒),仅当eternal=false时使用,默认值是0表示可存活时间无穷大
                             即Cache中的某元素从创建到清楚的生存时间,也就是说从创建开始计时,当超过这个时间时,此元素将从Cache中清除
        overflowToDisk=======内存不足时,是否启用磁盘缓存(即内存中对象数量达到maxElementsInMemory时,Ehcache会将对象写到磁盘中)
                             会根据标签中path值查找对应的属性值,写入磁盘的文件会放在path文件夹下,文件的名称是cache的名称,后缀名是data
        diskPersistent=======是否持久化磁盘缓存,当这个属性的值为true时,系统在初始化时会在磁盘中查找文件名为cache名称,后缀名为index的文件
                             这个文件中存放了已经持久化在磁盘中的cache的index,找到后会把cache加载到内存
                             要想把cache真正持久化到磁盘,写程序时注意执行net.sf.ehcache.Cache.put(Element element)后要调用flush()方法
        diskExpiryThreadIntervalSeconds==磁盘缓存的清理线程运行间隔,默认是120秒
        diskSpoolBufferSizeMB============设置DiskStore（磁盘缓存）的缓存区大小,默认是30MB
        memoryStoreEvictionPolicy========内存存储与释放策略,即达到maxElementsInMemory限制时,Ehcache会根据指定策略清理内存
                                         共有三种策略,分别为LRU(最近最少使用)、LFU(最常用的)、FIFO(先进先出)
        -->
  {%  endhighlight %}      
##使用缓存

###缓存数据

{% highlight java %}
@Cacheable(value = "myCache", key = "'setAge'+#age")
public int getAge(int age) {
    System.out.println("未使用缓存");
    return age;
}
@CacheEvict(value = "myCache", key = "'setAge'+#age")
public void setAge(int age) {
    System.out.println("重新缓存");
    this.age = age;
}
//allEntries为true表示清除value中的全部缓存,默认为false
@CacheEvict(value = "myCache", allEntries = true)
public void removeAll() {
    }
{%  endhighlight %}


###缓存指定页面
修改`web.xml`配置



{% highlight xml %}
<filter>    
        <filter-name>indexCacheFilter<filter-name>    
        <filter-class>   
            net.sf.ehcache.constructs.web.filter.SimplePageCachingFilter   
        <filter-class>  
<filter>  
<filter-mapping>   
        <filter-name>indexCacheFilter<filter-name>   
        <url-pattern>*index.action<url-pattern>  
<filter-mapping>  

{%  endhighlight %}

###缓存部分页面
{% highlight xml %}
<filter>   
        <filter-name>indexCacheFilter<filter-name>    
        <filter-class>   
            net.sf.ehcache.constructs.web.filter.SimplePageFragmentCachingFilter   
       <filter-class>    
<filter>  
<filter-mapping>    
        <filter-name>indexCacheFilter<filter-name>    
        <url-pattern>*/index_right.jsp<url-pattern>   
<filter-mapping> 
{%  endhighlight %}

##Ehcache API使用方法

`CacheManager`主要的缓存管理类，一般一个应用为一个实例,CacheManager.create();也可以使用new CacheManager的方式创建默认的配置文件为ehcache.xml文件，也可以使用不同的配置：CacheManager manager = new CacheManager("src/config/other.xml");     

{% highlight java %}
//缓存的创建，采用自动的方式
 
CacheManager singletonManager = CacheManager.create();
singletonManager.addCache("testCache");
Cache test = singletonManager.getCache("testCache");     

//或者直接创建Cache
 
CacheManager singletonManager = CacheManager.create();
Cache memoryOnlyCache = new Cache("testCache", 5000, false, false, 5, 2);
manager.addCache(memoryOnlyCache);
Cache test = singletonManager.getCache("testCache");     

//删除cache
 
CacheManager singletonManager = CacheManager.create();
singletonManager.removeCache("sampleCache1");     

//在使用ehcache后，需要关闭
 
CacheManager.getInstance().shutdown()     

//caches 的使用
 
Cache cache = manager.getCache("sampleCache1");     

//执行crud操作
 
Cache cache = manager.getCache("sampleCache1");
Element element = new Element("key1", "value1");
cache.put(element);     

//update
 
Cache cache = manager.getCache("sampleCache1");
cache.put(new Element("key1", "value1");
//This updates the entry for "key1"
cache.put(new Element("key1", "value2");     

//get Serializable
 
Cache cache = manager.getCache("sampleCache1");
Element element = cache.get("key1");
Serializable value = element.getValue();     

//get non serializable
 
Cache cache = manager.getCache("sampleCache1");
Element element = cache.get("key1");
Object value = element.getObjectValue();     

//remove
 
Cache cache = manager.getCache("sampleCache1");
Element element = new Element("key1", "value1"
cache.remove("key1"); 

{%  endhighlight %}

























