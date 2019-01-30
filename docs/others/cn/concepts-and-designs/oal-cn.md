https://github.com/MarssZ/incubator-skywalking/blob/master/docs/en/concepts-and-designs/oal.md

# 可视化分析语言
可视化分析语言（OAL）是用来以流模式（streaming mode）来分析输入数据的。

OAL 专注于`服务``服务实例`和`终端`的信息。因此，OAL是很容易学习和使用的

考虑到性能，可读性和调试的需求，OAL被设计成一种编译语言。

OAL代码将被编译为普通的Java代码。

## 语法
代码被命名为 `*.oal`
```

METRIC_NAME = from(SCOPE.(* | [FIELD][,FIELD ...]))
[.filter(FIELD OP [INT | STRING])]
.FUNCTION([PARAM][, PARAM ...])
```

## 作用域

 **主作用域**有 `All`, `Service`, `ServiceInstance`, `Endpoint`, `ServiceRelation`, `ServiceInstanceRelation`, `EndpointRelation`.
当然还有一些次要作用域，他们属于某一个主作用域。

阅读 [作用域定义](scope-definitions-cn.md), 来了解所有的作用域和字段。


## 过滤
可以基于字段的名称和表达式来创建过滤。

表达式支持联接符`and`, `or` 和 `(...)`. 以及运算符  `=`, `!=`, `>`, `<`, `in (v1, v2, ...`, `like "%..."

在编译时会基于字段的类型做验证，如果使用了错误的表达式和运算符会报错。


## 聚合方法
SkyWalking的OAP（可视化分析平台）会提供默认的聚合方法，并能够被用以实现更多的功能。

提供的方法有：
- `longAvg`. 统计每个作用域实体的平均值，这个作用域必须是`long`类型的。例如
> instance_jvm_memory_max = from(ServiceInstanceJVMMemory.max).longAvg();
在这个例子里，我们统计了所有服务实例的JVM内存的平均值，基于的字段是 `max`.

- `doubleAvg`. 该作用域下所有实体的平均值，这个作用域字段必须是`double`类型的
> instance_jvm_cpu = from(ServiceInstanceJVMCPU.usePercent).doubleAvg();
例如CPU的占用百分比就是double类型的，所以要用`doubleAvg`来统计平均值。

- `percent`. 将一定条件下符合的条数转换为百分比
> endpoint_percent = from(Endpoint.*).percent(status == true);
例如这条显示的是status为true的endpoint的比例

- `sum`. 求合
> Service_Calls_Sum = from(Service.*).sum();

- `p99`, `p95`, `p90`, `p75`, `p50`. Read [p99 in WIKI](https://en.wikipedia.org/wiki/Percentile)
> All_p99 = from(All.latency).p99(10);
p99是一个百分位数，指的是低于该值可以找到99%的结果。在这个例子中我们得到的是99%的请求延时都不会超过某个值All_p99

> All_heatmap = from(All.latency).thermodynamic(100, 20);
thermodynamic headmap即是热力图的意思。

## 统计指标名称
统计指标的名称是用来为存储、警告与查询模块所用。而这些指标的类型推断则是由核心模块完成的。

## Group
All metric data will be grouped by Scope.ID and min-level TimeBucket. 

- In `Endpoint` scope, the Scope.ID = Endpoint id (the unique id based on service and its Endpoint)

## Examples
```
// Caculate p99 of both Endpoint1 and Endpoint2
Endpoint_p99 = from(Endpoint.latency).filter(name in ("Endpoint1", "Endpoint2")).summary(0.99)

// Caculate p99 of Endpoint name started with `serv`
serv_Endpoint_p99 = from(Endpoint.latency).filter(name like ("serv%")).summary(0.99)

// Caculate the avg response time of each Endpoint
Endpoint_avg = from(Endpoint.latency).avg()

// Caculate the histogram of each Endpoint by 50 ms steps.
// Always thermodynamic diagram in UI matches this metric. 
Endpoint_histogram = from(Endpoint.latency).histogram(50)

// Caculate the percent of response status is true, for each service.
Endpoint_success = from(Endpoint.*).filter(status = "true").percent()

// Caculate p99 of both Endpoint1 and Endpoint2
Endpoint_p99 = from(Endpoint.latency).filter(name in ("Endpoint1", "Endpoint2")).summary(0.99)

// Caculate p99 of Endpoint name started with `serv`
serv_Endpoint_p99 = from(Endpoint.latency).filter(name like ("serv%")).summary(0.99)

// Caculate the avg response time of each Endpoint
Endpoint_avg = from(Endpoint.latency).avg()

// Caculate the histogram of each Endpoint by 50 ms steps.
// Always thermodynamic diagram in UI matches this metric. 
Endpoint_histogram = from(Endpoint.latency).histogram(50)

// Caculate the percent of response status is true, for each service.
Endpoint_success = from(Endpoint.*).filter(status = "true").percent()

// Caculate the percent of response code in [200, 299], for each service.
Endpoint_200 = from(Endpoint.*).filter(responseCode like "2%").percent()

// Caculate the percent of response code in [500, 599], for each service.
Endpoint_500 = from(Endpoint.*).filter(responseCode like "5%").percent()

// Caculate the sum of calls for each service.
EndpointCalls = from(Endpoint.*).sum()

