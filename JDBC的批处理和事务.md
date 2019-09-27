##### 一.JDBC批处理
批量处理允许将相关的SQL语句分组到批处理中，并通过对数据库的一次调用提交它们。

当需要一次向数据库发送多个SQL语句时，可以减少连接数据库的开销，从而提高性能。
###### 1.1 Statement批处理
步骤：
- 1 注册驱动获取连接
- 2 使用createStatement（）方法创建Statement对象。
- 3 使用setAutoCommit（）将auto-commit设置为false 。（可选）
- 4 使用addBatch（）方法在创建的语句对象上添加您喜欢的SQL语句到批处理中。
- 5 在创建的语句对象上使用*executeBatch（）*方法执行所有SQL语句。
- 6 使用*ommit（）*方法提交所有更改。（可选）
- 7 释放资源

==传统方式的statement批处理==
```
public class BatchTest2 {
    public static void main(String[] args) throws SQLException {
        Connection connection = DButil.getConnection();  //获取连接
        Statement sta = connection.createStatement();   //创建创建Statement对象
        for (int i = 101; i <=1000 ; i++) {
        //j将sql添加到批处理中
            sta.addBatch( "insert into user values ("+i+",'李四','123456','郑州','666666')");
            if(i%100==0){
            //每100条数据执行一次  数组返回的是执行数据的数量
                int[] ints = sta.executeBatch();
                sta.clearBatch();  //清空批处理的队列
                System.out.println("长度为:"+ints.length);
            }
        }
        int[] res=sta.executeBatch();
        System.out.println(res.length);
        sta.clearBatch();

        //4添加更新语句
        sta.addBatch("update user set password='999999' where userid=1;");
        sta.addBatch("update user set password='888888' where userid=2;");
        //5执行
        int[] results = sta.executeBatch();
        System.out.println("数组的长度："+results.length);
        for (int result : results) {
            System.out.println(result);
        }
        //6关闭
        sta.close();
        connection.close();

    }
}
```
==事务方式的批处理==

```
public class BatchTest2 {
    public static void main(String[] args) throws SQLException {
        Connection connection = DbUtils.getConnection();
        Statement sta = connection.createStatement();
        //开启事务
        connection.setAutoCommit(false);
        long start = System.currentTimeMillis();
        for (int i = 1; i <=10000 ; i++) {
            sta.addBatch( "insert into person values ("+i+",'张三','6666')");
            if(i%1000==0){
                int[] ints = sta.executeBatch();
                sta.clearBatch();
                System.out.println("长度为:"+ints.length);
            }
        }
        int[] res=sta.executeBatch();
        System.out.println(res.length);
        sta.clearBatch();
        connection.commit();
        long end = System.currentTimeMillis();
        System.out.println("消耗时间:"+(end-start));
        //6关闭
        sta.close();
        connection.close();
    }
}
```

在属性文件中的url中添加rewriteBatchedStatements=true  会是处理更快

```
url=jdbc:mysql://localhost:3306/mydb1?useSSL=true&characterEncoding=utf8&rewriteBatchedStatements=true
```


###### 1.1.2 PrepareStatement批处理
1. 使用占位符创建SQL语句。
2. 使用*prepareStatement（）* 方法创建PrepareStatement对象。
3. 使用*setAutoCommit（）*将auto-commit设置为false 。（可选）
4. 使用*addBatch（）*方法在创建的语句对象上添加您喜欢的SQL语句到批处理中。
5. 在创建的语句对象上使用*executeBatch（）*方法执行所有SQL语句。
6. 最后，使用*commit（）*方法提交所有更改。（可选）

==传统添加   耗时间  效率不高==

```
public class BatchTest {
    public static void main(String[] args) throws SQLException {
        Connection connection = DbUtils.getConnection();
        PreparedStatement psta = connection.prepareStatement("insert into user(userid,username,password,address,phone) values(?,?,?,?,?)");
        for (int i = 1; i <=10000 ; i++) {
            psta.setInt(1,i);
            psta.setString(2,"张三");
            psta.setString(3,"123456");
            psta.setString(4,"上海");
            psta.setString(5,"12423423545");
            psta.addBatch();
            if(i%1000==0){
                int[] ints = psta.executeBatch();
                System.out.println("长度为:"+ints.length);
                psta.clearBatch();
            }
        }
        DbUtils.closeAll(connection,psta,null);
    }
}

```

==事务方式  最快速度  1w数据插入   0.2s==

```
public class BatchTest {
    public static void main(String[] args) throws SQLException {
        Connection connection = DbUtils.getConnection();
        PreparedStatement psta = connection.prepareStatement("insert into person values(?,?,?)");
       connection.setAutoCommit(false);
        long start = System.currentTimeMillis();
        for (int i = 1; i <=10000 ; i++) {
            psta.setInt(1,i);
            psta.setString(2,"张三");
            psta.setString(3,"123456");
            psta.addBatch();
            if(i%1000==0){
                int[] ints = psta.executeBatch();
                System.out.println("长度为:"+ints.length);
                psta.clearBatch();
            }
        }
        connection.commit();
        long end = System.currentTimeMillis();
        System.out.println("所耗时间: "+(end-start));
        DbUtils.closeAll(connection,psta,null);
    }
}
```
Statement批处理和PrepareStatement批处理的区别：
（1）Statment批处理可以添加不同Sql语句，而PrepareStatment只能添加一种sql语句，因为是  预编译 sql早设定好的  要想插入多种数据  需要另外创PrepareStatement对象。
（2）PrepareStatment效率比Statment高，而且更安全。


##### 二.数据库事务
###### 2.1事务的概述
##### 3.1 事务概述

​	一组要么同时执行成功，要么同时失败的SQL语句。是数据库操作的一个不能分割执行单元。
​	数据库事务(Database Transaction) ，是指作为单个逻辑工作单元执行的一系列操作，要么完全地执行，要么完全地不执行。 事务处理可以确保除非事务性单元内的所有操作都成功完成，否则不会永久更新面向数据的资源。通过将一组相关操作组合为一个要么全部成功要么全部失败的单元，可以简化错误恢复并使应用程序更加可靠。一个逻辑工作单元要成为事务，必须满足所谓的ACID（原子性、一致性、隔离性和持久性）属性。事务是数据库运行中的逻辑工作单位，由DBMS中的事务管理子系统负责事务的处理。

**事务开始于**
- 连接到数据库上，并执行一条DML语句insert、update或delete
- 前一个事务结束后，又输入了另一条DML语句

**事务结束于**

- 执行commit或rollback语句。
- 执行一条DDL语句，例如create table语句，在这种情况下，会自动执行commit语句。
- 执行一条DDL语句，例如grant语句，在这种情况下，会自动执行commit。
- 断开与数据库的连接
- 执行了一条DML语句，该语句却失败了，在这种情况中，会为这个无效的DML语句执行rollback语句。

**事务的四大特点（ACID）**
>- **Atomicity(原子性)**
　表示一个事务内的所有操作是一个整体，要么全部成功，要么全部失败- **Consistency(一致性)**
　表示一个事务内有一个操作失败时，所有的更改过的数据都必须回滚到修改前状态- 
**Isolation(隔离性)**
事务查看数据时数据所处的状态，要么是另一并发事务修改它之前的状态，要么是另一事务修改它之后的状态，事务不会查看中间状态的数据。
>- **Durability(持久性)**
　持久性事务完成之后，它对于系统的影响是永久性的。


**事务的隔离级别**
　　SQL标准定义了4类隔离级别，包括了一些具体规则，用来限定事务内外的哪些改变是可见的，哪些是不可见的。低级别的隔离级一般支持更高的并发处理，并拥有更低的系统开销。

**Read Uncommitted**（读取未提交内容)

==注意  这里的读取未提交的数据  实际上未提交的数据修然执行了但是未提交  数据库中的数据是不发生改变的，读取到的是虚表中的内容，这也就是脏读==
​       在该隔离级别，所有事务都可以看到其他未提交事务的执行结果。本隔离级别很少用于实际应用，因为它的性能也不比其他级别好多少。读取未提交的数据，也被称之为脏读（Dirty Read）。
案例：
这里因为卖方的级别是最低的，可以读没有提交的数据 当a仿制型时候  没有提交 表中数据实际没有变化  但是 b查询能在虚拟表中查询到  以为a已经付过钱了  发货之后  a执行回滚 数据又恢复以前  这就是  脏读。(需要将b的级别提高一下为Read Committed)

```
# A方 买 本伟
SELECT @@tx_isolation;
START TRANSACTION;
UPDATE account SET money=money-2000 WHERE id=1;
UPDATE account SET money=money+2000 WHERE id=2;
COMMIT;
ROLLBACK;

# B方  卖  郑帅
#（修改隔离级别）
SET SESSION TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;
#查看隔离级别
SELECT @@tx_isolation;
SELECT *FROM account;
#发货
```

**Read Committed**（读取提交内容）

==这个级别也是会出现问题的，在数据库进行统计时候，通常是进行多次查询，当多次查询结果相同时才会认为数据无误，提交过去，但是 再查询过程中用户如果进行数据修改操作就会使查询结果不一致，从而造成影响  这就需要设置更高隔离级别==
​       这是大多数数据库系统的默认隔离级别（但不是MySQL默认的）。它满足了隔离的简单定义：一个事务只能看见已经提交事务所做的改变。这种隔离级别出现不可重复读（Nonrepeatable Read）问题，因为同一事务的其他实例在该实例处理其间可能会有新的commit，所以同一select可能返回不同结果。

案例：

```
//在查询时  (模拟银行存钱)下方代码 表示用户进行存钱操作  回对查询造成影响
START TRANSACTION;
	SELECT SUM(money) FROM account;
	SELECT SUM(money) FROM account;
	SELECT SUM(money) FROM account;
COMMIT;	

START TRANSACTION;
UPDATE account SET money=money+1000 WHERE id=2;
COMMIT;
```

**Repeatable Read **可重读

==这个设置可重读的级别  在进行统计时候  用户进行修改操作时候 系统会进行屏蔽 等统计结束之后  提交完 在进行操作  将用户存的钱加进来==

​       这是MySQL的默认事务隔离级别，它确保同一事务的多个实例在并发读取数据时，会看到同样的数据行。不过理论上，这会导致另一个棘手的问题：幻读（Phantom Read）。简单的说，幻读指当用户读取某一范围的数据行时，另一个事务又在该范围内插入了新行，当用户再读取该范围的数据行时，会发现有新的“幻读” 行。InnoDB和Falcon存储引擎通过多版本并发控制（MVCC，Multiversion Concurrency Control）机制解决了该问题。

**Serializable** 可串行化
​       这是最高的隔离级别，它通过强制事务排序，使之不可能相互冲突，从而解决幻读问题。简言之，它是在每个读的数据行上加上共享锁。在这个级别，可能导致大量的超时现象和锁竞争。效率最低的。

​       这四种隔离级别采取不同的锁类型来实现，若读取的是同一个数据的话，就容易发生问题。




**事物的提交和回滚**
开启事务在完成表的修改止之后  需要进行提交
回滚代码案例

```
public class TransactionTest {
    public static void main(String[] args) {
        Connection connection = DbUtils.getConnection();
        PreparedStatement psta1=null;
        PreparedStatement psta2=null;
        try {
            //设置自动提交为false
            connection.setAutoCommit(false);
            psta1 = connection.prepareStatement("update person set money=money-1000 where id=1");
            psta1.executeUpdate();
            int a=8/0;  //在这里会报错  到catch中  执行回滚 确保数据不会出现错误
            psta2 = connection.prepareStatement("update person set money=money+1000 where id=2");
            psta2.executeUpdate();
            connection.commit();
            System.out.println("转账成功！");

        } catch (Exception e) {
            e.printStackTrace();
            try {
                connection.rollback();//回滚  当程序运行中出现问题之后  就会回滚 回到执行之前的样子
            } catch (SQLException e1) {
                e1.printStackTrace();
            }
            System.out.println("转账失败！");
        }finally {
            try {
                if(connection!=null)
                connection.close();
                if(psta1!=null)
                psta1.close();
                if(psta2!=null)
                psta2.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }
}
```


回滚点的操作案例

==什么是回滚点？==
回滚点即是在代码中设置的一个位置，在程序执行过程中出现错误时候，执行回滚操作时候指定位置，就会回滚到指定的位置。
==回滚操作之后还需不需要提交？==
回滚之后如果是回滚到指定的回滚点的话，前面有已经执行的代码时候 需要提交，如果是默认的回滚到事务开始位置，则不需要提交 因为之前执行的都回到原始状态了，在提交也没什么意义。

>设置保存点时，可以在事务中定义逻辑回滚点。如果通过保存点发生错误，则可以使用回滚方法来撤消所有更改或仅保存在保存点之后所做的更改。
Connection对象有两种新的方法来帮助您管理保存点 -
>- **setSavepoint（String savepointName）：**定义新的保存点。它还返回一个Savepoint对象。
>- **releaseSavepoint（Savepoint savepointName）：**删除保存点。请注意，它需要一个Savepoint对象作为参数。此对象通常是由setSavepoint（）方法生成的保存点。

```
public class RollpointTest {
    public static void main(String[] args) {
        Connection connection=null;
        Savepoint sp1=null;
        Savepoint sp2=null;
        Statement stat=null;
        try {
            //1获取连接
            connection= DbUtils.getConnection();
            //2开启事务
            connection.setAutoCommit(false);
            //3创建命令对象
            stat = connection.createStatement();
            //4执行
            stat.executeUpdate("insert into person(id,name,money) values(3,'aaa',1000)");
            sp1=connection.setSavepoint("savepoint1");

            stat.executeUpdate("insert into person(id,name,money) values(4,'bbb',1000)");
            sp2=connection.setSavepoint("savepoint2");

            stat.executeUpdate("insert into person(id,name,money) values(5,'ccc',1000)");

            connection.releaseSavepoint(sp1);
            int i=10/0;
            //成功提交
            connection.commit();
            System.out.println("添加成功");

        } catch (Exception e) {
            e.printStackTrace();
            try {
                connection.rollback(sp1);
                connection.commit();//回滚没有回滚到事务开始的位置，需要提交
                System.out.println("添加失败");
            } catch (SQLException e1) {
                e1.printStackTrace();
            }

        }finally {
            try {
                stat.close();
                connection.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }
}
```

