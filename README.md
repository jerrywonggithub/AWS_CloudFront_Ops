# CloudFront

## DEMO page

https://dnm30dd8vm9g7.cloudfront.net/index.html


### 预热

>pop.list: 需预热的pop节点， 从 https://www.feitsui.com/en/article/3 中拿

>prewarm_top50.list 就是要预热的文件列表

```
#!/bin/bash
cf_instance_host="d3ntgasp7s0lac"
for pop in `cat ./pop.list`
do
for file in `cat ./prewarm_top50.list`
do
curl -H "Host:${cf_instance_host}.cloudfront.net" -v -o /dev/null http://${cf_instance_host}.$pop.cloudfront.net/$file 1>>out.log 2>&1
done
done
```

[pop.list](https://api.quip-amazon.com/2/blob/OOS9AA2ob8y/zefL3Ovlu2pPHbvI49EF7Q?name=pop.list&oauth_token=TGFZOU1BZk42dEg%3D%7C1686289711%7CogJO3q5PUELeT8lEYnp%2Fay%2Bz01wgusCVskXWdcl0sOc%3D&s=JV8fA9ab5PQS) [prewarm.list](https://api.quip-amazon.com/2/blob/OOS9AA2ob8y/AkEgURGUVRGfRJNLS7anXg?name=prewarm.list&oauth_token=TGFZOU1BZk42dEg%3D%7C1686289711%7CogJO3q5PUELeT8lEYnp%2Fay%2Bz01wgusCVskXWdcl0sOc%3D&s=JV8fA9ab5PQS)

### 失效请求

```
import boto3
client = boto3.client('cloudfront')
response = client.create_invalidation(
    DistributionId='E32W5ITLCXXXXX',
    InvalidationBatch={
        'Paths': {
            'Quantity': 1, //Quantity数量根据路径数量，/*为一个路径
            'Items': [
                '/*',
            ]
        },
        'CallerReference': '20211808' //每次请求需携带唯一的CallerReference
    }
)
```

