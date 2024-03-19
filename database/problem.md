## 药库数据库表结构
### 表3.1 供货商表（Provider）

| 字段中文名 | 字段名 | 类型 | 空值 | 备注 |
| --- | --- | --- | --- | --- |
| 供应商代码 | ProviderCode | Char(4) | No | 主码 |
| 供应商 | ProviderName | Char(60) | No |  |
| 拼音简码 | PyCode | Char(10) | Yes |  |
| 地址 | Address | Char(50) | Yes |  |
| 电话 | Tel | Char(15) | Yes |  |
| 邮编 | Zip | Char(6) | Yes |  |
| Email | Email | Char(30) | Yes |  |
| 联系人 | Relation | Char(8) | Yes |  |

~~真是愚蠢，明明第一列叫做字段中文名。难道 `Email` 没有中文名`邮箱`吗?~~

## 表3.2 药品表（Medicine）
| 字段中文名 | 字段名 | 类型 | 空值 | 备注 |
| --- | --- | --- | --- | --- |
| 药品代码 | MedicineCode | Char(5) | No | 主码 |
| 药品名称 | MedicineName | Varchar(50) | No |  |
| 拼音简码 | PyCode | Char(10) | Yes |  |
| 剂型 | DosageForm | Char(6) | Yes |  |
| 规格 | Standard | Char(15) | Yes |  |
| 单位 | Unit | Char(10) | Yes |  |
| 批号 | BatchNumber | Char(20) | Yes |  |
| 生产日期 | ProductionDate | Date | Yes |  |
| 失效日期 | ExpirationDate | Date | Yes |  |
| 药品类别 | Category | Char(10) | Yes | 中成药、西药等 |
| 医保 | YB | Char(2) | Yes | 默认为“否” |

## 表3.3 供应表 （PM）
| 字段中文名 | 字段名 | 类型 | 空值 | 备注 |
| --- | --- | --- | --- | --- |
| 药品代码 | MedicineCode | Char(5) | No | 主码 |
| 供应商代码 | ProviderCode | Char(4) | No | 主码 |
| 供应日期 | PMDate | Date | No | 主码 |
| 价格 | Price | Int | Yes |  |
| 数量 | Qyt | Int | Yes |  |

## 数据示例

参见表3.4、表3.5、表3.6。
 
### 表3.4  供货商表（Provider）实例数据
| ProviderCode | ProviderName | PyCode | Address | Tel | Zip | Email | Relation |
| --- | --- | --- | --- | --- | --- | --- | --- |
| S001 | 河北东风药业 | Hbdfyy | 河北省永年县城西 | 0310-6806999 | 057150 | hbdf@163.com | 张三 |
| S002 | 浙江康恩贝 | Zzkeb | 杭州市高新技术开发区 | 0571-87774811 | 310045 | ttm_8512@126.com | 李四 |
| S003 | 青岛鲁健药业 | Qdljyy | 青岛市北区延安路 |  | 266000 | 1375685404@qq.com | 王五 |
| S004 | 哈药制药 | Hyzy | 哈尔滨市南岗区学府路 |  | 150000 | hh@sina.com | 王六 |

### 表3.5  药品表（Medicine）实例数据
| MedicineCode | MedicineName | PyCode | DosageForm | Standard | BatchNumber | ProductionDate | ExpirationDate | category | YB |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 10001 | 小儿感冒颗粒 | Xegmkl | 颗粒剂 | 12g/袋 | Z53020405 | 2009-01-01 | 2012-12-31 | 中成药 | 是 |
| 10002 | 维生素C银翘片 | Wsscyqp | 片剂 | 49.5mg/片 | Z41022318 | 2010-01-01 | 2012-06-30 | 中成药 | 是 |
| 10003 | 清热解毒胶囊 | Qrjdjn | 胶囊剂 | 0.3g/粒 | Z20054663 | 2012-06-30 | 2014-06-30 | 中成药 | 是 |
| 10004 | 小柴胡冲剂 | Xchcj | 颗粒剂 | 10g/袋 | Z44020709 | 2012-12-01 | 2014-08-30 | 中成药 | 是 |
| 20004 | 新康泰克 | Xktk | 胶囊剂 | 0.25g/粒 | H20010430 | 2011-02-25 | 2013.08-25 | 西药 | 是 |
| 20007 | 护彤 | ht | 颗粒剂 | 2g/袋 | H23022613 | 2004-10-07 | 2007-10-07 | 西药 | 否 |
| 20008 | 救急散 | Jjs | 散剂 | 1.5g/瓶 | Z11020138 | 2012-01-01 | 2015-01-01 | 西药 | 否 |

### 表3.6  供应表 （PM）实例数据
| MedicineCode | ProviderCode | Price | Qyt | PMDate |
| --- | --- | --- | --- | --- |
| 10002 | S001 | 3.00 | 150 | 2010-02-01 |
| 10003 | S001 | 24.00 | 230 | 2012-08-01 |
| 10004 | S001 | 9.00 | 500 | 2013-01-01 |
| 10004 | S002 | 9.00 | 100 | 2013-02-02 |
| 20004 | S002 | 35.00 | 200 | 2012-01-01 |
| 20008 | S003 | 70 | 100 | 2012-04-01 |


## 作业任务
1.	创建药品表（Medicine），药品代码是主码，批号取值唯一
2.	创建供货商表（Provider），主码建为表级约束。
3.	建立供应表（PM），包含主码、外码，均为表级约束。
4.	向Medicine表增加“使用说明（Memo）”列，其数据类型为字符串类型。
5.	将Medicine表的Memo列删除
6.	将Provider表的Address列的数据类型由Char(50)改为Char(60)。
7.	删除供应表（PM）
8.	为PM数据库中的Medicine，Provider 两张表建立唯一索引
9.	删除Medicine表的UNI_Medicine_MedicineCode索引
10.	查询所有药品的药品代码、药品名称
11.	查询所有药品的剂型、药品名称、药品类别
12.	查询所有药品的详细记录
13.	查询所有药品的药品代码、药品名称、生产年份
14.	查询中成药类的药品名称
15.	查询所有过期药品的药品代码、药品名称、失效日期
16.	查询供应价格在10元以内的药品代码
17.	查询生产日期在2011-06-01与2012-06-01之间的药品信息
18.	查询片剂、散剂和颗粒剂的药品代码、药品名称
19.	查询所有拼音简码以x开头的药品代码、药品名称
20.	查询邮箱为“ttm_8512@126.com”的供应商信息
21.	查询没有提供联系电话的供应商基本信息
22.	查询2012年以后生产的剂型为“胶囊”的药品名称
23.	查询药品基本信息，查询结果按照药品类别升序排列，同一类别按照生产日期降序排列
24.	查询药品的总个数
25.	查询供应药品的供应商个数
26.	计算“10004”号药品的平均供应价格
27.	查询“S001”供应商供应的药品总数量
28.	求每个供应商供应的药品个数
29.	查询供应了3种以上药品的供应商代码
30.	查询每个供应商及其供应药品的情况
31.	查询与小儿感冒颗粒相同剂型的药品信息（自身连接完成）
32.	左外连接：查询所有供应商供应药品的情况
33.	右外连接：查询所有药品被供应的情况
34.	查询每个供应商供应的药品代码，药品名称、价格、数量、供应商名称、供应年份。
35.	查询"浙江康恩贝"供应的药品代码和药品名称（嵌套查询完成）
36.	用嵌套查询完成31题：查询与小儿感冒颗粒相同剂型的药品信息。
37.	找出每种药品供应价格超出它的供应平均价格的供应商代码。
38.	查询S001供应商供应的药品名称（使用EXISTS谓词）
39.	查询颗粒剂的药品及中成药（集合查询完成）
40.	查询颗粒剂的药品与中成药的差集
41.	将（药品代码:10007，药品名称：藿香正气水，拼音简码：hxzqs，剂型：口服液，规格：ml/支，批号：Z51021352，生产日期：-12-20，失效日期：-12-20，药品类别：中成药，是否医保：是）插入到Medicine表中
42.	对每一种药品，求供应商供应的平均价格，并把结果存入数据库。
43.	将所有药品的供应价格提高5%
44.	删除供应表中，供应商代码为S001的所有记录
45.	删除青岛鲁健药业的供应药品记录
46.	创建“浙江康恩贝”供应商供应的药品信息（包含药品代码、药品名称、药品类别、供应商等属性列）的视图
47.	将各供应商供应的药品总个数定义为一个视图。 
48.	创建医保药品的视图，并要求进行修改和插入操作时仍须保证该视图只有医保药品。
49.	将医保药品视图ISYB_Medicine中药品代码为‘10004’的药品名称改为柴胡冲剂。
50.	删除医保药品视图ISYB_Medicine中西药的记录
51.	对供应表PM中药品（10002）供应的平均价格进行查询，如果药品的均价在10元以下，显示价格便宜，否则显示价格贵。
52.	使用IF-THEN-ELSE语句查询S001供应的编码为10001的药品价格，如果大于30，则将价格重新置为20，如果小于15，则价格提升20%。
53.	分别使用LOOP,WHILE,FOR语句，输出1+2+3+。。。+100的值。
54.	将供应商表（Provider）中供应商编号为S001的ProviderName存储到一个表变量中，并打印出来。
55.	编写一个PL/SQL程序块，对所有药品按他们基本价格的20%加价，如果增加的价格大于5元就取消加价。
56.	创建一个事后语句级触发器。当向药品表插入新数据后，该触发器将统计MEDICINE表中的行数并输出。
57.	建立一个带输入IN参数的存储过程UPDATE_MEDI,为该过程设置两个参数，分别用于接收使用户提供的medicinecode和medicinename值。例如调用该存储过程可以完成medicinecode为‘10001’的medicinename改为‘云南白药’；
58.	从medicine表中查询给药品的名称和和是否医保，并利用OUT模式参数传给调用者。
59.	创建一个存储过程，完成给定药品的编号后，删除该药品信息；
60.	创建一个存储过程，完成给定某个药品编号后，求出该种药品供应的最高和最低价。
61.	创建一个函数，完成给定一个药品编号，求出该药品所有供应的价格总和。
62.	给指定的药品加价。