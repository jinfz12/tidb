# 运维管理
运维管理功能支持对 TiDB Serverless 实例执行自动化运维操作，所有操作均会在您设定的运维窗口内进行，避免影响线上业务。

在实例详情页，点击 运维管理 即可进入运维管理页面。

运维管理当前支持配置运维窗口与自动 Analyze 功能。

## 运维窗口
运维窗口是指允许对实例执行运维操作的时间段。
默认窗口为 每天 00:00–23:59，您可根据业务需要自定义时间段。

### 设置方法：

在实例概览页的 运维窗口 区域，点击“编辑”按钮。

选择所需的时间段。

点击“确认”保存设置。

![](https://tidb-doc.cn-bj.ufileos.com/oprsconfig/opstime2.png)

> 提示：建议将运维窗口设置在业务低峰期。

## 自动 Analyze
自动 Analyze 功能可定期更新表统计信息，帮助优化 SQL 执行效率。
该功能默认处于 关闭 状态，您可按需开启。

### 开启/关闭自动 Analyze
在运维管理页面的 Analyze 区域，可通过开关控制功能的启停。

![](https://tidb-doc.cn-bj.ufileos.com/oprsconfig/analyze1.png)

开启后，该区域将展示：

执行规则数：已配置的自动 Analyze 规则数量。

最近一次执行时间：包括开始时间与结束时间。
若任务尚未执行完毕，结束时间将显示为“–”。

![](https://tidb-doc.cn-bj.ufileos.com/oprsconfig/analyze2.png)

### 配置执行规则
您可以设置自动 Analyze 的执行规则，包括：

执行库表：选择需要自动 Analyze 的数据库和数据表。

健康度阈值：当指定库表的统计信息健康度低于该阈值时，系统会自动触发 Analyze 操作。

![](https://tidb-doc.cn-bj.ufileos.com/oprsconfig/analyze3.png)
![](https://tidb-doc.cn-bj.ufileos.com/oprsconfig/analyze4.png)

> 注意：若当前已有自动 Analyze 任务正在执行，新修改的规则将在下一次运维窗口生效。