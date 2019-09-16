### ElasticSearch 开发经验
#### 环境
1. 版本兼容
    1. 高版本兼容低版本 : 高版本的服务器支持低版本的客户端的请求(必须是在同一个大版本下)

#### es 服务端脚本
  ```
    //创建活跃设备统计索引
    PUT /active_device
    {
        "mappings": {
        "_doc": {
            "properties": {
            "count": {
                "type": "long"
            },
            "interval": {
                "type": "long"
            },
            "productId": {
                "type": "long"
            },
            "time": {
                "type": "date"
            }
            }
        }
        }
        }
    }

    // 清空数据,并没有删除索引
    POST active_device / _delete_by_query
    {
    "query": {
        "match_all": { }
    }
    }

    //删除索引
    delete /uplink_data
    {

    }

    //添加字段;修改mapping
    PUT / device_info / _mappings / _doc
    {
    "properties": {
        "activeAt": {
        "type": "date"
        },

        "createProtocol": {
        "type": "long"
        },
        "deviceStatus": {

        "type": "long"

        }
    }
    }
  ```
#### 开发
2. CURD
    1. create
        1. 创建连接 
            ```
            //创建RESTAPI client
            RestHighLevelClient client = new RestHighLevelClient(
            RestClient.builder(new HttpHost("localhost", 9200, "http"),
            new HttpHost("localhost", 9201, "http")));
            // 关闭连接
            client.close();
            ```
        2. 插入数据
    2. update
        1. 更新数据
        2. 更新mapping
    3. read
        1. 查询数据 : 
            1. query Builder
                1. QueryBuilders
                2. [BoolBuilder](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-bool-query.html)
                3. RangeQueryBuilder
                4. TermQueryBuilder 
            2. search 
                1. Builder
                    5. AggregationBuilders
                    6. DateHistogramAggregationBuilder
                    7. SearchSourceBuilder
            3. aggregation
                1. [History Aggregation](https://www.elastic.co/guide/en/elasticsearch/reference/6.2/search-aggregations-bucket-histogram-aggregation.html)
        2. 分页
            1. from -size
            2. 通过游标
            3. 
    4. delete
        1. 删除索引
        2. 删除mapping
        3. 删除数据
    5. batch


