

# ElasticSearch命令

## 索引操作

### 创建
```json
PUT myindex

PUT myindex 
{
    "settings": {
        "number_of_shards": 2,     //主分片大小
        "number_of_replicas": 1    //副本数
    },
    "mappings": {
        "properties": {
            "cont": {
                "type": "text",
                "fields": {
                    "field": {
                        "type": "keyword"
                    }
                }
            }
        }
    },
    "aliases": {         //索引别名
        "myindex": {}
    }
}
```

### 删除
```json
DELETE myindex

POST myindex/_delete_by_query
{
    "query": {
        "match_all": {}
    }
}
```

### 查询
```json
GET myindex            //获取索引的基础信息
GET myindex/_search   //获取索引的数据信息
```


## 别名操作

### 增加
```json
POST /_aliases
{
    "actions": [
        {
            "add": {
                "index": "myindex_1",
                "alias": "my_index_alias"
            }
        }
    ]
}
```

### 查询
```json
GET _cat/aliases?v
```

## 模版操作

### 创建
```json
PUT _index_template/template_1
{
    "index_patterns": {
        "te*",
        "bar*"
    },
    "template": {
        "settings": {
            "number_of_shards": 1
        },
        "mappings": {
            "_source": {
                "enabled": false
            },
            "properties": {
                "host_name": {
                    "type": "keyword",
                },
                "created_at": {
                    "type": "date",
                    "format": "EEE MMM dd HH:mm:ss Z yyyy"
                }
            }
        }
    }
}
```

### 删除
```json
DELETE _index_template/template_1
```

### 查询
```json
GET _index_template/template_1
```


## 文档操作

### 新增
```json
PUT myindex/_doc/<id>    //id为文档ID, 可以不指定
{
    <content>
}
```

### 批量新增
```json
POST myindex/_bulk
{ "index": { "_id": 1} }
{ <content1> }
{ "index": { "_id": 2} }
{ <content2> }
{ "index": { "_id": 3} }
{ <content3> }
```

### 删除
```json
DELETE myindex/_doc/<id>
```


### 批量删除
```json
POST myindex/_delete_by_query
{
    "query": {
        "match": {
            "message": "xxxx"
        }
    }
}
```


### 新增字段

   


   