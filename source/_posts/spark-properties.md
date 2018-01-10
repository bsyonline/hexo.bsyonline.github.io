---
title: spark-properties
tags:
- untag
category:
- uncategory
author: bsyonline
lede: 没有摘要
date: 2017-12-26 15:16:56
thumbnail:
---

### Application Properties
<table class="table table-bordered table-striped table-condensed"><tr><th>Property Name</th><th>Default</th><th>Meaning</th></tr><tr><td><code>spark.app.name</code></td><td>(none)</td><td> 应用程序名称，会在 UI 和日志中显示。</td></tr></tr><tr><td><code>spark.driver.cores</code></td><td>1</td><td>驱动程序使用的核心数量，只在集群模式下有效。</td></tr><tr><td><code>spark.driver.maxResultSize</code></td><td>1g</td><td>限制每个 Spark action 所有分区序列化结果的大小。最小设置为 1 M，0 为不限制。如果超出限制，作业会终止。该值设置过大，会导致内存溢出。</td></tr><tr><td><code>spark.driver.memory</code></td><td>1g</td><td>SparkContext 初始化时驱动程序使用的内存总和 (e.g.<code>1g</code>, <code>2g</code>)。注意： 在 client 模式下，该设置不能通过配置 <code>SparkConf</code> 生效，因为启动程序的 JVM 已经启动了。可以通过<code>--driver-memory</code>命令行选项或在默认配置文件中设置。</td></tr><tr><td><code>spark.executor.memory</code></td><td>1g</td><td>每个 executor 使用的内存总和。 (e.g. <code>2g</code>, <code>8g</code>)</td></tr><tr><td><code>spark.extraListeners</code></td><td>(none)</td><td>可以设置多个实现<code>SparkListener</code>接口的类，用逗号分隔。SparkContext 初始化时，这些类的实例会被创建并注册到 Spark 的监听总线。创建时会调用只接收 SparkConf 为参数的构造函数或无参数的构造函数，如果没有合适的构造函数，SparkContext 会创建失败并抛出异常。</td></tr><tr><td><code>spark.local.dir</code></td><td>/tmp</td><td>Spark 的  "scratch" 空间路径, 用于存放 map 的输出文件和数据集。这个路径应该是一个系统本地路径，也可以是多个磁盘上的路径。	注意：从 Spark 1.0 开始，该参数会被  SPARK_LOCAL_DIRS (Standalone, Mesos)  或 LOCAL_DIRS (YARN) 覆盖。</td></tr><tr><td><code>spark.logConf</code></td><td>false</td><td>SparkContext 启动时，记录有效的 SparkConf 的 INFO 信息。</td></tr><tr><td><code>spark.master</code></td><td>(none)</td><td>集群管理节点地址。<a href="https://spark.apache.org/docs/2.1.1/submitting-applications.html#master-urls">allowed master URL's</a></td></tr><tr><td><code>spark.submit.deployMode</code></td><td>(none)</td><td>Spark 驱动程序的部署模式。"client" 驱动程序在本地启动，"cluster" 启动程序在集群中的某个节点上启动。</td></tr></table>

### Runtime Environment
<table class="table table-bordered table-striped table-condensed"><tr><th>Property Name</th><th>Default</th><th>Meaning</th></tr><tr><td><code>spark.driver.extraClassPath</code></td><td>(none)</td><td>启动程序 classpath 的预加载扩展 classpath 路径。<br>注意：在 client 模式下，不能通过<code>SparkConf</code>设置，应使用<code>--driver-class-path</code>命令行选项或默认配置文件设置。</td></tr><tr><td><code>spark.driver.extraJavaOptions</code></td><td>(none)</td><td>额外的 JVM 选项，比如 GC 或日志。<br>注意：最大堆大内存（-Xmx） 不能通过该项设置。集群模式下，最大堆内存使用<code>spark.driver.memory</code>设置，client 模式下使用<code>--driver-memory</code>命令行选项设置。<br/>注意：在 client 模式下，该项不能在<code>SparkConf</code>设置，应通过<code>--driver-java-options</code>命令行选项或默认配置文件设置。</td></tr><tr><td><code>spark.driver.extraLibraryPath</code></td><td>(none)</td><td>启动驱动程序 JVM 时设置一个特殊的库路径。<br/>注意：在 client 模式下，该项不能在<code>SparkConf</code>设置，应通过<code>--driver-library-path</code>命令行选项或默认配置文件设置。</td></tr><tr><td><code>spark.driver.userClassPathFirst</code></td><td>false</td><td>（实验）是否设置用户的 jar 优先于 Spark 的 jar 。该特性用于解决 Spark 依赖和用户依赖之间的冲突。这个特性还在实验中，只在集群模式下生效。</td></tr><tr><td><code>spark.executor.extraClassPath</code></td><td>(none)</td><td>预设额外的类路径条目，这个属性主要是为了先后兼容老的 Spark 版本，用户通常不需要设置。</td></tr><tr><td><code>spark.executor.extraJavaOptions</code></td><td>(none)</td><td>向 executor 传递一个额外的 JVM 选项，比如 GC 或日志。<br>注意：该项不能设置 Spark Properties 或最大堆大小（-Xmx）。Spark properties 应使用 SparkConf 对象或 spark-defaults.conf 来设置。最大堆内存应通过 <code> spark.executor.memory</code>设置。</td></tr><tr><td><code>spark.executor.extraLibraryPath</code></td><td>(none)</td><td>executor JVM 启动时设置一个特殊库路径。</td></tr><tr><td><code>spark.executor.logs.rolling.maxRetainedFiles</code></td><td>(none)</td><td>设置系统保存的最新日志滚动数量，老的日志文件将被删除。默认禁用。</td></tr><tr><td><code>spark.executor.logs.rolling.enableCompression</code></td><td>false</td><td>启用日志压缩。如果启动，滚动的日志将被压缩。默认禁用。</td></tr><tr><td><code>spark.executor.logs.rolling.maxSize</code></td><td>(none)</td><td>设置日志文件滚动的字节数组最大长度。默认禁用。参考 <code>spark.executor.logs.rolling.maxRetainedFiles</code> 清除旧日志文件。</td></tr><tr><td><code>spark.executor.logs.rolling.strategy</code></td><td>(none)</td><td>设置日志滚动策略。默认禁用，可以基于“时间”滚动或按照“大小”滚动。基于“时间”，使用<code>spark.executor.logs.rolling.time.interval</code>设置滚动频率。按照“大小”，使用<code>spark.executor.logs.rolling.maxSize</code>设置最大滚动文件大小。</td></tr><tr><td><code>spark.executor.logs.rolling.time.interval</code></td><td>daily</td><td>设置日志滚动的时间间隔。默认不滚动。可以设置的值包括<code>daily</code>, <code>hourly</code>, <code>minutely</code> 或任意秒数。参考<code>spark.executor.logs.rolling.maxRetainedFiles</code>自动清除旧的日志。</td></tr><tr><td><code>spark.executor.userClassPathFirst</code></td><td>false</td><td>（实验）和<code>spark.driver.userClassPathFirst</code>功能相同，但是是作用在 executor 实例。</td></tr><tr><td><code>spark.executorEnv.[EnvironmentVariableName]</code></td><td>(none)</td><td>通过指定<code>EnvironmentVariableName</code>添加 executor 的环境变量，可以设置多个值。</td></tr><tr><td><code>spark.python.profile</code></td><td>false</td><td>在 python 的 worker 节点上启动 profile 。profile 的结果通过<code>sc.show_profiles()</code>显示或在启动程序退出前显示。通过<code>sc.dump_profiles(path)</code>设置磁盘的路径。如果 profile 结果被手动隐藏，那么在驱动程序退出时不显示。默认<code>pyspark.profiler.BasicProfiler</code> 启用，但是可以给 SparkContext 构造函数传一个 profiler 类参数覆盖该值。</td></tr><tr><td><code>spark.files</code></td><td></td><td>每个 executor 的工作目录列表，用逗号分隔。</td></tr><tr><td><code>spark.jars</code></td><td></td><td>设置驱动程序和 executor 类路径包含的本地 jar，用逗号分隔。 </td></tr><tr><td><code>spark.jars.packages</code></td><td></td><td>驱动程序和 executor 类路径包含的 jar 包的 maven 坐标。按照本地 maven 仓库，maven 中心库和<code>spark.jars.ivy</code>指定的附加远程仓库顺序查找。格式为：groupId:artifactId:version</td></tr><tr><td><code>spark.jars.excludes</code></td><td></td><td>排除<code>spark.jars.packages</code>提供的 jar 以避免冲突。</td></tr><tr><td><code>spark.jars.ivy</code></td><td></td><td>查找附加远程仓库<code>spark.jars.packages</code>指定的坐标，用逗号分隔。</td></tr></table>

### Shuffle Behavior
<table class="table table-bordered table-striped table-condensed"><tr><th>Property Name</th><th>Default</th><th>Meaning</th></tr><tr><td><code>spark.reducer.maxSizeInFlight</code></td><td>48m</td><td>同时从每个 reduce 任务获取的 map 最大值。每个输出都需要创建缓冲区来接收，所以每个 reduce 任务都有固定的内存开销，因此应设置一个较小的值。</td></tr><tr><td><code>spark.reducer.maxReqsInFlight</code></td><td>Int.MaxValue</td><td>该选项限制任意给定的点接收远程请求的数量。但集群主机的数量增加，可能会有大量的请求连接到一个或多个节点，导致 worker 节点负载过大而出错。限制请求的数量可以避免出现此种问题。</td></tr><tr><td><code>spark.shuffle.compress</code></td><td>true</td><td>是否压缩 map 的输出文件。一般来说使用压缩是的好办法。压缩将使用<code>spark.io.compression.codec</code>属性。</td></tr><tr><td><code>spark.shuffle.file.buffer</code></td><td>32k</td><td>每个 shuffle 文件流的缓冲区大小。缓冲区会降低创建中间 shuffle 文件时的磁盘寻址和系统调用次数。</td></tr><tr><td><code>spark.shuffle.io.maxRetries</code></td><td>3</td><td>（Netty only）抓取到 IO 相关异常错误自动重试。重试使 shuffle 在遇到长时间 GC 暂停或短时间的网络异常更稳定。</td></tr><tr><td><code>spark.shuffle.io.numConnectionsPerPeer</code></td><td>1</td><td>（Netty only）在大规模集群中为了减少连接创建，主机之间的连接被重用。对于集群中有大量的硬盘少量的主机导致并发不足时，用户可以调大该值。</td></tr><tr><td><code>spark.shuffle.io.preferDirectBufs</code></td><td>true</td><td>（Netty only）在 shuffle 和缓存交换过程中，使用堆缓冲区来减少 GC 。对于堆缓冲区内存有限的环境，用户可能希望关闭这个功能，使所有分配都从 Netty 移到堆中。</td></tr><tr><td><code>spark.shuffle.io.retryWait</code></td><td>5s</td><td>（Netty only）每次抓取重试等待时间。默认最大延迟 15 秒<code>maxRetries * retryWait</code>。</td></tr><tr><td><code>spark.shuffle.service.enabled</code></td><td>false</td><td>应用额外的 shuffle 服务。该服务保存 executor 生成的 shuffle 文件，以保证 executor 被安全删除。如果<code>spark.dynamicAllocation.enabled</code>为 true，则该值必须设置为 true 。</td></tr><tr><td><code>spark.shuffle.service.port</code></td><td>7337</td><td>运行额外 shuffle 服务的端口号。</td></tr><tr><td><code>spark.shuffle.service.index.cache.entries</code></td><td>1024</td><td>shuffle 服务的缓存索引中保存的最大条目数量。</td></tr><tr><td><code>spark.shuffle.sort.bypassMergeThreshold</code></td><td>200</td><td>（高级）在基于排序的 shuffle 管理器中，如果没有 map 端的聚合，应避免合并排序数据并减少分区数量。</td></tr><tr><td><code>spark.shuffle.spill.compress</code></td><td>true</td><td>是否压缩 shuffle 过程中的溢出数据。压缩使用<code>spark.io.compression.codec</code>属性。</td></tr><tr><td><code>spark.io.encryption.enabled</code></td><td>false</td><td>使用 IO 加密。除 Mesos 外，其他模式都支持。使用该特性使建议使用 RPC 加密。</td></tr><tr><td><code>spark.io.encryption.keySizeBits</code></td><td>128</td><td>IO 加密的密钥大小。128，192，256 可选。</td></tr><tr><td><code>spark.io.encryption.keygen.algorithm</code></td><td>HmacSHA1</td><td>生成 IO 加密密钥的算法。支持的算法见Java密码体系结构标准算法名称文档的 KeyGenerator 部分。</td></tr></table>

### Spark UI
<table class="table table-bordered table-striped table-condensed"><tr><th>Property Name</th><th>Default</th><th>Meaning</th></tr><tr><td><code>spark.eventLog.compress</code></td><td>false</td><td>如果<code>spark.eventLog.enabled</code>为 true ，是否使用压缩。</td></tr><tr><td><code>spark.eventLog.dir</code></td><td>file:///tmp/spark-events</td><td>如果<code>spark.eventLog.enabled</code>为 true ，设置路径为 Spark 事件日志的根目录。在这个目录下，Spark 为每个应用程序创建一个子路径，记录特殊的事件。用户可以将其设置为一个统一的路径，如 HDFS 上的路径，以便历史可读。</td></tr><tr><td><code>spark.eventLog.enabled</code></td><td>false</td><td>是否记录 Spark 事件日志，程序结束后重现 Web UI 很有用。</td></tr><tr><td><code>spark.ui.enabled</code></td><td>true</td><td>是否运行 web UI 程序。</td></tr><tr><td><code>spark.ui.killEnabled</code></td><td>true</td><td>允许通过 UI 杀掉任务。</td></tr><tr><td><code>spark.ui.port</code></td><td>4040</td><td>应用程序面板端口号。</td></tr><tr><td><code>spark.ui.retainedJobs</code></td><td>1000</td><td>GC 之前 Spark UI 和状态 api 能记录多少作业。</td></tr><tr><td><code>spark.ui.retainedStages</code></td><td>1000</td><td>GC 之前 Spark UI 和状态 api 能记录多少阶段。</td></tr><tr><td><code>spark.ui.retainedTasks</code></td><td>100000</td><td>GC 之前 Spark UI 和状态 api 能记录多少任务。</td></tr><tr><td><code>spark.ui.reverseProxy</code></td><td>false</td><td> Spark 主节点作为 worker 节点和 UI 的反向代理。Spark 主节点不需要访问主机就可以反向代理 worker 节点和 UI 。需要注意的是，worker 节点和 UI 不需要访问路径，只能通过主节点或代理访问。该设置影像所有的 wroker 节点和 UI ，必须在所有的 worker 节点，驱动程序和主节点上设置。</td></tr><tr><td><code>spark.ui.reverseProxyUrl</code></td><td></td><td>代理的 URL 。应包括 （http/https）和端口。</td></tr><tr><td><code>spark.worker.ui.retainedExecutors</code></td><td>1000</td><td>GC 之前 Spark UI 和状态 api 能记录多少完成的 executor 。</td></tr><tr><td><code>spark.worker.ui.retainedDrivers</code></td><td>1000</td><td>GC 之前 Spark UI 和状态 api 能记录多少完成的驱动程序。</td></tr><tr><td><code>spark.sql.ui.retainedExecutions</code></td><td>1000</td><td>GC 之前 Spark UI 和状态 api 能记录多少完成的 execution 。</td></tr><tr><td><code>spark.streaming.ui.retainedBatches</code></td><td>1000</td><td>GC 之前 Spark UI 和状态 api 能记录多少完成的 batch 。</td></tr><tr><td><code>spark.ui.retainedDeadExecutors</code></td><td>100</td><td>GC 之前 Spark UI 和状态 api 能记录多少死亡的 executor 。</td></tr></table>

### Compression and Serialization
<table class="table table-bordered table-striped table-condensed">
<tr>
<th>Property Name</th>
<th>Default</th>
<th>Meaning</th>
</tr>
<tr>
<td><code>spark.broadcast.compress</code></td>
<td>true</td>
<td>
Whether to compress broadcast variables before sending them. Generally a good idea.
</td>
</tr>
<tr>
<td><code>spark.io.compression.codec</code></td>
<td>lz4</td>
<td>
The codec used to compress internal data such as RDD partitions, broadcast variables and shuffle outputs. By
default, Spark provides three codecs: <code>lz4</code>, <code>lzf</code>, and <code>snappy</code>. You can
also use fully qualified class names to specify the codec, e.g. <code>org.apache.spark.io.LZ4CompressionCodec</code>,
<code>org.apache.spark.io.LZFCompressionCodec</code>, and
<code>org.apache.spark.io.SnappyCompressionCodec</code>.
</td>
</tr>
<tr>
<td><code>spark.io.compression.lz4.blockSize</code></td>
<td>32k</td>
<td>
Block size used in LZ4 compression, in the case when LZ4 compression codec is used. Lowering this block size
will also lower shuffle memory usage when LZ4 is used.
</td>
</tr>
<tr>
<td><code>spark.io.compression.snappy.blockSize</code></td>
<td>32k</td>
<td>
Block size used in Snappy compression, in the case when Snappy compression codec is used. Lowering this
block size will also lower shuffle memory usage when Snappy is used.
</td>
</tr>
<tr>
<td><code>spark.kryo.classesToRegister</code></td>
<td>(none)</td>
<td>
If you use Kryo serialization, give a comma-separated list of custom class names to register with Kryo. See
the <a href="tuning.html#data-serialization">tuning guide</a> for more details.
</td>
</tr>
<tr>
<td><code>spark.kryo.referenceTracking</code></td>
<td>true</td>
<td>
Whether to track references to the same object when serializing data with Kryo, which is necessary if your
object graphs have loops and useful for efficiency if they contain multiple copies of the same object. Can
be disabled to improve performance if you know this is not the case.
</td>
</tr>
<tr>
<td><code>spark.kryo.registrationRequired</code></td>
<td>false</td>
<td>
Whether to require registration with Kryo. If set to 'true', Kryo will throw an exception if an unregistered
class is serialized. If set to false (the default), Kryo will write unregistered class names along with each
object. Writing class names can cause significant performance overhead, so enabling this option can enforce
strictly that a user has not omitted classes from registration.
</td>
</tr>
<tr>
<td><code>spark.kryo.registrator</code></td>
<td>(none)</td>
<td>
If you use Kryo serialization, give a comma-separated list of classes that register your custom classes with
Kryo. This
property is useful if you need to register your classes in a custom way, e.g. to specify a custom field
serializer. Otherwise <code>spark.kryo.classesToRegister</code> is simpler. It should be set to classes that
extend <a href="api/scala/index.html#org.apache.spark.serializer.KryoRegistrator">
<code>KryoRegistrator</code></a>. See the <a href="tuning.html#data-serialization">tuning guide</a> for more
details.
</td>
</tr>
<tr>
<td><code>spark.kryo.unsafe</code></td>
<td>false</td>
<td>
Whether to use unsafe based Kryo serializer. Can be substantially faster by using Unsafe Based IO.
</td>
</tr>
<tr>
<td><code>spark.kryoserializer.buffer.max</code></td>
<td>64m</td>
<td>
Maximum allowable size of Kryo serialization buffer. This must be larger than any object you attempt to
serialize. Increase this if you get a "buffer limit exceeded" exception inside Kryo.
</td>
</tr>
<tr>
<td><code>spark.kryoserializer.buffer</code></td>
<td>64k</td>
<td>
Initial size of Kryo's serialization buffer. Note that there will be one buffer <i>per core</i> on each
worker. This buffer will grow up to <code>spark.kryoserializer.buffer.max</code> if needed.
</td>
</tr>
<tr>
<td><code>spark.rdd.compress</code></td>
<td>false</td>
<td>
Whether to compress serialized RDD partitions (e.g. for <code>StorageLevel.MEMORY_ONLY_SER</code> in Java
and Scala or <code>StorageLevel.MEMORY_ONLY</code> in Python). Can save substantial space at the cost of
some extra CPU time.
</td>
</tr>
<tr>
<td><code>spark.serializer</code></td>
<td>
org.apache.spark.serializer.<br/>JavaSerializer
</td>
<td>
Class to use for serializing objects that will be sent over the network or need to be cached in serialized
form. The default of Java serialization works with any Serializable Java object but is quite slow, so we
recommend <a href="tuning.html">using <code>org.apache.spark.serializer.KryoSerializer</code> and
configuring Kryo serialization</a> when speed is
necessary. Can be any subclass of <a href="api/scala/index.html#org.apache.spark.serializer.Serializer">
<code>org.apache.spark.Serializer</code></a>.
</td>
</tr>
<tr>
<td><code>spark.serializer.objectStreamReset</code></td>
<td>100</td>
<td>
When serializing using org.apache.spark.serializer.JavaSerializer, the serializer caches objects to prevent
writing redundant data, however that stops garbage collection of those objects. By calling 'reset' you flush
that info from the serializer, and allow old objects to be collected. To turn off this periodic reset set it
to -1. By default it will reset the serializer every 100 objects.
</td>
</tr>
</table>

### Memory Management
<table class="table table-bordered table-striped table-condensed">
<tr>
<th>Property Name</th>
<th>Default</th>
<th>Meaning</th>
</tr>
<tr>
<td><code>spark.memory.fraction</code></td>
<td>0.6</td>
<td>
Fraction of (heap space - 300MB) used for execution and storage. The lower this is, the more frequently
spills and cached data eviction occur. The purpose of this config is to set aside memory for internal
metadata, user data structures, and imprecise size estimation in the case of sparse, unusually large
records. Leaving this at the default value is recommended. For more detail, including important information
about correctly tuning JVM garbage collection when increasing this value, see <a
    href="tuning.html#memory-management-overview">this description</a>.
</td>
</tr>
<tr>
<td><code>spark.memory.storageFraction</code></td>
<td>0.5</td>
<td>
Amount of storage memory immune to eviction, expressed as a fraction of the size of the region set aside by
<code>s​park.memory.fraction</code>. The higher this is, the less working memory may be available to
execution and tasks may spill to disk more often. Leaving this at the default value is recommended. For more
detail, see <a href="tuning.html#memory-management-overview">this description</a>.
</td>
</tr>
<tr>
<td><code>spark.memory.offHeap.enabled</code></td>
<td>false</td>
<td>
If true, Spark will attempt to use off-heap memory for certain operations. If off-heap memory use is
enabled, then <code>spark.memory.offHeap.size</code> must be positive.
</td>
</tr>
<tr>
<td><code>spark.memory.offHeap.size</code></td>
<td>0</td>
<td>
The absolute amount of memory in bytes which can be used for off-heap allocation. This setting has no impact
on heap memory usage, so if your executors' total memory consumption must fit within some hard limit then be
sure to shrink your JVM heap size accordingly. This must be set to a positive value when <code>spark.memory.offHeap.enabled=true</code>.
</td>
</tr>
<tr>
<td><code>spark.memory.useLegacyMode</code></td>
<td>false</td>
<td>
​Whether to enable the legacy memory management mode used in Spark 1.5 and before. The legacy mode rigidly
partitions the heap space into fixed-size regions, potentially leading to excessive spilling if the
application was not tuned. The following deprecated memory fraction configurations are not read unless this
is enabled: <code>spark.shuffle.memoryFraction</code><br/> <code>spark.storage.memoryFraction</code><br/>
<code>spark.storage.unrollFraction</code>
</td>
</tr>
<tr>
<td><code>spark.shuffle.memoryFraction</code></td>
<td>0.2</td>
<td>
(deprecated) This is read only if <code>spark.memory.useLegacyMode</code> is enabled. Fraction of Java heap
to use for aggregation and cogroups during shuffles. At any given time, the collective size of all in-memory
maps used for shuffles is bounded by this limit, beyond which the contents will begin to spill to disk. If
spills are often, consider increasing this value at the expense of <code>spark.storage.memoryFraction</code>.
</td>
</tr>
<tr>
<td><code>spark.storage.memoryFraction</code></td>
<td>0.6</td>
<td>
(deprecated) This is read only if <code>spark.memory.useLegacyMode</code> is enabled. Fraction of Java heap
to use for Spark's memory cache. This should not be larger than the "old" generation of objects in the JVM,
which by default is given 0.6 of the heap, but you can increase it if you configure your own old generation
size.
</td>
</tr>
<tr>
<td><code>spark.storage.unrollFraction</code></td>
<td>0.2</td>
<td>
(deprecated) This is read only if <code>spark.memory.useLegacyMode</code> is enabled. Fraction of <code>spark.storage.memoryFraction</code>
to use for unrolling blocks in memory. This is dynamically allocated by dropping existing blocks when there
is not enough free storage space to unroll the new block in its entirety.
</td>
</tr>
</table>

### Execution Behavior
<table class="table table-bordered table-striped table-condensed">
<tr>
<th>Property Name</th>
<th>Default</th>
<th>Meaning</th>
</tr>
<tr>
<td><code>spark.broadcast.blockSize</code></td>
<td>4m</td>
<td>
Size of each piece of a block for <code>TorrentBroadcastFactory</code>. Too large a value decreases
parallelism during broadcast (makes it slower); however, if it is too small, <code>BlockManager</code> might
take a performance hit.
</td>
</tr>
<tr>
<td><code>spark.executor.cores</code></td>
<td>
1 in YARN mode, all the available cores on the worker in standalone and Mesos coarse-grained modes.
</td>
<td>
The number of cores to use on each executor. In standalone and Mesos coarse-grained modes, setting this
parameter allows an application to run multiple executors on the same worker, provided that there are enough
cores on that worker. Otherwise, only one executor per application will run on each worker.
</td>
</tr>
<tr>
<td><code>spark.default.parallelism</code></td>
<td>
For distributed shuffle operations like <code>reduceByKey</code> and <code>join</code>, the largest number
of partitions in a parent RDD. For operations like <code>parallelize</code> with no parent RDDs, it depends
on the cluster manager:
<ul>
    <li>Local mode: number of cores on the local machine</li>
    <li>Mesos fine grained mode: 8</li>
    <li>Others: total number of cores on all executor nodes or 2, whichever is larger</li>
</ul>
</td>
<td>
Default number of partitions in RDDs returned by transformations like <code>join</code>,
<code>reduceByKey</code>, and <code>parallelize</code> when not set by user.
</td>
</tr>
<tr>
<td><code>spark.executor.heartbeatInterval</code></td>
<td>10s</td>
<td>Interval between each executor's heartbeats to the driver. Heartbeats let the driver know that the executor
is still alive and update it with metrics for in-progress tasks. spark.executor.heartbeatInterval should be
significantly less than spark.network.timeout
</td>
</tr>
<tr>
<td><code>spark.files.fetchTimeout</code></td>
<td>60s</td>
<td>
Communication timeout to use when fetching files added through SparkContext.addFile() from the driver.
</td>
</tr>
<tr>
<td><code>spark.files.useFetchCache</code></td>
<td>true</td>
<td>
If set to true (default), file fetching will use a local cache that is shared by executors that belong to
the same application, which can improve task launching performance when running many executors on the same
host. If set to false, these caching optimizations will be disabled and all executors will fetch their own
copies of files. This optimization may be disabled in order to use Spark local directories that reside on
NFS filesystems (see <a href="https://issues.apache.org/jira/browse/SPARK-6313">SPARK-6313</a> for more
details).
</td>
</tr>
<tr>
<td><code>spark.files.overwrite</code></td>
<td>false</td>
<td>
Whether to overwrite files added through SparkContext.addFile() when the target file exists and its contents
do not match those of the source.
</td>
</tr>
<tr>
<td><code>spark.files.maxPartitionBytes</code></td>
<td>134217728 (128 MB)</td>
<td>
The maximum number of bytes to pack into a single partition when reading files.
</td>
</tr>
<tr>
<td><code>spark.files.openCostInBytes</code></td>
<td>4194304 (4 MB)</td>
<td>
The estimated cost to open a file, measured by the number of bytes could be scanned in the same time. This
is used when putting multiple files into a partition. It is better to over estimate, then the partitions
with small files will be faster than partitions with bigger files.
</td>
</tr>
<tr>
<td><code>spark.hadoop.cloneConf</code></td>
<td>false</td>
<td>If set to true, clones a new Hadoop <code>Configuration</code> object for each task. This option should be
enabled to work around <code>Configuration</code> thread-safety issues (see <a
        href="https://issues.apache.org/jira/browse/SPARK-2546">SPARK-2546</a> for more details). This is
disabled by default in order to avoid unexpected performance regressions for jobs that are not affected by
these issues.
</td>
</tr>
<tr>
<td><code>spark.hadoop.validateOutputSpecs</code></td>
<td>true</td>
<td>If set to true, validates the output specification (e.g. checking if the output directory already exists)
used in saveAsHadoopFile and other variants. This can be disabled to silence exceptions due to pre-existing
output directories. We recommend that users do not disable this except if trying to achieve compatibility
with previous versions of Spark. Simply use Hadoop's FileSystem API to delete output directories by hand.
This setting is ignored for jobs generated through Spark Streaming's StreamingContext, since data may need
to be rewritten to pre-existing output directories during checkpoint recovery.
</td>
</tr>
<tr>
<td><code>spark.storage.memoryMapThreshold</code></td>
<td>2m</td>
<td>
Size of a block above which Spark memory maps when reading a block from disk. This prevents Spark from
memory mapping very small blocks. In general, memory mapping has high overhead for blocks close to or below
the page size of the operating system.
</td>
</tr>
</table>

### Networking
<table class="table table-bordered table-striped table-condensed">
<tr>
<th>Property Name</th>
<th>Default</th>
<th>Meaning</th>
</tr>
<tr>
<td><code>spark.rpc.message.maxSize</code></td>
<td>128</td>
<td>
Maximum message size (in MB) to allow in "control plane" communication; generally only applies to map output
size information sent between executors and the driver. Increase this if you are running jobs with many
thousands of map and reduce tasks and see messages about the RPC message size.
</td>
</tr>
<tr>
<td><code>spark.blockManager.port</code></td>
<td>(random)</td>
<td>
Port for all block managers to listen on. These exist on both the driver and the executors.
</td>
</tr>
<tr>
<td><code>spark.driver.blockManager.port</code></td>
<td>(value of spark.blockManager.port)</td>
<td>
Driver-specific port for the block manager to listen on, for cases where it cannot use the same
configuration as executors.
</td>
</tr>
<tr>
<td><code>spark.driver.bindAddress</code></td>
<td>(value of spark.driver.host)</td>
<td>
Hostname or IP address where to bind listening sockets. This config overrides the SPARK_LOCAL_IP environment
variable (see below). <br/>It also allows a different address from the local one to be advertised to
executors or external systems. This is useful, for example, when running containers with bridged networking.
For this to properly work, the different ports used by the driver (RPC, block manager and UI) need to be
forwarded from the container's host.
</td>
</tr>
<tr>
<td><code>spark.driver.host</code></td>
<td>(local hostname)</td>
<td>
Hostname or IP address for the driver. This is used for communicating with the executors and the standalone
Master.
</td>
</tr>
<tr>
<td><code>spark.driver.port</code></td>
<td>(random)</td>
<td>
Port for the driver to listen on. This is used for communicating with the executors and the standalone
Master.
</td>
</tr>
<tr>
<td><code>spark.network.timeout</code></td>
<td>120s</td>
<td>
Default timeout for all network interactions. This config will be used in place of <code>spark.core.connection.ack.wait.timeout</code>,
<code>spark.storage.blockManagerSlaveTimeoutMs</code>, <code>spark.shuffle.io.connectionTimeout</code>,
<code>spark.rpc.askTimeout</code> or <code>spark.rpc.lookupTimeout</code> if they are not configured.
</td>
</tr>
<tr>
<td><code>spark.port.maxRetries</code></td>
<td>16</td>
<td>
Maximum number of retries when binding to a port before giving up. When a port is given a specific value
(non 0), each subsequent retry will increment the port used in the previous attempt by 1 before retrying.
This essentially allows it to try a range of ports from the start port specified to port + maxRetries.
</td>
</tr>
<tr>
<td><code>spark.rpc.numRetries</code></td>
<td>3</td>
<td>
Number of times to retry before an RPC task gives up. An RPC task will run at most times of this number.
</td>
</tr>
<tr>
<td><code>spark.rpc.retry.wait</code></td>
<td>3s</td>
<td>
Duration for an RPC ask operation to wait before retrying.
</td>
</tr>
<tr>
<td><code>spark.rpc.askTimeout</code></td>
<td><code>spark.network.timeout</code></td>
<td>
Duration for an RPC ask operation to wait before timing out.
</td>
</tr>
<tr>
<td><code>spark.rpc.lookupTimeout</code></td>
<td>120s</td>
<td>
Duration for an RPC remote endpoint lookup operation to wait before timing out.
</td>
</tr>
</table>

### Scheduling
<table class="table table-bordered table-striped table-condensed">
<tr>
<th>Property Name</th>
<th>Default</th>
<th>Meaning</th>
</tr>
<tr>
<td><code>spark.cores.max</code></td>
<td>(not set)</td>
<td>
When running on a <a href="spark-standalone.html">standalone deploy cluster</a> or a <a
    href="running-on-mesos.html#mesos-run-modes">Mesos cluster in "coarse-grained" sharing mode</a>, the
maximum amount of CPU cores to request for the application from across the cluster (not from each machine).
If not set, the default will be <code>spark.deploy.defaultCores</code> on Spark's standalone cluster
manager, or infinite (all available cores) on Mesos.
</td>
</tr>
<tr>
<td><code>spark.locality.wait</code></td>
<td>3s</td>
<td>
How long to wait to launch a data-local task before giving up and launching it on a less-local node. The
same wait will be used to step through multiple locality levels (process-local, node-local, rack-local and
then any). It is also possible to customize the waiting time for each level by setting <code>spark.locality.wait.node</code>,
etc. You should increase this setting if your tasks are long and see poor locality, but the default usually
works well.
</td>
</tr>
<tr>
<td><code>spark.locality.wait.node</code></td>
<td>spark.locality.wait</td>
<td>
Customize the locality wait for node locality. For example, you can set this to 0 to skip node locality and
search immediately for rack locality (if your cluster has rack information).
</td>
</tr>
<tr>
<td><code>spark.locality.wait.process</code></td>
<td>spark.locality.wait</td>
<td>
Customize the locality wait for process locality. This affects tasks that attempt to access cached data in a
particular executor process.
</td>
</tr>
<tr>
<td><code>spark.locality.wait.rack</code></td>
<td>spark.locality.wait</td>
<td>
Customize the locality wait for rack locality.
</td>
</tr>
<tr>
<td><code>spark.scheduler.maxRegisteredResourcesWaitingTime</code></td>
<td>30s</td>
<td>
Maximum amount of time to wait for resources to register before scheduling begins.
</td>
</tr>
<tr>
<td><code>spark.scheduler.minRegisteredResourcesRatio</code></td>
<td>0.8 for YARN mode; 0.0 for standalone mode and Mesos coarse-grained mode</td>
<td>
The minimum ratio of registered resources (registered resources / total expected resources) (resources are
executors in yarn mode, CPU cores in standalone mode and Mesos coarsed-grained mode ['spark.cores.max' value
is total expected resources for Mesos coarse-grained mode] ) to wait for before scheduling begins. Specified
as a double between 0.0 and 1.0. Regardless of whether the minimum ratio of resources has been reached, the
maximum amount of time it will wait before scheduling begins is controlled by config <code>spark.scheduler.maxRegisteredResourcesWaitingTime</code>.
</td>
</tr>
<tr>
<td><code>spark.scheduler.mode</code></td>
<td>FIFO</td>
<td>
The <a href="job-scheduling.html#scheduling-within-an-application">scheduling mode</a> between jobs
submitted to the same SparkContext. Can be set to <code>FAIR</code> to use fair sharing instead of queueing
jobs one after another. Useful for multi-user services.
</td>
</tr>
<tr>
<td><code>spark.scheduler.revive.interval</code></td>
<td>1s</td>
<td>
The interval length for the scheduler to revive the worker resource offers to run tasks.
</td>
</tr>
<tr>
<td><code>spark.blacklist.enabled</code></td>
<td>
false
</td>
<td>
If set to "true", prevent Spark from scheduling tasks on executors that have been blacklisted due to too
many task failures. The blacklisting algorithm can be further controlled by the other "spark.blacklist"
configuration options.
</td>
</tr>
<tr>
<td><code>spark.blacklist.task.maxTaskAttemptsPerExecutor</code></td>
<td>1</td>
<td>
(Experimental) For a given task, how many times it can be retried on one executor before the executor is
blacklisted for that task.
</td>
</tr>
<tr>
<td><code>spark.blacklist.task.maxTaskAttemptsPerNode</code></td>
<td>2</td>
<td>
(Experimental) For a given task, how many times it can be retried on one node, before the entire node is
blacklisted for that task.
</td>
</tr>
<tr>
<td><code>spark.blacklist.stage.maxFailedTasksPerExecutor</code></td>
<td>2</td>
<td>
(Experimental) How many different tasks must fail on one executor, within one stage, before the executor is
blacklisted for that stage.
</td>
</tr>
<tr>
<td><code>spark.blacklist.stage.maxFailedExecutorsPerNode</code></td>
<td>2</td>
<td>
(Experimental) How many different executors are marked as blacklisted for a given stage, before the entire
node is marked as failed for the stage.
</td>
</tr>
<tr>
<td><code>spark.speculation</code></td>
<td>false</td>
<td>
If set to "true", performs speculative execution of tasks. This means if one or more tasks are running
slowly in a stage, they will be re-launched.
</td>
</tr>
<tr>
<td><code>spark.speculation.interval</code></td>
<td>100ms</td>
<td>
How often Spark will check for tasks to speculate.
</td>
</tr>
<tr>
<td><code>spark.speculation.multiplier</code></td>
<td>1.5</td>
<td>
How many times slower a task is than the median to be considered for speculation.
</td>
</tr>
<tr>
<td><code>spark.speculation.quantile</code></td>
<td>0.75</td>
<td>
Fraction of tasks which must be complete before speculation is enabled for a particular stage.
</td>
</tr>
<tr>
<td><code>spark.task.cpus</code></td>
<td>1</td>
<td>
Number of cores to allocate for each task.
</td>
</tr>
<tr>
<td><code>spark.task.maxFailures</code></td>
<td>4</td>
<td>
Number of failures of any particular task before giving up on the job. The total number of failures spread
across different tasks will not cause the job to fail; a particular task has to fail this number of
attempts. Should be greater than or equal to 1. Number of allowed retries = this value - 1.
</td>
</tr>
<tr>
<td><code>spark.task.reaper.enabled</code></td>
<td>false</td>
<td>
Enables monitoring of killed / interrupted tasks. When set to true, any task which is killed will be
monitored by the executor until that task actually finishes executing. See the other <code>spark.task.reaper.*</code>
configurations for details on how to control the exact behavior of this monitoring. When set to false (the
default), task killing will use an older code path which lacks such monitoring.
</td>
</tr>
<tr>
<td><code>spark.task.reaper.pollingInterval</code></td>
<td>10s</td>
<td>
When <code>spark.task.reaper.enabled = true</code>, this setting controls the frequency at which executors
will poll the status of killed tasks. If a killed task is still running when polled then a warning will be
logged and, by default, a thread-dump of the task will be logged (this thread dump can be disabled via the
<code>spark.task.reaper.threadDump</code> setting, which is documented below).
</td>
</tr>
<tr>
<td><code>spark.task.reaper.threadDump</code></td>
<td>true</td>
<td>
When <code>spark.task.reaper.enabled = true</code>, this setting controls whether task thread dumps are
logged during periodic polling of killed tasks. Set this to false to disable collection of thread dumps.
</td>
</tr>
<tr>
<td><code>spark.task.reaper.killTimeout</code></td>
<td>-1</td>
<td>
When <code>spark.task.reaper.enabled = true</code>, this setting specifies a timeout after which the
executor JVM will kill itself if a killed task has not stopped running. The default value, -1, disables this
mechanism and prevents the executor from self-destructing. The purpose of this setting is to act as a
safety-net to prevent runaway uncancellable tasks from rendering an executor unusable.
</td>
</tr>
</table>

### Dynamic Allocation
<table class="table table-bordered table-striped table-condensed">
<tr>
<th>Property Name</th>
<th>Default</th>
<th>Meaning</th>
</tr>
<tr>
<td><code>spark.dynamicAllocation.enabled</code></td>
<td>false</td>
<td>
Whether to use dynamic resource allocation, which scales the number of executors registered with this
application up and down based on the workload. For more detail, see the description <a
    href="job-scheduling.html#dynamic-resource-allocation">here</a>. <br/><br/> This requires <code>spark.shuffle.service.enabled</code>
to be set. The following configurations are also relevant: <code>spark.dynamicAllocation.minExecutors</code>,
<code>spark.dynamicAllocation.maxExecutors</code>, and <code>spark.dynamicAllocation.initialExecutors</code>
</td>
</tr>
<tr>
<td><code>spark.dynamicAllocation.executorIdleTimeout</code></td>
<td>60s</td>
<td>
If dynamic allocation is enabled and an executor has been idle for more than this duration, the executor
will be removed. For more detail, see this <a href="job-scheduling.html#resource-allocation-policy">description</a>.
</td>
</tr>
<tr>
<td><code>spark.dynamicAllocation.cachedExecutorIdleTimeout</code></td>
<td>infinity</td>
<td>
If dynamic allocation is enabled and an executor which has cached data blocks has been idle for more than
this duration, the executor will be removed. For more details, see this <a
    href="job-scheduling.html#resource-allocation-policy">description</a>.
</td>
</tr>
<tr>
<td><code>spark.dynamicAllocation.initialExecutors</code></td>
<td><code>spark.dynamicAllocation.minExecutors</code></td>
<td>
Initial number of executors to run if dynamic allocation is enabled. <br/><br/> If `--num-executors` (or
`spark.executor.instances`) is set and larger than this value, it will be used as the initial number of
executors.
</td>
</tr>
<tr>
<td><code>spark.dynamicAllocation.maxExecutors</code></td>
<td>infinity</td>
<td>
Upper bound for the number of executors if dynamic allocation is enabled.
</td>
</tr>
<tr>
<td><code>spark.dynamicAllocation.minExecutors</code></td>
<td>0</td>
<td>
Lower bound for the number of executors if dynamic allocation is enabled.
</td>
</tr>
<tr>
<td><code>spark.dynamicAllocation.schedulerBacklogTimeout</code></td>
<td>1s</td>
<td>
If dynamic allocation is enabled and there have been pending tasks backlogged for more than this duration,
new executors will be requested. For more detail, see this <a
    href="job-scheduling.html#resource-allocation-policy">description</a>.
</td>
</tr>
<tr>
<td><code>spark.dynamicAllocation.sustainedSchedulerBacklogTimeout</code></td>
<td><code>schedulerBacklogTimeout</code></td>
<td>
Same as <code>spark.dynamicAllocation.schedulerBacklogTimeout</code>, but used only for subsequent executor
requests. For more detail, see this <a href="job-scheduling.html#resource-allocation-policy">description</a>.
</td>
</tr>
</table>

### Security
<table class="table table-bordered table-striped table-condensed">
<tr>
<th>Property Name</th>
<th>Default</th>
<th>Meaning</th>
</tr>
<tr>
<td><code>spark.acls.enable</code></td>
<td>false</td>
<td>
Whether Spark acls should be enabled. If enabled, this checks to see if the user has access permissions to
view or modify the job. Note this requires the user to be known, so if the user comes across as null no
checks are done. Filters can be used with the UI to authenticate and set the user.
</td>
</tr>
<tr>
<td><code>spark.admin.acls</code></td>
<td>Empty</td>
<td>
Comma separated list of users/administrators that have view and modify access to all Spark jobs. This can be
used if you run on a shared cluster and have a set of administrators or devs who help debug when things do
not work. Putting a "*" in the list means any user can have the privilege of admin.
</td>
</tr>
<tr>
<td><code>spark.admin.acls.groups</code></td>
<td>Empty</td>
<td>
Comma separated list of groups that have view and modify access to all Spark jobs. This can be used if you
have a set of administrators or developers who help maintain and debug the underlying infrastructure.
Putting a "*" in the list means any user in any group can have the privilege of admin. The user groups are
obtained from the instance of the groups mapping provider specified by
<code>spark.user.groups.mapping</code>. Check the entry <code>spark.user.groups.mapping</code> for more
details.
</td>
</tr>
<tr>
<td><code>spark.user.groups.mapping</code></td>
<td><code>org.apache.spark.security.ShellBasedGroupsMappingProvider</code></td>
<td>
The list of groups for a user are determined by a group mapping service defined by the trait
org.apache.spark.security.GroupMappingServiceProvider which can configured by this property. A default unix
shell based implementation is provided
<code>org.apache.spark.security.ShellBasedGroupsMappingProvider</code> which can be specified to resolve a
list of groups for a user. <em>Note:</em> This implementation supports only a Unix/Linux based environment.
Windows environment is currently <b>not</b> supported. However, a new platform/protocol can be supported by
implementing the trait <code>org.apache.spark.security.GroupMappingServiceProvider</code>.
</td>
</tr>
<tr>
<td><code>spark.authenticate</code></td>
<td>false</td>
<td>
Whether Spark authenticates its internal connections. See <code>spark.authenticate.secret</code> if not
running on YARN.
</td>
</tr>
<tr>
<td><code>spark.authenticate.secret</code></td>
<td>None</td>
<td>
Set the secret key used for Spark to authenticate between components. This needs to be set if not running on
YARN and authentication is enabled.
</td>
</tr>
<tr>
<td><code>spark.authenticate.enableSaslEncryption</code></td>
<td>false</td>
<td>
Enable encrypted communication when authentication is enabled. This is supported by the block transfer
service and the
RPC endpoints.
</td>
</tr>
<tr>
<td><code>spark.network.sasl.serverAlwaysEncrypt</code></td>
<td>false</td>
<td>
Disable unencrypted connections for services that support SASL authentication. This is currently supported
by the external shuffle service.
</td>
</tr>
<tr>
<td><code>spark.core.connection.ack.wait.timeout</code></td>
<td><code>spark.network.timeout</code></td>
<td>
How long for the connection to wait for ack to occur before timing out and giving up. To avoid unwilling
timeout caused by long pause like GC, you can set larger value.
</td>
</tr>
<tr>
<td><code>spark.core.connection.auth.wait.timeout</code></td>
<td>30s</td>
<td>
How long for the connection to wait for authentication to occur before timing out and giving up.
</td>
</tr>
<tr>
<td><code>spark.modify.acls</code></td>
<td>Empty</td>
<td>
Comma separated list of users that have modify access to the Spark job. By default only the user that
started the Spark job has access to modify it (kill it for example). Putting a "*" in the list means any
user can have access to modify it.
</td>
</tr>
<tr>
<td><code>spark.modify.acls.groups</code></td>
<td>Empty</td>
<td>
Comma separated list of groups that have modify access to the Spark job. This can be used if you have a set
of administrators or developers from the same team to have access to control the job. Putting a "*" in the
list means any user in any group has the access to modify the Spark job. The user groups are obtained from
the instance of the groups mapping provider specified by <code>spark.user.groups.mapping</code>. Check the
entry <code>spark.user.groups.mapping</code> for more details.
</td>
</tr>
<tr>
<td><code>spark.ui.filters</code></td>
<td>None</td>
<td>
Comma separated list of filter class names to apply to the Spark web UI. The filter should be a standard <a
    href="http://docs.oracle.com/javaee/6/api/javax/servlet/Filter.html"> javax servlet Filter</a>.
Parameters to each filter can also be specified by setting a java system property of: <br/> <code>spark.&lt;class
name of filter&gt;.params='param1=value1,param2=value2'</code><br/> For example: <br/> <code>-Dspark.ui.filters=com.test.filter1</code>
<br/> <code>-Dspark.com.test.filter1.params='param1=foo,param2=testing'</code></td>
</tr>
<tr>
<td><code>spark.ui.view.acls</code></td>
<td>Empty</td>
<td>
Comma separated list of users that have view access to the Spark web ui. By default only the user that
started the Spark job has view access. Putting a "*" in the list means any user can have view access to this
Spark job.
</td>
</tr>
<tr>
<td><code>spark.ui.view.acls.groups</code></td>
<td>Empty</td>
<td>
Comma separated list of groups that have view access to the Spark web ui to view the Spark Job details. This
can be used if you have a set of administrators or developers or users who can monitor the Spark job
submitted. Putting a "*" in the list means any user in any group can view the Spark job details on the Spark
web ui. The user groups are obtained from the instance of the groups mapping provider specified by <code>spark.user.groups.mapping</code>.
Check the entry <code>spark.user.groups.mapping</code> for more details.
</td>
</tr>
</table>

### Encryption
<table class="table table-bordered table-striped table-condensed">
<tr>
<th>Property Name</th>
<th>Default</th>
<th>Meaning</th>
</tr>
<tr>
<td><code>spark.ssl.enabled</code></td>
<td>false</td>
<td>
Whether to enable SSL connections on all supported protocols. <br/>When <code>spark.ssl.enabled</code> is
configured, <code>spark.ssl.protocol</code> is required. <br/>All the SSL settings like
<code>spark.ssl.xxx</code> where <code>xxx</code> is a particular configuration property, denote the global
configuration for all the supported protocols. In order to override the global configuration for the
particular protocol, the properties must be overwritten in the protocol-specific namespace. <br/>Use <code>spark.ssl.YYY.XXX</code>
settings to overwrite the global configuration for particular protocol denoted by <code>YYY</code>. Example
values for <code>YYY</code> include <code>fs</code>, <code>ui</code>, <code>standalone</code>, and <code>historyServer</code>.
See <a href="security.html#ssl-configuration">SSL Configuration</a> for details on hierarchical SSL
configuration for services.
</td>
</tr>
<tr>
<td><code>spark.ssl.enabledAlgorithms</code></td>
<td>Empty</td>
<td>
A comma separated list of ciphers. The specified ciphers must be supported by JVM. The reference list of
protocols one can find on <a
    href="https://blogs.oracle.com/java-platform-group/entry/diagnosing_tls_ssl_and_https">this</a> page.
Note: If not set, it will use the default cipher suites of JVM.
</td>
</tr>
<tr>
<td><code>spark.ssl.keyPassword</code></td>
<td>None</td>
<td>
A password to the private key in key-store.
</td>
</tr>
<tr>
<td><code>spark.ssl.keyStore</code></td>
<td>None</td>
<td>
A path to a key-store file. The path can be absolute or relative to the directory where the component is
started in.
</td>
</tr>
<tr>
<td><code>spark.ssl.keyStorePassword</code></td>
<td>None</td>
<td>
A password to the key-store.
</td>
</tr>
<tr>
<td><code>spark.ssl.keyStoreType</code></td>
<td>JKS</td>
<td>
The type of the key-store.
</td>
</tr>
<tr>
<td><code>spark.ssl.protocol</code></td>
<td>None</td>
<td>
A protocol name. The protocol must be supported by JVM. The reference list of protocols one can find on <a
    href="https://blogs.oracle.com/java-platform-group/entry/diagnosing_tls_ssl_and_https">this</a> page.
</td>
</tr>
<tr>
<td><code>spark.ssl.needClientAuth</code></td>
<td>false</td>
<td>
Set true if SSL needs client authentication.
</td>
</tr>
<tr>
<td><code>spark.ssl.trustStore</code></td>
<td>None</td>
<td>
A path to a trust-store file. The path can be absolute or relative to the directory where the component is
started in.
</td>
</tr>
<tr>
<td><code>spark.ssl.trustStorePassword</code></td>
<td>None</td>
<td>
A password to the trust-store.
</td>
</tr>
<tr>
<td><code>spark.ssl.trustStoreType</code></td>
<td>JKS</td>
<td>
The type of the trust-store.
</td>
</tr>
</table>