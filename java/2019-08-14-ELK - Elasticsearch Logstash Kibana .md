# ELK - Elasticsearch Logstash Kibana 

## Elasticsearch - Distributed, RESTful search and analytics

* Version: 7.3.0, Release date: 2019-08-01

## Kibana - Visualize your data. Navigate the Stack

### 倒排索引 inverted index

***forward index*** 翻译成 ***正排索引*** 或 ***前向索引*** 。它是创建倒排索引的基础。

### 快速入门

```bash
$ bin/elasticsearch-plugin install https://github.com/medcl/elasticsearch-analysis-ik/releases/download/v7.3.0/elasticsearch-analysis-ik-7.3.0.zip
```

#### Data in: documents and indices
> By default, Elasticsearch indexes all data in every field and each indexed field has a dedicated, optimized data structure. For example, text fields are stored in inverted indices, and numeric and geo fields are stored in BKD trees. 

文本使用倒排索引。数字和地理位置使用 BKD 树。

> Defining your own mappings enables you to:
> 
> * Distinguish between full-text string fields and exact value string fields
> * Perform language-specific text analysis
> * Optimize fields for partial matching
> * Use custom date formats
> * Use data types such as ***geo_point*** and ***geo_shape*** that cannot be automatically detected

#### Information out: search and analyze

> The Elasticsearch REST APIs support structured queries, full text queries, and complex queries that combine the two. 

***Structured queries***结构化检索类似于***SQL***，***Full-text queries***即全文检索。

> In addition to searching for individual terms, you can perform phrase searches, similarity searches, and prefix searches, and get autocomplete suggestions.

可以使用***JSON***格式查询语句：[Query DSL](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl.html)，和 [SQL-style queries](https://www.elastic.co/guide/en/elasticsearch/reference/current/sql-overview.html)

***Kibana***是一个开源的分析和可视化平台，设计用于和***Elasticsearch***一起工作。你用***Kibana***来搜索，查看，并和存储在***Elasticsearch***索引中的数据进行交互。

#### Scalability and resilience 恢复力、容错性

> Scalability and resilience: clusters, nodes, and shards

shards翻译为“分片”。shard[ʃɑrd]原意为“陶瓷碎片”。

> Under the covers, an Elasticsearch index is really just a logical grouping of one or more physical shards, where each shard is actually a self-contained index. 

> There are two types of shards: primaries and replicas. A replica shard is a copy of a primary shard. Replicas provide redundant copies of your data to protect against hardware failure and increase capacity to serve read requests like searching or retrieving a document.

replica['rɛplɪkə]原意为“复制品”，这里意为“从库”。replication原意为“复制”。

> As with any enterprise system, you need tools to secure, manage, and monitor your Elasticsearch clusters. Security, monitoring, and administrative features that are integrated into Elasticsearch enable you to use Kibana as a control center for managing a cluster. Features like [data rollups](https://www.elastic.co/guide/en/elasticsearch/reference/current/rollup-overview.html) and [index lifecycle management](https://www.elastic.co/guide/en/elasticsearch/reference/current/index-lifecycle-management.html) help you intelligently manage your data over time.

## Beats - Collect, parse, and ship in a lightweight fashion


## References

* [Elasticsearch Reference][Elasticsearch Reference]
* [全文搜索引擎 Elasticsearch 入门教程][全文搜索引擎 Elasticsearch 入门教程]
* [正排索引(forward index)与倒排索引(inverted index)][正排索引(forward index)与倒排索引(inverted index)]

[Elasticsearch Reference]: https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html "Elasticsearch Reference"
[全文搜索引擎 Elasticsearch 入门教程]: http://www.ruanyifeng.com/blog/2017/08/elasticsearch.html "全文搜索引擎 Elasticsearch 入门教程"
[正排索引(forward index)与倒排索引(inverted index)]: https://blog.csdn.net/garfielder007/article/details/50479074 "正排索引(forward index)与倒排索引(inverted index)"