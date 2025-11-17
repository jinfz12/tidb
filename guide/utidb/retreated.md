# 回档管理

当数据库出现误操作或数据损坏等问题时，可通过 TiDB 回档功能将数据库恢复至指定时间点的数据状态，从而减少数据丢失和业务中断的风险。

TiDB 回档在GC safe point范围内支持回档至新实例或回档至原实例功能。
回档到新实例会在同项目同可用区下新建同规格实例。
回档到原实例提供覆盖原库表和不覆盖原库表两种方式。
选择时请注意。

在实例详情页中，点击“回档管理”菜单，进入回档管理页面。

![](https://tidb-doc.cn-bj.ufileos.com/utidb/retreated-list1.png)

## 回档至新实例

在回档管理页面中，点击“回档至新实例”按钮，选择待回档库表及回档目标时间，输入新实例名称及密码，确认购买金额后点击“立即购买”按钮。
![](https://tidb-doc.cn-bj.ufileos.com/utidb/retreated-new.png)

回档任务创建后，会返回资源列表页面，回档任务状态可在原实例详情页回档管理页中查看当前回档任务状态

![](https://tidb-doc.cn-bj.ufileos.com/utidb/retreated-list2.png)


## 回档至原实例

在回档管理页面中，点击“回档至原实例”按钮，选择待回档库表及回档目标时间，选择是否覆盖原库表。
确认无误后，点击“确定”按钮。

### 覆盖原库表
覆盖原库库表时，仅支持全库全表回档，回档期间数据库不可读写，回档效率高用时较短。
对于支持暂停业务场景，建议选择覆盖原库表方式。
![](https://tidb-doc.cn-bj.ufileos.com/utidb/retreated-cover01.png)

### 不覆盖原库表
不覆盖原库表时，可指定库表回档，回档期间数据库可读写，回档效率相对较低，用时较长。
对于指定库表回档或业务不可中断场景，建议选择不覆盖原库表方式。
![](https://tidb-doc.cn-bj.ufileos.com/utidb/retreated-cover02.png)
回档完成后，回档的库会重命名为原库名_bak_时间格式

确认创建后，会返回至回档任务列表页面.
![](https://tidb-doc.cn-bj.ufileos.com/utidb/retreated-list2.png)