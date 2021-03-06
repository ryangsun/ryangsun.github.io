---
title: Table2Entry将建表语句转义为实体
date: 2021-07-02 15:19:11
tags: [java,Table2Entry]
categories: [Table2Entry]
comments: false
---
Table2Entry可以方便的将mysql建表语句转化为实体对象，进而帮助你快速映射实体。

## Table2Entry源码地址

- https://github.com/ryangsun/table2entry

## 包引用
### 正常使用
```
<dependency>
    <groupId>com.bumao.model</groupId>
    <artifactId>table2entry</artifactId>
    <version>0.0.1</version>
</dependency>
```
### 包冲突的处理
```
<dependency>
    <groupId>com.bumao.model</groupId>
    <artifactId>table2entry</artifactId>
    <version>0.0.1</version>
    <exclusions>
        <!--druid-->
        <exclusion>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
        </exclusion>
        <!--lombok-->
        <exclusion>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </exclusion>
        <!--slf4j-api-->
        <exclusion>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-api</artifactId>
        </exclusion>
    </exclusions>
</dependency>

```

## Table2Entry的使用
```
String sql = "CREATE TABLE `card` (\n" +
        "  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT COMMENT 'PK',\n" +
        "  `batch_id` int(11) NOT NULL COMMENT '所属批次ID',\n" +
        "  `denomination` decimal(10,2) NOT NULL DEFAULT '0.00' COMMENT '面额',\n" +
        "  `card_no` varchar(20) NOT NULL COMMENT '实体卡号',\n" +
        "  `card_pass` varchar(20) NOT NULL COMMENT '实体密码',\n" +
        "  `status` tinyint(3) unsigned NOT NULL DEFAULT '0' COMMENT '状态，0=未核销 1=已核销',\n" +
        "  `active_time` datetime DEFAULT NULL COMMENT '核销核销时间',\n" +
        "  `active_userid` varchar(32) DEFAULT NULL COMMENT '核销用户ID',\n" +
        "  `active_mobile` varchar(20) DEFAULT NULL COMMENT '核销时用户的手机号码',\n" +
        "  `create_time` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',\n" +
        "  `stop_time` datetime NOT NULL COMMENT '结束核销时间',\n" +
        "  `active_openid` varchar(64) DEFAULT NULL COMMENT '用户openid',\n" +
        "  PRIMARY KEY (`id`) USING BTREE,\n" +
        "  KEY `batch_id` (`batch_id`) USING BTREE,\n" +
        "  KEY `status` (`status`) USING BTREE\n" +
        ") ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COMMENT='卡券批次表';" +
        "select * from card;";
ToEntry toEntry = new ToEntry();
List<TableEntryVo> tableEntryVos = toEntry.getEntry(sql);
log.info("tableEntryVos={}", JSONArray.toJSON(tableEntryVos));
```
- 可同时转义多条建表语句，请用“;”隔开。
- 会忽略建表语句以外的sql。

## TODO

~~针对Mysql建表语句的转义~~

针对Oracle建表语句的转义

针对MsSQL建表语句的转义
