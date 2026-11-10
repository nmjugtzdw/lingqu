# 灵渠 Lingqu

灵渠是面向实施与运维人员的数据库同步与迁移工具。它把「连接 → 结构迁移 → 全量装载 → 增量同步 → 校验修复 → 运行审计」组织成一条可复现的任务流程，支持在完全离线的内网环境中运行。

## 能做什么

- **异构数据库连接**：MySQL、MariaDB、PostgreSQL、Oracle、SQL Server、达梦、人大金仓、OceanBase，以及自定义 JDBC 连接
- **结构与数据一起搬**：表结构、索引、全量数据一次完成
- **增量同步**：轮询增量，或基于数据库日志的秒级 CDC（MySQL binlog、Oracle LogMiner）
- **跑得久也稳**：断点续传、并行分片、任务调度、失败重试、崩溃后重启恢复
- **数据对得上**：数据抽查、差异诊断、表级重跑、切换追赶、审计记录
- **实施工具**：ETL、字段映射、批量规划、驱动管理
- **本地化部署**：本地元数据仓库、凭据加密、账号权限、日志轮转，不依赖外网

普通用户只需要关注首页、新建任务、同步任务、连接和设置；ETL、校验、日志等专业能力收在「高级能力」中。

## 下载与启动

1. 从 [Releases](https://github.com/nmjugtzdw/lingqu/releases) 下载最新的 `lingqu-next.jar`
2. 安装 Java 17
3. 启动：

   ```bash
   java -jar lingqu-next.jar
   ```

4. 浏览器访问 <http://127.0.0.1:27402>
5. 首次启动的管理员密码保存在 `data/initial-password.txt`

端口可通过环境变量 `LINGQU_PORT` 或启动参数 `--server.port=端口` 修改。

需要覆盖外部配置时，使用追加配置方式，避免替换包内默认配置：

```bash
java -jar lingqu-next.jar --spring.config.additional-location=file:./config/
```

## 环境要求

- Java 17 或更高版本
- 建议 4 核 8 GB 以上内存；任务规模较大时按实际并发与批大小调整

## CDC 前置条件

默认轮询模式不读取源库日志；选择秒级变更捕获时，系统会在启动前检查日志能力。

- MySQL binlog 需要 `binlog_format=ROW`、`binlog_row_image=FULL`，同步账号需要 `REPLICATION SLAVE` 与 `REPLICATION CLIENT` 权限
- Oracle LogMiner 需要数据库处于 `ARCHIVELOG` 模式，开启 `SUPPLEMENTAL LOG`，并为同步表启用 `(ALL) COLUMNS` 补充日志

日志级实时捕获目前覆盖 MySQL binlog 与 Oracle LogMiner；其他数据库按其能力使用逻辑复制、CDC 表或安全轮询。

## 获取帮助

- 社区交流与常见问题：<https://bbs.zzzmxxkj.com>
- 问题反馈：使用产品内的「报告问题」入口，自动附带脱敏的诊断信息

## 关于本仓库

本仓库**仅用于发布安装包和更新说明，不包含源代码**。

灵渠为专有软件，保留所有权利。未经授权，不得复制、修改、反编译或再分发。
