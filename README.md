# exam-jvm2
java机试题目

## 需求
设计一个 api, 传入一组 skuid(不超过100个), 根据特定聚合规则(见下文), 输出一组 item, 并包含总库存和区间价. 业务需要的输出列表如下:

ITEM商品名称|货号|ITEMID|全渠道库存汇总|价格区间
---|---|---|---|---
测试商品1|xyz1||230|100~200
测试商品2||10001|0|200

### 聚合规则
对于 sku 类型为原始商品(ORIGIN)的, 按货号聚合成 item; 对于 sku 类型为ITEM商品(ITEM)的, 按 itemId 聚合成 item

### 注意事项
* ITEM商品名称可以取组内任意一个 sku 的商品名称
* 如果输入了不存在的 skuid, 后端的服务不会抛异常, 批量接口会有几个返回几个, 单条接口会返回空
* 为了依赖的后端服务 mock 起来比较方便, 测试时 skuid 请使用从1到100的字符串, 我们在 ExamTest 中提供了 setup 方法来生成输入条件

### 已有服务(接口说明见接口注释)

我们提供了一个服务工厂可以获取这些服务的实例. eg. `ServiceBeanFactory.getInstance().getServiceBean(SkuService.class)`

* 获取 sku 基本信息
* 获取 sku 库存信息(注意库存是分渠道的, 返回值里要把这些渠道库存累加)
* 获取 sku 价格