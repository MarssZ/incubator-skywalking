# 作用域和字段
使用聚合方法，可以让请求结果按时间或是**Group Key(s)**分组

### 作用域 `All`

| Name | Remarks | Group Key | Type | 
|---|---|---|---|
| endpoint  | 表示每个请求的端点path |   | string |
| latency  | 表示每个请求所花费的时间 |   |  int(in ms)  |
| status  | 表示请求成功或是失败 |   | bool(true for success)  |
| responseCode | 表示HTTP请求的返回码. 例如 200, 404, 302| | int |


### 作用域 `Service`

Calculate the metric data from each request of the service. 

| Name | Remarks | Group Key | Type | 
|---|---|---|---|
| id | 服务的唯一标识ID| yes | int |
| name | 服务名 | | string |
| serviceInstanceName | 引用的服务实例ID的名称 | | string |
| endpointName | 最终请求，通常为HTTP URI的完整路径. | | string |
| latency | 每个请求所花费的时间. | | int |
| status | 表示请求成功或是失败. | | bool(true for success)  |
| responseCode | 表示HTTP请求的返回码. 例如 200, 404, 302l | | int|
| type | 表示请求类型. Such as: Database, HTTP, RPC, gRPC. | | enum |

### 作用域 `ServiceInstance`

Calculate the metric data from each request of the service instance. 

| Name | Remarks | Group Key | Type | 
|---|---|---|---|
| id | 实例的唯一标识, 通常是个数字 | yes | int |
| name |  实例的名称. Such as `ip:port@Service Name`.  **注意**:当前的native agent 使用 `processId@Service name`作为实例名, 当你使用聚合或是过滤时，这是无效的. | | string|
| serviceName | 服务名. | | string |
| endpointName | 最终请求，通常为HTTP URI的完整路径. | | string|
| latency | 每个请求所花费的时间. | | int |
| status | 表示请求成功或是失败. | | bool(true for success) |
| responseCode | 表示HTTP请求的返回码. 例如 200, 404, 302 | | int |
| type | 表示请求类型. Such as: Database, HTTP, RPC, gRPC. | | enum |

####  `ServiceInstance` 的次要作用域

当实例是一台JVM（JAVA虚拟机）时，会使用如下的次要作用域

1. 作用域 `ServiceInstanceJVMCPU`

| Name | Remarks | Group Key | Type | 
|---|---|---|---|
| id | 实例的唯一标识, 通常是个数字 | yes | int |
| name |  实例的名称. Such as `ip:port@Service Name`.  **注意**: 当前的native agent 使用 `processId@Service name`作为实例名, 当你使用聚合或是过滤时，这是无效的. | | string|
| serviceName | 服务名. | | string |
| usePercent | CPU使用百分比| | double|

2. 作用域 `ServiceInstanceJVMMemory`

| Name | Remarks | Group Key | Type | 
|---|---|---|---|
| id | 实例的唯一标识, 通常是个数字. | yes | int |
| name |   实例的名称. Such as `ip:port@Service Name`.  **注意**:当前的native agent 使用 `processId@Service name`作为实例名, 当你使用聚合或是过滤时，这是无效的. | | string|
| serviceName | 服务名. | | string |
| heapStatus | 表示内存的统计指标为 heap 或其它| | bool |
| init | 参见 JVM 文档 | | long |
| max | 参见 JVM 文档 | | long |
| used | 参见 JVM 文档 | | long |
| committed | 参见 JVM 文档 | | long |

3. 作用域 `ServiceInstanceJVMMemoryPool`

| Name | Remarks | Group Key | Type | 
|---|---|---|---|
| id | 实例的唯一标识, 通常是个数字. | yes | int |
| name |   实例的名称. Such as `ip:port@Service Name`.  **注意**:当前的native agent 使用 `processId@Service name`作为实例名, 当你使用聚合或是过滤时，这是无效的. | | string|
| serviceName | 服务名. | | string |
| poolType | 包括 CODE_CACHE_USAGE, NEWGEN_USAGE, OLDGEN_USAGE, SURVIVOR_USAGE, PERMGEN_USAGE, METASPACE_USAGE 取决于JVM的不同版本. | | enum |
| init | 参见 JVM 文档 | | long |
| max | 参见 JVM 文档 | | long |
| used | 参见 JVM 文档 | | long |
| committed | 参见 JVM 文档 | | long |

4. 作用域 `ServiceInstanceJVMGC`

| Name | Remarks | Group Key | Type | 
|---|---|---|---|
| id | 实例的唯一标识, 通常是个数字. | yes | int |
| name |   实例的名称. Such as `ip:port@Service Name`.  **注意**:当前的native agent 使用 `processId@Service name`作为实例名, 当你使用聚合或是过滤时，这是无效的. | | string|
| serviceName | 服务名. | | string |
| phrase | 包括 NEW 和 OLD | | Enum |
| time | 内存回收时间| | long |
| count | 内存回收操作次数 | | long |

### 作用域 `Endpoint`

Calculate the metric data from each request of the endpoint in the service. 

| Name | Remarks | Group Key | Type | 
|---|---|---|---|
| id | endpoint的唯一标识，通常为数字. | yes | int |
| name | endpoint的名称, 通常是一个 HTTP URI. | | string |
| serviceName | 服务名. | | string |
| serviceInstanceName | 实例名. | | string |
| latency | 请求时间. | | int |
| status | 表示请求成功或是失败. | | bool(true for success) |
| responseCode | 表示HTTP请求的返回码. 例如 200, 404, 302 | | int |
| type | 表示请求类型. Such as: Database, HTTP, RPC, gRPC. | | enum |

### 作用域 `ServiceRelation`
一个服务与另一个服务间的相关性数据。
Calculate the metric data from each request between one service and the other service

| Name | Remarks | Group Key | Type | 
|---|---|---|---|
| sourceServiceId | 源服务ID. | yes | int |
| sourceServiceName | 源服务名. | | string |
| sourceServiceInstanceName | 源服务实例名. | | string |
| destServiceId | 目标服务ID. | yes | string |
| destServiceName | 目标服务名. | | string |
| destServiceInstanceName | 目标服务实例名.| | string|
| endpoint | 本次请求的endpoint. | | string
| componentId | 本次请求的组件ID. | yes | string
| latency | 每个请求所花费的时间. | | int |
| status | 表示请求成功或是失败. | | bool(true for success) |
| responseCode | 表示HTTP请求的返回码. 例如 200, 404, 302 | | int |
| type | 表示请求类型. Such as: Database, HTTP, RPC, gRPC. | | enum |
| detectPoint | 检测到此访问关系的地点. 可以是: client, server, proxy. | yes | enum|




### 作用域 `ServiceInstanceRelation`
一个实例与另一个实例间的相关性数据。
Calculate the metric data from each request between one service instance and the other service instance

| Name | Remarks | Group Key | Type | 
|---|---|---|---|
| sourceServiceInstanceId | 源服务实例ID. | yes | int|
| sourceServiceName | 源服务名. | | string |
| sourceServiceInstanceName | 源服务实例名. | | string |
| destServiceName | 目标服务名. | | string |
| destServiceInstanceId | 目标服务实例ID. | yes | int| 
| destServiceInstanceName | 目标服务实例名.| | string|
| endpoint | 本次请求的endpoint. | | string
| componentId | 本次请求的组件ID. | yes | string
| latency | 每个请求所花费的时间. | | int |
| status | 表示请求成功或是失败. | | bool(true for success) |
| responseCode | 表示HTTP请求的返回码. 例如 200, 404, 302 | | int |
| type | 表示请求类型. Such as: Database, HTTP, RPC, gRPC. | | enum |
| detectPoint | 检测到此访问关系的地点. 可以是: client, server, proxy. | yes | enum|

### 作用域 `EndpointRelation`

一个endpoint和另一个endpoint的相关性数据。
这种相关性是很难检测到的， 并且依赖于tracing库来找到其之前的endpoint。
所以`EndpointRelation`作用域聚合仅仅作用于SkyWalking的本地代理追踪中，包括 auto instrument agents（如Java, .NET)，OpenCensus或者其它合Skywalking的规范的propagate tracing context。

| Name | Remarks | Group Key | Type | 
|---|---|---|---|
| endpointId | 父endpoint的ID. | yes | int |
| endpoint | 父endpoint.| | string| 
| childEndpointId | 子endpoint的ID | yes | int| 
| childEndpoint| 子endpoint | | string |
| rpcLatency | 从父endpoint到子endpoint的RPC延时。包括了父endpoint自身造成的延时。
| componentId | 本次请求的组件ID. | yes | string
| status | 表示请求成功或是失败. | | bool(true for success) |
| responseCode | 表示HTTP请求的返回码. 例如 200, 404, 302 | | int |
| type | 表示请求类型. Such as: Database, HTTP, RPC, gRPC. | | enum |
| detectPoint | 检测到此访问关系的地点. 可以是: client, server, proxy. | yes | enum|
