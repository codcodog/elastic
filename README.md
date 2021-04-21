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

查询索引结构
```
GET /bank/_mapping
```

获取所有数据，并根据 `account_number` 降序排序
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
