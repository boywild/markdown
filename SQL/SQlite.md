#SQlite
小型嵌入式数据库，ADCI关系型数据库，开源 效率高，程序驱动
ADCI：Atomicity 原子性、Consistency 一致性、Isolation 隔离性、Durability 持久性

SQlite可以看作是一个API库，数据库引擎库
优点：

1. 轻量，高效 
2. 绿色无需安装 
3. 零配置
4. 动态数据类型
5. 单一文件，跨平台

缺点：

1. 并发性能
2. 锁表会导致后续的操作无法进行
3. 网络文件
4. 是SQL的子集

1.sqlite

    Integer:        整型
    varchar(number):字符串类型，字符串长度小于等于number
    float:          浮点型
    double:         双精度型
    char(number):   字符串类型，固定字符串长度为number
    text:           文本类型
    null:           空值
    real:           浮点数字，村塾为8-byteIEEE浮点数
    blob:           二进制对象

2.语法
2.1 创建表语句

    creat table 表名(字段名称 数据类型 约束，字段名称 数据名称 约束...)
    creat table person(_id Integer primary key,name varchar(10),age Integer not null)

2.2 删除表语句

    drop table 表名
    drop table person

2.3 插入数据

    insert into 表明(字段，字段) value(值1，值2....);
    insert into person(_id,name) value(1,20);
    insert into person value(2,'zs',30);

2.4 修改数据

    update 表名 set 字段=新值 where 修改的条件
    updata person set name='ls',age=20 where _id=1;

2.5 删除数据

    delete from 表名 where 删除的条件;
    delete from person where _id=2;

2.3 查询语句

    select 字段名 from 表名 where 查询条件 group by分组的字段 having筛选条件 order by排序字段;

    select * from person;//
    select _id,name from person;                 //查询_id和name两个字段
    select * from person where _id=1;            //查询_id=1
    select * from person where _id<>1;           //查询_id不等于1
    select * from person where _id=1 and age>18;//查询 _id为1 年级大于18
    select * from person where name like "%小%";//模糊查询中间含有小前面和后面可以有任意的字符
    select * from person where name like "_小%";//查询一个字符后面是‘小’后面可以是任意多
    select * from person where name is null;   //查询名字为空的
    select * from person where age between 10 and 20;//年级在10到20之间
    select * from person where age>18 order by _id; //年级大于18 根绝_id进行排序


#SQLite数据库使用
##一、windows系统下SQLite的使用
salite3.dll
salite3.exe
第三方SQLite的GUI软件，如：sqliteexpert

使用步骤：

    CMD命令行
    salite3.exe
    .open test.db//创建新数据库
    create table t2(col1,col2)//建表
    insert into t2 values(2,3)//插值
    select * from t2//查询
    .quit//退出

    salite3.exe test.db//直接创建一个新的数据库
    create table t2(col1,col2)//建表
    insert into t2 values(2,3)//插值
    select * from t2//查询
    .quit//退出

##二、Android平台SQLite使用

    SQLiteOpenHelper
    SQLiteDatabase.openOrCreateDatabase(file,factory)
    Context.openOrCreateDatabase(file,factory)
    adb shell
    SQLiteDatabase类封装了管理数据库的各种方法
    如 insert delete updata query
    SQLiteOpenHelper类封装了数据库创建和版本管理

##三、SQlite的常用方法
3.1 SQL92大部分规范

    CREATE :Table Viev Index Trigger
    TRANSACTION
    Insert select delete update
    Drop
    Alert
    ATTACH

3.2 约束条件

    NOT NULL      不能为空
    UNIQUE        字段是唯一的不能有冲突
    PRIMARY KEY   id是唯一的也是不能为空的
    CHECK         检查
    DEFAULT       如果没有值默认设定一个值

3.3 常用方法

    Group By  对要查找的数据按照列进行分组
    HAVING筛选符合表达式条件的数据
    Distinct剔除重复字段
    Order By
    Limit
    Join,On 把多个表连接起来，交叉连接(需要查询的表之间没有任何联系)，内连接，外链接，左连接，右连接 
    Like,GLOB 模糊匹配                                                               
    In
    Between


3.4 游标常用方法

    getCount()                               获得总的数据项数
    isFirst()                                判断是否第一条记录
    isLast()                                 判断是否最后一条记录
    moveToFirst()                            移动到第一条记录
    moveToLast()                             移动到最后一条记录
    move(int offset)                         移动到指定记录
    moveToNext()                             移动到下一条记录
    moveToPrevious()                         移动到上一条记录
    getColumnIndexOrThrow(String columnName) 根据名称获得列索引
    getInt(int columnIndex)                  获得指定列的int类型值
    getString(int columnIndex)               获得指定类的String类型值 

select * from sqlite_master where type='table' order by name;

```js
建表
create table dalei(id INT PRIMARY KEY NOT NULL,daleiname);
create table xiaolei(id INT PRIMARY KEY NOT NULL,daleiid,xiaoleiname);
create table record(xiaoleiid int not null,fee default 10.0 check(fee<10000),time);

插入数据
insert into dalei(id,daleiname) values(0,'饮食');
insert into dalei(id,daleiname) values(1,'交通');
insert into dalei(id,daleiname) values(2,'医疗');
insert into dalei(id,daleiname) values(3,'水电');

insert into xiaolei(id,daleiid,xiaoleiname) values(0,0,'早饭');
insert into xiaolei(id,daleiid,xiaoleiname) values(1,0,'午饭');
insert into xiaolei(id,daleiid,xiaoleiname) values(2,0,'晚饭');
insert into xiaolei(id,daleiid,xiaoleiname) values(3,0,'点心');
insert into xiaolei(id,daleiid,xiaoleiname) values(4,0,'宵夜');

insert into xiaolei(id,daleiid,xiaoleiname) values(5,1,'公交');
insert into xiaolei(id,daleiid,xiaoleiname) values(6,1,'打的');
insert into xiaolei(id,daleiid,xiaoleiname) values(7,1,'火车');
insert into xiaolei(id,daleiid,xiaoleiname) values(8,1,'飞机');

insert into xiaolei(id,daleiid,xiaoleiname) values(9,2,'药费');
insert into xiaolei(id,daleiid,xiaoleiname) values(10,3,'水');
insert into xiaolei(id,daleiid,xiaoleiname) values(11,3,'电');
insert into xiaolei(id,daleiid,xiaoleiname) values(12,3,'煤气');

insert into record(xiaoleiid,fee,time) values(0,6,'2016-09-01');

insert into record(xiaoleiid,fee,time) values(4,2,'2016-09-01');
insert into record(xiaoleiid,fee,time) values(1,12,'2016-09-01');
insert into record(xiaoleiid,fee,time) values(2,8,'2016-09-01');
insert into record(xiaoleiid,fee,time) values(5,14,'2016-09-01');
insert into record(xiaoleiid,fee,time) values(3,3,'2016-09-01');

insert into record(xiaoleiid,fee,time) values(0,4,'2016-09-02');

insert into record(xiaoleiid,fee,time) values(5,8,'2016-09-02');
insert into record(xiaoleiid,fee,time) values(8,28,'2016-09-02');
insert into record(xiaoleiid,fee,time) values(4,2,'2016-09-02');
insert into record(xiaoleiid,fee,time) values(1,8,'2016-09-02');
insert into record(xiaoleiid,fee,time) values(2,10,'2016-09-02');
insert into record(xiaoleiid,fee,time) values(4,2,'2016-09-02');
insert into record(xiaoleiid,fee,time) values(3,2,'2016-09-02');

insert into record(xiaoleiid,fee,time) values(0,6,'2016-09-03');
insert into record(xiaoleiid,fee,time) values(5,30,'2016-09-03');
insert into record(xiaoleiid,fee,time) values(6,50,'2016-09-03');
insert into record(xiaoleiid,fee,time) values(1,16,'2016-09-03');
insert into record(xiaoleiid,fee,time) values(2,14,'2016-09-03');

insert into record(xiaoleiid,fee,time) values(0,4,'2016-09-04');
insert into record(xiaoleiid,fee,time) values(6,50,'2016-09-04');
insert into record(xiaoleiid,fee,time) values(4,2,'2016-09-04');
insert into record(xiaoleiid,fee,time) values(1,8,'2016-09-04');
insert into record(xiaoleiid,fee,time) values(2,8,'2016-09-04');
insert into record(xiaoleiid,fee,time) values(3,6,'2016-09-04');

insert into record(xiaoleiid,fee,time) values(9,56,'2016-09-04');
insert into record(xiaoleiid,fee,time) values(10,126.3,'2016-09-04');
insert into record(xiaoleiid,fee,time) values(11,34.8,'2016-09-04');

insert into record(xiaoleiid,fee,time) values(0,6,'2016-10-01');
insert into record(xiaoleiid,fee,time) values(4,2,'2016-10-01');
insert into record(xiaoleiid,fee,time) values(1,8,'2016-10-01');
```


    select * from record group by time;
    select *,sun(fee) from record group by time;
    select *,sum(fee) from record group by xiaoleiid;
    select *,sum(fee) from record group by xiaoleiid having sum(fee)>50;
    select distinct time from record;
    select distinct xiaoleiid from record;
    select distinct xiaoleiid from record order by xiaoleiid;
    select distinct xiaoleiid from record order by xiaoleiid asc;//升序
    select distinct xiaoleiid from record order by xiaoleiid desc;//降序
    select * from record limit 3;//只显示3条
    select * from record limit 3,4;//从第3条开始显示4条
    select * from dalei cross join xiaolei;//交叉连接
    select * from record inner join xiaolei on record.xiaoleiid=xiaolei.id;
    select * from record inner join xiaolei on record.xiaoleiid=xiaolei.id join dalei on xiaolei.daleiid=dalei.id;
    select * from record where fee like '5_';
    select * from record where fee like '5%';

设置正确格式化的输出

    .header on
    .mode column

其他命令

|命令|描述|
|---|---|
|.backup ?DB? FILE|备份 DB 数据库（默认是 "main"）到 FILE 文件|
|.bail ON/OFF|发生错误后停止。默认为 OFF|
|.databases|列出数据库的名称及其所依附的文件|
|.dump ?TABLE?|以 SQL 文本格式转储数据库。如果指定了 TABLE 表，则只转储匹配 LIKE 模式的 TABLE 表|
|.echo ON/OFF |开启或关闭 echo 命令|
|.exit|退出 SQLite 提示符|
| .explain ON/OFF |	开启或关闭适合于 EXPLAIN 的输出模式。如果没有带参数，则为 EXPLAIN on，及开启 EXPLAIN|
| .header(s) ON/OFF|开启或关闭头部显示|
| .help|显示消息|
| .import FILE TABLE|导入来自 FILE 文件的数据到 TABLE 表中|
| .indices ?TABLE?	|显示所有索引的名称。如果指定了 TABLE 表，则只显示匹配 LIKE 模式的 TABLE 表的索引|
| .load FILE ?ENTRY?|加载一个扩展库|
| .log FILE/off|开启或关闭日志。FILE 文件可以是 stderr（标准错误）/stdout（标准输出）|
| .mode MODE||
| .nullvalue STRING	|在 NULL 值的地方输出 STRING 字符串|
| .output FILENAME	|发送输出到 FILENAME 文件|
| .output stdout	|发送输出到屏幕|
| .print STRING...|逐字地输出 STRING 字符串|
| .prompt MAIN CONTINUE	|替换标准提示符|
| .quit|退出 SQLite 提示符|
| .read FILENAME	|执行 FILENAME 文件中的 SQL|
| .schema ?TABLE?|显示 CREATE 语句。如果指定了 TABLE 表，则只显示匹配 LIKE 模式的 TABLE 表|
| .separator STRING|改变输出模式和 .import 所使用的分隔符|
| .show	|显示各种设置的当前值|
| .stats ON/OFF|开启或关闭统计|
| .tables ?PATTERN?|列出匹配 LIKE 模式的表的名称|
| .timeout MS	|尝试打开锁定的表 MS 毫秒|
| .width NUM NUM|为 "column" 模式设置列宽度|
| .timer ON/OFF|开启或关闭 CPU 定时器|

##聚合函数
    avg(x) 求某个字段的平均值，如果x为字符串就当做0来处理
    count(x|*) 在x这个字段下非空值有多少行
    group_concat(x,[y]) 把某个字段x所有满足条件的内容变成一长串字符串 y就是字符串中的分隔符
    max(x) min(x) 某个字段中最大值和最小值
    sum(x) 对某个字段进行求和
    total(x) 对某个字段进行求和

##核心函数

    last_insert_rowid() rowid是sqlite中的隐藏字段，每次插入数据的时候都会自增
    例子：select rowid,* from record;
    abs(x) 绝对值
    coalesce(x,y,...) 拷贝当前表中所有字段中第一个非空字段的值
    ifnull(x,y) 查看xy中哪个字段是非空，并拷贝出非空字段的值
    length(x)
    lower(x) 把字段x中的字符串都变成小写
    upper(x) 把字段x中的字符串都变成大写
    nullif(x,y) 比较xy如果x和y不一样的就返回x，一样的就返回null
    like(x,y)  y字段的值是不是符合x这种模式，如果符合就返回1 ，不符合就返回0
    random() 产生随机整数，有正有负
    replace(x,y,z) 在字段x中如果出现y字符串，就用z替换
    round(x,[y]) 对x字段进行四舍五入，表刘小数点后y位
    substr(x,y,z) 对字段x进行字符串截取，从第y位开始截取z个字符串，第一位是的索引是从1开始而不是0
    changes() 数据库改动的次数
    total_changes() 从数据库连上开始所有的改动次数
    trim(x) 字段x中字符串两边的空格去掉
    trim(x,[y]) 在字段x中去掉y相关的字符串
    ltrim() 去掉左边的空格
    rtrim() 去掉右边的空格
    typeof(x) 判断x字段下的值属于什么类型
    zeroblob(n),randomblob(n)
    quote(x) 
    instr(x,y) y字符串在x字段的每一条数据中的什么位置出现，0则表示没有出现相应的字符串


