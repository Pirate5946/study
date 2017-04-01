SQL*Loader  
命令行下的操作工具，对应的命令为  sqlldr        （只能是小写）
在数据表已经建好的情况下，进行数据导入导出

控制文件参数        
INFILE：表示要加载的数据文件位置，*表示数据文件在控制文件中   

INTO TABLE tbl_name ： 数据要加载的目标表
INTO之前可选的参数：INSERT,APPEND,REPLACE,TRUNCATE       

FIELDS TERMINATED BY ","：设置数据字段分隔符
（列名）：逗号分隔，顺序与数据文件中字段顺序要相同，列名称要与目标表中的字段名称相同

日志文件：.log  
错误文件：.bad	 错误记录       
废弃文件：.dsc  未导入的记录        

#### 三思笔记 p83
导入数据 不同情况的解决方法     
- 导入excel文件
将Excel文件另存为CSV（逗号分隔）文件，然后导入
- 导入定长字符串				

- 导入数据文件列 比表字段少
在控制文件中 定义缺失字段的 初始值
- 导入的数据文件列 比 表字段 多     
1、数据量小的话，可以修改数据文件，删除多余数据      
2、修改控制文件，删除指定 列 或者 定长 字符串      
格式相同的数据文件 可以导入同一张表

在控制文件中设置多个 INFILE值
数据文件中的不同数据导入 不同的表
在控制文件中 添加 when 条件		

p89
- 前N行数据不希望导入
1、修改文本文件
2、skip参数	和 load参数				p91

==换行符的处理==    
指定str属性处理数据文件换行符，二进制字符改为十六进制编码      p96
infile   '***.dat'   "str X'170A'"	

- 导入LOB型数据

TRAILING NULLCOLS	某行没有对应的列值时，赋值为NULL
>导入date类型数据要注意格式一致				p103

优化导入效率的选项      
ROWS	默认一次加载的行数      
BINDSIZE        
DIRECT	直接路径加载        

#### 外部表加载数据								p113-p124
创建一个directory对象指定一个目录使Oracle能访问到，并授权用户对该对象具有读写权限

   建立物理目录 /u01/app/... ，设置读写权限，对应逻辑目录  expdir 
   
	create directory expdir as '/u01/app/...';			--sysdba创建逻辑目录
	
	grant read,write on directory expdir to bips;		--授予bips用户对该目录的读写权限
	
只有用ORACLE_DATAPUMP（数据泵）的方式导出的数据，才能通过外部表用ORACLE_DATAPUMP实现导入      

1、源数据导出：     
create TABLE EXT_BIGTBL_DP   	--创建导出对象的外部表EXT_BIGTBL_DP
ORGANIZATION  EXTERNAL 
(
TYPE    ORACLE_DATAPUMP
DEFAULT  DIRECTOR  EXPDIR
LOCATION ('EXT_BIGTBL_DP.DMP' )
)
AS SELECT * FROM OBJECTS;	        	--选择目标对象数据      

2、通过DBMS_METADATA.GET_DDL函数生成外部表的创建语句        
select DBMS_METADATA.GET_DDL('TABLE','EXT_BIGTBL_DP') FROM DUAL;

3、将生成的EXT_BIGTBL_DP.DMP复制到目标数据库相应位置，执行DBMS_METADATA.GET_DDL函数生成的创建语句        

4、导入目标数据库目标表
INSERT /*+ APPEND */ INTO OBJECTS SELECT * FROM EXT_BIGTBL_DP


主要有三方面影响外部表加载性能					p118
CPU性能、内存、I/O能力
外部表在创建时需要指定访问驱动				p120











