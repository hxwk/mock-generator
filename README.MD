-- 创建ODS层对应的数据库
CREATE DATABASE IF NOT EXISTS rtdw_ods;
-- 字典表
CREATE TABLE IF NOT EXISTS rtdw_ods.tbl_ods_dict (
	id BIGINT COMMENT '自增ID', 
	dictCode STRING COMMENT '编码',
	dictDesc STRING COMMENT '名称',
	categoryCode STRING COMMENT '分类编码',
	categoryDesc STRING COMMENT '分类说明',
	sortNo INT COMMENT '排序编号', 
	dataType STRING COMMENT '数据类型', 
	remark STRING COMMENT '附加说明', 
	locateCode STRING COMMENT '检索标识',
	cid BIGINT COMMENT '创建人ID',
	uid BIGINT COMMENT '修改人ID', 
	ctime TIMESTAMP COMMENT '创建时间',
	utime TIMESTAMP COMMENT '更新时间',
	version INT COMMENT '乐观锁版本号',
	PRIMARY KEY(id) NOT ENFORCED
) WITH (
	'connector' = 'jdbc',
	'url' = 'jdbc:mysql:///rtdw',
	'table-name' = 'tbl_dict',
	'username' = 'root',
	'password' = 'MySQL_PWD_123'
);
-- 优惠券表
CREATE TABLE IF NOT EXISTS rtdw_ods.tbl_ods_discount (
    id INT COMMENT '优惠活动类型:1-新用户立减；2-满减；3-抵价券；4-套餐赠送；5-满赠；6-超时赔付；7-特价菜；8-首单返优惠券；9-使用红包；11-提前下单减；12-满返优惠券；16-满免配送费；17-折扣商品；18-美团专送再减；19-点评券；20-第二份半价；21-会员免配送费；22-门店新客立减；23-买赠；24-平台新用户立减；25-满减配送费；27-指定商品满减；28-新客满减；30-阶梯满减配送费；36-平台新客减配送费；40-加价购；41-新客折扣菜；45-外卖拼团；46-外卖加价购；48-拼团减配送费；53-新客专享减包装费54-新客专享减配送费；100-满返商家代金券；101-使用商家代金券；103-进店领券；117-商品券；118-商品折扣券；305-津贴联盟；306-立减金；307-套餐推荐津贴', 
    ctime TIMESTAMP COMMENT '创建时间',
    utime TIMESTAMP COMMENT '更新时间',
    remark STRING COMMENT '优惠说明',
	PRIMARY KEY(id) NOT ENFORCED
) WITH (
 	'connector' = 'jdbc',
 	'url' = 'jdbc:mysql:///rtdw',
 	'table-name' = 'tbl_discount',
 	'username' = 'root',
 	'password' = 'MySQL_PWD_123'
);
-- 餐品表 
CREATE TABLE IF NOT EXISTS rtdw_ods.tbl_ods_food (
    app_food_code STRING COMMENT '菜品id',
    food_name STRING COMMENT '菜品名', 
    price FLOAT COMMENT '菜品价格', 
    sku_id STRING COMMENT '菜品sku', 
    unit STRING COMMENT '单位(份)', 
    PRIMARY KEY (app_food_code) NOT ENFORCED
) WITH (
  	'connector' = 'jdbc',
  	'url' = 'jdbc:mysql:///rtdw',
  	'table-name' = 'tbl_food',
  	'username' = 'root',
  	'password' = 'MySQL_PWD_123'
);
-- 订单表
CREATE TABLE IF NOT EXISTS rtdw_ods.tbl_ods_order ( 
    ctime TIMESTAMP COMMENT '订单创建时间',
    caution STRING COMMENT '备注',
    cityId BIGINT COMMENT '城市Id', 
    deliveryTime TIMESTAMP COMMENT '用户预计送达时间，“立即送达”时为0', 
    ePoiId STRING COMMENT '三方系统中的门店Id',
    hasInvoiced INT COMMENT '是否需要发票:1-需要发票；0-不需要',
    invoiceTitle STRING COMMENT '发票抬头', 
    taxpayerId STRING COMMENT '发票税号',
    isThirdShipping INT COMMENT '是否第三方配送 0：否；1：是（目前基本上不支持第三方配送）', 
    latitude double COMMENT '实际送餐地址纬度（高德系坐标）',
    longitude double COMMENT '实际送餐地址经度（高德系坐标）', 
    logisticsCancelTime TIMESTAMP COMMENT '取消配送时间',
    logisticsCode STRING COMMENT '配送类型码',
    logisticsCompletedTime TIMESTAMP COMMENT '配送完成时间', 
    logisticsConfirmTime TIMESTAMP COMMENT '配送单确认时间，骑手接单时间', 
    logisticsDispatcherMobile STRING COMMENT '骑手电话', 
    logisticsDispatcherName STRING COMMENT '骑手姓名', 
    logisticsFetchTime TIMESTAMP COMMENT '骑手取单时间', 
    logisticsId INT COMMENT '配送方ID',
    logisticsName STRING COMMENT '配送方名称',
    logisticsSendTime TIMESTAMP COMMENT '配送单下单时间', 
    logisticsStatus INT COMMENT '配送订单状态', 
    orderCompletedTime TIMESTAMP COMMENT '订单完成时间', 
    orderConfirmTime TIMESTAMP COMMENT '商家确认时间', 
    orderCancelTime TIMESTAMP COMMENT '订单取消时间',
    orderId BIGINT COMMENT '订单ID',
    orderIdView BIGINT COMMENT '订单展示Id',
    orderSendTime TIMESTAMP COMMENT '用户下单时间',
    originalPrice FLOAT COMMENT '订单原价', 
    payType INT COMMENT '订单支付类型（1：货到付款；2：在线支付）',
    poiAddress STRING COMMENT '门店地址', 
    poiId BIGINT COMMENT '门店Id',
    poiName STRING COMMENT '门店名称',
    poiPhone STRING COMMENT '门店服务电话',
    recipientAddress STRING COMMENT '收货人地址',
    recipientName STRING COMMENT '收货人名称',
    recipientPhone STRING COMMENT '收货人电话',
    backupRecipientPhone STRING COMMENT '备份隐私号',
    shippERPhone STRING COMMENT '骑手电话',
    shippingFee FLOAT COMMENT '配送费用', 
    status INT COMMENT '订单状态',
    total FLOAT COMMENT '总价', 
    uTime TIMESTAMP COMMENT '订单更新时间',
    daySeq INT COMMENT '门店当天的订单流水号（每天流水号从1开始）', 
    dinnersNumber INT COMMENT '就餐人数', 
    pickType INT COMMENT '取餐类型',
    isFavorites tinyint COMMENT '用户是否收藏此门店', 
    isPoiFirstOrder tinyint COMMENT '用户是否第一次在此门店点餐', 
    orderTagList STRING COMMENT '订单业务打标枚举，即业务身份（json字符串格式的数组，例："[16]"）', 
    PRIMARY KEY(orderId) NOT ENFORCED
) WITH (
 	'connector' = 'kafka',
 	'topic' = 'order',
    'properties.bootstrap.servers' = 'node1.itcast.cn:9092',
    'properties.group.id' = 'testGroup',
    'scan.startup.mode' = 'earliest-offset',
    'format' = 'canal-json',
    'canal-json.ignore-parse-errors' = 'true',
    'canal-json.database.include' = 'rtdw',
    'canal-json.table.include' = 'tbl_order'
);
-- 订单优惠表
CREATE TABLE IF NOT EXISTS rtdw_ods.tbl_ods_order_discount_map (
    id BIGINT, 
    orderId BIGINT COMMENT '订单ID',
    discountId BIGINT COMMENT '优惠ID', 
    ctime TIMESTAMP COMMENT '创建时间',
    utime TIMESTAMP COMMENT '更新时间',
    remark STRING COMMENT '备注', 
    PRIMARY KEY (id) NOT ENFORCED
) WITH (
 	'connector' = 'kafka',
 	'topic' = 'order',
    'properties.bootstrap.servers' = 'node1.itcast.cn:9092',
    'properties.group.id' = 'testGroup',
    'scan.startup.mode' = 'earliest-offset',
    'format' = 'canal-json',
    'canal-json.ignore-parse-errors' = 'true',
    'canal-json.database.include' = 'rtdw',
    'canal-json.table.include' = 'tbl_order_discount_map'
);
-- 订单餐品表
CREATE TABLE IF NOT EXISTS rtdw_ods.tbl_ods_order_food_map (
    id BIGINT,
    orderId BIGINT COMMENT '订单ID', 
    foodId STRING COMMENT '餐品ID', 
    ctime TIMESTAMP COMMENT '创建时间', 
    utime TIMESTAMP COMMENT '更新时间', 
    remark STRING COMMENT '备注',
    PRIMARY KEY(id) NOT ENFORCED
) WITH (
 	'connector' = 'kafka',
 	'topic' = 'order',
    'properties.bootstrap.servers' = 'node1.itcast.cn:9092',
    'properties.group.id' = 'testGroup',
    'scan.startup.mode' = 'earliest-offset',
    'format' = 'canal-json',
    'canal-json.ignore-parse-errors' = 'true',
    'canal-json.database.include' = 'rtdw',
    'canal-json.table.include' = 'tbl_order_food_map'
);
-- 商户表
CREATE TABLE IF NOT EXISTS rtdw_ods.tbl_ods_poi (
    id BIGINT COMMENT '门店Id', 
    epoiId STRING COMMENT 'ERP系统的门店ID', 
    poiAddress STRING COMMENT '门店地址',
    poiName STRING COMMENT '门店名称', 
    poiPhone STRING COMMENT '门店服务电话', 
    ctime TIMESTAMP COMMENT '创建时间', 
    utime TIMESTAMP COMMENT '更新时间', 
    remark STRING COMMENT '备注',
    PRIMARY KEY(id) NOT ENFORCED
) WITH (
 	'connector' = 'jdbc',
 	'url' = 'jdbc:mysql:///rtdw',
 	'table-name' = 'tbl_poi',
 	'username' = 'root',
 	'password' = 'MySQL_PWD_123'
);
-- 商户优惠券表
CREATE TABLE IF NOT EXISTS rtdw_ods.tbl_ods_poi_discount_map (
    id BIGINT, 
    poiId BIGINT COMMENT '门店Id',
    discountId BIGINT COMMENT '优惠ID', 
    reduceFee FLOAT COMMENT '活动优惠金额，在原价基础上减免的金额', 
    mtCharge FLOAT COMMENT '该活动中美团承担的费用', 
    poiCharge FLOAT COMMENT '该活动中商家承担的费用',
    ctime TIMESTAMP COMMENT '创建时间',
    utime TIMESTAMP COMMENT '更新时间',
    remark STRING,
    PRIMARY KEY (id) NOT ENFORCED
) WITH (
 	'connector' = 'jdbc',
 	'url' = 'jdbc:mysql:///rtdw',
 	'table-name' = 'tbl_poi_discount_map',
 	'username' = 'root',
 	'password' = 'MySQL_PWD_123'
);
-- 商户餐品表
CREATE TABLE IF NOT EXISTS rtdw_ods.tbl_ods_poi_food_map (
    id BIGINT, 
    poiId BIGINT COMMENT '门店Id',
    foodId STRING COMMENT '餐品ID',
    PRIMARY KEY (id) NOT ENFORCED
) WITH (
 	'connector' = 'jdbc',
 	'url' = 'jdbc:mysql:///rtdw',
 	'table-name' = 'tbl_poi_food_map',
 	'username' = 'root',
 	'password' = 'MySQL_PWD_123'
);

-- 创建DWD层
CREATE DATABASE IF NOT EXISTS rtdw_dwd;
-- DWD层订单表
CREATE TABLE IF NOT EXISTS rtdw_dwd.tbl_dwd_order (
    ctime TIMESTAMP COMMENT '订单创建时间',
    caution STRING COMMENT '备注',
    cityId BIGINT COMMENT '城市Id', 
    deliveryTime TIMESTAMP COMMENT '用户预计送达时间，“立即送达”时为0', 
    ePoiId STRING COMMENT '三方系统中的门店Id',
    hasInvoiced INT COMMENT '是否需要发票:1-需要发票；0-不需要',
    invoiceTitle STRING COMMENT '发票抬头', 
    taxpayerId STRING COMMENT '发票税号',
    isThirdShipping INT COMMENT '是否第三方配送 0：否；1：是（目前基本上不支持第三方配送）', 
    latitude double COMMENT '实际送餐地址纬度（高德系坐标）',
    longitude double COMMENT '实际送餐地址经度（高德系坐标）', 
    logisticsCancelTime TIMESTAMP COMMENT '取消配送时间',
    logisticsCode STRING COMMENT '配送类型码',
    logisticsCompletedTime TIMESTAMP COMMENT '配送完成时间', 
    logisticsConfirmTime TIMESTAMP COMMENT '配送单确认时间，骑手接单时间',
    logisticsDispatcherMobile STRING COMMENT '骑手电话', 
    logisticsDispatcherName STRING COMMENT '骑手姓名', 
    logisticsFetchTime TIMESTAMP COMMENT '骑手取单时间', 
    logisticsId INT COMMENT '配送方ID',
    logisticsName STRING COMMENT '配送方名称',
    logisticsSendTime TIMESTAMP COMMENT '配送单下单时间', 
    logisticsStatus INT COMMENT '配送订单状态', 
    orderCompletedTime TIMESTAMP COMMENT '订单完成时间', 
    orderConfirmTime TIMESTAMP COMMENT '商家确认时间', 
    orderCancelTime TIMESTAMP COMMENT '订单取消时间',
    orderId BIGINT COMMENT '订单ID',
    orderIdView BIGINT COMMENT '订单展示Id',
    orderSendTime TIMESTAMP COMMENT '用户下单时间',
    originalPrice FLOAT COMMENT '订单原价', 
    payType INT COMMENT '订单支付类型（1：货到付款；2：在线支付）',
    poiAddress STRING COMMENT '门店地址', 
    poiId BIGINT COMMENT '门店Id',
    poiName STRING COMMENT '门店名称',
    poiPhone STRING COMMENT '门店服务电话',
    recipientAddress STRING COMMENT '收货人地址',
    recipientName STRING COMMENT '收货人名称',
    recipientPhone STRING COMMENT '收货人电话',
    backupRecipientPhone STRING COMMENT '备份隐私号',
    shippERPhone STRING COMMENT '骑手电话',
    shippingFee FLOAT COMMENT '配送费用', 
    status INT COMMENT '订单状态',
    total FLOAT COMMENT '总价', 
    uTime TIMESTAMP COMMENT '订单更新时间',
    daySeq INT COMMENT '门店当天的订单流水号（每天流水号从1开始）', 
    dinnersNumber INT COMMENT '就餐人数', 
    pickType INT COMMENT '取餐类型',
    isFavorites tinyint COMMENT '用户是否收藏此门店', 
    isPoiFirstOrder tinyint COMMENT '用户是否第一次在此门店点餐', 
    orderTagList STRING COMMENT '订单业务打标枚举，即业务身份（json字符串格式的数组，例："[16]"）', 
    payTypeName STRING COMMENT '订单支付类型名称',
    logisticsStatusName STRING COMMENT '配送状态名称',
    statusName STRING COMMENT '订单状态名称',
    foods MULTISET<STRING> COMMENT '所有的餐品',
    discounts MULTISET<STRING> COMMENT '所有的优惠券'
) WITH (
 	'connector' = 'kafka',
 	'topic' = 'dwd_order',
    'properties.bootstrap.servers' = 'node1.itcast.cn:9092',
    'properties.group.id' = 'testGroup',
    'scan.startup.mode' = 'earliest-offset',
    'format' = 'json'
);
-- 从ODS层->DWD层tbl_ods_order_food_map
INSERT INTO rtdw_dwd.tbl_dwd_order SELECT t02.*,t04.categoryDesc AS payTypeName,t05.categoryDesc AS logisticsStatusName,t06.categoryDesc AS statusName,t01.foods,t03.discounts FROM (SELECT t.orderId,collect(t.food) AS foods FROM (SELECT t1.orderId,CONCAT_WS(',',t2.app_food_code,t2.food_name,CAST(t2.price AS STRING),CAST(t2.sku_id AS STRING),t2.unit) AS food FROM rtdw_ods.tbl_ods_order_food_map t1 LEFT JOIN rtdw_ods.tbl_ods_food t2 ON t2.app_food_code=t1.foodId) t GROUP BY t.orderId) t01 LEFT JOIN rtdw_ods.tbl_ods_order t02 ON t02.orderId=t01.orderId LEFT JOIN (SELECT t21.orderId,COLLECT(t21.discounts) AS discounts FROM (SELECT t1.orderId,CONCAT_WS(',',CAST(t2.poiId AS STRING),CAST(t3.id AS STRING),t3.remark,CAST(t2.reduceFee AS STRING),CAST(t2.mtCharge AS STRING),CAST(t2.poiCharge AS STRING)) AS discounts FROM rtdw_ods.tbl_ods_order_discount_map t1 LEFT JOIN rtdw_ods.tbl_ods_poi_discount_map t2 ON t2.discountId=t1.discountId LEFT JOIN rtdw_ods.tbl_ods_discount t3 ON t3.id=t2.discountId WHERE t2.poiId IS NOT NULL) t21 GROUP BY t21.orderId) t03 ON t03.orderId=t02.orderId LEFT JOIN (SELECT CAST(categoryCode AS INT) AS categoryCode,categoryDesc FROM rtdw_ods.tbl_ods_dict WHERE dictCode='100003') t04 ON t04.categoryCode=t02.payType LEFT JOIN (SELECT CAST(categoryCode AS INT) AS categoryCode,categoryDesc FROM rtdw_ods.tbl_ods_dict WHERE dictCode='100005') t05 ON t05.categoryCode=t02.logisticsStatus LEFT JOIN (SELECT CAST(categoryCode AS INT) AS categoryCode,categoryDesc FROM rtdw_ods.tbl_ods_dict WHERE dictCode='100006') t06 ON t06.categoryCode=t02.status;
-- 
DROP TABLE default_catalog.rtdw_dwd.tbl_dwd_order;

