   # Hive + TableStore {#concept_sqb_xrn_hfb .concept}

本章节介绍如何在 Hive 中处理 TableStore 中的数据。

## Hive接入TableStore {#section_zzs_fsn_hfb .section}

-   准备一张数据表

    创建一张表 pet，其中 name 为主键。

    |name|owner|species|sex|birth|death|
    |----|-----|-------|---|-----|-----|
    |Fluffy|Harold|cat|f|1993-02-04| |
    |Claws|Gwen|cat|m|1994-03-17| |
    |Buffy|Harold|dog|f|1989-05-13| |
    |Fang|Benny|dog|m|1990-08-27| |
    |Bowser|Diane|dog|m|1979-08-31|1995-07-29|
    |Chirpy|Gwen|bird|f|1998-09-11| |
    |Whistler|Gwen|bird| |1997-12-09| |
    |Slim|Benny|snake|m|1996-04-29| |
    |Puffball|Diane|hamster|f|1999-03-30| |

-   下面这个例子示例了如何在 Hive 中处理 TableStore 中的数据。
    1.  命令行

        ```
        $ HADOOP_HOME=YourHadoopDir HADOOP_CLASSPATH=emr-tablestore-<version>.jar:tablestore-4.1.0-jar-with-dependencies.jar:joda-time-2.9.4.jar bin/hive
        ```

    2.  创建一张外表

        ```
        CREATE EXTERNAL TABLE pet
          (name STRING, owner STRING, species STRING, sex STRING, birth STRING, death STRING)
          STORED BY 'com.aliyun.openservices.tablestore.hive.TableStoreStorageHandler'
          WITH SERDEPROPERTIES(
            "tablestore.columns.mapping"="name,owner,species,sex,birth,death")
          TBLPROPERTIES (
            "tablestore.endpoint"="YourEndpoint",
            "tablestore.access_key_id"="YourAccessKeyId",
            "tablestore.access_key_secret"="YourAccessKeySecret",
            "tablestore.table.name"="pet");
        ```

    3.  向外表中插入数据

        ```
        INSERT INTO pet VALUES("Fluffy", "Harold", "cat", "f", "1993-02-04", null);
        INSERT INTO pet VALUES("Claws", "Gwen", "cat", "m", "1994-03-17", null);
        INSERT INTO pet VALUES("Buffy", "Harold", "dog", "f", "1989-05-13", null);
        INSERT INTO pet VALUES("Fang", "Benny", "dog", "m", "1990-08-27", null);
        INSERT INTO pet VALUES("Bowser", "Diane", "dog", "m", "1979-08-31", "1995-07-29");
        INSERT INTO pet VALUES("Chirpy", "Gwen", "bird", "f", "1998-09-11", null);
        INSERT INTO pet VALUES("Whistler", "Gwen", "bird", null, "1997-12-09", null);
        INSERT INTO pet VALUES("Slim", "Benny", "snake", "m", "1996-04-29", null);
        INSERT INTO pet VALUES("Puffball", "Diane", "hamster", "f", "1999-03-30", null);
        ```

    4.  查询数据

        ```
        > SELECT * FROM pet;
        Bowser  Diane   dog     m       1979-08-31      1995-07-29
        Buffy   Harold  dog     f       1989-05-13      NULL
        Chirpy  Gwen    bird    f       1998-09-11      NULL
        Claws   Gwen    cat     m       1994-03-17      NULL
        Fang    Benny   dog     m       1990-08-27      NULL
        Fluffy  Harold  cat     f       1993-02-04      NULL
        Puffball        Diane   hamster f       1999-03-30      NULL
        Slim    Benny   snake   m       1996-04-29      NULL
        Whistler        Gwen    bird    NULL    1997-12-09      NULL
        > SELECT * FROM pet WHERE birth > "1995-01-01";
        Chirpy  Gwen    bird    f       1998-09-11      NULL
        Puffball        Diane   hamster f       1999-03-30      NULL
        Slim    Benny   snake   m       1996-04-29      NULL
        Whistler        Gwen    bird    NULL    1997-12-09      NULL
        ```


## 数据类型转换 {#section_gn4_5tn_hfb .section}

| |TINYINT|SMALLINT|INT|BIGINT|FLOAT|DOUBLE|BOOLEAN|STRING|BINARY|
|--|-------|--------|---|------|-----|------|-------|------|------|
|INTEGER|支持, 损失精度|支持, 损失精度|支持, 损失精度|支持|支持, 损失精度|支持, 损失精度| | | |
|DOUBLE|支持, 损失精度|支持, 损失精度|支持, 损失精度|支持, 损失精度|支持, 损失精度|支持| | | |
|BOOLEAN| | | | | | |支持| | |
|STRING| | | | | | | |支持| |
|BINARY| | | | | | | | |支持|

## 附录 {#section_ezh_dvn_hfb .section}

完整示例代码请参见：

-   [Hive+TableStore](https://github.com/aliyun/aliyun-emapreduce-sdk/blob/master/examples/src/main/java/com/aliyun/openservices/tablestore/pet.sql)

