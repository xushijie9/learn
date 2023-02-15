# 控制 Dynamic Mappings

|            | true |false|strict|
|    ----    | ---- | ----| ---- |
|  文档可索引  | YES | YES  |  NO  |
|  字段可索引  | YES |  NO  |  NO  |
|Mapping被更新| YES |  NO  |  NO  |

##### 1.当dynamic被设置成false的时候，存在新增字段的数据写入，该数据可以被索引，但是新增字段被丢弃
##### 2. 当设置成Strict模式的时候，数据写入直接出错

### Mapping setting
```http
// index = false mobile 不可被查询
PUT index_name
{
    "mappings": {
        "properties": {
            mobile: {
                "type": "text",
                "index": false
            }
        }
    }
}
```

#### copy to
```http
// copy_to 字段不会存在 _source 中，只能查询时指定
// class 字段指定 keyword 不分词
// url 字段指定分词规则
PUT index_name
{
    "mappings": {
        "properties": {
            mobile: {
                "type": "text",
                "index": false
            },
            "firstName": {
                "type": "text",
                "copy_to": "fullName"
            },
             "lastName": {
                "type": "text",
                "copy_to": "fullName"
            },
            "class": {
                "type": "keyword",
                "index": true,
            },
            "url": {
                "type": "test",
                "analyzer": "ik_max_word"
            }
        }
    }
}
```

**注:
  index options
  dynamic Template**