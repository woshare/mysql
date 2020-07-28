# mysql 日常使用笔记

## mysql性能分析
在测试SQL性能的过程中。
>1，一是通过设置STATISTICS查看执行SQL时的系统情况。选项有PROFILE，IO ，TIME。

```实例如下：
SET STATISTICS PROFILE ON//显示分析、编译和执行查询所需的时间（以毫秒为单位）
SET STATISTICS IO ON//报告与语句内引用的每个表的扫描数、逻辑读取数（在高速缓存中访问的页数）和物理读取数（访问磁盘的次数）有关的信息
SET STATISTICS TIME ON//显示每个查询执行后的结果集，代表查询执行的配置文件
GO
–你的SQL脚本开始
SELECT [TestCase] FROM [TestCaseSelect]
–你的SQL脚本结束
GO
SET STATISTICS PROFILE OFF
SET STATISTICS IO OFF
SET STATISTICS TIME OFF
```
>2，通过SQL 2008的“查询”按钮下的“包括实际的执行计划”和“包括客户端统计信息”