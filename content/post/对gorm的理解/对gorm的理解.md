---
title: 对gorm的理解
description: 持续更新中
date: 2024-10-16T22:01:54+08:00
slug: 对gorm的理解
image: https://ucarecdn.com/9aa429b5-25e4-432f-a9f6-899820700e41/
categories:
  - gorm
  - 理解输出
---

# 对gorm的理解
## Gorm简介:以下内容来自[GORM指南](https://gorm.io/zh_CN/docs/index.html)
Gorm 是 Go 语言中常用的 ORM（对象关系映射）框架，帮助开发者以面向对象的方式操作数据库，简化了 SQL 语句的编写。
### 特点：
* 全功能 ORM
* 关联 (Has One，Has Many，Belongs To，Many To Many，多态，单表继承)
* Create，Save，Update，Delete，Find 中钩子方法
* 支持 Preload、Joins 的预加载
* 事务，嵌套事务，Save Point，Rollback To Saved Point
* Context、预编译模式、DryRun 模式
* 批量插入，FindInBatches，Find/Create with Map，使用 SQL 表达式、Context Valuer 进行 CRUD
* SQL 构建器，Upsert，数据库锁，Optimizer/Index/Comment Hint，命名参数，子查询
* 复合主键，索引，约束
* Auto Migration
* 自定义 Logger
* 灵活的可扩展插件 API：Database Resolver（多数据库，读写分离）、Prometheus…
* 每个特性都经过了测试的重重考验
开发者友好
### 核心功能：
* 模型定义：通过定义结构体，Gorm 可以将 Go 的结构体映射为数据库中的表，如字段类型、约束等都可以通过结构体的标签（Tag）定义。
* 查询操作：支持常见的查询操作，如 First() 查找第一条记录、Find() 查找多条记录，还可以通过 Where() 方法实现条件查询。
* 关联查询：Gorm 支持模型之间的一对一、一对多、多对多等关联关系，提供了 Preload() 和 Joins() 等方法实现关联数据的查询。
* 创建、更新、删除：Gorm 提供了简便的创建、更新和删除数据的方法，如 Create() 创建记录，Save() 保存或更新记录，Delete() 删除记录。
### 事务管理：
支持通过 Transaction() 方法进行事务管理，开发者可以在同一个事务中执行多个数据库操作，确保数据的一致性。
### 适用场景：
* Gorm 适用于 Go 项目中与数据库交互频繁的场景，特别是需要简化数据库操作并保证代码可读性的项目。
* 由于 Gorm 提供了丰富的 ORM 功能，适合用在需要与多个表进行复杂交互的项目中，比如电商平台、企业级管理系统等。

## 我对GORM的使用
1. 安装
```
go get -u gorm.io/gorm
go get -u gorm.io/driver/sqlite
```
2. 文件中导入要使用的包(ps:这里以sqlite为例)
```
import (
	"gorm.io/driver/sqlite"
	"gorm.io/gorm"
)
```
3. 打开数据库连接
```
DB, err := gorm.Open(sqlite.Open("test.db"), &gorm.Config{})
if err != nil {
	fmt.Println("连接数据库失败：", err)
	return
}
```
4. 迁移
```
DB.AutoMigrate(&StudentNew{})
``` 
5. 对数据库的操作：
  * 查询
```
if err := DB.Where("studentID = ?", studentID).First(&student).Error; err != nil {
	fmt.Println("查询学生ID失败:", err)
	return false
}
```
  * 插入
```
result := DB.Create(&student)
```
  * 删除
```
if err := utils.DB.Where("ksh = ?", id).Delete(&stu).Error; err != nil {
	ctx.JSON(500, gin.H{
		"error": "Failed to delete record",
	})
	return
}
```
  * 修改
```
if err := utils.DB.Model(&user).Where("ksh = ?", id).Updates(stu).Error; err != nil {
	c.JSON(500, gin.H{
		"error": "Failed to update record",
	})
	return
}
```
#### 未完待续
