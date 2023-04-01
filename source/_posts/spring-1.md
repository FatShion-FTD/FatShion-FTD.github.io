---
title: spring-1
date: 2022-12-17 02:56:08
tags:
keywords:
---

# Mybatis
不想自己写SQL? 不想和JDBC打交道? SQL语句自定义? 想直接返回对象集而不是ResultSet? 

那就用Mybatis吧, 全自动映射, 半自动框架, 自定义各种你想要的.

Mybatis 数据库访问框架

## 好处
1. 把混在Java代码里的JDBC去掉了
2. 手动写SQL比Hibernate自动生成稳定

## 配置
Eclipse + SQLite + Maven + Mybatis

Maven工程内核心如下:

```xml
<dependencies>
		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<version>4.11</version>
			<scope>test</scope>
		</dependency>
		<dependency>
			<groupId>org.mybatis</groupId>
			<artifactId>mybatis</artifactId>
			<version>3.5.11</version>
		</dependency>
		<dependency>
			<groupId>org.xerial</groupId>
			<artifactId>sqlite-jdbc</artifactId>
			<version>3.8.7</version>
		</dependency>
	</dependencies>
```