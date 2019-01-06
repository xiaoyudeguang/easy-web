# easy-web

## 介绍

想想一个日常开发的场景：你正在用专注的写代码，好不容器盼到了一个热加载插件，不用重启项目了，结果，为了搞一个业务模块(比如XX管理之类的)，还得停下你天马行空的想法去写一堆包和类。后来，有了各种代码生成器。但是也没好到拿去，你甚至得停止你的程序先去写一堆配置，然后写个启动类生成代码。。。。fuck，劳资不想停止程序的运行，也不想每天重复写那么多Controller，Service，Dao。。。

那么，有没有一种办法，不需要停止正在运行中的程序，改改配置就可以生成代码呢？答案是有的！easy-web，一款专注于释放你的双手的开发利器，相信我，你值得拥有。github项目地址：https://github.com/xiaoyudeguang/easy-web

## 使用说明

#### Maven引用(如果版本有变化，请自行去maven中央仓库引用)
```
 <dependency>
    <groupId>io.github.xiaoyudeguang</groupId>
    <artifactId>easy-web</artifactId>
    <version>3.0.6</version>
 </dependency>
 <dependency>
    <groupId>io.github.xiaoyudeguang</groupId>
    <artifactId>easy-swagger</artifactId>
    <version>3.0.6</version>
 </dependency>
```

#### 引入jdbc驱动包和配置数据源

###### 引入jdbc驱动包和数据库连接池
```
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.0.11</version>
</dependency>
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
</dependency> 
```
###### 配置数据源（在application.properties文件中配置）

```
#datasource config
spring.datasource.type=com.alibaba.druid.pool.DruidDataSource
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
spring.datasource.url=jdbc:mysql://数据库ip地址:3306/数据库名?useUnicode=true&characterEncoding=utf8&useSSL=false
spring.datasource.username=账号
spring.datasource.password=密码
```

#### 使用示例
```
@EasyGenerater(isGenerate = true, todo = { "easy-web代码生成器示例" }, value = "")
public class EasyGenerator extends AbstractGenerator{

	@Override
	public void afterPropertiesSet() throws Exception {
		this.entityName = "Person";
		this.packageName = "com.zlyx.easytest.test";
                ... 其他配置 ...
	}
}

```
好了，到这里，如果你的springboot程序是热加载方式运行中的，选中你的项目刷新一下，应该就可以看到生成的代码了。如果还没启动呢，那就启动一下呗。之后，即使你不删除EasyGenerator类，也不会覆盖你已经生成的代码。

> 小贴士：代码生成前，将@EasyGenerater注解的isGenerate属性设置为false即可避免你的配置还没完成点保存就执行生成代码。同时，生成完成后，此操作还可以在不删除EasyGenerator类的情况下屏蔽对程序启动速度的影响。也就是说，这个类，可以重复使用的！

> easy-web默认引用了easy-swagger(项目地址：https://gitee.com/xiaoyudeguang/easy-swagger)和easy-log（项目地址：https://gitee.com/xiaoyudeguang/easy-log）。
> 
## 生成代码效果：

#### Controller类

```
package com.zlyx.easytest.test.controller;


import com.zlyx.easyswagger.annotations.SpringMapping;
import com.zlyx.easyswagger.annotations.SpringController;
import com.zlyx.easyweb.web.controller.AbstractController;
import com.zlyx.easytest.test.service.PersonService;
import com.zlyx.easytest.test.entity.Person;

/**
 * 前端控制器
 * @author easy-web 1316851612@qq.com
 * @date 2019-01-06
 */
@SpringController(value = {"/person"}, todo = {"前端控制器"})
public class PersonController extends AbstractController<PersonService, Person> {
    
    @SpringMapping(value = {"/demo"}, todo = {"easy-web示例方法"}, notes = "easy-swagger示例方法简介")
    public String demoMethod(){
        return null;
    }
}

```

#### Service接口

```

package com.zlyx.easytest.test.service;

import com.zlyx.easytest.test.entity.Person;
import com.zlyx.easytest.test.mapper.PersonMapper;
import com.zlyx.easyweb.web.service.AbstractService;

/**
 *  服务类
 * @author easy-web 1316851612@qq.com
 * @Date 2019-01-06
 */
public interface PersonService extends AbstractService<PersonMapper,Person> {

}
```

#### Service实现类

```
package com.zlyx.easytest.test.service.impl;

import com.zlyx.easytest.test.entity.Person;
import com.zlyx.easytest.test.mapper.PersonMapper;
import com.zlyx.easytest.test.service.PersonService;
import com.zlyx.easyweb.web.service.impl.AbstractServiceImpl;

import com.zlyx.easyweb.annotations.SpringService;

/**
 *  服务实现类
 * @author easy-web 1316851612@qq.com
 * @since 2019-01-06
 */
@SpringService(todo = "服务实现类")
public class PersonServiceImpl extends AbstractServiceImpl<PersonMapper, Person> implements PersonService {

}

```

#### Mapper接口

```
package com.zlyx.easytest.test.mapper;

import org.apache.ibatis.annotations.Mapper;

import com.zlyx.easytest.test.entity.Person;
import com.zlyx.easyweb.web.mapper.AbstractMapper;

/**
 * Mapper接口
 * @author easy-web 1316851612@qq.com
 * @since 2019-01-06
 */
@Mapper
public interface PersonMapper extends AbstractMapper<Person> {

}

```

#### Entity类

```
package com.zlyx.easytest.test.entity;

import com.baomidou.mybatisplus.annotation.IdType;
import com.baomidou.mybatisplus.annotation.TableId;
import com.baomidou.mybatisplus.annotation.TableName;

import java.time.LocalDateTime;
import java.io.Serializable;

import com.zlyx.easyweb.annotations.TableBean;

/**
 * 
 * @author easy-web 1316851612@qq.com
 * @since 2019-01-06
 */
@TableBean(value = "test_person", todo = {"test_person实体类"})
@TableName("test_person")
public class Person implements Serializable {

    private static final long serialVersionUID = 1L;

    /**
     * 主键
     */
    @TableId(value = "person_id", type = IdType.AUTO)
    private Long personId;
    
    ...其他属性略...
}
```








