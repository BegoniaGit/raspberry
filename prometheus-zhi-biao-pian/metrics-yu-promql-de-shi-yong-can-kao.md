# Metrics 与 PromQL 的使用参考

为了能够帮助用户理解和区分这些不同监控指标之间的差异，Prometheus定义了4种不同的指标类型\(metric type\)：Counter（计数器）、Gauge（仪表盘）、Histogram（直方图）、Summary（摘要）。

返回的样本数据中，其注释中也包含了该样本的类型。例如：

```text
# HELP node_cpu Seconds the cpus spent in each mode.
# TYPE node_cpu counter
node_cpu{cpu="cpu0",mode="idle"} 362812.7890625
```

## Counter：只增不减的计数器

Counter类型的指标其工作方式和计数器一样，只增不减（除非系统发生重置）。常见的监控指标，如http\_requests\_total，node\_cpu都是Counter类型的监控指标。 一般在定义Counter类型指标的名称时推荐使用\_total作为后缀。

样记录值：每个时刻对应的总数，因此随着时间增加该值递增或不变。

### Counter 的应用

**最近10分钟cpu时间增加量：**

```text
increase(node_cpu_seconds_total[10m])
```

**最近10分钟cpu的增长率（增量/时间间隔）：**

```text
rate(node_cpu_seconds_total[10m])
```

**最近10分钟cpu的增长率（该时间段的最后两个值之差/时间间隔）：**`i`

```text
irate(node_cpu_seconds_total[10m])
```

**cpu时间排名前10的：**

```text
topk(10,node_cpu_seconds_total)
```

## Gauge：可增可减的仪表盘

与Counter不同，Gauge类型的指标侧重于反应系统的当前状态。因此这类指标的样本数据可增可减。常见指标如：node\_memory\_MemFree（主机当前空闲的内容大小）、node\_memory\_MemAvailable（可用内存大小）都是Gauge类型的监控指标。

样记录值：每个抓取时刻对应的设置值，因此没有设置值即0。

### Gauge 的应用

Gauge 用法比较单一，一般直接展示即可，也可做预测。

**最近10分钟的变化情况**：

```text
delta(node_load1[10m])
```

**预测系统磁盘空间在4个小时之后的剩余情况：**

```text
predict_linear(node_filesystem_free{job="node"}[1h], 4 * 3600)
```

## Histogram 和 Summary 分析数据分布情况

Histogram和Summary主用用于统计和分析样本的分布情况。

这两个指标类型拥有Counter的全部功能，都有\_sum记录值之和， \_count记录值的个数，其独特的功能是拥有获取数据分布情况的能力。

### Histogram

* 由 Prometheus server 计算分布，这也是使用 Histogram 的情况比 Summary 多的原因。
* 分桶标签 ‘le’
* 使用 quantile 函数查看分为数值

metrics 样例

```text
# HELP ts_http_seconds_bucket web http response time in seconds
# TYPE ts_http_seconds_bucket histogram
ts_http_seconds_bucket_bucket{error="false",method="put",path="/app/tri",serverName="Java-application",stage="beta",statusCode="200",le="1"} 0
ts_http_seconds_bucket_bucket{error="false",method="put",path="/app/tri",serverName="Java-application",stage="beta",statusCode="200",le="2"} 0
ts_http_seconds_bucket_bucket{error="false",method="put",path="/app/tri",serverName="Java-application",stage="beta",statusCode="200",le="4"} 0
ts_http_seconds_bucket_bucket{error="false",method="put",path="/app/tri",serverName="Java-application",stage="beta",statusCode="200",le="8"} 0
ts_http_seconds_bucket_bucket{error="false",method="put",path="/app/tri",serverName="Java-application",stage="beta",statusCode="200",le="16"} 0
ts_http_seconds_bucket_bucket{error="false",method="put",path="/app/tri",serverName="Java-application",stage="beta",statusCode="200",le="32"} 0
ts_http_seconds_bucket_bucket{error="false",method="put",path="/app/tri",serverName="Java-application",stage="beta",statusCode="200",le="64"} 0
ts_http_seconds_bucket_bucket{error="false",method="put",path="/app/tri",serverName="Java-application",stage="beta",statusCode="200",le="128"} 0
ts_http_seconds_bucket_bucket{error="false",method="put",path="/app/tri",serverName="Java-application",stage="beta",statusCode="200",le="512"} 0
ts_http_seconds_bucket_bucket{error="false",method="put",path="/app/tri",serverName="Java-application",stage="beta",statusCode="200",le="1024"} 877
ts_http_seconds_bucket_bucket{error="false",method="put",path="/app/tri",serverName="Java-application",stage="beta",statusCode="200",le="+Inf"} 1062
ts_http_seconds_bucket_sum{error="false",method="put",path="/app/tri",serverName="Java-application",stage="beta",statusCode="200"} 1.19617e+06
ts_http_seconds_bucket_count{error="false",method="put",path="/app/tri",serverName="Java-application",stage="beta",statusCode="200"} 1062
```

### Summary

* 由 Prometheus client 计算分布
* 分布标签 'quantile'

metrics 样例

```text
# HELP go_gc_duration_seconds A summary of the pause duration of garbage collection cycles.
# TYPE go_gc_duration_seconds summary
go_gc_duration_seconds{quantile="0"} 1.161e-05
go_gc_duration_seconds{quantile="0.25"} 2.1318e-05
go_gc_duration_seconds{quantile="0.5"} 3.4168e-05
go_gc_duration_seconds{quantile="0.75"} 5.7184e-05
go_gc_duration_seconds{quantile="1"} 0.000319401
go_gc_duration_seconds_sum 0.720901663
go_gc_duration_seconds_count 15197
```

## 内置基础统计函数

* `sum` \(求和\)
* `min` \(最小值\)
* `max` \(最大值\)
* `avg` \(平均值\)
* `stddev` \(标准差\)
* `stdvar` \(标准方差\)
* `count` \(计数\)
* `count_values` \(对value进行计数\)
* `bottomk` \(后n条时序\)
* `topk` \(前n条时序\)
* `quantile` \(分位数\)

都是基于瞬时向量计算。比如 sum 函数，返回当前指标所选序当前时刻的最大值。其余函数同理。



