# 实例

## 创建TiDB集群
- 点击【创建集群】

![](https://tidb-doc.cn-bj.ufileos.com/utidb/utidb-create01.png)

- 选择节点配置

计算规格：可按一定CPU内存比选择，目前支持1:2、1:4、1:8

磁盘大小：TiKV最小磁盘1000G，加减步长500；TiDB最小200G，加减步长100；PD磁盘不计费，不做选择

节点数量：TiKV/PD 最小3节点，TiDB最小2节点，加减步长为1

![](https://tidb-doc.cn-bj.ufileos.com/utidb/utidb-create02.png)

- 网络设置

选择实例所属VPC及子网， 系统会默认选择列表中的第一项。

- 指定IP及端口创建实例

默认情况下系统会自动分配一个IP及一个端口用来访问数据库。 如果用户需要指定IP及端口， 请选中“指定IP及端口”框， 页面会增加显示IP及端口输入框， 在对应的输入框中输入指定内容即可。

> 端口范围 1~65535, 禁用端口: 22,25,111,7002,7041,8080,9100,9115
 
![](https://tidb-doc.cn-bj.ufileos.com/utidb/utidb_create03.png)

- 管理设置

![](https://tidb-doc.cn-bj.ufileos.com/utidb/create_password.png)

可以为实例自定义实例名称， 系统默认会生成“TiDB”。 设置管理员(root)密码， 可以自行输入， 也可以点击“随机生成”。

- 询价购买

![](https://tidb-doc.cn-bj.ufileos.com/utidb/buy_p.png)

当节点配置信息确定以后， 右上角选择付费周期，会显示当前付费周期合计费用。

确认合计费用后，点击“立即购买” 按钮来创建实例。

## 查看TiDB实例列表

进入产品主页，选择TiDB，会默认列出当前地域的实例列表。 

![](https://tidb-doc.cn-bj.ufileos.com/utidb/tidb-list.png)


## 查看TiDB实例详情

进入产品主页面，选择TiDB，会默认列出当前地域的实例列表。 找到实例，点击操作栏中的“详情”按钮进入详情页面。

![](https://tidb-doc.cn-bj.ufileos.com/utidb/detail.png)

详情页面左侧会显示实例的基础信息等内容，可以在页面左侧进行续费操作， 右上侧会展示集群当前节点配置及状态信息，右下侧会展示监控信息，监控项有数据量，QPS，TPS，内存使用量等。

<!-- ## 查看TiDB实例Dashboard

进入产品主页，选择TiDB，会默认列出当前地域的实例列表。 找到实例，点击操作栏中的“Dashboard”按钮跳转TiDB原生Dashboard页面。

![](https://tidb-doc.cn-bj.ufileos.com/utidb/utidb-dashboardbutton01.png)

也可以通过详情页面Dashboard按钮跳转

![](https://tidb-doc.cn-bj.ufileos.com/utidb/utidb-dashboardbutton02.png)

打开Dashboard页面后，输入集群root账户信息登录 -->

<!-- ## 查看TiDB实例Grafana/Prometheus

进入产品主页，选择TiDB，会默认列出当前地域的实例列表。 找到实例，点击操作栏中的“...”按钮，选择Grafana/Prometheus按钮，跳转TiDB原生Grafana/Prometheus页面。

![](https://tidb-doc.cn-bj.ufileos.com/utidb/utidb-grafanabutton01.png)

也可以通过详情页面Grafana/Prometheus按钮跳转

![](https://tidb-doc.cn-bj.ufileos.com/utidb/utidb-grafanabutton02.png)

打开Grafana页面后，输入集群root账户信息登录

若您未购买外网ULB资源，仅临时需要查看监控页面，则可根据Proxy节点信息，自行搭建带外网的代理服务
![](https://tidb-doc.cn-bj.ufileos.com/utidb/utidb-proxy.png) -->

## 删除TiDB实例

进入产品主页，选择TiDB，会默认列出当前地域的实例列表。 找到需要删除的实例，点击操作栏中的“删除实例”按钮进入删除确认页面。

![](https://tidb-doc.cn-bj.ufileos.com/utidb/delete.png)

