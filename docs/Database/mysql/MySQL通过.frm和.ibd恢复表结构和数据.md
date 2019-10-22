此种情况适用于将表给误删了，然后在进行表结构和数据恢复

在开发环境上将一个表给删除了

MySQL中.frm文件：保存了每个表的元数据，包括表结构的定义等，该文件与数据库引擎无关。

 MySQL中.ibd文件：InnoDB引擎开启了独立表空间(my.ini中配置innodb_file_per_table = 1)产生的存放该表的数据和索引的文件。
 
 1. 在一个可以正常使用的MySQL数据库中建立一个库
 
  create database back;
 
 2. 在库中创建一个与要恢复表的同名表（字段数量与要恢复的表数量相同，否则后面步骤3中会出错，如果不知道有多少个字段可以在下一步出错时在错误信息中找到）

 create table test1( id1 int, id2 int, id3 int );
 
 3. 关闭数据库服务，然后用需要恢复的test1.frm覆盖这个新建的back数据库的test1.frm，接着对配置文件(my.ini)设置innodb_force_recovery = 6，然后使用启动数据库服务

  desc test1;

 +-------+-------------+------+-----+---------+-------+
 | Field | Type        | Null | Key | Default | Extra |
 +-------+-------------+------+-----+---------+-------+
 | id    | int(11)     | NO   | PRI | NULL    |       |
 | name  | varchar(10) | YES  |     | NULL    |       |
 | age   | int(11)     | YES  |     | NULL    |       |
 +-------+-------------+------+-----+---------+-------+

 4. 查看并复制创建表的SQL语句

 5.关闭数据库服务，接着对配置文件中的innodb_force_recovery = 6删除或者注释，然后使用启动数据库服务
 
 6.删除test1表，再用复制出来的建表语句重建test1表
 
 7.将原.ibd文件与原.frm文件解除绑定
 
 alter table test1 discard tablespace;

 8.停掉数据库服务，将需要恢复的test1.ibd文件覆盖这个新建的back数据库的test1.ibd，开启数据库服务。
 
 9. 将复制过来的test1.ibd文件与test1.frm文件关联
 
 alter table test1 import tablespace;
 
 
恢复完成！