# Request Search Body

#### Query DSL
```http
// 查询索引所有内容
// 分页
// 根据 order_data 字段排序
// 返回 _source 中指定的字段, 支持通配符
POST /index_name,index_name,.../_search?ignore_unavailable=true
{
    "profile": true,
    "query": {
        "match_all": {}
    }
    "from": 1,
    "size": 10,
    "sort": [{"order_data": "desc"}],
    "_source": ["order_data", "owner", "class"]
}
```

#### Script fields
```http
GET /index_name/_search
{
    "script_fields": {
        "new_fields": {
            "script": {
                "lang": "painless",
                "source": "doc['order_date'].value+'_hello'"
            }
        }
    },
    "query": {
        "match_all": {}
    }
}
```

#### 使用表达式查询
```http
// Last Or One
GET /index_name/_search
{
    "query": {
        "match": {
            "title": "Last One"
        }
    }
}
```
```http
// Last AND One
GET /index_name/_search
{
    "query": {
        "match": {
            "title": {
                "query": "Last One",
                "operator": "AND"
            }
        }
    }
}
```
#### 使用短语查询
```http
// Last One 前后左右可以多一个单词匹配
GET /index_name/_search
{
    "query": {
        "match_phrase": {
            "title": {
                "query": "Last One",
                "slop": 1
            }
        }
    }
}
```
#### Query string && Simple Query string
```http
POST /index_name/_search
{
    "query": {
        "query_string": {
            "default_fields": "name",
            "query": "Ruan AND Yiming"
        }
    }
}
```
```http
// 不支持 AND OR NOT, 会当作字符串处理
// Term之间默认的关系是OR, 可以指定Operator
POST /index_name/_search
{
    "query": {
        "simple_query_string": {
            "query": "Runa -Yiming",
            "fields": ["name"],
            "default_operator": "AND"
        }
    }
}
```