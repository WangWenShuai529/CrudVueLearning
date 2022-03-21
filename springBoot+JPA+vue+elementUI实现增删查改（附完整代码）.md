# 运行效果
首先，我们看一下运行截图。
![在这里插入图片描述](https://img-blog.csdnimg.cn/56c6388d499946c4a7ca740948544ee1.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5rGq56iL5bqP54y_,size_20,color_FFFFFF,t_70,g_se,x_16)

![在这里插入图片描述](https://img-blog.csdnimg.cn/1bf2a2b6d2844134a1b347b0858d182a.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5rGq56iL5bqP54y_,size_20,color_FFFFFF,t_70,g_se,x_16)

![在这里插入图片描述](https://img-blog.csdnimg.cn/635b0e72b9774969944622375917f37f.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5rGq56iL5bqP54y_,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/aef10874ef634102855a57974cbf8a0c.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5rGq56iL5bqP54y_,size_20,color_FFFFFF,t_70,g_se,x_16)
***
# 资源的获取
[github](https://github.com/WangWenShuai529/CrudVueLearning/tree/master)
[csdn](https://download.csdn.net/download/m0_61504367/85012767)
# 前端代码编写
![在这里插入图片描述](https://img-blog.csdnimg.cn/1a45f130cc9141b4835ba22f2945e012.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5rGq56iL5bqP54y_,size_20,color_FFFFFF,t_70,g_se,x_16)
前端只有一个页面：index.html
这是element的官网：[element的官网](https://element.eleme.cn/#/zh-CN/component/installation)
![在这里插入图片描述](https://img-blog.csdnimg.cn/dd0aa7f93c0a47809b24dbc528d1f5d4.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5rGq56iL5bqP54y_,size_20,color_FFFFFF,t_70,g_se,x_16)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>用户信息</title>
    <!-- 引入样式 -->
    <link rel="stylesheet" href="element.css">
</head>
<body>
<div id="app" style="width: 80%; margin: 0 auto">
    <h2>用户信息表</h2>
    <el-row>
        <el-col :span="6" style="margin-bottom: 10px">
<!--            -->
            <el-button type="primary" @click="add">新增</el-button>
<!--            按回车，调用loadTable-->
            <el-input v-model="name" style="width: 70%" @keyup.enter.native="loadTable(1)"></el-input>
        </el-col>
    </el-row>

    <el-table
            :data="page.content"
            stripe
            border
            style="width: 100%">
        <el-table-column
                prop="name"
                label="用户名"
        >
        </el-table-column>
        <el-table-column
                prop="age"
                label="年龄"
                width="180">
        </el-table-column>
        <el-table-column
                prop="sex"
                label="性别">
        </el-table-column>
        <el-table-column
                prop="address"
                label="地址">
        </el-table-column>
        <el-table-column
                prop="phone"
                label="电话">
        </el-table-column>
        <el-table-column
                fixed="right"
                label="操作"
                width="100">
            <template slot-scope="scope">
<!--                这是进行编辑,传入本行的数据-->
                <el-button type="primary" icon="el-icon-edit" size="small" circle @click="edit(scope.row)"></el-button>
<!--                这是进行删除-->
                <el-button type="danger" icon="el-icon-delete" size="small" circle @click="del(scope.row.id)"></el-button>
            </template>
        </el-table-column>
    </el-table>
    <el-row type="flex" justify="center" style="margin-top: 10px">
        <el-pagination
                layout="prev, pager, next"
                :page-size="pageSize"
                :current-page="pageNum"
                @prev-click="loadTable"
                @current-change="loadTable"
                @next-click="loadTable"
                :total="page.totalElements">
        </el-pagination>
    </el-row>

    <el-dialog
            title="用户信息"
            :visible.sync="dialogVisible"
            width="30%">
        <el-form ref="form" :model="form" label-width="80px">
            <el-form-item label="用户名">
                <el-input v-model="form.name"></el-input>
            </el-form-item>
            <el-form-item label="年龄">
                <el-input v-model="form.age"></el-input>
            </el-form-item>
            <el-form-item label="性别">
                <el-radio v-model="form.sex" label="男">男</el-radio>
                <el-radio v-model="form.sex" label="女">女</el-radio>
            </el-form-item>
            <el-form-item label="地址">
                <el-input v-model="form.address"></el-input>
            </el-form-item>
            <el-form-item label="电话">
                <el-input v-model="form.phone"></el-input>
            </el-form-item>
        </el-form>
        <span slot="footer" class="dialog-footer">
<!--            -->
            <el-button @click="dialogVisible = false">取 消</el-button>
            <el-button type="primary" @click="save">确 定</el-button>
        </span>
    </el-dialog>

</div>

<script src="jquery.min.js"></script>
<script src="vue.js"></script>
<!-- 引入组件库 -->
<script src="element.js"></script>

<script>
    new Vue({
        el: '#app',
        data: {
            page: {},
            name: '',
            //默认显示1页，这一页为八个数据
            pageNum: 1,
            pageSize: 8,
            //默认不显示
            dialogVisible: false,
            form: {}
        },
        created() {
            //初始化
            this.loadTable(this.pageNum);
        },
        methods: {
            loadTable(num) {
                this.pageNum = num;
                //请求路径和参数
                $.get("/user/page?pageNum=" + this.pageNum + "&pageSize=" + this.pageSize + "&name=" + this.name).then(res => {
                    this.page = res.data;
                });
            },
            add() {
                //显示编辑的界面
                this.dialogVisible = true;
                this.form = {};
            },
            edit(row) {
                this.form = row;
                this.dialogVisible = true;
            },
            save() {
                let data = JSON.stringify(this.form);
                if (this.form.id) {
                    // 编辑
                    $.ajax({
                        url: '/user',
                        type: 'put',
                        contentType: 'application/json',
                        data: data
                    }).then(res => {
                        this.dialogVisible = false;
                        this.loadTable(1);
                    })
                } else {
                    // 新增
                    $.ajax({
                        url: '/user',
                        type: 'post',
                        contentType: 'application/json',
                        data: data
                    }).then(res => {
                        this.dialogVisible = false;
                        this.loadTable(1);
                    })
                }
            },
            del(id) {
                $.ajax({
                    url: '/user/' + id,
                    type: 'delete',
                    contentType: 'application/json'
                }).then(res => {
                    this.loadTable(1);
                })
            }
        }
    })
</script>
</body>
</html>

```

application.yal
```yal
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    username: root
    password: root
    url: jdbc:mysql://localhost:3306/test?useUnicode=true&characterEncoding=utf-8&allowMultiQueries=true&useSSL=false&serverTimezone=GMT%2b8

```

# 后端代码
后端采用JPA
```java
package com.example.service;

import com.example.dao.UserRepository;
import com.example.entity.User;
import org.springframework.data.domain.Page;
import org.springframework.data.domain.PageRequest;
import org.springframework.data.domain.Pageable;
import org.springframework.data.domain.Sort;
import org.springframework.stereotype.Service;

import javax.annotation.Resource;
import java.text.SimpleDateFormat;
import java.util.Date;
import java.util.List;

@Service
public class UserService {

    @Resource
    private UserRepository userRepository;

    public void save(User user) {
        String now = new SimpleDateFormat("yyyy-MM-dd").format(new Date());
        user.setCreateTime(now);
        userRepository.save(user);
    }

    public void delete(Long id) {
        userRepository.deleteById(id);
    }

    public User findById(Long id) {
        return userRepository.findById(id).orElse(null);
    }

    public List<User> findAll() {
        return userRepository.findAll();
    }

    public Page<User> findPage(Integer pageNum, Integer pageSize, String name) {
        // 构建分页查询条件
        Sort sort = new Sort(Sort.Direction.DESC, "create_time");
        Pageable pageRequest = PageRequest.of(pageNum - 1, pageSize, sort);
        return userRepository.findByNameLike(name, pageRequest);
    }
}

```

```java
package com.example.controller;

import com.example.common.Result;
import com.example.entity.User;
import com.example.service.UserService;
import org.springframework.data.domain.Page;
import org.springframework.web.bind.annotation.*;

import javax.annotation.Resource;
import java.util.List;

@RestController
@RequestMapping("/user")
public class UserController {

    @Resource
    private UserService userService;

    // 新增用户
    @PostMapping
    public Result add(@RequestBody User user) {
        userService.save(user);
        return Result.success();
    }

    // 修改用户
    @PutMapping
    public Result update(@RequestBody User user) {
        userService.save(user);
        return Result.success();
    }

    // 删除用户
    @DeleteMapping("/{id}")
    public void delete(@PathVariable("id") Long id) {
        userService.delete(id);
    }

    // 根据id查询用户
    @GetMapping("/{id}")
    public Result<User> findById(@PathVariable Long id) {
        return Result.success(userService.findById(id));
    }

    // 查询所有用户
    @GetMapping
    public Result<List<User>> findAll() {
        return Result.success(userService.findAll());
    }

    // 分页查询用户
    @GetMapping("/page")
    public Result<Page<User>> findPage(@RequestParam(defaultValue = "1") Integer pageNum,
                                       @RequestParam(defaultValue = "10") Integer pageSize,
                                       @RequestParam(required = false) String name) {
        return Result.success(userService.findPage(pageNum, pageSize, name));
    }

}

```
