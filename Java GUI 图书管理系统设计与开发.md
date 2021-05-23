@[toc]

**源码下载：关注微信公众号【C you again】，回复“Java GUI图书管理系统”免费领取。**

## 01 概述

一款功能强大的图书馆管理系统，功能齐全，小白/大学生项目实训，学习的不二之选。

## 02 技术

**此系统使用 java awt 实现**。java.awt是一个软件包，包含用于创建用户界面和绘制图形图像的所有分类。在AWT术语中，诸如按钮或滚动条之类的用户界面对象称为组件。Component类是所有 AWT 组件的根。

## 03 功能详解

#### 基础维护

##### 图书维护

 - 添加：输入图书编号、图书名称、图书页数、图书作者、出版社、库存数量、所属类型等图书信息，点击Save按钮添加新图书。
 - 修改：首先根据图书编号查询到所要修改的图书，然后对图书的名称、图书页数、作者、出版时间、定价、库存等信息进行修改。
 - 删除：首先根据图书编号查询到所要删除的图书，然后进行删除操作。

##### 读者维护

 - 添加：输入读者编号、读者姓名、读者类别、读者性别、可借天数等信息，然后点击“Add”按钮添加新读者。
 - 修改：首先根据读者编号查询到要修改的读者信息，再对读者编号、读者姓名、读者类别、读者性别、可借天数等信息进行修改，修改完成点击“保存”按钮完成修改。
 - 删除：首先根据读者编号查询到要删除的读者信息，然后进行删除操作。

#### 借阅管理

 - 借书管理：首先根据图书编号和读者编号查询到图书和读者信息，在点击“借出”按钮完成借书。
 - 还书管理：首先根据图书编号和读者编号查询到图书和读者信息，在点击“还书”按钮完成还书。

#### 查询管理
 

 - 图书查询：输入图书名称、作者、出版时间中的任意一项，点击“查询”按钮查询图书。
 - 读者查询：输入读者姓名、读者类型中的任意一项，点击“查询”按钮查询读者。

#### 系统管理

- 修改密码：首先输入旧密码等待校验，旧密码输入正确后即可设定新的密码。
- 退出系统：退出图书管理系统程序。

## 04 运行截图

#### 添加图书

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200824205253483.png)

#### 添加读者

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200824205410691.png)

#### 借书管理

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200824205603727.png)

#### 图书查询

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200824205755671.png)

#### 修改密码

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200824205839333.png)

## 05 主要代码

#### 添加图书

```java
package com.jason.frame;//com.jason.frame.BookAdd.java
import java.awt.*;
import java.awt.event.*;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.text.ParseException;
import java.text.SimpleDateFormat;
import javax.swing.JOptionPane;
public class BookAdd extends Frame implements ActionListener{
	Toolkit tool= getToolkit();
	String url="src/bookbk.png";
	Image img=tool.getImage(url);
		public void paint(Graphics g){
		g.drawImage(img,0,0,this);
	}

	public void clearAndSetBookId(){
		for(int j=0;j<booktxt.length;j++){
			booktxt[j].setText("");
		}
		String str=getInsertOrderedList();
		booktxt[0].setEditable(false);
		booktxt[0].setText(str);

	}

	String[] lbname={"图书编号","图书名称","图书页数","图书作者","翻     译","出 版 社","出版时间","定价","库存数量","所属类型"};
	Label[] booklb=new Label[10];
	TextField[] booktxt=new TextField[10];
	Button savebtn=new Button("Save");
	Button closebtn=new Button("Close");
	Choice booktype=new Choice();
	public BookAdd(){
		setTitle("添加新书");
		setLayout(null);
		setSize(500,250);
		setResizable(false);
		//this.setOpaque(false);
		this.setForeground(Color.BLACK);
		int lx=50,ly=50;
		booktype.add("程序设计");
		booktype.add("图形设计");
		booktype.add("其他");
		booktype.add("科技");
		booktype.add("文学");
		booktype.add("历史");
		booktype.add("百科");
		booktype.add("英语");
		booktype.add("计算机");
		booktype.add("Internet");
		booktype.add("数学");
		String str=getInsertOrderedList();
		for(int i=0;i<booklb.length;i++){
			if(lx>240){
				lx=50;
				ly=ly+30;
			}
			booklb[i]=new Label(lbname[i]);
			booklb[i].setBounds(lx,ly,50,20);
			booktxt[i]=new TextField();
			booktxt[i].setBounds(lx+60,ly,100,20);
			lx=lx+190;
			add(booklb[i]);add(booktxt[i]);
			}
		booktxt[0].setEditable(false);
		booktxt[0].setText(str);

		booktxt[9].setVisible(false);
		booktype.setBounds(300,170,100,20);
		add(booktype);
		savebtn.setBounds(150,210,80,25);
		closebtn.setBounds(280,210,80,25);
		add(savebtn);add(closebtn);
		addWindowListener(new WindowAdapter(){
			public void windowClosing(WindowEvent e){
				DbOp.close();
				dispose();	
			}
		});
		savebtn.addActionListener(this);
		closebtn.addActionListener(this);
		setLocationRelativeTo(null);
		setVisible(true);
	}
	public static String getInsertOrderedList(){
		String returnstring="";
		String sql="select * from book";
		
		try{
		int count=0;
		ResultSet rs=DbOp.executeQuery(sql);
		while(rs.next()){
			
			count++;
		}
		String[] allid=new String[count];
		int[] intofid=new int[count];
	
		int i=0;
		ResultSet rs1=DbOp.executeQuery(sql);
		while(rs1.next()){
			allid[i]=rs1.getString("id");
			String mystr=allid[i].substring(1);
			intofid[i]=Integer.parseInt(mystr);
			i++;
		}
		int temp=-1;
		for(int j=0;j<intofid.length;j++){
			if(intofid[j]>temp){
				temp=intofid[j];		
			}
		}
		returnstring=String.valueOf(temp+1);
		int len=returnstring.length();
		for(int f=0;f<5-len;f++){
			returnstring="0"+returnstring;
			
		}
		returnstring="A"+returnstring;
		DbOp.close();
		}catch(SQLException ee){

		}
		
		
		return returnstring;
	}

	public  void actionPerformed(ActionEvent e){
		Object ob=e.getSource();
		if(ob==savebtn){
			savebtnActionPerformed();
			clearAndSetBookId();
		}
		if(ob==closebtn){
				DbOp.close();
				dispose();				
			
		}
	}
	public  void savebtnActionPerformed(){
		String id=booktxt[0].getText();
		String typestr=booktype.getSelectedItem().toString();
		String[] inputstring=new String[9];
		boolean emptyequals=false;
		for(int i=0;i<inputstring.length;i++){
			inputstring[i]=booktxt[i].getText();
			if(inputstring[i].equals("")){
				JOptionPane.showMessageDialog(null,"请务必填写完整");
				return;
			}
		}
		if(id.equals("")){
			JOptionPane.showMessageDialog(null,"图书编号不能为空");
			return;
		}
		if(IfBookIdExit(id)){
			JOptionPane.showMessageDialog(null,"图书编号已存在");
			return;
		}
		try{
			SimpleDateFormat sdf=new SimpleDateFormat("yyyy-MM");
			sdf.parse(inputstring[6]);
			float price=Float.parseFloat(inputstring[7]);
			int stock= Integer.parseInt(inputstring[8]);
			int page=Integer.parseInt(inputstring[2]);
			String sql="insert into book(id,bookname,booktype,author,translator,publisher,publish_time,price,stock,page)";
			sql=sql+"values('"+id+"','"+inputstring[1]+"','"+typestr+"','"+inputstring[3]+"','"+inputstring[4]+"','";
			sql=sql+inputstring[5]+"','"+inputstring[6]+"',"+price+","+stock+","+page+")";
			int i=DbOp.executeUpdate(sql);
			if(i==1){
				JOptionPane.showMessageDialog(null,"图书添加成功！");
				clearAllText();
			}
		}catch(ParseException e2){
			JOptionPane.showMessageDialog(null,"出版时间格式错误，正确为（年-月）");
		}catch(NumberFormatException e1){
			JOptionPane.showMessageDialog(null,"库存数量，价格，页数错误，应为数字");
		}
	}
	public boolean IfBookIdExit(String id){
		boolean right=false;
		String sql="select*from book where id='"+id+"'";
		ResultSet rs=DbOp.executeQuery(sql);
		try{
			while(rs.next()){
			
				right = true;
			}
				//right = false;
		}catch(SQLException e){
			JOptionPane.showMessageDialog(null,"无法正常读取数据");
		}
		return right;
	}
	public void clearAllText(){
		for(int i=0;i<booktxt.length;i++){
			booktxt[i].setText("");
		}
	}
	public static void main(String[] args){
		new BookAdd();
	}
}
```

#### 添加读者


```java
package com.jason.frame;//com.jason.frame.ReaderAdd.java
import java.awt.*;
import java.awt.event.*;
import java.sql.ResultSet;
import java.sql.SQLException;
import javax.swing.JOptionPane;
public class ReaderAdd extends Frame{
	Toolkit tool= getToolkit();
	String url="src/bookbk.png";
	Image img=tool.getImage(url);
		public void paint(Graphics g){
		g.drawImage(img,0,0,this);
	}

	public void clearAndSetReaderId(){
		for(int j=0;j<readertxt.length;j++){
			readertxt[j].setText("");
		}
		String str=getInsertOrderedList();
		readertxt[0].setEditable(false);
		readertxt[0].setText(str);

	}
	String[] labelsign={"读者编号","读者姓名","读者类别","读者性别","可借数量","可借天数"};
	Label[] readerlb=new Label[6];
	static TextField[] readertxt=new TextField[6];
	Button querybtn,closebtn;
	static Choice readertype,readersex;
	public ReaderAdd(){
		setLayout(null);
		setSize(500,200);setResizable(false);
		setTitle("添加新读者");
		String str=getInsertOrderedList();
		int lx=50,ly=50;
			for(int i=0;i<readertxt.length;i++){
				if(lx>240){
					ly=ly+30;
					lx=50;
				}
				readerlb[i]=new Label(labelsign[i]);
				readertxt[i]=new TextField();
				readerlb[i].setBounds(lx,ly,50,20);
				readertxt[i].setBounds(lx+60,ly,100,20);
				lx=lx+190;
				add(readerlb[i]);
				add(readertxt[i]);
			}
		readertxt[0].setEditable(false);
		readertxt[0].setText(str);
		readertype=new Choice();
		readertype.add("教师");
		readertype.add("学生");
		readertype.add("作家");
		readertype.add("职工");
		readertype.add("其他");
		readersex=new Choice();
		readersex.add("男");
		readersex.add("女");
		readertxt[2].setVisible(false);
		readertxt[3].setVisible(false);
		readertype.setBounds(110,80,100,20);
		readersex.setBounds(300,80,100,20);
		add(readertype);add(readersex);
		querybtn=new Button("Add");
		closebtn=new Button("Close");
		querybtn.setBounds(130,140,50,20);
		closebtn.setBounds(310,140,50,20);
		add(querybtn);add(closebtn);
		querybtn.addActionListener(new ActionListener(){
			public void actionPerformed(ActionEvent e){
				updateActionPerformed(e);
				clearAndSetReaderId();
			}
		});
		closebtn.addActionListener(new ActionListener(){
			public void actionPerformed(ActionEvent e){
				DbOp.close();
				dispose();
				//System.exit(0);
			}
		});
		addWindowListener(new WindowAdapter(){
			public void windowClosing(WindowEvent e){
				DbOp.close();
				dispose();
				//System.exit(0);
			}
		});
		setLocationRelativeTo(null);
		setVisible(true);
	}
	public static String getInsertOrderedList(){
		String returnstring="";
		String sql="select * from reader";
		
		try{
		int count=0;
		ResultSet rs=DbOp.executeQuery(sql);
		while(rs.next()){
			
			count++;
		}
		String[] allid=new String[count];
		int[] intofid=new int[count];
	
		int i=0;
		ResultSet rs1=DbOp.executeQuery(sql);
		while(rs1.next()){
			allid[i]=rs1.getString("id");
			intofid[i]=Integer.parseInt(allid[i]);
			i++;
		}
		int temp=-1;
		for(int j=0;j<intofid.length;j++){
			if(intofid[j]>temp){
				temp=intofid[j];		
			}
		}
		returnstring=String.valueOf(temp+1);
		int len=returnstring.length();
		for(int f=0;f<5-len;f++){
			returnstring="0"+returnstring;
			
		}
		DbOp.close();
		}catch(SQLException ee){

		}
		
		
		return returnstring;
	}
	public static void updateActionPerformed(ActionEvent e){
		String[] readerstr=new String[6];
		readerstr[2]=readertype.getSelectedItem().toString();
		readerstr[3]=readersex.getSelectedItem().toString();
		for(int i=0;i<readerstr.length;i++){
			if(i==2||i==3){
				continue;
			}
			readerstr[i]=readertxt[i].getText();
			if(readerstr[i].equals("")){
				JOptionPane.showMessageDialog(null,"请务必填写完整");
				return;
			}
		}
		String id=readerstr[0];
		if(IfReaderExit(id)){
			JOptionPane.showMessageDialog(null,"该读者已经存在！");
			return;
		}
		try{
			int max_num=Integer.parseInt(readerstr[4]);
			int days_num=Integer.parseInt(readerstr[5]);
			String sql="insert into reader(id,readername,readertype,sex,max_num,days_num) values('";
			sql=sql+id+"','"+readerstr[1]+"','"+readerstr[2]+"','"+readerstr[3]+"',"+max_num+","+days_num+")";
			int a=DbOp.executeUpdate(sql);
			if(a==1){
				JOptionPane.showMessageDialog(null,"读者添加成功");
			}else{
				JOptionPane.showMessageDialog(null,"读者添加失败");
			}
			DbOp.close();
		}catch(NumberFormatException e1){
			JOptionPane.showMessageDialog(null,"可借数量和可借天数必须是整数");
		}
	}
	public static boolean IfReaderExit(String id){
		String sql="select * from reader where id='"+id+"'";
		try{
			ResultSet rs=DbOp.executeQuery(sql);
			if(rs.next()){
				return true;
			}else{
				return false;
			}
		}catch(SQLException e){
			JOptionPane.showMessageDialog(null,"查询数据错误");
		}
		return false;
	}
	public static void main(String[] args){
		new ReaderAdd();
	}
}
```

#### 修改密码

```java
package com.jason.frame;//com.jason.frame.ChangePassWord.java;
import java.awt.*;
import java.awt.event.*;
import java.sql.ResultSet;
import java.sql.SQLException;
import javax.swing.JOptionPane;
public class ChangePassWord extends Frame{
	String[] sign={"旧密码：","设定新密码：","重复新密码："};
	Label[] textlb=new Label[3];
	TextField[] passtxt=new TextField[3];
	Button reset=new Button("新密码设定");
	public ChangePassWord(){
		setTitle("修改密码");
		setSize(300,250);
		setLayout(null);
		setResizable(false);
		int y=50;
		for(int i=0;i<3;i++){
			textlb[i]=new Label(sign[i]);
			passtxt[i]=new TextField();
			textlb[i].setBounds(50,y,80,20);
			passtxt[i].setBounds(130,y,100,20);
			passtxt[i].setEchoChar('●');
			add(textlb[i]);add(passtxt[i]);
			y=y+50;
		}
		reset.setBounds(110,200,80,20);
		add(reset);
		setLocationRelativeTo(null);
		reset.addActionListener(new ActionListener(){
			public void actionPerformed(ActionEvent e){
				setNewPassWord(e);
			}
		});
		addWindowListener(new WindowAdapter(){
			public void windowClosing(WindowEvent e){
				System.exit(0);
			}
		});
		setVisible(true);
	}
	public void setNewPassWord(ActionEvent e){
		String[] password=new String[3];
		String sql;
		for (int i=0;i<password.length;i++){
			password[i]=passtxt[i].getText();
		}
		if(!password[0].equals("")){
			sql="select * from user where username='"+GlobalVar.login_user+"'";
		}else{
			JOptionPane.showMessageDialog(null,"Empty String Just Input");
			return;
		}
		if(password[0].equals(password[1])){

		}
		try{
			ResultSet rs=DbOp.executeQuery(sql);
			while(rs.next()){
				if(password[0].equals(rs.getString("password"))){
					
				}else{
					JOptionPane.showMessageDialog(null,"Sorroy It's Fail It's not Old PassWord");
					return;

				}
			}
		}catch(SQLException ee){
			JOptionPane.showMessageDialog(null,"Sorroy It's Fail Some Data Wrong");
			return;
			
		}
		if(password[0].equals(password[1])){
			JOptionPane.showMessageDialog(null,"Sorroy It's Fail Same From Old Password");
			return;
		}
		if(!password[1].equals(password[2])){
			JOptionPane.showMessageDialog(null,"Sorroy It's Fail  Again Password wrong");
			return;
		}
		if(password[1].equals("")&&password[1].equals("")){
			JOptionPane.showMessageDialog(null,"Sorroy It's Fail  one Empty");
			return;
			
		}
		sql="";
		sql="update user set password='"+password[1]+"'where username='"+GlobalVar.login_user+"'";
		int da=DbOp.executeUpdate(sql);
		if(da==1){
			JOptionPane.showMessageDialog(null,"yet It's Successed  Update PassWord");
		}else{
			JOptionPane.showMessageDialog(null,"Sorroy It's Fail  try again");
		}
	} 
	public static void main(String[] args){
		new ChangePassWord();
	}
}
```

## 06 源码下载

关注公众号【C you again】，回复“Java GUI图书管理系统”免费领取。

亦可直接扫描主页二维码关注，回复“Java GUI图书管理系统”免费领取，[点此打开个人主页](https://gzh.cyouagain.cn/)


**说明：此源码来源于网络，若有侵权，请联系删除！！**
