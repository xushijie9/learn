#### Inside the JVM Heap
* Elasticsearch 的缓存主要分成三大类
    - Node Query Cache(filter context)
    - Shard Query Cache(cache query的结果)
    - Fielddata Cache

#### Node Query Cache
* 每一个节点有一个Node Query 缓存
    - 由该节点的所有Shard共享,只缓存Filter Context相关内容
    - Cache 采用 LRU 算法
* 静态配置, 需要设置在每个Data Node上
    - Node Level - indices.queries.cache.size: “10%”
    - Index Level - index.queries.cache,enabled: true 

#### Shard Request Cache
* 缓存每个分片上的查询结果
    - 只会缓存设置了 size=0 的查询对应的结果, 不会缓存hits, 但是会缓存Aggregations 和 Suggestions
* Cache Key
    - LRU算法, 将整个JSON查询串作为Key,与JSON对象顺序相关
* 静态配置
    - 数据节点 indices.requests.cache.size: “1%”

#### Fielddata Cache
* 除了 Text 类型, 默认都采用 doc_values, 节约了内存
    - Aggregation 的 Global ordinals 也保存在Fielddata Cache中
* Text类型的字段需要打开Fileddata才能对其进行聚合和排序
    - Text 经过分词,排序和聚合效果不佳,建议不要轻易使用
* 配置
    - 可以控制 indices.fielddata.cache.size 避免产生GC,默认无限制

#### 缓存失效
* Node Query Cache
    - 保存的是Segment级缓存命中的结果,Segment被合并后,缓存会丢失
* Shard Query Cache
    - 分片Refresh时候,Shard Query Cache会失效,如果Shard对应的数据频繁发生变化,该缓存的效率会很差
* Fielddata Cache
    - Segment 被合并后,会失效

#### 管理内存的重要性
* Elasticsearch 高效运维依赖于内存的合理分配
    - 可用内存一半分配给JVM, 一半留给操作系统, 缓存索引文件
* 内存问题,引发的问题
    - 长时间GC,影响节点,导致集群响应缓慢
    - OOM,导致节点丢失