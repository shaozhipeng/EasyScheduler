### 开发模式运行

#### 前端配置，修改后端接口地址

escheduler-ui/.env

```properties
# 后端接口地址
API_BASE = http://127.0.0.1:12345
```

#### 后端配置，修改数据源dao/data_source.properties

escheduler-dao/src/main/resources/dao/data_source.properties

```properties
# base spring data source configuration
# 数据库连接
spring.datasource.type=com.alibaba.druid.pool.DruidDataSource
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
spring.datasource.url=jdbc:mysql://127.0.0.1:3306/escheduler?characterEncoding=UTF-8
spring.datasource.username=root
spring.datasource.password=123456

# connection configuration
spring.datasource.initialSize=5
# min connection number
spring.datasource.minIdle=5
# max connection number
spring.datasource.maxActive=50

# max wait time for get a connection in milliseconds. if configuring maxWait, fair locks are enabled by default and concurrency efficiency decreases.
# If necessary, unfair locks can be used by configuring the useUnfairLock attribute to true.
spring.datasource.maxWait=60000

# milliseconds for check to close free connections
spring.datasource.timeBetweenEvictionRunsMillis=60000

# the Destroy thread detects the connection interval and closes the physical connection in milliseconds if the connection idle time is greater than or equal to minEvictableIdleTimeMillis.
spring.datasource.timeBetweenConnectErrorMillis=60000

# the longest time a connection remains idle without being evicted, in milliseconds
spring.datasource.minEvictableIdleTimeMillis=300000

#the SQL used to check whether the connection is valid requires a query statement. If validation Query is null, testOnBorrow, testOnReturn, and testWhileIdle will not work.
spring.datasource.validationQuery=SELECT 1
#check whether the connection is valid for timeout, in seconds
spring.datasource.validationQueryTimeout=3

# when applying for a connection, if it is detected that the connection is idle longer than time Between Eviction Runs Millis,
# validation Query is performed to check whether the connection is valid
spring.datasource.testWhileIdle=true

#execute validation to check if the connection is valid when applying for a connection
spring.datasource.testOnBorrow=true
#execute validation to check if the connection is valid when the connection is returned
spring.datasource.testOnReturn=false
spring.datasource.defaultAutoCommit=true
spring.datasource.keepAlive=true

# open PSCache, specify count PSCache for every connection
spring.datasource.poolPreparedStatements=true
spring.datasource.maxPoolPreparedStatementPerConnectionSize=20

# data quality analysis is not currently in use. please ignore the following configuration
# task record flag
task.record.flag=false
task.record.datasource.url=jdbc:mysql://192.168.xx.xx:3306/etl?characterEncoding=UTF-8
task.record.datasource.username=xx
task.record.datasource.password=xx
```

#### 后端配置，修改common/common.properties

escheduler-common/src/main/resources/common/common.properties

```properties
#task queue implementation, default "zookeeper" 
# 保持不变
escheduler.queue.impl=zookeeper

# user data directory path, self configuration, please make sure the directory exists and have read write permissions
# 本地路径
data.basedir.path=/Users/shaozhipeng/Development/escheduler/fs

# directory path for user data download. self configuration, please make sure the directory exists and have read write permissions
# 本地路径
data.download.basedir.path=/Users/shaozhipeng/Development/escheduler/download

# process execute directory. self configuration, please make sure the directory exists and have read write permissions
# 本地路径
process.exec.basepath=/Users/shaozhipeng/Development/escheduler/exec

# Users who have permission to create directories under the HDFS root path
# HDFS user
hdfs.root.user=shaozhipeng

# data base dir, resource file will store to this hadoop hdfs path, self configuration, please make sure the directory exists on hdfs and have read write permissions。"/escheduler" is recommended
# HDFS root path
data.store2hdfs.basepath=/escheduler

# resource upload startup type : HDFS,S3,NONE
res.upload.startup.type=HDFS

# whether kerberos starts
hadoop.security.authentication.startup.state=false

# java.security.krb5.conf path
java.security.krb5.conf.path=/opt/krb5.conf

# loginUserFromKeytab user
login.user.keytab.username=hdfs-mycluster@ESZ.COM

# loginUserFromKeytab path
login.user.keytab.path=/opt/hdfs.headless.keytab

# system env path. self configuration, please make sure the directory and file exists and have read write execute permissions
escheduler.env.path=/opt/.escheduler_env.sh

#resource.view.suffixs 
# 增加资源文件扩展名json
resource.view.suffixs=txt,log,sh,conf,cfg,py,java,sql,hql,xml,json

# is development state? default "false"
# 开发模式
development.state=true
```

#### 后端配置，修改common/hadoop/hadoop.properties

escheduler-common/src/main/resources/common/hadoop/hadoop.properties

```properties
# ha or single namenode,If namenode ha needs to copy core-site.xml and hdfs-site.xml
# to the conf directory，support s3，for example : s3a://escheduler
# HDFS配置
fs.defaultFS=hdfs://127.0.0.1:8020

# s3 need，s3 endpoint
fs.s3a.endpoint=http://192.168.199.91:9010

# s3 need，s3 access key
fs.s3a.access.key=A3DXS30FO22544RE

# s3 need，s3 secret key
fs.s3a.secret.key=OloCLq3n+8+sdPHUhJ21XrSxTC+JK

#resourcemanager ha note this need ips , this empty if single
yarn.resourcemanager.ha.rm.ids=

# If it is a single resourcemanager, you only need to configure one host name. If it is resourcemanager HA, the default configuration is fine
# YARN RM配置
yarn.application.status.address=http://127.0.0.1:8088/ws/v1/cluster/apps/%s
```

#### 后端配置，修改quartz.properties

escheduler-common/src/main/resources/quartz.properties

```properties
#============================================================================
# Configure Datasources  这里修改一下数据库连接
#============================================================================

org.quartz.dataSource.myDs.driver = com.mysql.jdbc.Driver
org.quartz.dataSource.myDs.URL = jdbc:mysql://127.0.0.1:3306/escheduler?characterEncoding=UTF-8
org.quartz.dataSource.myDs.user = root
org.quartz.dataSource.myDs.password = 123456
org.quartz.dataSource.myDs.maxConnections = 10
org.quartz.dataSource.myDs.validationQuery = select 1
```

#### 后端配置，修改zookeeper.properties

escheduler-common/src/main/resources/zookeeper.properties

```properties
#zookeeper cluster
zookeeper.quorum=127.0.0.1:2184,127.0.0.1:2185,127.0.0.1:2186
```

#### 前后端启动

后端主要启动，Main方法直接运行即可：

escheduler-server/src/main/java/cn/escheduler/server/master/MasterServer.java

escheduler-server/src/main/java/cn/escheduler/server/worker/WorkerServer.java

如果要看日志：

escheduler-server/src/main/java/cn/escheduler/server/rpc/LoggerServer.java

后台管理系统的API：

escheduler-api/src/main/java/cn/escheduler/api/ApiApplicationServer.java

前端：

```bash
$ npm start
Project is running at http://localhost:8888/
```

初始用户名密码 admin/escheduler123

### 编译打包

#### 后端

```bash
$ mvn -U clean package assembly:assembly -Dmaven.test.skip=true
```

#### 前端

```bash
$ cd escheduler-ui
$ npm install -g cnpm --registry=https://registry.npm.taobao.org
$ npm run build
```

### 后端改动

#### 修改数据库升级的版本号sql/upgrade/1.1.0_schema/mysql/escheduler_dml.sql

```sql
UPDATE `t_escheduler_version` SET `version`= '1.1.0';
```

#### 修改package.xml打包包括跟文件夹

```xml
<includeBaseDirectory>true</includeBaseDirectory>
```

#### 修改pom.xml 支持高版本的MySQL Server

```xml
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.18</version>
</dependency>
```

### 前端改动

#### 修改js，前端控制不允许执行非查询SQL

escheduler-ui/src/js/conf/home/pages/dag/_source/formModel/tasks/_source/commcon.js

```javascript
/**
 * sqlType
 */
const sqlTypeList = [
  {
    id: 0,
    code: `${i18n.$t('Query')}`
  }
  // ,
  // {
  //   id: 1,
  //   code: `${i18n.$t('Non Query')}`
  // }
]
```

#### 修改js，前端资源文件扩展名增加json选项

escheduler-ui/src/js/conf/home/pages/resource/pages/file/pages/_source/common.js

```javascript
/**
 * Create file type
 */
let filtTypeArr = ['txt', 'log', 'sh', 'conf', 'cfg', 'py', 'java', 'sql', 'xml', 'hql', 'json']

export { filtTypeArr }
```



Easy Scheduler
==============
[![License](https://img.shields.io/badge/license-Apache%202-4EB1BA.svg)](https://www.apache.org/licenses/LICENSE-2.0.html)

> Easy Scheduler for Big Data

**设计特点：** 一个分布式易扩展的可视化DAG工作流任务调度系统。致力于解决数据处理流程中错综复杂的依赖关系，使调度系统在数据处理流程中`开箱即用`。
其主要目标如下：
 - 以DAG图的方式将Task按照任务的依赖关系关联起来，可实时可视化监控任务的运行状态
 - 支持丰富的任务类型：Shell、MR、Spark、SQL(mysql、postgresql、hive、sparksql),Python,Sub_Process、Procedure等
 - 支持工作流定时调度、依赖调度、手动调度、手动暂停/停止/恢复，同时支持失败重试/告警、从指定节点恢复失败、Kill任务等操作
 - 支持工作流优先级、任务优先级及任务的故障转移及任务超时告警/失败
 - 支持工作流全局参数及节点自定义参数设置
 - 支持资源文件的在线上传/下载，管理等，支持在线文件创建、编辑
 - 支持任务日志在线查看及滚动、在线下载日志等
 - 实现集群HA，通过Zookeeper实现Master集群和Worker集群去中心化
 - 支持对`Master/Worker` cpu load，memory，cpu在线查看
 - 支持工作流运行历史树形/甘特图展示、支持任务状态统计、流程状态统计
 - 支持补数
 - 支持多租户
 - 支持国际化
 - 还有更多等待伙伴们探索

### 与同类调度系统的对比

![调度系统对比](http://geek.analysys.cn/static/upload/47/2019-03-01/9609ca82-cf8b-4d91-8dc0-0e2805194747.jpeg)

### 系统部分截图

![](http://geek.analysys.cn/static/upload/221/2019-03-29/0a9dea80-fb02-4fa5-a812-633b67035ffc.jpeg)

![](http://geek.analysys.cn/static/upload/221/2019-04-01/83686def-a54f-4169-8cae-77b1f8300cc1.png)

![](http://geek.analysys.cn/static/upload/221/2019-03-29/83c937c7-1793-4d7a-aa28-b98460329fe0.jpeg)

### 文档

- <a href="https://analysys.github.io/easyscheduler_docs_cn/后端部署文档.html" target="_blank">后端部署文档</a>

- <a href="https://analysys.github.io/easyscheduler_docs_cn/前端部署文档.html" target="_blank">前端部署文档</a>

- [**使用手册**](https://analysys.github.io/easyscheduler_docs_cn/系统使用手册.html?_blank "系统使用手册") 

- [**升级文档**](https://analysys.github.io/easyscheduler_docs_cn/升级文档.html?_blank "升级文档") 

- <a href="http://52.82.13.76:8888" target="_blank">我要体验</a> 

更多文档请参考 <a href="https://analysys.github.io/easyscheduler_docs_cn/" target="_blank">easyscheduler中文在线文档</a>


### 近期研发计划

EasyScheduler的工作计划：<a href="https://github.com/analysys/EasyScheduler/projects/1" target="_blank">研发计划</a> ，其中 In Develop卡片下是1.1.0版本的功能，TODO卡片是待做事项(包括 feature ideas)

### 贡献代码

非常欢迎大家来参与贡献代码，提交代码流程请参考：
https://github.com/analysys/EasyScheduler/blob/master/CONTRIBUTING.md


### 感谢

Easy Scheduler使用了很多优秀的开源项目，比如google的guava、guice、grpc，netty，ali的bonecp，quartz，以及apache的众多开源项目等等，
正是由于站在这些开源项目的肩膀上，才有Easy Scheduler的诞生的可能。对此我们对使用的所有开源软件表示非常的感谢！我们也希望自己不仅是开源的受益者，也能成为开源的
贡献者，于是我们决定把易调度贡献出来，并承诺长期维护。也希望对开源有同样热情和信念的伙伴加入进来，一起为开源献出一份力！


### 帮助
The fastest way to get response from our developers is to submit issues,   or add our wechat : 510570367
 







