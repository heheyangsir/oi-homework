# 药库数据库作业1

[在这里查看作业](problem.md)

## 作业任务
1.	创建药品表（Medicine），药品代码是主码，批号取值唯一

```sql
CREATE TABLE Medicine (
    MedicineCode Char(5) PRIMARY KEY,
    MedicineName Varchar(50) NOT NULL,
    PyCode Char(10),
    DosageForm Char(18),
    Standard Char(15),
    Unit Char(10),
    BatchNumber Char(20) UNIQUE,
    ProductionDate Date,
    ExpirationDate Date,
    Category Char(10),
    YB Char(3) DEFAULT '否'
);
```

这个SQL语句创建了一个名为Medicine的表，其中MedicineCode是主键，MedicineName是非空字段，BatchNumber是唯一字段，YB字段默认值为'否'。其他字段允许为空。

> ⚠ 作业中给出的字段类型并不是良好的。例如，`Char(6)`是一个固定长度的字符串，但是剂型可能会有不同的长度。`Varchar2(50)`是一个可变长度的字符串，这个类型更适合存储药品名称。

> ⚠ 列`YB`上储存的是中文字符，作业中给出的是`Char(2)`，但默认情况下UTF-8编码使用3个字节储存1个中文字符，并且这个类型不适合存储中文字符。应该使用`Varchar2(3)`或者`Char(3)`或者`NChar(1)`。

2.	创建供货商表（Provider），主码建为表级约束。

```sql
CREATE TABLE Provider (
    ProviderCode Char(4) NOT NULL,
    ProviderName Char(60) NOT NULL,
    PyCode Char(10),
    Address Char(50),
    Tel Char(15),
    Zip Char(6),
    Email Char(30),
    Relation Char(8),
    CONSTRAINT provider_pk PRIMARY KEY (ProviderCode)
);
```

这个SQL语句创建了一个名为Provider的表，其中ProviderCode是主键，ProviderName是非空字段。其他字段允许为空。


3.	建立供应表（PM），包含主码、外码，均为表级约束。

```sql
CREATE TABLE PM (
    MedicineCode Char(5) NOT NULL,
    ProviderCode Char(4) NOT NULL,
    PMDate Date NOT NULL,
    Price Int,
    Qyt Int,
    CONSTRAINT pm_pk PRIMARY KEY (MedicineCode, ProviderCode, PMDate),
    CONSTRAINT pm_medicine_fk FOREIGN KEY (MedicineCode) REFERENCES Medicine(MedicineCode),
    CONSTRAINT pm_provider_fk FOREIGN KEY (ProviderCode) REFERENCES Provider(ProviderCode)
);
```


4.	向Medicine表增加“使用说明（Memo）”列，其数据类型为字符串类型。

```sql
ALTER TABLE Medicine ADD (Memo Varchar(255));
```

5.	将Medicine表的Memo列删除

```sql
ALTER TABLE Medicine DROP COLUMN Memo;
```

6.	将Provider表的Address列的数据类型由Char(50)改为Char(60)。

```sql
ALTER TABLE Provider MODIFY (Address Char(60));
```

7.	删除供应表（PM）

```sql
DROP TABLE PM;
```

8.	为PM数据库中的Medicine，Provider两张表建立唯一索引

```sql
CREATE UNIQUE INDEX UNI_PM_Medicine_MedicineCode ON PM(MedicineCode);
CREATE UNIQUE INDEX UNI_PM_Provider_ProviderCode ON PM(ProviderCode);
```

9.	删除Medicine表的UNI_Medicine_MedicineCode索引

```sql
DROP INDEX UNI_PM_Medicine_MedicineCode;
```

10.	查询所有药品的药品代码、药品名称

哦！让我们在开始之前往表里填充一点信息。

```sql
INSERT INTO medicine VALUES (
    '10001',
    '小儿感冒颗粒',
    'Xegmkl',
    '颗粒剂',
    '12g/袋',
    NULL,
    'Z53020405',
    TO_DATE('2009-01-01', 'YYYY-MM-DD'),
    TO_DATE('2012-12-31', 'YYYY-MM-DD'),
    '中成药',
    '是'
);

INSERT INTO medicine VALUES (
    '10002',
    '维生素C银翘片',
    'Wsscyqp',
    '片剂',
    '49.5mg/片',
    NULL,
    'Z41022318',
    TO_DATE('2010-01-01', 'YYYY-MM-DD'),
    TO_DATE('2012-06-30', 'YYYY-MM-DD'),
    '中成药',
    '是'
);

INSERT INTO medicine VALUES (
    '10003',
    '清热解毒胶囊',
    'Qrjdjn',
    '胶囊剂',
    '0.3g/粒',
    NULL,
    'Z20054663',
    TO_DATE('2012-06-30', 'YYYY-MM-DD'),
    TO_DATE('2014-06-30', 'YYYY-MM-DD'),
    '中成药',
    '是'
);

INSERT INTO medicine VALUES (
    '10004',
    '小柴胡冲剂',
    'Xchcj',
    '颗粒剂',
    '10g/袋',
    NULL,
    'Z44020709',
    TO_DATE('2012-12-01', 'YYYY-MM-DD'),
    TO_DATE('2014-08-30', 'YYYY-MM-DD'),
    '中成药',
    '是'
);

INSERT INTO medicine VALUES (
    '20004',
    '新康泰克',
    'Xktk',
    '胶囊剂',
    '0.25g/粒',
    NULL,
    'H20010430',
    TO_DATE('2011-02-25', 'YYYY-MM-DD'),
    TO_DATE('2013-08-25', 'YYYY-MM-DD'),
    '西药',
    '是'
);

INSERT INTO medicine VALUES (
    '20007',
    '护彤',
    'ht',
    '颗粒剂',
    '2g/袋',
    NULL,
    'H23022613',
    TO_DATE('2004-10-07', 'YYYY-MM-DD'),
    TO_DATE('2007-10-07', 'YYYY-MM-DD'),
    '西药',
    '否'
);

INSERT INTO medicine VALUES (
    '20008',
    '救急散',
    'Jjs',
    '散剂',
    '1.5g/瓶',
    NULL,
    'Z11020138',
    TO_DATE('2012-01-01', 'YYYY-MM-DD'),
    TO_DATE('2015-01-01', 'YYYY-MM-DD'),
    '西药',
    '否'
);
```

接着向供应商表里填充一点信息。

```sql
INSERT INTO Provider VALUES ('S001', '河北东风药业', 'Hbdfyy', '河北省永年县城西', '0310-6806999', '057150', 'hbdf@163.com', '张三');
INSERT INTO Provider VALUES ('S002', '浙江康恩贝', 'Zzkeb', '杭州市高新技术开发区', '0571-87774811', '310045', 'ttm_8512@126.com', '李四');
INSERT INTO Provider VALUES ('S003', '青岛鲁健药业', 'Qdljyy', '青岛市北区延安路', NULL, '266000', '1375685404@qq.com', '王五');
INSERT INTO Provider VALUES ('S004', '哈药制药', 'Hyzy', '哈尔滨市南岗区学府路', NULL, '150000', 'hh@sina.com', '王六');
```

然后我们向供应表里填充一点信息。

```sql
INSERT INTO PM VALUES ('10002', 'S001', TO_DATE('2010-02-01', 'YYYY-MM-DD'),3.00, 150);
INSERT INTO PM VALUES ('10003', 'S001', TO_DATE('2012-08-01', 'YYYY-MM-DD'),24.00, 230);
INSERT INTO PM VALUES ('10004', 'S001', TO_DATE('2013-01-01', 'YYYY-MM-DD'),9.00, 500);
INSERT INTO PM VALUES ('10004', 'S002', TO_DATE('2013-02-02', 'YYYY-MM-DD'),9.00, 100);
INSERT INTO PM VALUES ('20004', 'S002', TO_DATE('2012-01-01', 'YYYY-MM-DD'),35.00, 200);
INSERT INTO PM VALUES ('20008', 'S003', TO_DATE('2012-04-01', 'YYYY-MM-DD'),70, 100);
```

好的，让我们开始吧！

```sql
SELECT MedicineCode, MedicineName FROM Medicine;
```

11.	查询所有药品的剂型、药品名称、药品类别

```sql
SELECT DosageForm, MedicineName, Category FROM Medicine;
```

12.	查询所有药品的详细记录

```sql
SELECT * FROM Medicine;
```

13.	查询所有药品的药品代码、药品名称、生产年份

```sql
SELECT MedicineCode, MedicineName, TO_CHAR(ProductionDate, 'YYYY') FROM Medicine;
```

14.	查询中成药类的药品名称

```sql
SELECT MedicineName FROM Medicine WHERE Category = '中成药';
```
15.	查询所有过期药品的药品代码、药品名称、失效日期

```sql
SELECT MedicineCode, MedicineName, ExpirationDate FROM Medicine WHERE ExpirationDate < SYSDATE;
```

16.	查询供应价格在10元以内的药品代码

```sql
SELECT MedicineCode FROM PM WHERE Price < 10;
```

17.	查询生产日期在2011-06-01与2012-06-01之间的药品信息

```sql
SELECT * FROM Medicine WHERE ProductionDate BETWEEN TO_DATE('2011-06-01', 'YYYY-MM-DD') AND TO_DATE('2012-06-01', 'YYYY-MM-DD');
```

18.	查询片剂、散剂和颗粒剂的药品代码、药品名称

```sql
SELECT MedicineCode, MedicineName FROM Medicine WHERE DosageForm IN ('片剂', '散剂', '颗粒剂');
```

19.	查询所有拼音简码以x开头的药品代码、药品名称

```sql
SELECT MedicineCode, MedicineName FROM Medicine WHERE PyCode LIKE 'X%';
```

20.	查询邮箱为“ttm_8512@126.com”的供应商信息

```sql
SELECT * FROM Provider WHERE Email = 'ttm_8512@126.com';
```

21.	查询没有提供联系电话的供应商基本信息

```sql
SELECT * FROM Provider WHERE Tel IS NULL;
```

22.	查询2012年以后生产的剂型为“胶囊”的药品名称

```sql
SELECT MedicineName FROM Medicine WHERE DosageForm = '胶囊剂' AND ProductionDate > TO_DATE('2012-01-01', 'YYYY-MM-DD');
```

23.	查询药品基本信息，查询结果按照药品类别升序排列，同一类别按照生产日期降序排列

```sql
SELECT * FROM Medicine ORDER BY Category ASC, ProductionDate DESC;
```
24.	查询药品的总个数

```sql
SELECT COUNT(*) FROM Medicine;
```

25.	查询供应药品的供应商个数

```sql
SELECT COUNT(DISTINCT ProviderCode) FROM PM;
```

26.	计算“10004”号药品的平均供应价格

```sql
SELECT AVG(Price) FROM PM WHERE MedicineCode = '10004';
```

27.	查询“S001”供应商供应的药品总数量

```sql
SELECT SUM(Qyt) FROM PM WHERE ProviderCode = 'S001';
```

28.	求每个供应商供应的药品个数

```sql
SELECT ProviderCode, COUNT(*) FROM PM GROUP BY ProviderCode;
```

29.	查询供应了3种以上药品的供应商代码

```sql
SELECT ProviderCode FROM PM GROUP BY ProviderCode HAVING COUNT(DISTINCT MedicineCode) > 3;
```
哦显然，这没有**三种以上**的药品供应商，也许题目是想表达**三种及以上**。把大于号改成大于等于即可，你应该会的，对吧？

30.	查询每个供应商及其供应药品的情况

```sql
SELECT ProviderCode, MedicineCode, MedicineName, Price, Qyt, PMDate FROM PM;
```

31.	查询与小儿感冒颗粒相同剂型的药品信息（自身连接完成）

```sql
SELECT M1.*
FROM Medicine M1, Medicine M2
WHERE M1.DosageForm = M2.DosageForm AND M2.MedicineName = '小儿感冒颗粒';
```

32.	左外连接：查询所有供应商供应药品的情况

```sql
SELECT * FROM Provider LEFT JOIN PM ON Provider.ProviderCode = PM.ProviderCode;
```

33.	右外连接：查询所有药品被供应的情况

```sql
SELECT * FROM Medicine RIGHT JOIN PM ON Medicine.MedicineCode = PM.MedicineCode;
```

34.	查询每个供应商供应的药品代码，药品名称、价格、数量、供应商名称、供应年份。

```sql
SELECT PM.ProviderCode, PM.MedicineCode, Medicine.MedicineName, PM.Price, PM.Qyt, Provider.ProviderName, PM.PMDate 
FROM PM 
JOIN Provider ON PM.ProviderCode = Provider.ProviderCode 
JOIN Medicine ON PM.MedicineCode = Medicine.MedicineCode;
```

35.	查询"浙江康恩贝"供应的药品代码和药品名称（嵌套查询完成）

```sql
SELECT MedicineCode, MedicineName FROM Medicine WHERE MedicineCode IN (SELECT MedicineCode FROM PM WHERE ProviderCode = 'S002');
```

36.	用嵌套查询完成31题：查询与小儿感冒颗粒相同剂型的药品信息。

```sql
SELECT * FROM Medicine WHERE DosageForm = (SELECT DosageForm FROM Medicine WHERE MedicineCode = '10001');
```

37.	找出每种药品供应价格超出它的供应平均价格的供应商代码。

```sql
SELECT ProviderCode, MedicineCode, Price FROM PM WHERE Price > (SELECT AVG(Price) FROM PM WHERE MedicineCode = PM.MedicineCode);
```

38.	查询S001供应商供应的药品名称（使用EXISTS谓词）

```sql
SELECT MedicineName FROM Medicine WHERE EXISTS (SELECT * FROM PM WHERE Medicine.MedicineCode = PM.MedicineCode AND PM.ProviderCode = 'S001');
```

39.	查询颗粒剂的药品及中成药（集合查询完成）

```sql
SELECT MedicineCode, MedicineName FROM Medicine WHERE DosageForm = '颗粒剂' UNION SELECT MedicineCode, MedicineName FROM Medicine WHERE Category = '中成药';
```

40.	查询颗粒剂的药品与中成药的差集

```sql
SELECT MedicineCode, MedicineName FROM Medicine WHERE DosageForm = '颗粒剂' MINUS SELECT MedicineCode, MedicineName FROM Medicine WHERE Category = '中成药';
```

41.	将（药品代码:10007，药品名称：藿香正气水，拼音简码：hxzqs，剂型：口服液，规格：ml/支，批号：Z51021352，生产日期：-12-20，失效日期：-12-20，药品类别：中成药，是否医保：是）插入到Medicine表中

```sql
INSERT INTO Medicine VALUES (
    '10007',
    '藿香正气水',
    'hxzqs',
    '口服液',
    'ml/支',
	NULL,
    'Z51021352',
    TO_DATE('2012-12-20', 'YYYY-MM-DD'),
    TO_DATE('2012-12-20', 'YYYY-MM-DD'),
    '中成药',
    '是'
);
```
42.	对每一种药品，求供应商供应的平均价格，并把结果存入数据库。

```sql
ALTER TABLE Medicine ADD (Avgprice DECIMAL(10, 2));

UPDATE Medicine 
SET AvgPrice = (
  SELECT AVG(Price) 
  FROM PM 
  WHERE PM.MedicineCode = Medicine.MedicineCode
);
```
43.	将所有药品的供应价格提高5%

```sql
UPDATE PM 
SET Price = Price * 1.05;
```

44.	删除供应表中，供应商代码为S001的所有记录

```sql
DELETE FROM PM 
WHERE ProviderCode = 'S001';
```
45.	删除青岛鲁健药业的供应药品记录

```sql
DELETE FROM PM 
WHERE ProviderCode = (
  SELECT ProviderCode 
  FROM Provider 
  WHERE ProviderName = '青岛鲁健药业'
);
```

46.	创建“浙江康恩贝”供应商供应的药品信息（包含药品代码、药品名称、药品类别、供应商等属性列）的视图

```sql
CREATE VIEW Zzkeb_Medicine AS 
SELECT PM.MedicineCode, Medicine.MedicineName, Medicine.CATEGORY, Provider.ProviderName 
FROM PM 
JOIN Medicine ON PM.MedicineCode = Medicine.MedicineCode 
JOIN Provider ON PM.ProviderCode = Provider.ProviderCode 
WHERE Provider.ProviderName = '浙江康恩贝';
```

47.	将各供应商供应的药品总个数定义为一个视图。 

```sql
CREATE VIEW Provider_Medicine_Count AS 
SELECT ProviderCode, COUNT(MedicineCode) AS TotalMedicine 
FROM PM 
GROUP BY ProviderCode;
```

48.	创建医保药品的视图，并要求进行修改和插入操作时仍须保证该视图只有医保药品。

```sql
CREATE VIEW ISYB_Medicine AS 
SELECT * 
FROM Medicine 
WHERE YB = '是'
WITH CHECK OPTION CONSTRAINT ISYB_Medicine_Check;
```

49.	将医保药品视图ISYB_Medicine中药品代码为‘10004’的药品名称改为柴胡冲剂。

```sql
UPDATE ISYB_Medicine 
SET MedicineName = '柴胡冲剂' 
WHERE MedicineCode = '10004';
```

50.	删除医保药品视图ISYB_Medicine中西药的记录

```sql
DELETE FROM PM 
WHERE MedicineCode IN (
  SELECT MedicineCode 
  FROM ISYB_Medicine 
  WHERE Category = '西药'
);

DELETE FROM ISYB_Medicine 
WHERE Category = '西药';
```

注意，在这里我们若直接删除视图中的记录，会因这些记录在PM表中有相关联的子记录而导致删除失败。所以我们先删除PM表中的相关记录，再删除视图中的记录。

51.	对供应表PM中药品（10002）供应的平均价格进行查询，如果药品的均价在10元以下，显示价格便宜，否则显示价格贵。

```sql
SELECT MedicineCode, 
       AVG(Price) AS AvgPrice,
       CASE 
           WHEN AVG(Price) < 10 THEN '价格便宜'
           ELSE '价格贵'
       END AS PriceLevel
FROM PM
WHERE MedicineCode = '10002'
GROUP BY MedicineCode;
```

52.	使用IF-THEN-ELSE语句查询S001供应的编码为10001的药品价格，如果大于30，则将价格重新置为20，如果小于15，则价格提升20%。

奶奶滴，根本就没有S001供应的10001药品，这里改成10002。

```sql
DECLARE
  v_price PM.Price%TYPE;
BEGIN
    SELECT Price INTO v_price FROM PM WHERE ProviderCode = 'S001' AND MedicineCode = '10002';
    IF v_price > 30 THEN
        UPDATE PM SET Price = 20 WHERE ProviderCode = 'S001' AND MedicineCode = '10002';
    ELSIF v_price < 15 THEN
        UPDATE PM SET Price = Price * 1.2 WHERE ProviderCode = 'S001' AND MedicineCode = '10002';
    END IF;
END;
```

这里应该是把S001的10002药品价格升价20%，即从3.00升到4.00。

53.	分别使用LOOP,WHILE,FOR语句，输出1+2+3+。。。+100的值。

```sql
DECLARE
  v_sum NUMBER := 0;
BEGIN
    FOR i IN 1..100 LOOP
        v_sum := v_sum + i;
    END LOOP;
    DBMS_OUTPUT.PUT_LINE('FOR: ' || v_sum);
    
    v_sum := 0;
    WHILE v_sum < 100 LOOP
        v_sum := v_sum + 1;
    END LOOP;
    DBMS_OUTPUT.PUT_LINE('WHILE: ' || v_sum);
    
    v_sum := 0;
    LOOP
        v_sum := v_sum + 1;
        EXIT WHEN v_sum = 100;
    END LOOP;
    DBMS_OUTPUT.PUT_LINE('LOOP: ' || v_sum);
END;
```

54.	将供应商表（Provider）中供应商编号为S001的ProviderName存储到一个表变量中，并打印出来。

```sql
DECLARE
  v_provider_name Provider.ProviderName%TYPE;
BEGIN
    SELECT ProviderName INTO v_provider_name FROM Provider WHERE ProviderCode = 'S001';
    DBMS_OUTPUT.PUT_LINE(v_provider_name);
END;
```

55.	编写一个PL/SQL程序块，对所有药品按他们基本价格的20%加价，如果增加的价格大于5元就取消加价。

```sql
DECLARE
  v_price PM.Price%TYPE;
  v_new_price PM.Price%TYPE;
BEGIN
    FOR r IN (SELECT MedicineCode, Price FROM PM) LOOP
        v_new_price := r.Price * 1.2;
        IF v_new_price - r.Price <= 5 THEN
            UPDATE PM SET Price = v_new_price WHERE MedicineCode = r.MedicineCode;
        END IF;
        DBMS_OUTPUT.PUT_LINE(r.MedicineCode || ' ' || r.Price || ' ' || v_new_price);
    END LOOP;
END;
```

56.	创建一个事后语句级触发器。当向药品表插入新数据后，该触发器将统计MEDICINE表中的行数并输出。

```sql
CREATE OR REPLACE TRIGGER Medicine_After_Insert
AFTER INSERT ON Medicine
DECLARE
  v_count NUMBER;
BEGIN
  SELECT COUNT(*) INTO v_count FROM Medicine;
  DBMS_OUTPUT.PUT_LINE('Total rows in Medicine: ' || v_count);
END;
```

这里我们插入一条数据，看看触发器是否会触发。

```sql
INSERT INTO medicine VALUES (
    '20009',
    '好吃的东西棒棒棒',
    'Hcddx',
    '散剂',
    '1.5t/棒',
    NULL,
    'LYYDDC',
    TO_DATE('2012-01-01', 'YYYY-MM-DD'),
    TO_DATE('2015-01-01', 'YYYY-MM-DD'),
    '西药',
    '否',
    NULL
);
```
57.	建立一个带输入IN参数的存储过程UPDATE_MEDI,为该过程设置两个参数，分别用于接收使用户提供的medicinecode和medicinename值。例如调用该存储过程可以完成medicinecode为‘10001’的medicinename改为‘云南白药’；

```sql
CREATE OR REPLACE PROCEDURE UPDATE_MEDI (
    p_medicinecode Medicine.MedicineCode%TYPE,
    p_medicinename Medicine.MedicineName%TYPE
)
AS
BEGIN
    UPDATE Medicine SET MedicineName = p_medicinename WHERE MedicineCode = p_medicinecode;
END;
```

让我们尝试一下。

```sql
EXECUTE UPDATE_MEDI('10001', '云南白药');
```

58.	从medicine表中查询给药品的名称和和是否医保，并利用OUT模式参数传给调用者。

```sql
CREATE OR REPLACE PROCEDURE GET_MEDICINE_YB (
    p_medicinecode Medicine.MedicineCode%TYPE,
    p_medicinename OUT Medicine.MedicineName%TYPE,
    p_yb OUT Medicine.YB%TYPE
)
AS
BEGIN
    SELECT MedicineName, YB INTO p_medicinename, p_yb FROM Medicine WHERE MedicineCode = p_medicinecode;
END;
```

让我们尝试下。

```sql
DECLARE
  v_medicinename Medicine.MedicineName%TYPE;
  v_yb Medicine.YB%TYPE;
BEGIN
    GET_MEDICINE_YB('10001', v_medicinename, v_yb);
    DBMS_OUTPUT.PUT_LINE(v_medicinename || ' ' || v_yb);
END;
```

59.	创建一个存储过程，完成给定药品的编号后，删除该药品信息；

```sql
CREATE OR REPLACE PROCEDURE DELETE_MEDICINE (
    p_medicinecode Medicine.MedicineCode%TYPE
)
AS
BEGIN
    DELETE FROM Medicine WHERE MedicineCode = p_medicinecode;
END;
```

让我们尝试一下。

```sql
EXECUTE DELETE_MEDICINE('20009');
```

这样上面创建的`好吃的东西棒棒棒`就会被删除。

60.	创建一个存储过程，完成给定某个药品编号后，求出该种药品供应的最高和最低价。

```sql
CREATE OR REPLACE PROCEDURE GET_MEDICINE_PRICE (
    p_medicinecode Medicine.MedicineCode%TYPE,
    p_maxprice OUT PM.Price%TYPE,
    p_minprice OUT PM.Price%TYPE
)
AS
BEGIN
    SELECT MAX(Price), MIN(Price) INTO p_maxprice, p_minprice FROM PM WHERE MedicineCode = p_medicinecode;
END;
```

让我们尝试一下。

```sql
DECLARE
  v_maxprice PM.Price%TYPE;
  v_minprice PM.Price%TYPE;
BEGIN
    GET_MEDICINE_PRICE('10002', v_maxprice, v_minprice);
    DBMS_OUTPUT.PUT_LINE(v_maxprice || ' ' || v_minprice);
END;
```

61.	创建一个函数，完成给定一个药品编号，求出该药品所有供应的价格总和。

```sql
CREATE OR REPLACE FUNCTION GET_MEDICINE_TOTAL_PRICE (
    p_medicinecode Medicine.MedicineCode%TYPE
) RETURN PM.Price%TYPE
AS
  v_total_price PM.Price%TYPE;
BEGIN
    SELECT SUM(Price) INTO v_total_price FROM PM WHERE MedicineCode = p_medicinecode;
    RETURN v_total_price;
END;
```

让我们尝试一下。

```sql
DECLARE
  v_total_price PM.Price%TYPE;
BEGIN
    v_total_price := GET_MEDICINE_TOTAL_PRICE('10002');
    DBMS_OUTPUT.PUT_LINE(v_total_price);
END;
```

62.	给指定的药品加价。

```sql
CREATE OR REPLACE FUNCTION ADD_PRICE (
    p_medicinecode Medicine.MedicineCode%TYPE,
    p_add_price PM.Price%TYPE
) RETURN PM.Price%TYPE
AS
  v_new_price PM.Price%TYPE;
BEGIN
    SELECT Price + p_add_price INTO v_new_price FROM PM WHERE MedicineCode = p_medicinecode;
    RETURN v_new_price;
END;
```

让我们尝试一下。

```sql
DECLARE
  v_new_price PM.Price%TYPE;
BEGIN
    v_new_price := ADD_PRICE('10002', 5);
    DBMS_OUTPUT.PUT_LINE(v_new_price);
END;
```

这里将对10002药品加价5元，即从5块钱到10块钱。