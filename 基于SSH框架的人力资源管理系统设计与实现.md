
> 大家好，应各位粉丝的要求的，今天给大家分享下人力资源管理系统，需要源码的同志在公众号【C you again】后台回复“基于SSH框架的人力资源管理系统”获取。提前声明：此系统来源于网络，本人只做了收集整理，如果侵权，请联系删除。

**关注公众号【C you again】，回复“基于SSH框架的人力资源管理”免费下载。**

## 01 概述

人力资源管理系统（Human Resource Management system ，以下简称HRMS）是将以计算机为基础的管理信息系统应用于人力资源管理而形成的一种现代化的人力资源管理方法和手段，是对信息技术与人力资源管理技术结合的最佳定义。

　　人力资源是企业的第一资源，如何有效地管理、利用和开发这一资源 ，是摆在每一位管理者面前必须重视的大事。人力资源管理工作可分为建立规章制度的基础性工作、基于标准操作流程的例行性工作、人力资源规划等战略性工作以及企业文化建设、职工职业生涯设计等开拓性工作。其中，大量的例行性工作往往占据了人力资源管理工作人员的大部分时间。如果能建立起人力资源管理信息系统，把这部分工作分离出来，用计算机来进行管理，必将能大大提高人力资源管理人员的工作效率。同时，利用人力资源管理信息系统中存储的大量历史信息，建立起企业人力资源决策支持系统，可为领导决策提供有用的参考信息。

## 02 技术

> Spring+SpringMVC+ Hibernate+ MySql 

## 03 运行环境

> Java1.8 + MySql + Eclipse

## 04 功能概述

本系统主要有部门管理、员工管理、招聘管理、培训管理、奖罚管理、薪资管理、个人信息管理七大模块。

部门管理：此模块可以查看所有部门的详细信息，如：部门名称，部门创建时间，部门人数。也可以对某个部门进行修改删除操作，除此以外，还可以添加部门。

员工管理：员工管理模块有查看、修改、添加、删除员工信息的功能。

招聘管理：本模块可以查看求职人员的具体信息，包括姓名、性别、应聘职位、工作经验等等，也可以对应聘人员进行删除、录用。

培训管理：此模块用来发布企业的培训信息，如培训时间，培训地址，培训课程和培训人员等等。

奖罚管理：记录企业员工的奖罚情况。

薪资管理：管理企业员工薪资，有调整薪资，添加员工及薪资，删除员工及薪资等功能。

个人信息管理：查看修改个人信息。

## 05 运行截图

#### 部门管理

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200827190417116.png)

#### 员工管理

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020082719045618.png)

#### 招聘管理

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020082719052441.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200827190538527.png)

#### 奖罚管理

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200827190607911.png)

#### 薪资管理

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200827190630366.png)

#### 个人信息管理

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200827190710265.png)

## 06 主要代码

#### 部门管理

```java
package com.wy.action;

import java.util.List;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;
import org.apache.struts.action.ActionForm;
import org.apache.struts.action.ActionForward;
import org.apache.struts.action.ActionMapping;
import org.apache.struts.actions.DispatchAction;

import com.wy.dao.ObjectDao;
import com.wy.form.DepartmentForm;
import com.wy.form.ManagerForm;

public class DepartmentAction extends DispatchAction {
	private ObjectDao objectDao;

	public ObjectDao getObjectDao() {
		return objectDao;
	}

	public void setObjectDao(ObjectDao objectDao) {
		this.objectDao = objectDao;
	}

	// 部门察看操作
	public ActionForward queryDepartment(ActionMapping mapping,
			ActionForm form, HttpServletRequest request,
			HttpServletResponse response) {
		List list = objectDao.getObjectList("from DepartmentForm order by id desc");
		request.setAttribute("list", list);
		request.setAttribute("employeeList", objectDao.getObjectList("from EmployeeForm"));
		return mapping.findForward("queryDepartment");
	}

	// 部门信息保存
	public ActionForward insertDepartment(ActionMapping mapping,
			ActionForm form, HttpServletRequest request,
			HttpServletResponse response) {
		DepartmentForm departmentForm = (DepartmentForm) form;
		DepartmentForm departmentform = (DepartmentForm)objectDao
				.getObjectForm("from DepartmentForm where dt_name='"
						+ departmentForm.getDt_name() + "'");
		if (departmentform== null) {
			objectDao.insertObjectForm(departmentForm);
			return queryDepartment(mapping, form, request, response);
		} else {
			request.setAttribute("result", "不能够重复提交！！！");
			return mapping.findForward("operationDepartment");
		}
	}
	//部门信息删除
	public ActionForward deleteDepartment(ActionMapping mapping,
			ActionForm form, HttpServletRequest request,
			HttpServletResponse response) {
		DepartmentForm departmentform = (DepartmentForm)objectDao
		.getObjectForm("from DepartmentForm where id='"
				+ request.getParameter("id") + "'");	
		if(objectDao.deleteObjectForm(departmentform)){			
		}else{
			request.setAttribute("result", "删除部门信息失败！！！");
		
		}
		return mapping.findForward("operationDepartment");
		
	}
	
	
	
}

```

#### 培训管理

```java
package com.wy.action;

import java.util.List;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.apache.struts.action.ActionForm;
import org.apache.struts.action.ActionForward;
import org.apache.struts.action.ActionMapping;
import org.apache.struts.actions.DispatchAction;

import com.wy.dao.ObjectDao;
import com.wy.form.TrainForm;

public class TrainAction extends DispatchAction {
	private ObjectDao objectDao;

	public ObjectDao getObjectDao() {
		return objectDao;
	}

	public void setObjectDao(ObjectDao objectDao) {
		this.objectDao = objectDao;
	}

	// 培训察看操作
	public ActionForward queryTrain(ActionMapping mapping,
			ActionForm form, HttpServletRequest request,
			HttpServletResponse response) {
		List list = objectDao.getObjectList("from TrainForm order by id desc");
		request.setAttribute("list", list);
		return mapping.findForward("queryTrain");
	}

	
	// 添加培训操作
	public ActionForward deleteTrain(ActionMapping mapping,
			ActionForm form, HttpServletRequest request,
			HttpServletResponse response) {
		String id=request.getParameter("id");
		TrainForm trainForm=(TrainForm)objectDao.getObjectForm("from TrainForm where id='"+id+"'");
		this.objectDao.deleteObjectForm(trainForm);
		return this.queryTrain(mapping, form, request, response);
	}

	//添加培训操作
	public ActionForward saveTrain(ActionMapping mapping,
			ActionForm form, HttpServletRequest request,
			HttpServletResponse response) {
		TrainForm trainForm=(TrainForm)form;
		this.objectDao.insertObjectForm(trainForm);
		return mapping.findForward("operationTrain");
	}
	
	//培训详细查询
	public ActionForward queryOneTrain(ActionMapping mapping,
			ActionForm form, HttpServletRequest request,
			HttpServletResponse response) {
		String id=request.getParameter("id");
		TrainForm trainForm=(TrainForm)objectDao.getObjectForm("from TrainForm where id='"+id+"'");
		request.setAttribute("trainForm", trainForm);
		return mapping.findForward("queryOneTrain");
	}
	
}

```

#### 薪资管理

```java
package com.wy.action;

import java.util.List;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;
import org.apache.struts.action.ActionForm;
import org.apache.struts.action.ActionForward;
import org.apache.struts.action.ActionMapping;
import org.apache.struts.actions.DispatchAction;

import com.wy.dao.ObjectDao;
import com.wy.form.DepartmentForm;
import com.wy.form.EmployeeForm;
import com.wy.form.ManagerForm;
import com.wy.form.PayForm;
import com.wy.tool.GetAutoNumber;

public class PayAction extends DispatchAction {
	private ObjectDao objectDao;

	public ObjectDao getObjectDao() {
		return objectDao;
	}

	public void setObjectDao(ObjectDao objectDao) {
		this.objectDao = objectDao;
	}
	//薪资删除
	public ActionForward deletePay(ActionMapping mapping,
			ActionForm form, HttpServletRequest request,
			HttpServletResponse response) {
		String condition = "from PayForm where id='"+request.getParameter("id")+"'";
		PayForm payForm=(PayForm)objectDao.getObjectForm(condition);
		if(payForm!=null)
		objectDao.deleteObjectForm(payForm);
		return queryPay(mapping,form,request,response);
	}
	
	

	// 薪资查看
	public ActionForward queryPay(ActionMapping mapping,
			ActionForm form, HttpServletRequest request,
			HttpServletResponse response) {
		List list = objectDao.getObjectList("from PayForm");	
		if(request.getParameter("emNumber")!=null){
		String emNumber= request.getParameter("emNumber");
		list = objectDao.getObjectList("from PayForm where pay_emNumber='"+emNumber+"'");	
		request.setAttribute("result1",emNumber);
		}
		if(request.getParameter("pay_month")!=null){
		String pay_month= request.getParameter("pay_month");
		list = objectDao.getObjectList("from PayForm where pay_month='"+pay_month+"'");	
		request.setAttribute("result2",pay_month);	
		}
		
		
		request.setAttribute("list", list);
		String condition = "from EmployeeForm order by id desc";
		request.setAttribute("employeeList",objectDao.getObjectList(condition));
		return mapping.findForward("queryPay");
	}

	// 转向添加新姿的页面
	public ActionForward forwardInsertPay(ActionMapping mapping,
			ActionForm form, HttpServletRequest request,
			HttpServletResponse response) {
		this.saveToken(request);
		String condition = "from EmployeeForm order by id desc";
		List list = objectDao.getObjectList(condition);
		request.setAttribute("employeeList",list);
		return mapping.findForward("forwardInsertPay");
	}
	// 添加薪资
	public ActionForward savePay(ActionMapping mapping, ActionForm form,
			HttpServletRequest request, HttpServletResponse response) {
		PayForm payForm = (PayForm) form;
		String payCondition= "from PayForm where pay_month='"+payForm.getPay_month()+"' and pay_emNumber='"+payForm.getPay_emNumber()+"'";
		if(objectDao.getObjectForm(payCondition)!=null){
			request.setAttribute("result", "该员工已经工资已经发送完毕");
			return mapping.findForward("operationPay");
		}	
		String emCondition = "from EmployeeForm where em_serialNumber='"+payForm.getPay_emNumber()+"'";
		EmployeeForm employeeForm= (EmployeeForm)objectDao.getObjectForm(emCondition);
		payForm.setPay_emName(employeeForm.getEm_name());
		if (this.isTokenValid(request)) {
			this.resetToken(request);		
			objectDao.insertObjectForm(payForm);
		} else {
			this.saveToken(request);
		}
		return queryPay(mapping, form, request, response);
	}	
}

```

## 07 使用说明

 - mysql导入sql文件，修改（WebContent\WEB-INF\applicationContext.xml）applicationContext.xml文件修改数据库配置信息。
 - eclipse导入项目并部署，tomcat部署后，访问http://localhost:8080/PersonManager/，用户名：adain，密码：123

## 08 源码下载

关注公众号【C you again】，回复“基于SSH框架的人力资源管理”免费领取。

亦可直接扫描主页二维码关注，回复“基于SSH框架的人力资源管理”免费领取，[点此打开个人主页](https://gzh.cyouagain.cn/)

**说明：此源码来源于网络，若有侵权，请联系删除！！**
