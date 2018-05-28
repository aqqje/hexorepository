---
title: Mybaties learn
date: 2018-05-29 07:38:08
tags: mybaties
---
Mybaties learn note

<!-- more -->
## mybaties-HelloWord

- 1. 加入必要 jar 包
 - log4j.jar： 日志包 与 log4j.xml 文件共同使用
 - mybatis-3.4.1.jar：mybaties 必要包
 - mysql-connector-java-5.1.45.jar：mysql 数据库连接包

- 2.创建 mybaties 全局配置文件 mybaties-config.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <!-- 配置数据源 -->
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql:///mybaties"/>
                <property name="username" value="root"/>
                <property name="password" value="root"/>
            </dataSource>
        </environment>
    </environments>
</configuration>
```

- 3.编写 Employee bean 

- 4.创建 EmployyeMapper.xml 文件并注册在 mybaties 全局配置文件中

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--
    namespace:命令空间,指定为接口的全类名
    id: 唯一标识符
    resultType:返回值类型
	#{id}: 从传递过来的参数中取出id值
-->

EmployyeMapper.xml：

<mapper namespace="com.aqqje.mybaties.mapper">
    <select id="selectEmployee" resultType="com.aqqje.mybaties.beans.Employee">
	<!-- 若数据库的字段名与 bean 的属性不一致时请使用别名 -->
	select id, last_name lastName, email, gender from tbl_employee where id = #{id}
	</select>
</mapper>

mybaties-config.xml：
	<!-- 注册 EmployyeMapper对应的 sql 映射  -->
    <mappers>
        <mapper resource="conf/EmployeeMapper.xml"/>
    </mappers>

```
- 5.编写测试类 MybatiesTest

 - a. Mybaties 框架主要是围绕 SqlSessionFactory 这个类进行工作的, 创建 SqlSessionFactory 对象 
 - b. 获取 sqlSession 实例
 - c. 使用sqlSession工厂，获取到sqlSession对象使用他来执行增删改查
 - d. 一个sqlSession就是代表和数据库的一次会话，用完关闭
 
```java
 public SqlSessionFactory getSqlSessionFactory() throws Exception {
        String resource = "conf/mybaties-config.xml";
        InputStream inputStream = Resources.getResourceAsStream(resource);
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        return sqlSessionFactory;
    }
    @Test
    public void testSelectOne() throws Exception  {
        SqlSessionFactory sqlSessionFactory = getSqlSessionFactory();
        SqlSession session = sqlSessionFactory.openSession();
        try {
            Employee employee = session.selectOne("com.aqqje.mybaties.mapper.selectEmployee", 1);
            System.out.println(employee);
        } finally {
            session.close();
        }
    } 
```

## mybaties-HelloWord基础之上实现接口式编程

- 新建一个接口 EmployeeMapper 抽象 getEmpById(Integer id) 方法
- EmployeeMapper.xml 文件中绑定 EmployeeMapper 接口 和 方法
- 使用 sqlSession.getMapper(Class<T> var1) 方法获取 EmployyeMapper 对象
- 调用 getEmpById(Integer id)

```xml

EmployeeMapper.xml:

<mapper namespace="com.aqqje.mybaties.dao.EmployeeMapper">
    <select id="getEmpById" resultType="com.aqqje.mybaties.beans.Employee">
select id, last_name lastName, email, gender from tbl_employee where id = #{id}
</select>
</mapper>

MybatiesTest.java:
 //获取接口的实现类对象
 //会为接口自动的创建一个代理对象，代理对象去执行增删改查方法
 EmployeeMapper employeeMapper = sqlSession.getMapper(EmployeeMapper.class);
 Employee employee = employeeMapper.getEmpById(1);
 System.out.println(employee);

```

## properties标签

- mybatis可以使用properties来引入外部properties配置文件的内容；
	resource：引入类路径下的资源
	url：引入网络路径或者磁盘路径下的资源


## settings 标签

设置|描述|有效值|默认
-|-|-|-
mapUnderscoreToCamelCase| 可以所数据库中以 last_name 命令方式转化为 lastName 的驼峰形式| false, true| false


## typeAliases(类型别名)

别名处理器：可以为我们的java类型起别名,别名不区分大小写

1.typeAlias:为某个java类型起别名
 - type:指定类型,默认别名为类名小写：employee 
 - alias:指定新的别名

2.package:为某个包下的所有类批量起别名 
 - name：指定包名（为当前包以及下面所有的后代包的每一个类都起一个默认别名（类名小写），）

3. 批量起别名的情况下，使用@Alias注解为某个类型指定新的别名

4. mybaties 中对于常见Java类型有许多内置类型别名

- 它们都是大小写不敏感的，注意由于重载名称而对基元的特殊处理。
- 8大基础类型, 在类型前加 "_": alias -> _byte, _long, int, ...... 
- 引用类, 类型转小写： alias -> string , byte, date, .....

## typeHandlers 类型处理器

- Mybaties 3.4以前的版本需要我们手动注册这些处理, 以后的版本都是自动注册的

```xml 
	<typeHandlers>
        <typeHandler handler="org.apache.ibatis.type.IntegerTypeHandler"/>
    </typeHandlers>
```

## plugins(插件)

- Execotor
- ParamterHandler
- ResultSetHandler
- StatementHandler

## environments：环境们，mybatis可以配置多种环境 ,default指定使用某种环境。可以达到快速切换环境。
	environment：配置一个具体的环境信息；必须有两个标签；id代表当前环境的唯一标识
		transactionManager：事务管理器；
			type：事务管理器的类型;JDBC(JdbcTransactionFactory)|MANAGED(ManagedTransactionFactory)
				自定义事务管理器：实现TransactionFactory接口.type指定为全类名
		
		dataSource：数据源;
			type:数据源类型;UNPOOLED(UnpooledDataSourceFactory)
						|POOLED(PooledDataSourceFactory)
						|JNDI(JndiDataSourceFactory)
			自定义数据源：实现DataSourceFactory接口，type是全类名

## databaseIdProvider 支持多数据库厂商的

- type="DB_VENDOR"：VendorDatabaseIdProvider
	作用就是得到数据库厂商的标识(驱动getDatabaseProductName())，mybatis就能根据数据库厂商标识来执行不同的sql;
	MySQL，Oracle，SQL Server,xxxx
- 使用： mybaties-config <environments default="dev_mysql"> 切换要配置环境  && sql映射文件中添加 databaseId="mysql" 标识使用数据库类型
- 

	
```xml

mybaties-config.xml:

<databaseIdProvider type="DB_VENDOR">
		<!-- 为不同的数据库厂商起别名 -->
		<property name="MySQL" value="mysql"/>
		<property name="Oracle" value="oracle"/>
		<property name="SQL Server" value="sqlserver"/>
	</databaseIdProvider>
	

EmployeeMapper.xml:
	
<select id="getEmpById" resultType="com.atguigu.mybatis.bean.Employee"
		databaseId="mysql">
		select * from tbl_employee where id = #{id}
	</select>
```

## mappers

- mapper:注册一个sql映射 
	注册配置文件
	resource：引用类路径下的sql映射文件
		mybatis/mapper/EmployeeMapper.xml
	url：引用网路路径或者磁盘路径下的sql映射文件
		file:///var/mappers/AuthorMapper.xml

- 注册接口
	class：引用（注册）接口，
		1、有sql映射文件，映射文件名必须和接口同名，并且放在与接口同一目录下；
		2、没有sql映射文件，所有的sql都是利用注解写在接口上;
		推荐：
			比较重要的，复杂的Dao接口我们来写sql映射文件
			不重要，简单的Dao接口为了开发快速可以使用注解；

- 批量注册： <package name="com.aqqje.dao"/>
			
## 简单的 CRUD

- 1、mybatis允许增删改直接定义以下类型返回值
	Integer、Long、Boolean、void
- 2、我们需要手动提交数据
    sqlSessionFactory.openSession();===》手动提交
	sqlSessionFactory.openSession(true);===》自动提交
- 3.mapper.xml文件中 parameterType 可以省略 	

```xml
	<select id="getEmpById" resultType="com.aqqje.mybaties.beans.Employee">
      select * from tbl_employee where id = #{id}
    </select>
    <insert id="insertEmp" parameterType="com.aqqje.mybaties.beans.Employee">
        insert into tbl_employee(last_name, gender, email) values(#{lastName}, #{gender}, #{email})
    </insert>
    <update id="updateEmpById" >
        update tbl_employee set last_name = #{lastName}, gender = #{gender}, email = #{email} where id = #{id}
    </update>
    <delete id="deletEmpById">
        delete from tbl_employee where id = #{id}
    </delete>
```

## 参数处理

```

单个参数：mybatis不会做特殊处理，
	#{参数名/任意名}：取出参数值。
	
多个参数：mybatis会做特殊处理。
	多个参数会被封装成 一个map，
		key：param1...paramN,或者参数的索引也可以
		value：传入的参数值
	#{}就是从map中获取指定的key的值；
	
	异常：
	org.apache.ibatis.binding.BindingException: 
	Parameter 'id' not found. 
	Available parameters are [1, 0, param1, param2]
	操作：
		方法：public Employee getEmpByIdAndLastName(Integer id,String lastName);
		取值：#{id},#{lastName}

【命名参数】：明确指定封装参数时map的key；@Param("id")
	多个参数会被封装成 一个map，
		key：使用@Param注解指定的值
		value：参数值
	#{指定的key}取出对应的参数值


POJO：
如果多个参数正好是我们业务逻辑的数据模型，我们就可以直接传入pojo；
	#{属性名}：取出传入的pojo的属性值	

Map：
如果多个参数不是业务模型中的数据，没有对应的pojo，不经常使用，为了方便，我们也可以传入map
	#{key}：取出map中对应的值

TO：
如果多个参数不是业务模型中的数据，但是经常要使用，推荐来编写一个TO（Transfer Object）数据传输对象
Page{
	int index;
	int size;
}

========================思考================================	
public Employee getEmp(@Param("id")Integer id,String lastName);
	取值：id==>#{id/param1}   lastName==>#{param2}

public Employee getEmp(Integer id,@Param("e")Employee emp);
	取值：id==>#{param1}    lastName===>#{param2.lastName/e.lastName}

##特别注意：如果是Collection（List、Set）类型或者是数组，
		 也会特殊处理。也是把传入的list或者数组封装在map中。
			key：Collection（collection）,如果是List还可以使用这个key(list)
				数组(array)
public Employee getEmpById(List<Integer> ids);
	取值：取出第一个id的值：   #{list[0]}
	
========================结合源码，mybatis怎么处理参数==========================
总结：参数多时会封装map，为了不混乱，我们可以使用@Param来指定封装时使用的key；
#{key}就可以取出map中的值；

(@Param("id")Integer id,@Param("lastName")String lastName);
ParamNameResolver解析参数封装map的；
//1、names：{0=id, 1=lastName}；构造器的时候就确定好了

	确定流程：
	1.获取每个标了param注解的参数的@Param的值：id，lastName；  赋值给name;
	2.每次解析一个参数给map中保存信息：（key：参数索引，value：name的值）
		name的值：
			标注了param注解：注解的值
			没有标注：
				1.全局配置：useActualParamName（jdk1.8）：name=参数名
				2.name=map.size()；相当于当前元素的索引
	{0=id, 1=lastName,2=2}
				

args【1，"Tom",'hello'】:

public Object getNamedParams(Object[] args) {
    final int paramCount = names.size();
    //1、参数为null直接返回
    if (args == null || paramCount == 0) {
      return null;
     
    //2、如果只有一个元素，并且没有Param注解；args[0]：单个参数直接返回
    } else if (!hasParamAnnotation && paramCount == 1) {
      return args[names.firstKey()];
      
    //3、多个元素或者有Param标注
    } else {
      final Map<String, Object> param = new ParamMap<Object>();
      int i = 0;
      
      //4、遍历names集合；{0=id, 1=lastName,2=2}
      for (Map.Entry<Integer, String> entry : names.entrySet()) {
      
      	//names集合的value作为key;  names集合的key又作为取值的参考args[0]:args【1，"Tom"】:
      	//eg:{id=args[0]:1,lastName=args[1]:Tom,2=args[2]}
        param.put(entry.getValue(), args[entry.getKey()]);
        
        
        // add generic param names (param1, param2, ...)param
        //额外的将每一个参数也保存到map中，使用新的key：param1...paramN
        //效果：有Param注解可以#{指定的key}，或者#{param1}
        final String genericParamName = GENERIC_NAME_PREFIX + String.valueOf(i + 1);
        // ensure not to overwrite parameter named with @Param
        if (!names.containsValue(genericParamName)) {
          param.put(genericParamName, args[entry.getKey()]);
        }
        i++;
      }
      return param;
    }
  }
}
===========================参数值的获取======================================
#{}：可以获取map中的值或者pojo对象属性的值；
${}：可以获取map中的值或者pojo对象属性的值；


select * from tbl_employee where id=${id} and last_name=#{lastName}
Preparing: select * from tbl_employee where id=2 and last_name=?
	区别：
		#{}:是以预编译的形式，将参数设置到sql语句中；PreparedStatement；防止sql注入
		${}:取出的值直接拼装在sql语句中；会有安全问题；
		大多情况下，我们去参数的值都应该去使用#{}；
		
		原生jdbc不支持占位符的地方我们就可以使用${}进行取值
		比如分表、排序。。。；按照年份分表拆分
			select * from ${year}_salary where xxx;
			select * from tbl_employee order by ${f_name} ${order}

#{}:更丰富的用法：
	规定参数的一些规则：
	javaType、 jdbcType、 mode（存储过程）、 numericScale、
	resultMap、 typeHandler、 jdbcTypeName、 expression（未来准备支持的功能）；

	jdbcType通常需要在某种特定的条件下被设置：
		在我们数据为null的时候，有些数据库可能不能识别mybatis对null的默认处理。比如Oracle（报错）；
		
		JdbcType OTHER：无效的类型；因为mybatis对所有的null都映射的是原生Jdbc的OTHER类型，oracle不能正确处理;
		
		由于全局配置中：jdbcTypeForNull=OTHER；oracle不支持；两种办法
		1、#{email,jdbcType=OTHER};
		2、jdbcTypeForNull=NULL
			<setting name="jdbcTypeForNull" value="NULL"/>
			
			
```

## 获取自增主键的值：
	mysql支持自增主键，自增主键值的获取，mybatis也是利用statement.getGenreatedKeys()；
	useGeneratedKeys="true"；使用自增主键获取主键值策略
	keyProperty；指定对应的主键属性，也就是mybatis获取到主键值以后，将这个值封装给javaBean的哪个属性
	
```xml
	<insert id="insertEmp" parameterType="com.aqqje.mybaties.beans.Employee" useGeneratedKeys="true" keyProperty="id">
        insert into tbl_employee(last_name, gender, email) values(#{lastName}, #{gender}, #{email})
    </insert>
```


 
## 获取非自增主键的值：

	Oracle不支持自增；Oracle使用序列来模拟自增；
	每次插入的数据的主键是从序列中拿到的值；如何获取到这个值；
		keyProperty:查出的主键值封装给javaBean的哪个属性
		order="BEFORE":当前sql在插入sql之前运行
			   AFTER：当前sql在插入sql之后运行
		resultType:查出的数据的返回值类型
		BEFORE运行顺序：
			先运行selectKey查询id的sql；查出id值封装给javaBean的id属性
			在运行插入的sql；就可以取出id属性对应的值
		AFTER运行顺序：
			先运行插入的sql（从序列中取出新值作为id）；
			再运行selectKey查询id的sql；

```xml

	<insert id="addEmp" databaseId="oracle">
		<selectKey keyProperty="id" order="BEFORE" resultType="Integer">
			<!-- BEFORE-->
			select EMPLOYEES_SEQ.nextval from dual 
			<!-- AFTER：
			 select EMPLOYEES_SEQ.currval from dual -->
		</selectKey>
		<!-- BEFORE:-->
		insert into employees(EMPLOYEE_ID,LAST_NAME,EMAIL) 
		values(#{id},#{lastName},#{email<!-- ,jdbcType=NULL -->}) 
		<!-- AFTER：
		insert into employees(EMPLOYEE_ID,LAST_NAME,EMAIL) 
		values(employees_seq.nextval,#{lastName},#{email}) -->
	</insert>
```

## resultMap:自定义结果集映射规则

```xml
	<!--自定义某个javaBean的封装规则
	type：自定义规则的Java类型
	id:唯一id方便引用
	  -->
	<resultMap id="myMap" type="com.aqqje.mybaties.beans.Employee">		
		<!--指定主键列的封装规则
		id定义主键会底层有优化；
		column：指定哪一列
		property：指定对应的javaBean属性
		  -->
		<id column="id" property="id"/>
		<!-- 定义普通列封装规则 -->
        <result column="last_name" property="lastName"/>
		<!-- 其他不指定的列会自动封装：我们只要写resultMap就把全部的映射规则都写上。 -->
        <result column="email" property="email"/>
        <result column="gender" property="gender"/>
    </resultMap>
	
	<!--
		联合查询：
		1.支持级联属性封装对象 
		2.使用 association
	-->
	
	<resultMap id="mydifMap" type="com.aqqje.mybaties.beans.Employee">
        <id column="id" property="id"/>
        <result column="last_name" property="lastName"/>
        <result column="email" property="email"/>
        <result column="gender" property="gender"/>
        <!--<result column="did" property="department.id"/>
        <result column="name" property="department.name"/>-->
        <association property="department" javaType="com.aqqje.mybaties.beans.Department">
            <id column="did" property="id"/>
            <result column="name" property="name"/>
        </association>
	<!--
		使用association进行分步查询：
		1、先按照员工id查询员工信息
		2、根据查询员工信息中的d_id值去部门表查出部门信息
		3、部门设置到员工中；
	-->
	
	<resultMap id="mydifMap" type="com.aqqje.mybaties.beans.Employee">
        <id column="id" property="id"/>
        <result column="last_name" property="lastName"/>
        <result column="email" property="email"/>
        <result column="gender" property="gender"/>
        
		<!-- association定义关联对象的封装规则
	 		select:表明当前属性是调用select指定的方法查出的结果
	 		column:指定将哪一列的值传给这个方法
	 		
	 		流程：使用select指定的方法（传入column指定的这列参数的值）查出对象，并封装给property指定的属性
	 	 -->
        
		<association property="department"
                     select="com.aqqje.mybaties.dao.DepartmentMapper.getDeptById"
                     column="did"/>
    </resultMap>
```

## 分步延迟加载设置

```xml
	<!--显示的指定每个我们需要更改的配置的值，即使他是默认的。防止版本更新带来的问题  -->
		<!-- 开启加载 -->
		<setting name="lazyLoadingEnabled" value="true"/>
		<!-- 关闭全加载 -->
		<setting name="aggressiveLazyLoading" value="false"/>
```

## collection标签定义关联的集合类型


```xml
<!--嵌套结果集的方式，使用collection标签定义关联的集合类型的属性封装规则  -->
<resultMap id="deptListMap" type="com.aqqje.mybaties.beans.Department">
        <id column="deptid" property="id"/>
        <result column="deptname" property="name"/>
		<!-- 
			collection定义关联集合类型的属性的封装规则 
			ofType:指定集合里面元素的类型
		-->
        <collection property="employeeList" ofType="com.aqqje.mybaties.beans.Employee">
            <!-- 定义这个集合中元素的封装规则 -->
			<id column="empid" property="id"/>
            <result column="empname" property="lastName"/>
            <result column="email" property="email"/>
            <result column="gender" property="gender"/>
        </collection>
    </resultMap>
    <select id="getDeptByIdPlus" resultMap="deptListMap">
        SELECT
		 d.id deptid, d.id deptname,
		 e.id empid, e.last_name empname, e.email, e.gender
		 FROM tbl_dept d
		 LEFT JOIN tbl_employee e
		 ON e.dept_id = #{id}
    </select>
	
	
	<!-- 扩展：多列的值传递过去：
			将多列的值封装map传递；
			column="{key1=column1,key2=column2}"
		fetchType="lazy"：表示使用延迟加载；
				- lazy：延迟
				- eager：立即
	 -->
	 
	<collection property="emps" 
			select="com.atguigu.mybatis.dao.EmployeeMapperPlus.getEmpsByDeptId"
			column="{deptId=id}" fetchType="lazy"></collection>
```

## 鉴别器 

mybatis可以使用discriminator判断某列的值，然后根据某列的值改变封装行为


```xml
<!--
封装Employee：
			如果查出的是女生：就把部门信息查询出来，否则不查询；
			如果是男生，把last_name这一列的值赋值给email; -->

			<resultMap id="mydifMap" type="com.aqqje.mybaties.beans.Employee">
        <id column="id" property="id"/>
        <result column="last_name" property="lastName"/>
        <result column="email" property="email"/>
        <result column="gender" property="gender"/>
		<!--
	 		column：指定判定的列名
	 		javaType：列值对应的java类型  -->
        <discriminator column="gender" javaType="string" >
		<!--女生  resultType:指定封装的结果类型；不能缺少。/resultMap-->
            <case value="0" resultType="com.aqqje.mybaties.beans.Employee">
                <association property="department"
                             select="com.aqqje.mybaties.dao.DepartmentMapper.getDeptById"
                             column="did"/>
            </case>
			<!--男生 ;如果是男生，把last_name这一列的值赋值给email; -->
            <case value="1" resultType="com.aqqje.mybaties.beans.Employee">
                <id column="id" property="id"/>
                <result column="last_name" property="lastName"/>
                <result column="last_name" property="email"/>
                <result column="gender" property="gender"/>
                <association property="department"
                             select="com.aqqje.mybaties.dao.DepartmentMapper.getDeptById"
                             column="did"/>
            </case>
        </discriminator>
    </resultMap>
```

## mybaties 动态sqL

• if:判断
• choose (when, otherwise):分支选择；带了break的swtich-case
	如果带了id就用id查，如果带了lastName就用lastName查;只会进入其中一个
• trim 字符串截取(where(封装查询条件), set(封装修改条件))
• foreach 遍历集合


- if:判断
```xml
<!-- 查询员工，要求，携带了哪个字段查询条件就带上这个字段的值 -->
<select id="getEmpList" resultMap="mydifMap">
        SELECT * FROM tbl_employee
        <where>
		<!-- test：判断表达式（OGNL）
		 	OGNL参照PPT或者官方文档。
		 	  	 c:if  test
		 	从参数中取值进行判断
		 	
		 	遇见特殊符号应该去写转义字符：
		 	&&：
		 	-->
            <if test="id!=null">
              id=#{id}
            </if>
            <if test="lastName !=null and lastName!=''">
                and last_name=#{lastName}
            </if>
            <if test="email!=null and email.trim()!=''">
                and email=#{email}
            </if>
			<!-- ognl会进行字符串与数字的转换判断  "0"==0 -->
            <if test="gender == 0 or gender ==1">
                and gender=#{gender}
            </if>
        </where>
    </select>
	
	<!-- 查询的时候如果某些条件没带可能sql拼装会有问题
			1、给where后面加上1=1，以后的条件都and xxx.
			2、mybatis使用where标签来将所有的查询条件包括在内。mybatis就会将where标签中拼装的sql，多出来的and或者or去掉
				where只会去掉第一个多出来的and或者or。-->
```

- trim 

```xml
	<!-- 后面多出的and或者or where标签不能解决 
	 	prefix="":前缀：trim标签体中是整个字符串拼串 后的结果。
	 			prefix给拼串后的整个字符串加一个前缀 
	 	prefixOverrides="":
	 			前缀覆盖： 去掉整个字符串前面多余的字符
	 	suffix="":后缀
	 			suffix给拼串后的整个字符串加一个后缀 
	 	suffixOverrides=""
				后缀覆盖：去掉整个字符串后面多余的字符
    -->

	<select id="getEmpList" resultMap="mydifMap">
        SELECT * FROM tbl_employee
        <trim prefix="where" suffixOverrides="and">
            <if test="id!=null">
              id=#{id}
            </if>
            <if test="lastName !=null and lastName!=''">
                and last_name=#{lastName}
            </if>
            <if test="email!=null and email.trim()!=''">
                and email=#{email}
            </if>
            <if test="gender == 0 or gender ==1">
                and gender=#{gender}
            </if>
        </trim>
    </select>
```

- choose

```xml
	<select id="getEmpList" resultMap="mydifMap">
	 	select * from tbl_employee 
	 	<where>
	 		<!-- 如果带了id就用id查，如果带了lastName就用lastName查;只会进入其中一个 -->
	 		<choose>
	 			<when test="id!=null">
	 				id=#{id}
	 			</when>
	 			<when test="lastName!=null">
	 				last_name like #{lastName}
	 			</when>
	 			<when test="email!=null">
	 				email = #{email}
	 			</when>
	 			<otherwise>
	 				gender = 0
	 			</otherwise>
	 		</choose>
	 	</where>
	 </select>
```

- Set标签的使用

```xml
	<update id="updateEmpSet" >
        update tbl_employee
        <set>
            <if test="lastName !=null and = lastName != ''">
                last_name=#{lastName},
            </if>
            <if test="email!=null and email.trim()!=''">
                email=#{email},
            </if>
            <if test="gender == 0 or gender ==1">
                gender=#{gender}
            </if>
        </set>
        <where>
            <if test="id!=null">
                id=#{id}
            </if>
        </where>
    </update>
```


- foreach

```xml

	<!--
		collection：指定要遍历的集合：
			list类型的参数会特殊处理封装在map中，map的key就叫list
		item：将当前遍历出的元素赋值给指定的变量
		separator:每个元素之间的分隔符
		open：遍历出所有结果拼接一个开始的字符
		close:遍历出所有结果拼接一个结束的字符
		index:索引。遍历list的时候是index就是索引，item就是当前值
					  遍历map的时候index表示的就是map的key，item就是map的值
		
		#{变量名}就能取出变量的值也就是当前遍历出的元素
	-->
	<select id="getEmpForEach" resultMap="mydifMap">
        select id, last_name, email, gender, dept_id did from tbl_employee where id in
        <foreach collection="ids" separator="," item="emp_id" open="(" close=")">
            #{emp_id}
        </foreach>
    </select>
```

- 批量新增

```xml
	<!-- 批量保存 -->
	 <!--public void addEmps(@Param("emps")List<Employee> emps);  -->
	 <!--MySQL下批量保存：可以foreach遍历   mysql支持values(),(),()语法-->
	<!--<insert id="insertList">
        INSERT INTO
	tbl_employee (last_name, gender, email, dept_id)
	VALUES
	<foreach collection="emps" item="emps" separator=",">
        (#{emps.lastName} ,#{emps.gender}, #{emps.email}, #{emps.department.id})
    </foreach>
    </insert>-->
    <insert id="insertList">

        <foreach collection="emps" item="emps" separator=";">
            INSERT INTO
            tbl_employee (last_name, gender, email, dept_id)
            VALUES
            (#{emps.lastName} ,#{emps.gender}, #{emps.email}, #{emps.department.id})
        </foreach>
    </insert>
	
	<!-- 这种方式需要数据库连接属性allowMultiQueries=true；
	 	这种分号分隔多个sql可以用于其他的批量操作（删除，修改） -->
	 <!-- <insert id="addEmps">
	 	<foreach collection="emps" item="emp" separator=";">
	 		insert into tbl_employee(last_name,email,gender,d_id)
	 		values(#{emp.lastName},#{emp.email},#{emp.gender},#{emp.dept.id})
	 	</foreach>
	 </insert> -->
	 
	 <!-- Oracle数据库批量保存： 
	 	Oracle不支持values(),(),()
	 	Oracle支持的批量方式
	 	1、多个insert放在begin - end里面
	 		begin
			    insert into employees(employee_id,last_name,email) 
			    values(employees_seq.nextval,'test_001','test_001@atguigu.com');
			    insert into employees(employee_id,last_name,email) 
			    values(employees_seq.nextval,'test_002','test_002@atguigu.com');
			end;
		2、利用中间表：
			insert into employees(employee_id,last_name,email)
		       select employees_seq.nextval,lastName,email from(
		              select 'test_a_01' lastName,'test_a_e01' email from dual
		              union
		              select 'test_a_02' lastName,'test_a_e02' email from dual
		              union
		              select 'test_a_03' lastName,'test_a_e03' email from dual
		       )	
	 -->
	 
	 	 <insert id="addEmps" databaseId="oracle">
	 	<!-- oracle第一种批量方式 -->
	 	<!-- <foreach collection="emps" item="emp" open="begin" close="end;">
	 		insert into employees(employee_id,last_name,email) 
			    values(employees_seq.nextval,#{emp.lastName},#{emp.email});
	 	</foreach> -->
	 	
	 	<!-- oracle第二种批量方式  -->
	 	insert into employees(
	 		<!-- 引用外部定义的sql -->
	 		<include refid="insertColumn">
	 			<property name="testColomn" value="abc"/>
	 		</include>
	 	)
	 			<foreach collection="emps" item="emp" separator="union"
	 				open="select employees_seq.nextval,lastName,email from("
	 				close=")">
	 				select #{emp.lastName} lastName,#{emp.email} email from dual
	 			</foreach>
	 </insert>
```

## _parameter ||  _databaseId || bind 

```xml

	<!-- 两个内置参数：
	 	不只是方法传递过来的参数可以被用来判断，取值。。。
	 	mybatis默认还有两个内置参数：
	 	_parameter:代表整个参数
	 		单个参数：_parameter就是这个参数
	 		多个参数：参数会被封装为一个map；_parameter就是代表这个map
	 	
	 	_databaseId:如果配置了databaseIdProvider标签。
	 		_databaseId就是代表当前数据库的别名oracle
	-->
	select id="getEmpdatabaseId" resultMap="mydifMap">
		<!-- bind：可以将OGNL表达式的值绑定到一个变量中，方便后来引用这个变量的值 -->
		<!-- <bind name="_lastName" value="'%'+lastName+'%'"/> -->
        <if test="_databaseId == 'mysql'">
            select id, last_name, email, gender, dept_id did from tbl_employee
            <if test="_parameter!=null">
                where id = #{_parameter.id}
				<!-- where last_name = #{_lastName} ->
            </if>
        </if>
        <if test="_databaseId == 'oracle'">
            select id, last_name, email, gender, dept_id did from tbl_employee
            <if test="_parameter!=null">
                where id = #{_parameter.id}
            </if>
        </if>
    </select>
```

- sql 标签

```xml
抽取可重用的sql片段。方便后面引用 
	1、sql抽取：经常将要查询的列名，或者插入用的列名抽取出来方便引用
	2、include来引用已经抽取的sql：
	3、include还可以自定义一些property，sql标签内部就能使用自定义的属性
			include-property：取值的正确方式${prop},
			#{不能使用这种方式}
			
	<sql id="selectColumn">
        <if test="_databaseId == 'mysql'">
            id, last_name, email, gender,${dept_id}
        </if>
    </sql>
	
	<select id="getEmpdatabaseId" resultMap="mydifMap">
        <if test="_databaseId == 'mysql'">
            select dept_id did
            <include refid="selectColumn">
                <property name="dept_id" value="did"/>
            </include>
            from tbl_employee
		</if>
	</select>
```

## mybaties 缓存机制

 两级缓存：
 一级缓存：（本地缓存）：sqlSession级别的缓存。一级缓存是一直开启的；SqlSession级别的一个Map
		与数据库同一次会话期间查询到的数据会放在本地缓存中。
		以后如果需要获取相同的数据，直接从缓存中拿，没必要再去查询数据库；
 
		一级缓存失效情况（没有使用到当前一级缓存的情况，效果就是，还需要再向数据库发出查询）：
		1、sqlSession不同。
		2、sqlSession相同，查询条件不同.(当前一级缓存中还没有这个数据)
		3、sqlSession相同，两次查询之间执行了增删改操作(这次增删改可能对当前数据有影响)
		4、sqlSession相同，手动清除了一级缓存（缓存清空）
 
   二级缓存：（全局缓存）：基于namespace级别的缓存：一个namespace对应一个二级缓存：
		工作机制：
		1、一个会话，查询一条数据，这个数据就会被放在当前会话的一级缓存中；
		2、如果会话关闭；一级缓存中的数据会被保存到二级缓存中；新的会话查询信息，就可以参照二级缓存中的内容；
		3、sqlSession===EmployeeMapper==>Employee
						DepartmentMapper===>Department
			不同namespace查出的数据会放在自己对应的缓存中（map）
			效果：数据会从二级缓存中获取
				查出的数据都会被默认先放在一级缓存中。
				只有会话提交或者关闭以后，一级缓存中的数据才会转移到二级缓存中
		使用：
			1）、开启全局二级缓存配置：<setting name="cacheEnabled" value="true"/>
			2）、去mapper.xml中配置使用二级缓存：
				<cache></cache>
			3）、我们的POJO需要实现序列化接口
	和缓存有关的设置/属性：
		1）、cacheEnabled=true：false：关闭缓存（二级缓存关闭）(一级缓存一直可用的)
		2）、每个select标签都有useCache="true"：
				false：不使用缓存（一级缓存依然使用，二级缓存不使用）
		3）、【每个增删改标签的：flushCache="true"：（一级二级都会清除）】
				增删改执行完成后就会清楚缓存；
				测试：flushCache="true"：一级缓存就清空了；二级也会被清除；
				查询标签：flushCache="false"：
					如果flushCache=true;每次查询之后都会清空缓存；缓存是没有被使用的；
		4）、sqlSession.clearCache();只是清楚当前session的一级缓存；
		5）、localCacheScope：本地缓存作用域：（一级缓存SESSION）；当前会话的所有数据保存在会话缓存中；
							STATEMENT：可以禁用一级缓存；	
							
	第三方缓存整合：
	 		1）、导入第三方缓存包即可； mybatis-ehcache-1.0.3.jar, slf4j-api-1.6.1.jar, slf4j-log4j12-1.6.2.jar   
	 		2）、导入与第三方缓存整合的适配包；官方有；
	 		3）、mapper.xml中使用自定义缓存
	 		<cache type="org.mybatis.caches.ehcache.EhcacheCache"></cache>
			
```xml

	<!-- <cache eviction="FIFO" flushInterval="60000" readOnly="false" size="1024"></cache> -->
	<!--  
	eviction:缓存的回收策略：
		• LRU – 最近最少使用的：移除最长时间不被使用的对象。
		• FIFO – 先进先出：按对象进入缓存的顺序来移除它们。
		• SOFT – 软引用：移除基于垃圾回收器状态和软引用规则的对象。
		• WEAK – 弱引用：更积极地移除基于垃圾收集器状态和弱引用规则的对象。
		• 默认的是 LRU。
	flushInterval：缓存刷新间隔
		缓存多长时间清空一次，默认不清空，设置一个毫秒值
	readOnly:是否只读：
		true：只读；mybatis认为所有从缓存中获取数据的操作都是只读操作，不会修改数据。
				 mybatis为了加快获取速度，直接就会将数据在缓存中的引用交给用户。不安全，速度快
		false：非只读：mybatis觉得获取的数据可能会被修改。
				mybatis会利用序列化&反序列的技术克隆一份新的数据给你。安全，速度慢
	size：缓存存放多少元素；
	type=""：指定自定义缓存的全类名；
			实现Cache接口即可；
	-->
```
	
## ssm 整合：

web 配置：
		1.spring mvc
		2.spring IoC

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://java.sun.com/xml/ns/javaee" xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd" id="WebApp_ID" version="2.5">
  <display-name>MyBatis_06_ssm</display-name>
  
  <!--Spring配置 -->
	<context-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>classpath:applicationContext.xml</param-value>
	</context-param>

	<listener>
		<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
	</listener>
	
	<!-- SpringMVC配置 -->
	<servlet>
		<servlet-name>spring</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
		<load-on-startup>1</load-on-startup>
	</servlet>

	<servlet-mapping>
		<servlet-name>spring</servlet-name>
		<url-pattern>/</url-pattern>
	</servlet-mapping>
  
</web-app>

```
		
springmvc 配置：
		1. 扫描控制器
		2. 视图解析器
		
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:mvc="http://www.springframework.org/schema/mvc"
	xsi:schemaLocation="http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc-4.0.xsd
		http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.0.xsd">

	<!--SpringMVC只是控制网站跳转逻辑  -->
	<!-- 只扫描控制器 -->
	<context:component-scan base-package="com.atguigu.mybatis" use-default-filters="false">
		<context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
	</context:component-scan>
	
	<!-- 视图解析器 -->
	<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<property name="prefix" value="/WEB-INF/pages/"></property>
		<property name="suffix" value=".jsp"></property>
	</bean>
	
	<mvc:annotation-driven></mvc:annotation-driven>
	<mvc:default-servlet-handler/>
</beans>

```

mybaties 配置：

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
 PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
 "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
	<settings>
		<setting name="mapUnderscoreToCamelCase" value="true"/>
		<setting name="jdbcTypeForNull" value="NULL"/>
		
		<!--显式的指定每个我们需要更改的配置的值，即使他是默认的。防止版本更新带来的问题  -->
		<setting name="cacheEnabled" value="true"/>
		<setting name="lazyLoadingEnabled" value="true"/>
		<setting name="aggressiveLazyLoading" value="false"/>
	</settings>
	
	<databaseIdProvider type="DB_VENDOR">
		<property name="MySQL" value="mysql"/>
		<property name="Oracle" value="oracle"/>
		<property name="SQL Server" value="sqlserver"/>
	</databaseIdProvider>
	
</configuration>
```

spring 配置：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:mybatis-spring="http://mybatis.org/schema/mybatis-spring"
	xmlns:tx="http://www.springframework.org/schema/tx"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
		http://mybatis.org/schema/mybatis-spring http://mybatis.org/schema/mybatis-spring-1.2.xsd
		http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-4.0.xsd
		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.0.xsd">

	<!-- Spring希望管理所有的业务逻辑组件，等。。。 -->
	<context:component-scan base-package="com.atguigu.mybatis">
		<context:exclude-filter type="annotation"
			expression="org.springframework.stereotype.Controller" />
	</context:component-scan>

	<!-- 引入数据库的配置文件 -->
	<context:property-placeholder location="classpath:dbconfig.properties" />
	<!-- Spring用来控制业务逻辑。数据源、事务控制、aop -->
	<bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
		<property name="jdbcUrl" value="${jdbc.url}"></property>
		<property name="driverClass" value="${jdbc.driver}"></property>
		<property name="user" value="${jdbc.username}"></property>
		<property name="password" value="${jdbc.password}"></property>
	</bean>
	<!-- spring事务管理 -->
	<bean id="dataSourceTransactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
		<property name="dataSource" ref="dataSource"></property>
	</bean>

	<!-- 开启基于注解的事务 -->
	<tx:annotation-driven transaction-manager="dataSourceTransactionManager"/>
	
	<!-- 
	整合mybatis 
		目的：1、spring管理所有组件。mapper的实现类。
				service==>Dao   @Autowired:自动注入mapper；
			2、spring用来管理事务，spring声明式事务
	-->
	<!--创建出SqlSessionFactory对象  -->
	<bean id="sqlSessionFactoryBean" class="org.mybatis.spring.SqlSessionFactoryBean">
		<property name="dataSource" ref="dataSource"></property>
		<!-- configLocation指定全局配置文件的位置 -->
		<property name="configLocation" value="classpath:mybatis-config.xml"></property>
		<!--mapperLocations: 指定mapper文件的位置-->
		<property name="mapperLocations" value="classpath:mybatis/mapper/*.xml"></property>
	</bean>
	
	<!--配置一个可以进行批量执行的sqlSession  -->
	<bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
		<constructor-arg name="sqlSessionFactory" ref="sqlSessionFactoryBean"></constructor-arg>
		<constructor-arg name="executorType" value="BATCH"></constructor-arg>
	</bean>
	
	<!-- 扫描所有的mapper接口的实现，让这些mapper能够自动注入；
	base-package：指定mapper接口的包名
	 -->
	<mybatis-spring:scan base-package="com.atguigu.mybatis.dao"/>
	<!-- <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
		<property name="basePackage" value="com.atguigu.mybatis.dao"></property>
	</bean> -->
	
</beans>
```

dbconfig.properties:

```properties
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/mybatis?allowMultiQueries=true
jdbc.username=root
jdbc.password=123456
```

- 测试

## mybaties 逆向工程

jar包: mybatis-generator-core-1.3.2.jar

编写 mbg 文件：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
  PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
  "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
<generatorConfiguration>

	<!-- 
		targetRuntime="MyBatis3Simple":生成简单版的CRUD
		MyBatis3:豪华版
	
	 -->
  <context id="DB2Tables" targetRuntime="MyBatis3">
  	<!-- jdbcConnection：指定如何连接到目标数据库 -->
    <jdbcConnection driverClass="com.mysql.jdbc.Driver"
        connectionURL="jdbc:mysql://localhost:3306/mybatis?allowMultiQueries=true"
        userId="root"
        password="123456">
    </jdbcConnection>

	<!--  -->
    <javaTypeResolver >
      <property name="forceBigDecimals" value="false" />
    </javaTypeResolver>

	<!-- javaModelGenerator：指定javaBean的生成策略 
	targetPackage="test.model"：目标包名
	targetProject="\MBGTestProject\src"：目标工程
	-->
    <javaModelGenerator targetPackage="com.aqqje.mybatis.bean" 
    		targetProject=".\src">
      <property name="enableSubPackages" value="true" />
      <property name="trimStrings" value="true" />
    </javaModelGenerator>

	<!-- sqlMapGenerator：sql映射生成策略： -->
    <sqlMapGenerator targetPackage="com.aqqje.mybatis.dao"  
    	targetProject=".\conf">
      <property name="enableSubPackages" value="true" />
    </sqlMapGenerator>

	<!-- javaClientGenerator:指定mapper接口所在的位置 -->
    <javaClientGenerator type="XMLMAPPER" targetPackage="com.aqqje.mybatis.dao"  
    	targetProject=".\src">
      <property name="enableSubPackages" value="true" />
    </javaClientGenerator>

	<!-- 指定要逆向分析哪些表：根据表要创建javaBean -->
    <table tableName="tbl_dept" domainObjectName="Department"></table>
    <table tableName="tbl_employee" domainObjectName="Employee"></table>
  </context>
</generatorConfiguration>

``` 

- 测试：

```java
package com.aqqje.mybatis.test;

import java.io.File;
import java.io.IOException;
import java.io.InputStream;
import java.util.ArrayList;
import java.util.List;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junit.Test;
import org.mybatis.generator.api.MyBatisGenerator;
import org.mybatis.generator.config.Configuration;
import org.mybatis.generator.config.xml.ConfigurationParser;
import org.mybatis.generator.internal.DefaultShellCallback;

import com.aqqje.mybatis.bean.Employee;
import com.aqqje.mybatis.bean.EmployeeExample;
import com.aqqje.mybatis.bean.EmployeeExample.Criteria;
import com.aqqje.mybatis.dao.EmployeeMapper;

/*import com.aqqje.mybatis.bean.Employee;
import com.aqqje.mybatis.dao.EmployeeMapper;*/

public class MyBatisTest {

	public SqlSessionFactory getSqlSessionFactory() throws IOException {
		String resource = "mybatis-config.xml";
		InputStream inputStream = Resources.getResourceAsStream(resource);
		return new SqlSessionFactoryBuilder().build(inputStream);
	}

	@Test
	public void testMbg() throws Exception {
		List<String> warnings = new ArrayList<String>();
		boolean overwrite = true;
		File configFile = new File("mbg.xml");
		ConfigurationParser cp = new ConfigurationParser(warnings);
		Configuration config = cp.parseConfiguration(configFile);
		DefaultShellCallback callback = new DefaultShellCallback(overwrite);
		MyBatisGenerator myBatisGenerator = new MyBatisGenerator(config,
				callback, warnings);
		myBatisGenerator.generate(null);
	}
	
	@Test
	public void testMyBatis3Simple() throws IOException{
		SqlSessionFactory sqlSessionFactory = getSqlSessionFactory();
		SqlSession openSession = sqlSessionFactory.openSession();
		try{
			EmployeeMapper mapper = openSession.getMapper(EmployeeMapper.class);
			List<Employee> list = mapper.selectByExample(null);
			for (Employee employee : list) {
				System.out.println(employee.getId());
			}
		}finally{
			openSession.close();
		}
	}
	
	@Test
	public void testMyBatis3() throws IOException{
		SqlSessionFactory sqlSessionFactory = getSqlSessionFactory();
		SqlSession openSession = sqlSessionFactory.openSession();
		try{
			EmployeeMapper mapper = openSession.getMapper(EmployeeMapper.class);
			//xxxExample就是封装查询条件的
			//1、查询所有
			//List<Employee> emps = mapper.selectByExample(null);
			//2、查询员工名字中有e字母的，和员工性别是1的
			//封装员工查询条件的example
			EmployeeExample example = new EmployeeExample();
			//创建一个Criteria，这个Criteria就是拼装查询条件
			//select id, last_name, email, gender, d_id from tbl_employee 
			//WHERE ( last_name like ? and gender = ? ) or email like "%e%"
			Criteria criteria = example.createCriteria();
			criteria.andLastNameLike("%e%");
			criteria.andGenderEqualTo("1");
			
			Criteria criteria2 = example.createCriteria();
			criteria2.andEmailLike("%e%");
			example.or(criteria2);
			
			List<Employee> list = mapper.selectByExample(example);
			for (Employee employee : list) {
				System.out.println(employee.getId());
			}
			
		}finally{
			openSession.close();
		}
	}
}

```

## pageHepler 

官网详解：pageHepler<a href="https://github.com/pagehelper/Mybatis-PageHelper/blob/master/wikis/zh/HowToUse.md"> pageHepler</a>

