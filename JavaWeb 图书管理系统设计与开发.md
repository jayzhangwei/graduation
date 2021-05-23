@[toc]

**源码下载：关注公众号【C you again】，回复“JavaWeb 图书管理系统”免费下载。**

## 01 系统简述

图书管理系统就是利用计算机，结合互联网对图书进行结构化、自动化管理的一种软件，来提高对图书的管理效率。

## 02 系统特点

集成主流框架，简单精简化开发，高拓展性

## 03 技术

> springboot + jpa + mybatis + springsecurity +javaex

#### 后端：

 - 基础框架： SpringBoot 
 - 简单数据操作： Spring Data Jpa 
 - 复杂数据操作： Mybatis 
 - 安全框架：SpringSecurity 
 - 模板引擎： Thymeleaf

#### 前端：

 - javaEx, 其实就是对html,css,js的封装。比较接近原生 修改起来比较方便
 -  jQuery , 讲真的jQuery用着还是很舒服, 突破各种前端框架的限制

## 04  运行环境

> jdk1.8 + maven3 + mysql5.7

## 05 功能介绍

#### 图书管理

##### 图书列表：显示已经上架的图书信息，可对上架图书进行搜索、修改、删除操作。
##### 图书上架：录入图书信息，输入图书名称、作者、图书分类，页数，定价等数据进行图书录入。

#### 借阅管理

##### 搜索图书：根据图书名称、作者名称，图书分类等搜索图书。
##### 借阅图书：录入图书信息，输入图书名称、作者、图书分类，页数，定价等数据进行图书借阅。
##### 归还图书：对已经借阅的图书进行归还操作。

####  读者管理

##### 读者列表：显示已经注册的读者用户。
##### 读者添加：录入用户的昵称、用户名、密码、生日、电话、邮箱等信息添加新用户。

#### 用户中心

##### 个人信息：查看、修改个人信息。
##### 用户管理：对已经添加的用户进行搜索、删除、使用权限信息进行设置。
##### 添加管理员：录入管理员的昵称、用户名、密码、生日、电话、邮箱等信息添加新管理员。

## 06 运行截图

#### 登录界面

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200824155218898.png)

#### 首页

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200824161417528.png)

#### 图书列表界面

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200824161454178.png)

#### 添加图书界面

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200824161540493.png)

#### 图书归还界面

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200824161628239.png)

#### 读者列表界面

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200824161701562.png)

#### 个人信息界面

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200824161733917.png)

#### 用户管理界面

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200824161922752.png)

## 07 主要代码

#### 图书管理

```java
package com.book.manager.controller;

import com.book.manager.entity.Book;
import com.book.manager.service.BookService;
import com.book.manager.util.R;
import com.book.manager.util.http.CodeEnum;
import com.book.manager.util.ro.PageIn;
import io.swagger.annotations.Api;
import io.swagger.annotations.ApiOperation;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.*;

/**
 * @Description 用户管理
 */
@Api(tags = "图书管理")
@RestController
@RequestMapping("/book")
public class BookController {

    @Autowired
    private BookService bookService;

    @ApiOperation("图书搜索列表")
    @PostMapping("/list")
    public R getBookList(@RequestBody PageIn pageIn) {
        if (pageIn == null) {
            return R.fail(CodeEnum.PARAM_ERROR);
        }

        return R.success(CodeEnum.SUCCESS,bookService.getBookList(pageIn));
    }

    @ApiOperation("添加图书")
    @PostMapping("/add")
    public R addBook(@RequestBody Book book) {
        return R.success(CodeEnum.SUCCESS,bookService.addBook(book));
    }

    @ApiOperation("编辑图书")
    @PostMapping("/update")
    public R modifyBook(@RequestBody Book book) {
        return R.success(CodeEnum.SUCCESS,bookService.updateBook(book));
    }


    @ApiOperation("图书详情")
    @GetMapping("/detail")
    public R bookDetail(Integer id) {
        return R.success(CodeEnum.SUCCESS,bookService.findBookById(id));
    }

    @ApiOperation("图书详情 根据ISBN获取")
    @GetMapping("/detailByIsbn")
    public R bookDetailByIsbn(String isbn) {
        return R.success(CodeEnum.SUCCESS,bookService.findBookByIsbn(isbn));
    }

    @ApiOperation("删除图书")
    @GetMapping("/delete")
    public R delBook(Integer id) {
        bookService.deleteBook(id);
        return R.success(CodeEnum.SUCCESS);
    }

}

```

#### 借阅管理

```java
package com.book.manager.controller;

import cn.hutool.core.date.DateUtil;
import com.book.manager.entity.Borrow;
import com.book.manager.service.BookService;
import com.book.manager.service.BorrowService;
import com.book.manager.util.R;
import com.book.manager.util.consts.Constants;
import com.book.manager.util.http.CodeEnum;
import com.book.manager.util.ro.RetBookIn;
import com.book.manager.util.vo.BackOut;
import com.book.manager.util.vo.BookOut;
import io.swagger.annotations.Api;
import io.swagger.annotations.ApiOperation;
import org.springframework.beans.BeanUtils;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.*;

import java.util.ArrayList;
import java.util.Date;
import java.util.List;

/**
 * @Description 用户管理
 */
@Api(tags = "借阅管理")
@RestController
@RequestMapping("/borrow")
public class BorrowController {

    @Autowired
    private BorrowService borrowService;

    @Autowired
    private BookService bookService;

    @ApiOperation("借阅列表")
    @GetMapping("/list")
    public R getBorrowList(Integer userId) {
        return R.success(CodeEnum.SUCCESS,borrowService.findAllBorrowByUserId(userId));
    }

    @ApiOperation("借阅图书")
    @PostMapping("/add")
    public R addBorrow(@RequestBody Borrow borrow) {
        Integer result = borrowService.addBorrow(borrow);
        if (result == Constants.BOOK_BORROWED) {
            return R.success(CodeEnum.BOOK_BORROWED);
        }else if (result == Constants.USER_SIZE_NOT_ENOUGH) {
            return R.success(CodeEnum.USER_NOT_ENOUGH);
        }else if (result == Constants.BOOK_SIZE_NOT_ENOUGH) {
            return R.success(CodeEnum.BOOK_NOT_ENOUGH);
        }
        return R.success(CodeEnum.SUCCESS,Constants.OK);
    }

    @ApiOperation("编辑借阅")
    @PostMapping("/update")
    public R modifyBorrow(@RequestBody Borrow borrow) {
        return R.success(CodeEnum.SUCCESS,borrowService.updateBorrow(borrow));
    }


    @ApiOperation("借阅详情")
    @GetMapping("/detail")
    public R borrowDetail(Integer id) {
        return R.success(CodeEnum.SUCCESS,borrowService.findById(id));
    }

    @ApiOperation("删除归还记录")
    @GetMapping("/delete")
    public R delBorrow(Integer id) {
        borrowService.deleteBorrow(id);
        return R.success(CodeEnum.SUCCESS);
    }


    @ApiOperation("已借阅列表")
    @GetMapping("/borrowed")
    public R borrowedList(Integer userId) {
        List<BackOut> outs = new ArrayList<>();
        if (userId!=null&&userId>0) {
            // 获取所有 已借阅 未归还书籍
            List<Borrow> borrows = borrowService.findBorrowsByUserIdAndRet(userId, Constants.NO);
            for (Borrow borrow : borrows) {
                BackOut backOut = new BackOut();
                BookOut out = bookService.findBookById(borrow.getBookId());
                BeanUtils.copyProperties(out,backOut);

                backOut.setBorrowTime(DateUtil.format(borrow.getCreateTime(),Constants.DATE_FORMAT));

                String endTimeStr = DateUtil.format(borrow.getEndTime(), Constants.DATE_FORMAT);
                backOut.setEndTime(endTimeStr);
                // 判断是否逾期
                String toDay = DateUtil.format(new Date(), Constants.DATE_FORMAT);
                int i = toDay.compareTo(endTimeStr);
                if (i>0) {
                    backOut.setLate(Constants.YES_STR);
                }else {
                    backOut.setLate(Constants.NO_STR);
                }

                outs.add(backOut);
            }
        }

        return R.success(CodeEnum.SUCCESS,outs);
    }

    @ApiOperation("归还书籍")
    @PostMapping("/ret")
    public R retBook(Integer userId, Integer bookId) {
        // 归还图书
        borrowService.retBook(userId,bookId);
        return R.success(CodeEnum.SUCCESS);
    }

}

```

用户管理

```java
package com.book.manager.controller;

import cn.hutool.core.bean.BeanUtil;
import cn.hutool.core.date.DateUtil;
import cn.hutool.core.util.StrUtil;
import com.book.manager.entity.Users;
import com.book.manager.service.UserService;
import com.book.manager.util.R;
import com.book.manager.util.consts.Constants;
import com.book.manager.util.consts.ConvertUtil;
import com.book.manager.util.http.CodeEnum;
import com.book.manager.util.vo.PageOut;
import com.book.manager.util.ro.PageIn;
import com.book.manager.util.vo.UserOut;
import com.github.pagehelper.PageInfo;
import io.swagger.annotations.Api;
import io.swagger.annotations.ApiOperation;
import org.springframework.beans.BeanUtils;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.core.context.SecurityContextHolder;
import org.springframework.web.bind.annotation.*;

import java.util.ArrayList;
import java.util.List;
import java.util.Map;

/**
 * @Description 用户管理
 */
@Api(tags = "用户管理")
@RestController
@RequestMapping("/user")
public class UsersController {

    @Autowired
    private UserService userService;

    @ApiOperation("用户列表")
    @PostMapping("/list")
    public R getUsers(@RequestBody PageIn pageIn) {
        if (pageIn == null) {
            return R.fail(CodeEnum.PARAM_ERROR);
        }
        // 封装分页出参对象
        PageInfo<Users> userList = userService.getUserList(pageIn);
        PageOut pageOut = new PageOut();
        pageOut.setCurrPage(userList.getPageNum());
        pageOut.setPageSize(userList.getPageSize());
        pageOut.setTotal((int) userList.getTotal());
        List<UserOut> outs = new ArrayList<>();
        for (Users users : userList.getList()) {
            UserOut out = new UserOut();
            BeanUtils.copyProperties(users,out);
            out.setIdent(ConvertUtil.identStr(users.getIdentity()));
            out.setBirth(DateUtil.format(users.getBirthday(),Constants.DATE_FORMAT));
            outs.add(out);
        }

        pageOut.setList(outs);

        return R.success(CodeEnum.SUCCESS,pageOut);
    }

//    @ApiOperation("添加用户")
//    @PostMapping("/add")
//    public R addUsers(@RequestBody Users users) {
//        return R.success(CodeEnum.SUCCESS,userService.addUser(users));
//    }

    @ApiOperation("添加读者")
    @PostMapping("/addReader")
    public R addReader(@RequestBody Users users) {
        if (users == null) {
            return R.fail(CodeEnum.PARAM_ERROR);
        }
        // 读者默认是普通用户
        users.setIsAdmin(1);
        return R.success(CodeEnum.SUCCESS,userService.addUser(users));
    }

    @ApiOperation("添加管理员")
    @PostMapping("/addAdmin")
    public R addAdmin(@RequestBody Users users) {
        if (users == null) {
            return R.fail(CodeEnum.PARAM_ERROR);
        }
        // 设置管理员权限
        users.setIsAdmin(0);
        return R.success(CodeEnum.SUCCESS,userService.addUser(users));
    }


    @ApiOperation("编辑用户")
    @PostMapping("/update")
    public R modifyUsers(@RequestBody Users users) {
        return R.success(CodeEnum.SUCCESS,userService.updateUser(users));
    }


    @ApiOperation("用户详情")
    @GetMapping("/detail")
    public R userDetail(Integer id) {
        Users user = userService.findUserById(id);
        if (user!=null) {
            UserOut out = new UserOut();
            BeanUtils.copyProperties(user,out);
            out.setBirth(DateUtil.format(user.getBirthday(),Constants.DATE_FORMAT));
            out.setIdent(ConvertUtil.identStr(user.getIdentity()));
            return R.success(CodeEnum.SUCCESS,out);
        }

        return R.fail(CodeEnum.NOT_FOUND);
    }

    @ApiOperation("删除用户")
    @GetMapping("/delete")
    public R delUsers(Integer id) {
        userService.deleteUser(id);
        return R.success(CodeEnum.SUCCESS);
    }

    @ApiOperation("获取当前用户登陆信息")
    @GetMapping("/currUser")
    public R getCurrUser() {
        Object principal = SecurityContextHolder.getContext().getAuthentication().getPrincipal();
        if (principal!=null) {
            Map<String,Object> map = BeanUtil.beanToMap(principal);
            String username = (String) map.get("username");
            if (StrUtil.isNotBlank(username)) {
                Users users = userService.findByUsername(username);
                UserOut out = new UserOut();
                BeanUtils.copyProperties(users,out);
                out.setBirth(DateUtil.format(users.getBirthday(),Constants.DATE_FORMAT));
                Integer identity = users.getIdentity();
                String ident = "";
                if (identity == Constants.STUDENT) {
                    ident = Constants.STU_STR;
                }else if (identity == Constants.TEACHER) {
                    ident = Constants.TEA_STR;
                }else if (identity == Constants.OTHER) {
                    ident = Constants.OTHER_STR;
                }else if (identity == Constants.ADMIN) {
                    ident = Constants.ADMIN_STR;
                }
                out.setIdent(ident);
                return R.success(CodeEnum.SUCCESS,out);
            }
        }
        return R.fail(CodeEnum.USER_NOT_FOUND);
    }
}

```

## 08 使用说明

 1. 本地搭建好java8环境,数据库MySQL5.5+；
 2. 导入sql文件至数据库中，修改数据连接(你自己库名，用户名，密码等)；
 3. 导入项目，配置maven, 等待依赖下载完成；
 4.  安装IDE，打开项目；
 5.  启动访问http://localhost:8080 即可；
 6. 账号：【学生： stu/123】【教师： tea/123】【其他：other/123】【管理员：admin/123】 

## 09 如何导入？

 - idea：直接open打开源码文件夹，记住是pom文件所在的目录 
 - eclipse： 直接导入- 选择已存在导入maven项目
 - 检查maven是否配置好

## 10 源码下载

关注公众号【C you again】，回复“JavaWeb 图书管理系统”免费领取。

亦可直接扫描主页二维码关注，回复“JavaWeb 图书管理系统”免费领取，[点此打开个人主页](https://gzh.cyouagain.cn/)

**说明：此源码来源于网络，若有侵权，请联系删除！！**