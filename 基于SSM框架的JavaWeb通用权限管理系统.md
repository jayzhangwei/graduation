@[toc]

**关注公众号【C you again】，回复“基于SSM框架的JavaWeb通用权限管理系统”免费下载。**

## 01 概述

这是一个通用权限管理系统项目，基于SSM（Spring + Spring-MVC +Mybatis）框架开发，其SQL语句持久在Hibernate 中，对原生SQL的支持较好。制作该系统的初衷是用来帮助JavaWeb开发者或初学者学习、借鉴的需要。读者可以在这个 系统基础上引入其它技术或完全依赖本系统技术进行功能拓展，来开发实际应用需求的项目，免去了应用系统中对于“ 权限设计”这一部分的麻烦。

## 02 技术

> Jsp 、SSM（Spring + Spring-MVC + Mybatis）、Shiro 、Mvc、Jdbc、MySQL、DWZ富客户端框架 + Jquery + Ajax

## 03 环境

> JDK:JDK1.6+ 、WEB:Tomcat6.0+ 、DB:MySQL5+ 、IDE: MyEclipse8.5+/Eclipse4.4+ 

## 04 工程结构

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200825222028100.png)

## 05 运行截图

#### 登录界面

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200825222431271.png)

#### 员工管理界面

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020082522252173.png)

#### 部门管理界面

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200825222550263.png)

#### 角色管理界面


![在这里插入图片描述](https://img-blog.csdnimg.cn/20200825222633694.png)

## 06 主要代码

#### 员工部门管理

```java
package com.kzfire.portal.action.user;

import java.util.List;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.servlet.ModelAndView;

import com.kzfire.portal.base.BaseAction;
import com.kzfire.portal.entiy.SysDept;
import com.kzfire.portal.service.DeptService;
import com.kzfire.portal.service.UserService;
import com.kzfire.portal.utils.JSONUtils;
import com.kzfire.portal.utils.VoFactory;
import com.kzfire.portal.vo.ConditionVo;

@RequestMapping("/user/dept")
@Controller
public class DeptAction extends BaseAction{
	@Autowired
	DeptService deptService;
	@Autowired
	UserService userService;
	
	/**
	 * 设置员工部门
	 * @param model
	 * @param request
	 * @param response
	 * @return
	 */
	@RequestMapping("/setUserDept")
	public String setUserDept(Model model,HttpServletRequest request,HttpServletResponse response) {
		
		String userId=request.getParameter("userId");
		model.addAttribute("userId", userId);
		//设置部门树
		List<SysDept> list=deptService.getAllDept();
		model.addAttribute("data", JSONUtils.parseList(list));
		return VIEW+"user/dept/setUserDept";
	}
	
	@RequestMapping("/saveUserdept")
	public ModelAndView saveUserdept(Model model, HttpServletRequest request,
			HttpServletResponse response) {
		try {
			Integer userId=Integer.parseInt(request.getParameter("userId"));
			Integer deptId=Integer.parseInt(request.getParameter("deptId"));
			deptService.saveUserdept(userId,deptId);
		} catch (Exception e) {
			e.printStackTrace();
			return ajaxDoneError("操作失败:"+e.getMessage());
		}
		return ajaxDoneSuccess("操作成功");
	}
	
	@RequestMapping("/main")
	public String list(Model model,HttpServletRequest request,HttpServletResponse response) {
		//设置部门树
		List<SysDept> list=deptService.getAllDept();
		System.out.println("json格式----->"    + JSONUtils.parseList(list).toString() );
		model.addAttribute("data", JSONUtils.parseList(list));
		return VIEW+"user/dept/dept";
	}
	
	@RequestMapping("/userList")
	public String userList(Model model,HttpServletRequest request,HttpServletResponse response) {
		ConditionVo cvo=VoFactory.getConditionVo(request);
		String deptId=request.getParameter("deptId");
		if("1".equals(deptId))
		{
			cvo.setText4("1");
		}else
		{
			cvo.setText3(request.getParameter("deptId"));
		}
		request.setAttribute("deptId", deptId);
		cvo.setTotalCount(userService.getUserCount(cvo));
		model.addAttribute("vo", cvo);
		model.addAttribute("list", userService.getList(cvo));
		return VIEW+"user/dept/userList";
	}
	
	@RequestMapping("/add")
	public String add(Model model,HttpServletRequest request,HttpServletResponse response) {
		SysDept dept=new SysDept();
		dept.setPid(Integer.parseInt(request.getParameter("selDept")));
		model.addAttribute("dept", dept);
		return VIEW+"user/dept/deptEdit";
	}
	@RequestMapping("/edit")
	public String edit(Model model,HttpServletRequest request,HttpServletResponse response) {
		SysDept dept=deptService.getDeptById(Integer.parseInt(request.getParameter("selDept")));
		model.addAttribute("dept", dept);
		return VIEW+"user/dept/deptEdit";
	}
	
	@RequestMapping("/del")
	public ModelAndView del(Model model, HttpServletRequest request)
	{
		try {
			String deptId=request.getParameter("selDept");
			deptService.delDeptById(Integer.parseInt(deptId));
		} catch (Exception e) {
			e.printStackTrace();
			return ajaxDoneError("操作失败:"+e.getMessage());
		}
		return ajaxDoneSuccess("操作成功");
	}
	
	@RequestMapping("/save")
	public ModelAndView save(SysDept dept,Model model, HttpServletRequest request,
			HttpServletResponse response) {
		try {
			if(dept!=null)
			{
				deptService.saveDept(dept);
			}
		} catch (Exception e) {
			e.printStackTrace();
			return ajaxDoneError("操作失败:"+e.getMessage());
		}
		return ajaxDoneSuccess("操作成功");
	}
}

```

#### 角色管理

```java
package com.kzfire.portal.action.user;

import java.util.List;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.servlet.ModelAndView;

import com.kzfire.portal.base.BaseAction;
import com.kzfire.portal.entiy.SysRole;
import com.kzfire.portal.service.RoleService;
import com.kzfire.portal.utils.VoFactory;
import com.kzfire.portal.vo.ConditionVo;
import com.kzfire.portal.vo.PerGroupVo;

@RequestMapping("/user/role")
@Controller
public class RoleAction extends BaseAction{
	@Autowired
	RoleService roleService;
	
	@RequestMapping("/list")
	public String list(Model model,HttpServletRequest request,HttpServletResponse response) {
		ConditionVo cvo=VoFactory.getConditionVo(request);
		cvo.setTotalCount(roleService.getTableCount("sys_role"));
		model.addAttribute("vo", cvo);
		model.addAttribute("list", roleService.getList(cvo));
		return VIEW+"permission/role/list";
	}
	
	/**
	 * 权限编辑页面
	 * @param model
	 * @param request
	 * @return
	 */
	@RequestMapping("/editPermission")
	public String editPermission(Model model, HttpServletRequest request)
	{
		String roleId=request.getParameter("roleId");
		//获取角色权限
		List<PerGroupVo> group=roleService.getPerGroupVoByUserId(Integer.parseInt(roleId));
		model.addAttribute("group", group);
		model.addAttribute("roleId", roleId);
		return VIEW+"permission/role/editPermission";
	}
	
	@RequestMapping("/savePer")
	public ModelAndView savePer(Model model, HttpServletRequest request,
			HttpServletResponse response) {
		try {
			String[] perIds=request.getParameterValues("perId");
			roleService.savePermission(perIds,Integer.parseInt(request.getParameter("roleId")));
		} catch (Exception e) {
			e.printStackTrace();
			return ajaxDoneError("操作失败");
		}
		
		return ajaxDoneSuccess("操作成功");
	}
	
	@RequestMapping("/add")
	public String add(Model model, HttpServletRequest request)
	{
		model.addAttribute("role", new SysRole());
		return VIEW+"permission/role/roleEdit";
	}
	
	
	
	@RequestMapping("/edit")
	public String edit(Model model, HttpServletRequest request)
	{
		String roleId=request.getParameter("roleId");
		SysRole role=roleService.getRoleById(Integer.parseInt(roleId));
		model.addAttribute("role", role);
		return VIEW+"permission/role/roleEdit";
	}
	
	
	
	@RequestMapping("/del")
	public ModelAndView del(Model model, HttpServletRequest request)
	{
		try {
			String roleId=request.getParameter("roleId");
			roleService.delRoleById(Integer.parseInt(roleId));
		} catch (Exception e) {
			e.printStackTrace();
			return ajaxDoneError("操作失败");
		}
		return ajaxDoneSuccess("操作成功");
	}
	
	@RequestMapping("/save")
	public ModelAndView save(SysRole role,Model model, HttpServletRequest request,
			HttpServletResponse response) {
		if(role!=null)
		{
			roleService.saveShop(role);
		}
		return ajaxDoneSuccess("操作成功");
	}
	

}

```

## 07 其它

1、MySQL数据库账户

MySQL数据库默认端口：“3306”、数据库名：“kzfire”、账户名：“root”、密码：空。

2、SQL文件

SQL文件放在“MySQL数据库SQL文件” 目录，需通过“Navicat for MySQL”工具执行此SQL文件。

3、系统启动文件

系统启动文件是“webroot”目录下的“login.jsp”

4、系统登录用户名及密码

“login.jsp”启动（运行）后，正常情况下进入登录界面，用户名输入“admin”,密码输入“123456”。如果登录不进去，很有可能是数据库参数配置问题导致，请检查数据库参数配置文件，数据库参数配置文件放

## 08 源码下载

关注公众号【C you again】，回复“基于SSM框架的JavaWeb通用权限管理系统”免费领取。

亦可直接扫描主页二维码关注，回复“基于SSM框架的JavaWeb通用权限管理系统”免费领取，[点此打开个人主页](https://gzh.cyouagain.cn/)


**说明：此源码来源于网络，若有侵权，请联系删除！！**

