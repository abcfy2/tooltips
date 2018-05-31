# Grails

在微服务如此流行的今天，我们使用Grails来构建后台服务。下面的这些技巧都针对与rest-api项目而言。

// TODO：提现下列技巧的grails工程模板。

## GORM

- Postgresql特定技巧：
  - Plugin: [grails-postgresql-extensions](https://github.com/kaleidos/grails-postgresql-extensions)
  - Domain的自增ID被映射到PG的serial类型
  ~~~
        grails.gorm.default.mapping = {
            id generator: 'identity'
        }
  ~~~
- 把“failOnError”设置为True。
- 用 [Hikari 连接池](https://github.com/brettwooldridge/HikariCP).
- 使用产品环境中要用的数据库进行测试，这样可尽早发现数据库相关的bug。
- “dbCreate”的值：
  - development & production: none，使用db migration。
  - test: create-drop
- 数据库的字符集和编码使用unicode。
- 不要用数据库的admin账户来连接，创建专门用于应用的账户并赋予足够的权限。
- 给Domain的列加上注释，如：
~~~
    class Order {

        User buyer
        Map address
        LocalDateTime dateCreated

        static constraints = { ... }

        static mapping = {
            comment '订单'
            buyer comment: '用户', index: 'idx_order_user_id'
            address comment: '收货地址'
            dateCreated comment: '创建时间'
        }

    }
~~~
- 使用“idx_$table”做表索引的前缀：
  - 单列：idx_$table_$col
  - 多列：idx_$table_$name
- 对于货币列，用整数，如long，单位为“分”。
- 对于insert only domain class，关闭version。
~~~
    static mapping = {
        version false
    }
~~~
- 在Domain中使用java8 datetime类型。
- 打开“showSql”和“formatSql”来调试Hibernate产生的sql。

## 测试

## Db Migration

## 安全