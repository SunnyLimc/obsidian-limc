HBase is a [[NoSQL]] database, using [[MapReduce]] to process large amount of data. Optionally it relies on [[HDFS]] as its [[High Reliability]] Storage.

- Main features:
	- [[serialize]] data into Uninterpreted String for [[NoSQL]] storage. 
	- single table operation
	- [[Decomposition Storage Model (DSM)]] : distribute column cluster into **files** for [[High Concurrency]] storage
		- it's different from [[N-ary Storage Model (NSM)]] that used in many relational database
	- used row as [[Primary Key]] for index
		- row key is a **Unique** [[Timestamp]] to identity different data 
	- add new value instead of update
	- [[Scalability]] : easy to scale by adding or subtracting hardware

- Shortcomings:
	- no cross-row atomicity
	- [[Transaction]] are not supported

- 结构
	- Master 负责表映射的负载均衡，创建表和列族，容错管理等
	- [[Zookeeper]] 负责高一致性的集群管理
		- 配置中心：数据发布和订阅
		- 只存 root 表
		- 剩下的交给客户端直接对region进行访问
	- 根据行键值进行分区，如果行数量太大，则为该区域划分一个 region
	- 使用 region 映射表来标识
		- Unique Region ID
		- Server ID
	- 一个 Server 有多个 region
	- 标识的表为 meta 元数据表
	- 如果 meta 表数量太多，就会对其拆分成 region，最终由 root 表对其进行记录（ROOT 不可拆分）
	- 为加快索引，表存在内存中
	- 客端缓存结果，若失效才继续寻址 

关于 Zookeeper 请参考 [[Concept of ZooKeeper]]

- Region 工作
	- 经历 MemStore -> HLog -> HLog persistence -> commit 的过程
	- 而 StoreFile **持久化到 HDFS** 之后就会从 HLog 中标记完成
	- 每次启动时检查 HLog，如果 HLog 存在未持久化的记录，则对这部分记录重演并持久化
	- 每个 region 都有自己的多个 store 分别对应不同的列族，每个 store 里面存有 MemStore 和 StoreFile
	- StoreFIle 多了之后就会被合并
	- 合并后如果 StoreFile 过大，region 就会对半分裂，父 region 下线，文件被移动到子 region 中 (Master 负责分配)

- 故障处理
	- 由于 HLog 的持久化，所以出现故障时 Master 只需要找到对应故障节点的 HLog，分配新的 Region 服务器，对 HLog 进行重演即可
	- HLog 在每个 region 服务器中是唯一的