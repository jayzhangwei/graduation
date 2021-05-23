
@[toc]

**关注公众号【C you again】，回复“基于java的企业进销存管理系统”免费下载。**

## 01 概述

销存管理系统是一个基于本地与网络的应用系统，它是一个面对当前的进销存管理工作基本还处于手工和半信息自动化处理状态而应运而生的一个基于本地与网络的一个完全信息自动化的系统，整个系统从符合操作简便、界面友好、灵活、实用、安全的要求出发，完成进货、销售、库存管理的全过程。本文所设计的企业进销存管理系统可以满足企业进货、销售和库存管理方面的需要。

## 02 系统结构及说明

本系统包括基础资料、进货管理、销售管理、库存管理、信息查询、系统维护等 6 大部分。系统结构如图所示：


![在这里插入图片描述](https://img-blog.csdnimg.cn/20200826130307761.png)

#### 进货管理

“进货管理”功能模块用于管理企业的进货采购业务，是进销存管理系统中不可缺少的重要组成部分，它主要负责为系统记录进货单及其退货信息，相应的进货商品会添加到库存管理中。所包含的子功能模块如图所示。

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020082613050279.png)

#### 基础资料

“基础资料”是每个系统都必须具备的功能，该模块用于管理企业进销存管理系统中的客户、商品和供应商信息，其功能主要是对这些基础信息进行添加、修改和删除。包括的子功能模块如图所示。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200826130547698.png)

#### 销售管理

“销售管理”功能模块用于管理企业的销售业务，商品销售是进销存管理中的重要环节之一，进货商品在入库之后就可以开始销售了。所包含的子功能模块如图所示。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200826130637370.png)

#### 库存管理

“库存管理”模块是企业进销存管理系统中的库存管理模块包括库存盘点和价格调整两个功能，所包含的子功能模块如图所示。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200826130711195.png)

#### 查询统计

“查询统计”模块是进销存管理系统中不可缺少的重要组成部分，它主要包括销售查询和商品查询，所包含的子功能模块如图所示。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200826130746430.png)

#### 系统管理

“系统管理”模块主要有更改密码、退出系统两个模块，所包含的子功能模块如图所示。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200826130816962.png)

## 03 工程结构

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200826131720288.png)

## 04 详细设计

#### 系统运行环境

 - 操作系统：Windows 10；
 - JDK环境：jdk1.8；
 - 开发工具：Eclipse8.0； 
 - 数据库管理软件：My SQL 5.7
 
 #### 系统开发技术
 
 - Java
 - My SQL 数据库
 
 #### 公共类设计

 公共类是代码重用的一种形式，他将各个功能模块经常调用的方法提取到共用的Java类中，例如访问数据库的Dao类容纳了所有访问数据库的方法，并同时管理者数据库的连接和关闭。这样不但实现了项目代码的重用，还提高了程序的性能和代码可读性。

数据库DB链接(dao/Dao.java):

```java
protected static String dbClassName = "com.mysql.jdbc.Driver";// MySQL数据库驱动类的名称
protected static String dbUrl = "jdbc:mysql://127.0.0.1:3306/db_database28";// 访问MySQL数据库的路径
protected static String dbUser = "root";// 访问MySQL数据库的用户名（根据自己数据库而定）
protected static String dbPwd = "";// 访问MySQL数据库的密码（根据自己数据库而定）
protected static String dbName = "db_database28";// 访问MySQL数据库中的实例(db_database28)
protected static String second = null;//
public static Connection conn = null;// MySQL数据库的连接对象
```
#### 主窗体设计

主窗体界面是该系统的欢迎界面。应用程序的主窗体必须设计层次清晰的系统菜单和工具栏，其中系统菜单包含系统中所有功能的菜单项，而工具栏主要提供常用功能的快捷访问按钮。企业进销存管理系统采用导航面板综合了系统菜单和工具栏的优点，而且导航面板的界面更加美观，操作更快捷。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200826131253196.png)

#### 销售管理设计

商品销售时进销存管理中的重要环节之一，进货商在入库之后就可以开始销售。销售单模块主要负责根据经手人的销售单据，操作进销存管理系统的库存商品和记录销售信息，方便以后查询和统计。

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020082613132687.png)

#### 信息查询设计

“信息查询”模块是进销存管理系统中不可缺少的重要组成部分，它主要包括销售查询、商品查询功能。

销售查询：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200826131434891.png)

该功能主要用于查询系统中的销售信息，其查询方式可以按照客户全称、销售票号进行匹配查询和模糊查询。另外，还可以指定销售日期查询。

其关键代码如下：

```java
// 条件查询
private final class QueryAction implements ActionListener {
public void actionPerformed(final ActionEvent e) {
boolean selDate = selectDate.isSelected();
if(content.getText().equals("")) {
JOptionPane.showMessageDialog(getContentPane(), "请输入查询内容！");
return;
}
if(selDate) {
if(startDate.getText()==null||startDate.getText().equals("")) {
JOptionPane.showMessageDialog(getContentPane(), "请输入查询的开始日期！");
return;
}
if(endDate.getText()==null||endDate.getText().equals("")) {
JOptionPane.showMessageDialog(getContentPane(), "请输入查询的结束日期！");
return;
}
}
List list=null;// 结果集
String con = condition.getSelectedIndex() == 0 ? "khname " : "sellId ";
int oper = operation.getSelectedIndex();
String opstr = oper == 0 ? "= " : "like ";
String cont = content.getText();
list = Dao.findForList("select * from v_sellView where "
+ con + opstr
+ (oper == 0 ? "'"+cont+"'" : "'%" + cont + "%'")
+ (selDate ? " and xsdate>'" + startDate.getText()
+ "' and xsdate<='" + endDate.getText()+" 23:59:59'" : ""));
// 执行拼接的SQL语句后获得的结果集
Iterator iterator = list.iterator();// 与结果集list相应的迭代器
updateTable(iterator);
}
}
}
```

商品查询：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200826131535864.png)

该功能主要用于查询系统中的商品信息，其查询方式可以按照商品的名称、供应商全称、产地、规格等信息进行查询。

其关键代码如下：

```java
// 点击“显示全部数据”按钮后，更新表格内容
private void updateTable(List list, final DefaultTableModel dftm) {
int num = dftm.getRowCount();
for (int i = 0; i < num; i++)
dftm.removeRow(0);
Iterator iterator = list.iterator();
TbSpinfo spInfo;// 商品信息
while (iterator.hasNext()) {
List info = (List) iterator.next();
Item item = new Item();
item.setId((String) info.get(0));
item.setName((String) info.get(1));
spInfo = Dao.getSpInfo(item);
Vector rowData = new Vector();
rowData.add(spInfo.getId().trim());// 商品编号
rowData.add(spInfo.getSpname().trim());// 商品名称
rowData.add(spInfo.getJc());// 商品简称
rowData.add(spInfo.getCd());// 产地
rowData.add(spInfo.getDw());// 商品计量单位
rowData.add(spInfo.getGg());// 商品规格
rowData.add(spInfo.getBz());// 包装
rowData.add(spInfo.getPh());// 批号
rowData.add(spInfo.getPzwh());// 批准文号
rowData.add(spInfo.getGysname());// 供应商名称
rowData.add(spInfo.getMemo());// 备注
dftm.addRow(rowData);// 向表格对象添加行数据（商品信息）
}
}
```
## 05 使用说明

详细使用说明见工程中“readme.txt”文件。

## 06 源码下载

关注公众号【C you again】，回复“基于java的企业进销存管理系统”免费领取。
亦可直接扫描主页二维码关注，回复“基于java的企业进销存管理系统”免费领取，[点此打开个人主页](https://gzh.cyouagain.cn/)


原文链接：http://www.demodashi.com/demo/15938.html

**说明：此源码来源于网络，若有侵权，请联系删除！！**
