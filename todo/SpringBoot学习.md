* ***10:30 ~ 11:53*** 信用住支付产品需求评审会

[Accessing Relational Data using JDBC with Spring](https://spring.io/guides/gs/relational-data-access/)

* ***13:59 ~ 14:23*** [Accessing data with MySQL](https://spring.io/guides/gs/accessing-data-mysql/) 别的项目没有用 ```org.springframework.boot:spring-boot-starter-data-jpa``` 或者 ```org.springframework:spring-jdbc```
* ***14:27 ~ 15:12*** ```MyBatis-Spring-Boot-Starter``` (以前项目用的 ***1.3.2*** ，现在版本是 ***2.1*** ，兼容 Spring Boot >= 2.1 )
	* ***15:10*** https://blog.csdn.net/weixin_42074377/article/details/82493301
	* ***15:12*** 解决了

* 15:47 JPA 和 MyBatis 的对比
	* ***Eric Evans*** [的领域驱动设计(DDD:Domain-Driven Design)](https://www.jdon.com/ddd.html)，其对应传统的 ***CRUD*** 或 ***过程脚本*** 或者 ***面向数据表*** 等。领域驱动设计中有两个广为大家熟知的概念，entity（实体）和 value object（值对象）。
	* 仓储 Repository 模式是领域驱动设计中另一个经典的模式。在早期，我们常常将数据访问层命名为：***DAO***，而在 SpringData JPA 中，其称之为 Repository（仓储），这也不是巧合，而是设计者有意为之。
	* JPA 需要为数据库的实体类添加 ```@Entity``` (还有 ```@Table``` ```@Id``` ```@OneToMany``` )。

https://blog.csdn.net/yangsnow_rain_wind/article/details/79650616

按照 MBG 官方命名：http://mybatis.org/generator/configreference/xmlconfig.html
分别为：
javaModelGenerator: model
sqlMapGenerator: xml
javaClientGenerator: dao

druid 就用到了 mybatis: https://github.com/alibaba/druid/tree/master/src/main


* 17:51

```
$ curl -X POST -H"Content-Type: application/json" --data '{"merOrderNo":"123", "id": 1}' http://localhost:8080/agency
{"timestamp":"2019-09-24T09:46:56.369+0000","status":406,"error":"Not Acceptable","message":"Could not find acceptable representation","path":"/agency"}

Resolved [org.springframework.web.HttpMediaTypeNotAcceptableException: Could not find acceptable representation]
```

18:11 [在 Spring 中集成 Fastjson](https://github.com/alibaba/fastjson/wiki/%E5%9C%A8-Spring-%E4%B8%AD%E9%9B%86%E6%88%90-Fastjson)








