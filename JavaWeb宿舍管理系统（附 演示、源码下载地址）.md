
> 宿舍管理是高校管理的重要组成部分，一套优秀的管理系统不仅可以降低宿舍管理的难度，也能在一定程度上减少学校管理费用的支出，能是建设现代化高校管理体系的重要标志。

本篇文章将带你从运行环境搭建、系统设计、系统编码到整个系统的实现，对整个过程进行详细描述，特别适合作为程序员的进阶项目案列，同样也是高校学生毕业设计系统实现的不二之选！

**演示地址：[宿舍管理系统演示地址，点我查看](http://dorm.cyouagain.cn/)**

**源码下载：若需获取本系统源码请在公众号【C you again】回复“宿舍管理系统”**

## 1、系统架构模式
**本宿舍管理系统采用B/S架构模式。**


B/S架构的全称为Browser/Server，即浏览器/服务器结构。Browser指的是Web浏览器，与C/S架构相比，B/S模式极少数事务逻辑在前端实现，它的主要事务逻辑在服务器端实现。B/S架构的系统无须特别安装，只有Web浏览器即可。

**B/S架构的分层：**

与C/S架构只有两层不同的是，B/S架构有三层，分别为：

 - 第一层表现层：主要完成用户和后台的交互及最终查询结果的输出功能。
 - 第二层逻辑层：主要是利用服务器完成客户端的应用逻辑功能。
 - 第三层数据层：主要是进行数据持久化存储。
 
## 2、技术选型

选择合适的技术，整个项目就已经成功了一半，经过对系统需求和系统自身特点的分析，加上现代B/S模式主流架构解决方案，对本系统技术选型如下：

**数据表现层：** Html+JavaScript+CSS+VUE

**业务逻辑层** Java+Spring+SpringMVC

**数据持久层：** MySql+MyBatis

**开发工具：** Eclipse

## 3、用户分析

本系统主要应用于高校宿舍管理，使用人群如下：

- 系统管理员：管理整个系统的安全运行，各个功能使用。
- 宿舍管理员：管理自己负责的宿管和学生
- 学生：查看浏览信息，提交任务

## 4、功能分析

**系统管理员：**

 1. 添加、修改、删除公告信息
 2. 添加、修改、删除宿舍管理员信息
 3. 添加、修改、删除学生信息
 4. 宿舍楼管理及其宿舍管理员分配
 5. 学生寝室管理
 6. 发布考勤、打卡任务
 7. 查看、修改个人信息

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200926123800845.png)

**宿舍管理员：**

 1. 查看公告
 2. 查看、删除自己管理的学生
 3. 添加、修改、删除考勤记录
 4. 查看学生打卡记录
 5. 查看、修改个人信息

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200926124203648.png)

**学生：**

 1. 查看公告
 2. 查看考勤记录
 3. 完成打卡任务，查看打卡记录
 4. 查看、修改个人信息
 
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200926124529377.png)

## 5、数据库设计

分析系统需求，数据库应有以下几张表：

**t_admin:** 主要用于存储系统管理员数据

|字段名称|类型  | 是否主键|说明 |
|--|--|--|--|
|adminId  | int |是| 管理员Id，唯一|
|userName| varchar|否|用户名|
|password|    varchar     |           否       |      密码   |
|name|   varchar      |           否       |      真实名称|
|sex|    varchar     |           否       |      性别    |
|sex|   varchar      |           否       |      电话  |

**t_dormbuild:** 存储宿舍楼信息

|字段名称|类型  | 是否主键|说明 |
|--|--|--|--|
|dormBuildId|   int|           是       |      宿舍楼Id，唯一  |
|dormBuildName|   varchar      |           否       |     宿舍楼名称  |
|dormBuildDetail|   varchar      |           否       |      描述  |

**t_dormmanager：** 主要存储宿舍管理员信息

|字段名称|类型  | 是否主键|说明 |
|--|--|--|--|
|dormManId|   varchar      |           是       |      宿舍管理员Id，唯一  |
|userName|   varchar      |           否       |      用户名，用于登录系统  |
|password|   varchar      |           否       |    密码    |
|dormBuildId|   int|           否       |      宿舍楼Id  |
|dormBuildDetail|   varchar      |           否       |      描述  |
|name|   varchar      |           否       |      真实姓名  |
|sex|   varchar      |           否       |      性别  |
|tel|   varchar      |           否       |      电话  |

**t_notice:** 用于存储公告信息

|字段名称|类型  | 是否主键|说明 |
|--|--|--|--|
|noticeId|   int|           是      |      公告Id，唯一  |
|noticePerson|   varchar      |           否       |      公告发布人  |
|date|   date|           否       |      公告发布日期  |
|content|   varchar      |           否       |      发布内容  |

**t_punchclock：** 用于存储打卡发布记录

|字段名称|类型  | 是否主键|说明 |
|--|--|--|--|
|id|   int|           是      |      Id，唯一  |
|theme|   varchar      |           否       |      打卡主题  |
|detail|   varchar      |           否       |     打卡说明  |
|date|   varchar      |           否       |      发布日期  |
|person|   varchar      |           否       |      发布人  |

**t_punchclockrecord：** 用于存储打卡信息

|字段名称|类型  | 是否主键|说明|
|--|--|--|--|
|id	|   int|           是     |      记录Id，唯一  |
|punchClock_id|   varchar      |           否       |      打卡Id  |
|punchClock_date	|   date|           否       |      发布日期  |
|punchClock_theme	|   varchar      |           否       |      打卡主题  |
|punchClock_detail	|   varchar      |           否       |      打卡说明  |
|punchClock_person	|   varchar      |           否       |      发布人  |
|name	|   varchar      |           否       |      学生姓名  |
|dormName	|   varchar      |           否       |      寝室号  |
|tel	|   varchar      |           否       |      学生电话  |
|stuNum	|   varchar      |           否       |      学生学号  |
|dormBuildId	|   int|           否       |      宿舍楼  |
|isRecord	|   tinyint	|           否       |      是否已经打卡 |

**t_record：** 用于存储考勤记录

|字段名称|类型  | 是否主键|说明|
|--|--|--|--|
|recordId		|   int|           是     |      考勤Id，唯一  |
|studentNumber|   varchar	|           否       |      学生学号 |
|dormBuildId	|   int|           否       |     宿舍楼 |
|dormName	|   varchar	|           否       |      寝室号 |
|date	|   varchar	|           否       |      考勤日期 |
|detail	|   varchar	|           否       |     详细说明 |

**t_student:** 学生表，用于存放学生信息

|字段名称|类型  | 是否主键|说明|
|--|--|--|--|
|studentId	|   int|           是     |      学生Id，唯一  |
|stuNum	|   varchar	|           否       |     学号 |
|password	|   varchar	|           否       |     密码 |
|name	|   varchar	|           否       |     姓名 |
|dormBuildId	|   int|           否       |     宿舍楼 |
|dormName	|   varchar	|           否       |     寝室号 |
|sex	|   varchar	|           否       |     性别 |
|tel	|   varchar	|           否       |     电话 |


## 6、运行环境搭建

前面已经提到，本系统使用SSM框架，搭建过程较为繁琐，因此将此部分独立出来，作为一个专题，具体搭建过程请参考[《手把手教你搭建SSM框架（Eclipse版）》](https://blog.csdn.net/qq_40625778/article/details/108738764) 这篇文章。搭建过程若出现其他问题，可以在[公众号【C you again】](https://gzh.cyouagain.cn) 后台私信。

## 7、项目工程结构

根据第六步搭建完系统运行环境后，工程结构目录如下图所示

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200926161618970.png)

**对工程结构各个目录的解释：**

```java
com.cya.controller
```
controller包用于存放接收请求的类，充当前后端数据交互的“桥梁”

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020092616250568.png)

```java
com.cya.service
```
service包是所有业务逻辑的接口

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200926162803593.png)

```java
com.cya.service.impl
```
service.impl包用于存放service接口的所有实现类

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200926162956990.png)

```java
com.cya.mapper
```
mapper包用于存放对数据库操纵的接口和对应的xml实现

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200926163235344.png)

```java
com.cya.entity
```
entity包用于存放项目中用到的所有实体类，它与数据库中的表相对应

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200926163450155.png)

resources文件下存放SSM框架整合的必要配置文件，详情请看[《手把手教你搭建SSM框架（Eclipse版）》](https://blog.csdn.net/qq_40625778/article/details/108738764)

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020092616374144.png)

dorm是存放所有model层文件的父级文件夹，其中admin，dormManager、student三个子文件夹存放系统管理员、宿舍管理员、学生三个角色对应的HTML文件。

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020092616420276.png)

## 8、功能实现及展示

由于系统包含功能众多，在此无法一一列举，所以挑选几个代表做展示，如需获取完整源码请在公众号【C you again】回复“宿舍管理系统”。

#### 8.1 登录功能实现

项目启动成功后，在浏览器地址栏输入：http://localhost:8080/dormManage/ 进入用户登录界面如下图所示：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200926165247339.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200926165939202.png)



在此界面用户可以选择不同的角色登录，输入每个角色对应的登录信息后，首先会判断输入信息的有效性，做出相应的响应或提示。登录功能具体的实现代码如下，此处仅展示controller层代码，如下：

```java
package com.cya.controller;

import java.util.ArrayList;
import java.util.List;
import java.util.Map;

import javax.management.relation.Role;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpSession;

import org.apache.tomcat.util.digester.ArrayStack;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

import com.cya.entity.Login;
import com.cya.entity.Result;
import com.cya.service.ILoginService;
import com.cya.service.impl.LoginServiceImpl;

@Controller
@ResponseBody
public class LoginController {
	
	@Autowired
	private ILoginService loginServiceImpl;
	
	@RequestMapping("login")
	public List login(HttpServletRequest request, @RequestBody Login login) {
		List list=loginServiceImpl.login(login);
		if(list.size()==1) {
			HttpSession session=request.getSession();
			session.setAttribute(login.getRole(), list);
			System.out.println("session="+session.getAttribute(login.getRole()));
		}
		return list;
	}
	
	@RequestMapping("getSession")
	public List getSession(HttpServletRequest request,@RequestBody Login login){
		System.out.println(login);
		System.out.println(request.getSession().getAttribute(login.getRole()));
		List list=new ArrayList<>();
		list.add(request.getSession().getAttribute(login.getRole()));
		return list;
	}
	
	@RequestMapping("exitSys")
	public Result exitSys(HttpServletRequest request) {
		String exit="";
		try {
			if(request.getParameter("exit")!=null) {
				exit=request.getParameter("exit");
			}
			request.getSession().removeAttribute(exit);
			return new Result(true, "注销成功");
		} catch (Exception e) {
			// TODO: handle exception
			e.printStackTrace();
			return new Result(false, "注销失败");
		}
	}
}

```
#### 8.2 发布公告功能实现

在系统管理员端，有发布公告的权限，系统管理员点击添加公告按钮，输入相关信息后进行有效性校验，校验成功及代表公告发布成功。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200926170458326.png)

成功发布公告后，会出现在宿舍管理员端和学生端界面，效果图如下：

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020092617100845.png)

公告模块主要代码以mapper层实现为例：

```xml
 <!-- ******************** 公告 ******************* -->
 
  <select id="getNoticeManage" resultType="com.cya.entity.Notice">
    select * from t_notice
    <where>
      <if test="filter=='date' and key !='' ">
        date like concat("%",#{key},"%")
      </if>
    </where>
  </select>
  
   <insert id="addNoticeManage" parameterType="com.cya.entity.Notice">
    insert into t_notice(noticePerson,date,content) values(#{noticePerson},current_date,#{content})
  </insert>
  
  <select id="getNoticeMangerById" parameterType="Integer" resultType="com.cya.entity.Notice">
     select * from t_notice where noticeId=#{noticeId}
  </select>
  
  <update id="updataNoticeManageById" parameterType="com.cya.entity.Notice">
    update t_notice set noticePerson=#{noticePerson},content=#{content} where noticeId=#{noticeId}
  </update>
  
  <delete id="noticeManagerDeleteById" parameterType="Integer">
  
     delete from t_notice where noticeId=#{noticeId}
  </delete>
 
 <!-- ******************** 公告 ******************* -->
```
#### 8.3 考勤记录功能实现

宿舍管理员根据自己所管理楼，对住在管理范围内的学生进行考勤，并添加考勤记录，学生端也会显示考勤信息。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200926171810741.png)

主要实现代码如下：

```java
@RequestMapping("/getRecordMsg")
	public PageResult getRecordMsg(HttpServletRequest request){
		Integer pageNum=1;
		Integer pageSize=20;
		Integer dormBuildId=0;
		String filter=request.getParameter("filter");
		String key=request.getParameter("key");
		if(request.getParameter("pageNum")!=null && request.getParameter("pageNum")!="") {
			pageNum=Integer.parseInt(request.getParameter("pageNum"));
		}
		if(request.getParameter("pageSize")!=null && request.getParameter("pageSize")!="") {
			pageSize=Integer.parseInt(request.getParameter("pageSize"));
		}
		if(request.getParameter("dormBuildId")!=null && request.getParameter("dormBuildId")!="") {
			dormBuildId=Integer.parseInt(request.getParameter("dormBuildId"));
		}
		System.out.println("pageNum="+pageNum);
		System.out.println("pageSize="+pageSize);
		return dormManageServiceImpl.getRecordMsg(pageNum, pageSize, filter, key, dormBuildId);
	}
	
	@RequestMapping("getRecordById")
	public Record getRecordById(Integer recordId) {
		return dormManageServiceImpl.getRecordById(recordId);
	}
	
	
	@RequestMapping("updataRecordMsg")
	public Result updataRecordMsg(@RequestBody Record record) {
		try {
			dormManageServiceImpl.updataRecordMsg(record);
			return new Result(true, "更新成功");
		} catch (Exception e) {
			// TODO: handle exception
			e.printStackTrace();
			return new Result(false, "更新失败");
		}
	}
	
	@RequestMapping("addRecordMsg")
	public Result addRecordMsg(@RequestBody Record record) {
		try {
			System.out.println(record);
			dormManageServiceImpl.addRecordMsg(record);
			return new Result(true, "添加成功");
		} catch (Exception e) {
			// TODO: handle exception
			e.printStackTrace();
			return new Result(false, "添加失败");
		}
	}
	
	@RequestMapping("recordManagerDeleteById1")
	public Result recordManagerDeleteById(HttpServletRequest request) {
		Integer recordId=0;
		if(request.getParameter("recordId")!=null && request.getParameter("recordId")!="") {
			recordId=Integer.parseInt(request.getParameter("recordId"));
		}
		try {
			dormManageServiceImpl.recordManagerDeleteById1(recordId);
			return new Result(true, "删除成功");
		} catch (Exception e) {
			// TODO: handle exception
			e.printStackTrace();
			return new Result(false, "删除失败");
		}
	}
```

## 9、源码下载

若需获取本系统源码请在公众号【C you again】回复“宿舍管理系统”

你也可以点击此链接[快速回复](https://gzh.cyouagain.cn/)

## 10、相关说明

 1. 制作不易，记得点赞+收藏+转发
 3. 本人技术有限，若有错误欢迎指正
 4. 本系统和文章均属于【C you again】原创，欢迎个人博客、各大网站转载，请注明转载地址

#### 演示地址：[宿舍管理系统演示](http://dorm.cyouagain.cn/)

