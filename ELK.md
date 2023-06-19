----
# 背景：秒开优化


## **数据查询方式**

### elasticsearch-sql


```sql
SELECT 
**
FROM **
where pageName like "%text%"
order by time
```

**

```json
-
```

## 秒开率的聚合统计

| 统计秒开率 | e-sql  | sql     | kibana | 最终选择 |
| --------------------------------------------------------- | ------ | ------- | ------ | -------- |
| split 出域名                                              | 不支持 |         | 不支持 | 后端支持 |
| 百分比除法计算                                            |    不支持    | 难度+++ |   percentile rank 代替     | kibana   |
| 不同时间段聚合对比                                        | 不支持 | 难度+++ |   easy     | kibana   |


### IDE 数据开发平台 

```sql
SELECT 
    DATE_TRUNC('week', your_date_column) AS week_start_date,
    domain,
    AVG(loadtime) AS average_load_time,
    COUNT(CASE WHEN loadtime <= 1500 THEN 1 END) / COUNT(*) AS within_1500ms_percentage
FROM 
    your_table_name
GROUP BY 
    DATE_TRUNC('week', your_date_column),
    domain;
```



### Kibana 

**

# elastic 基本概念


**别名**  合并多个 **索引**  
**映射** 涉及 **类型**
**文档** 就是一条数据
**分片** 可以打开关闭
支持 API

![[Pasted image 20230618183714.png]]
 
- **映射**指的是在搜索引擎或数据库中定义文档结构的过程。它描述了文档中字段的类型、属性和索引方式等信息。映射告诉搜索引擎如何处理文档中的每个字段，包括其数据类型、分析器等。

- **类型**是指在映射中为文档字段定义的数据类型，例如文本、数字、日期等。类型决定了字段如何进行索引、搜索和排序。
    
- **文档**是一条数据记录，类似于数据库中的一行数据。它可以包含多个字段，每个字段存储特定类型的数据。
    
- **分片**是将一个大型的数据库或数据集划分成小的块（片段）的过程。每个分片都，可以分布在不同的物理节点上。分片允许数据水平扩展，提高并行性和容错性。分片可以根据需要进行打开或关闭，以便控制数据的存储和查询操作。


# 搜索

## KQL


![[Pasted image 20230618155914.png]]


## DSL

### 基础

#### term
```json
{
  "query": {
    "term": {
      "DeviceOsType": "Android",
    }
  }
}
```

#### terms
```json
{
  "query": {
    "terms": {
      "DeviceOsType": [ "Android", "iOS" ],
      "boost": 1.0
    }
  }
}
```

#### range
```json
{
  "range": {
    "loadTime": {
      "gte": 0,
      "lt": 2000
    }
  }
}
```

#### exist

```json
{
  "exists": {
    "field": "h5domain"
  }
}
```

#### prefix

```json

{
  "query": {
    "prefix": {
      "h5domain": {
        "value": "fs"
      }
    }
  }
}

```

#### regexp

```json
{
  "query": {
    "regexp": {
      "h5domain": {
        "value": "f.*"
      }
    }
  }
}
```

#### wildcard

```json
{
  "query": {
    "wildcard": {
      "h5domain": {
        "value": "f*",
        "boost": 1.0,
        "rewrite": "constant_score"
      }
    }
  }
}
```

```
- `?`, which matches any single character
- `*`, which can match zero or more characters, including an empty one


Avoid beginning patterns with `*` or `?`. This can increase the iterations needed to find matching terms and slow search performance.
```

### 全文搜索

#### match

```json
{
  "query": {
    "match": {
      "h5domain": {
        "query": ".com.cn",
        "type": "phrase"
      }
    }
  }
}
```


#### query_string

```json
{
	"query_string": {
		"query": "AppID : \"2\"",
		"analyze_wildcard": true,
		"default_field": "*"
	}
}
```



### 复合查询

#### must

#### filter

#### bool

#### must not


# 聚集

## 度量聚集

平均值、最大/小值、总和等

**_百分比聚集_**

**_百分比等级聚集_**

```json
{
	"percentile_ranks": {
		"field": "loadTime", 
		"values": [1500], 
		"keyed": false
	}
}

```
## 桶聚集

词条聚集，范围聚集，日期聚集

```json
{
	 "date_histogram": {             
		 "field": "srvtime",            
		 "interval": "1w",             
		 "time_zone": "Asia/Shanghai",             
		 "min_doc_count": 1           
	}
}
```

## 管道聚集

累计求和

# 案例

fs-image-analyse

```javascript

const { Client } = require('@elastic/elasticsearch');

const client = new Client({

	node: 'http://**:8080/elasticsearch',

	maxRetries: 5,

	requestTimeout: 600000,

	sniffOnStart: true,

	headers: {

		'kbn-xsrf': '**',

	},

});

```



```javascript 

// 执行Elasticsearch搜索

const { body } = await fastify.elastic.msearch({

body: [

	{
	
		index: !_index ? '*' : `${_index}`,   // 分片在线一天
	
	},
	
	{ ...DSL },
	
	],

});

```



```mermaid
graph LR
B(接口 1 获取ES聚合统计结果)
B --> C(处理并保存为JSON文件)
C --> D(接口 2 返回任意时间数据)
````

# 安装

https://www.elastic.co/guide/en/kibana/current/docker.html


验证是可选的；自定义密码、修改语言和分词参考 https://blog.csdn.net/m0_52070517/article/details/128795042


```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✅ Elasticsearch security features have been automatically configured!
✅ Authentication is enabled and cluster connections are encrypted.

ℹ️  Password for the elastic user (reset with `bin/elasticsearch-reset-password -u elastic`):
  CltBo*AGvQApUylmoYjr

ℹ️  HTTP CA certificate SHA-256 fingerprint:
  7cdfbfa3273474c857dba1ac27e0523379aa242ed7fb4d5c0da9b98100884ca1

ℹ️  Configure Kibana to use this cluster:
• Run Kibana and click the configuration link in the terminal when Kibana starts.
• Copy the following enrollment token and paste it into Kibana in your browser (valid for the next 30 minutes):
  eyJ2ZXIiOiI4LjguMSIsImFkciI6WyIxNzIuMjIuMC4yOjkyMDAiXSwiZmdyIjoiN2NkZmJmYTMyNzM0NzRjODU3ZGJhMWFjMjdlMDUyMzM3OWFhMjQyZWQ3ZmI0ZDVjMGRhOWI5ODEwMDg4NGNhMSIsImtleSI6Im5lZXZ6WWdCQjc5OTZzRzZBdXJIOk9CdUttXzViU0xTT2NvUVhaQTR2REEifQ==

ℹ️ Configure other nodes to join this cluster:
• Copy the following enrollment token and start new Elasticsearch nodes with `bin/elasticsearch --enrollment-token <token>` (valid for the next 30 minutes):
  eyJ2ZXIiOiI4LjguMSIsImFkciI6WyIxNzIuMjIuMC4yOjkyMDAiXSwiZmdyIjoiN2NkZmJmYTMyNzM0NzRjODU3ZGJhMWFjMjdlMDUyMzM3OWFhMjQyZWQ3ZmI0ZDVjMGRhOWI5ODEwMDg4NGNhMSIsImtleSI6Im0tZXZ6WWdCQjc5OTZzRzZBdXEtOkNYZy1hVFBtU2FTRFJjbkpfLW50N3cifQ==

  If you're running in Docker, copy the enrollment token and run:
  `docker run -e "ENROLLMENT_TOKEN=<token>" docker.elastic.co/elasticsearch/elasticsearch:8.8.1`
```



```
docker cp kib-01:/usr/share/kibana/config/kibana.yml ./
docker cp kibana.yml kib-01:/usr/share/kibana/config/
```



https://play.grafana.org/d/T512JVH7z/loki-nginx-service-mesh-json-version?orgId=1



