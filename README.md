Elasticsearch
===============

`Elasticsearch` 学习笔记.


### 安装

```bash
$ docker pull nshou/elasticsearch-kibana

$ docker run -d --name=es -p 9200:9200 -p 5601:5601 nshou/elasticsearch-kibana
```
> [elasticsearch-kibana](https://hub.docker.com/r/nshou/elasticsearch-kibana)


### 初始化数据

```bash
$ curl -H "Content-Type: application/json" -XPOST "localhost:9200/bank/_doc/_bulk?pretty&refresh" --data-binary "@accounts.json"
```

### 基础

1. 查询索引结构
```
GET /bank/_mapping
```

2. 获取所有数据，并根据 `account_number` 降序排序
```
GET /bank/_search
{
  "query": {
    "match_all": {}
  },
  "sort": [
    {
      "account_number": {
        "order": "desc"
      }
    }
  ]
}
```

3. 搜索年龄 20～25 岁，并按照年龄从小到大排序
```
GET /bank/_search
{
  "query": {
    "range": {
      "age": {
        "gte": 20,
        "lte": 25
      }
    }
  },
  "sort": [
    {
      "age": {
        "order": "desc"
      }
    }
  ]
}
```

4. 搜索年龄 20～25 岁，并且是女性，在 CA 或者 VA 州的
```
GET /bank/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "range": {
            "age": {
              "gte": 20,
              "lte": 25
            }
          }
        }
      ],
      "filter": [
        {
          "term": {
            "gender.keyword": "F"
          }
        }
      ],
      "should": [
        {
          "term": {
            "state.keyword": {
              "value": "CA"
            }
          }
        },
        {
          "term": {
            "state.keyword": {
              "value": "VA"
            }
          }
        }
      ],
      "minimum_should_match": 1
    }
  }
}
```
