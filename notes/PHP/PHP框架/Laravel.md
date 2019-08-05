## 前言

这里是对Laravel框架使用的总结，如果错误的还望指出



## 课程结构

![image-20190602111146814](/Users/houxiaobao/Library/Application Support/typora-user-images/image-20190602111146814.png)





## 一、Laravel 5.4 介绍



### 1. Laravel的特性

`优雅`、`简洁`、`工程化`

- 优雅：

  使用了很多设计模式，依赖注入，中间件，门脸模式，易读性

- 简洁：

  封装了很多实用的方法，比如获取当前用户信息，则直接auth::user方法就可以

- 工程化：

  PHP生态重点内容，有人认为laravel非常重，但是正是这一特性，导致laravel框架达到了工程化的标准，互联网网站生态发展到现在已经不是一个文件两个文件可以就可以解决，往往需要多个文件，多个分层，好的项目架构，那么工程化是必不可少的路程

  

### 2. Laravel的历史版本

<div align="center">
   <img src="../../../assets/laravel-history.png" width="400px">
   <img src="../../../assets/laravel-history-desc.png" width="400px">
  <br>图片来源：<a href="https://wc.yooooo.us/d2lraS9MYXJhdmVsIXpo">维基百科</a><br>
</div> 



### 描述：

- 1.0、2.0、3.0版本之间迭代时间很短，差不多几个月就迭代一个版本，说明这个时间Laravel还是很不稳定的，

- 4.0、5.0迭代的速度就以一年或两年才进行一次大的版本迭代，5.1版本较特殊，为LTS仍然被支持维护，如果有人对5.1进行提出意见和bug，就会进行修复



### 3. Laravel的优势

- Laravel的功能较为丰富
  - 队列

    支持多种队列驱动，数据库，redis，都可以支持，支持队列失败，重启，延迟等

  - 搜索

    搜索分页，搜索索引同步

  - 数据库迁移

    所有数据库创建，修改操作都可以在代码层操作，使用这个可以保证协作者开发中的字段同步，比如删了一个字段，协作者只需要执行一个脚本，就可以保持同步数据库结构

  - 定时脚本

    代码层进行管理修改定时任务

- Laravel使用了丰富的第三方包

  - [composer管理](https://www.phpcomposer.com/)

    现在很多PHP社区都已经支持composer包。合适使用包管理实战在巨人肩膀上进行开发，能更好的引入

    例：[数据填充包](https://github.com/fzaninotto/Faker/)

    测试编码格式问题，就需要引入各国语言的文字，而手动测试则会遗漏掉很多问题，这时候用现有的数据填充包去进行调试则很好解决了这个问题

- Laravel的思想更为先进

  服务容器及服务提供者，为最为核心的思想之一，也是代码层面的服务化，所有的项目用到的服务，是由服务提供者存放到容器中，当具体要使用什么服务时，直接从容器中使用就可以。

  这种思想的好处：

  ​	获取服务调用容器的人，是不需要考虑服务是谁提供的，这是一种解耦，在替换服务提供者方时就变的非常方便了，类似于工厂模式

  - 服务容器
  - 服务提供者

- Laravel的社区更为丰富

  - 国际化

    在Stack overFlow都会有很多讨论和教程，可以从上面获取到很多养分。

  - 基于Laravel的开源项目多

  - 开源

## 二、Laravel 5.4 安装

-  安装环境要求

  - PHP >= 5.6.4
  - PHP扩展
  - MySQl
  - Openssl PHP Extension
  - PDO PHP Extension
  - Mbstring PHP Extension
  - Tokenizer PHP Extension
  - XML PHP Extension

  ```shell
  # 查看PHP版本
  $ php -v
  # 查看已安装的PHP扩展包
  $ php -m
  ```

  

- Composer安装 

  - [如何安装 Composer](https://pkg.phpcomposer.com/#how-to-install-composer)

  - 创建Laravel项目

    ```shell
    $ composer create-project laravel/laravel laravel 54 "5.4.*"
    ```

    

- 启动laravel服务

  php artisan serve

-  Laravel目录结构

   ```shell
   app # 逻辑代码
   config # 配置文件
   database # 数据库管理，应用于数据迁移，数据填充
   public # 对外可见资源
   resources # 模版文件 mvc层中的view
   routes # 路由文件
   storage # 日志缓存信息
   tests # 测试用例文件，单元测试、集成测试
   vendor # 第三方包扩展文件夹
   
   注意：启动laravel项目时，启动的用户要保证和storage用户一致，也就是要保证文件夹有读写权限
   ```

   

- 配置文件说明和更改

  - databases.php `数据库配置参数文件`

    `default` 参数表示当前数据库连接默认使用某种连接方式，具体连接方式在 `connections` 参数中配置



## 三、实际开发总结

### 1. 文章模块

- 路由

  

- 模版

- 表设计

- 模型

- 页面逻辑

  - 文章列表
  - 添加文章
  - 编辑文章
  - 删除文章
  - 文章详情