> # 如何处理高并发

```
1、HTML静态化
2、图片服务器分离
3、数据库集群和库表散列
4、缓存
5、镜像
6、负载均衡
```

> # 单例模式

```
//懒汉式单例类.在第一次调用的时候实例化自己 
public class Singleton { 
       private Singleton() {}  
       private static Singleton single=null;  
       //静态工厂方法 
       public static Singleton getInstance() {  
       if(single == null) {    
             single = new Singleton();  
         }    
             return single;  
         }  
}
```

> # 反射机制

### 1.反射机制是什么

```
        反射机制是在运行状态中，对于任意一个类，都能够知道这个类的所有属性和方法；对于任意一个对象，都能够调用它的任意一个方法和属性；这种动态获取的信息以及动态调用的方法的功能称为Java语言的反射机制。
```

### 2.反射机制能做什么

反射机制主要提供了以下功能：

```
在运行时判断任意一个对象所属的类；
在运行时构造任意一个类的对象；
在运行时判断任意一个类所具有的的成员变量和方法；
在运行时调用任意一个对象的方法；
生成动态代理。
```

### 3.反射机制的相关API

[http://www.cnblogs.com/lzq198754/p/5780331.html](http://www.cnblogs.com/lzq198754/p/5780331.html)

> # 下载

```
@RequestMapping("download_file")
    public void download_file(String fileName, HttpServletRequest request,String filePath,
            HttpServletResponse response) {
     try {
         URL url = new URL(filePath);    
         HttpURLConnection conn = (HttpURLConnection)url.openConnection();    
         //设置超时间为3秒  
         conn.setConnectTimeout(3*1000);  
         //防止屏蔽程序抓取而返回403错误  
         conn.setRequestProperty("User-Agent", "Mozilla/4.0 (compatible; MSIE 5.0; Windows NT; DigExt)");  
         conn.setRequestMethod("GET");
         conn.connect();
         //得到输入流  
         InputStream inputStream = conn.getInputStream();    
         //获取自己数组  
         byte[] buffer = new byte[2048];
         //设置content-disposition响应头控制浏览器弹出保存框，若没有此句则浏览器会直接打开并显示文件。中文名要经过URLEncoder.encode编码，否则虽然客户端能下载但显示的名字是乱码
         response.setHeader("content-disposition", "attachment;filename=" + URLEncoder.encode(fileName, "UTF-8"));
         int len = 0;
         // 通过response对象获取OutputStream流
         OutputStream out = response.getOutputStream();
         // 将InputStream流写入到buffer缓冲区
         while ((len = inputStream.read(buffer)) > 0) {
             //使用OutputStream将缓冲区的数据输出到客户端浏览器
             out.write(buffer, 0, len);
         }
         inputStream.close();
    } catch (Exception e) {
        e.printStackTrace();
    }
    }
```

> # 微信支付签名算法

```
 public static String createSign(String characterEncoding,SortedMap<Object,Object> parameters){  
        StringBuffer sb = new StringBuffer();  
        Set es = parameters.entrySet();//所有参与传参的参数按照accsii排序（升序）  
        Iterator it = es.iterator();  
        while(it.hasNext()) {  
            Map.Entry entry = (Map.Entry)it.next();  
            String k = (String)entry.getKey();  
            Object v = entry.getValue();  
            if(null != v && !"".equals(v)   
                    && !"sign".equals(k) && !"key".equals(k)) {  
                sb.append(k + "=" + v + "&");  
            }  
        }  
        sb.append("key=" + Key);  
        String sign = MD5Encode(sb.toString(), characterEncoding).toUpperCase();  
        return sign;  
    }
```

> # 获取一个String的MD5值

```
public static String getMd5(String str) {
        byte[] bs = md5.digest(str.getBytes());
        StringBuilder sb = new StringBuilder(40);
        for(byte x:bs) {
            if((x & 0xff)>>4 == 0) {
                sb.append("0").append(Integer.toHexString(x & 0xff));
            } else {
                sb.append(Integer.toHexString(x & 0xff));
            }
        }
        return sb.toString();
    }
```

> # MD5加密

```
 public static String MD5Encode(String origin, String charsetname) {  
        String resultString = null;  
        try {  
            resultString = new String(origin);  
            MessageDigest md = MessageDigest.getInstance("MD5");  
            if (charsetname == null || "".equals(charsetname))  
                resultString = byteArrayToHexString(md.digest(resultString  
                        .getBytes()));  
            else  
                resultString = byteArrayToHexString(md.digest(resultString  
                        .getBytes(charsetname)));  
        } catch (Exception exception) {  
        }  
        return resultString;  
    }

    private static String byteArrayToHexString(byte b[]) {  
        StringBuffer resultSb = new StringBuffer();  
        for (int i = 0; i < b.length; i++)  
            resultSb.append(byteToHexString(b[i]));  

        return resultSb.toString();  
    }
```

> # 排序

```
public JsonResult new_class(Model model,HttpSession httpSession,String school_id){
    List<T9Class> listT9Class = new ArrayList<T9Class>();
    Collections.sort(listT9Class, new Comparator<T9Class>(){
        public int compare(T9Class t1,T9Class t2){
            //升序返回-1；降序返回1；相等返回0
            int flag=t2.getCreateTime().compareTo(t1.getCreateTime());   
            return flag; 
        }
    });
    return JsonResult.success(listT9Class);
}
```

> # mybatis好处

```
mybatis把sql语句从Java源程序中独立出来，放在单独的XML文件中编写，给程序的维护带来了很大便利。 mybatis封装了底层JDBC API的调用细节，
并能自动将结果集转换成Java Bean对象， 大大简化了Java数据库编程的重复工作。 因为mybatis需要程序员自己去编写sql语句， 程序员可以结合
数据库自身的特点灵活控制sql语句， 因此能够实现比hibernate等全自动orm框架更高的查询效率，能够完成复杂查询
```

> # mybatis缓存

```
MyBatis的缓存分为一级缓存和二级缓存, 一级缓存放在session里面,默认就有,二级缓存放在它的命名空间里,默认是打开的, 使用二级缓存
属性类需要实现Serializable序列化接 口(可用来保存对象的状态),可在它的映射文件中配置<cache/>

```

> # mybatis动态sql设定，语法

```
MyBatis里面的动态Sql一般是通过if节点来实现,通过OGNL语法来实现,但是如果要写的完 整,必须配合where,trim节点,where节点是判断
包含节点有内容就插入where,否则不插 入,trim节点是用来判断如果动态语句是以and 或or开始,那么会自动把这个and或者or取 掉
```

> # mybatis实现一对多

```
有联合查询和嵌套查询,联合查询是几个表联合查询,只查询一次,通过在resultMap里面配 置collection节点配置一对多的类就可以完成;
嵌套查询是先查一个表,根据这个表里面的 结果的外键id,去再另外一个表里面查询数据,也是通过配置collection,但另外一个表的 查询
通过select节点配置 
```

> # mybatis接口绑定实现方式

```
接口绑定有两种实现方式,一种是通过注解绑定,就是在接口的方法上面加上 @Select@Update等注解里面包含Sql语句来绑定,
另外一种就是通过xml里面写SQL来绑定, 在这种情况下,要指定xml映射文件里面的namespace必须为接口的全路径名
```

> # mybatis实体类中的属性名与字段名不相同的解决方法

```
1、在sql语句中定义字段名的别名，让其与实体类中的属性名相等
2、通过<resultMap>来映射字段名与实体类属性名的一一对应关系
```

> # mybatis如何获取自动生成的（主）键值

```
sql语句的标签内添加属性useGeneratedKeys 设置为true，设定 keyProperty的值为在实体类里面的属性名
<insert id="insertPerson" parameterType="com.PersonEntity" useGeneratedKeys="true" keyProperty="personId" >
</insert>

用实体类属性的get方法可以直接获取值
```

> # mybatis在mapper中如何传递多个参数

```
1、直接参数传值
dao层的函数方法
Public User selectUser(String name,String area);
对应的mapper
<select id="selectUser" resultMap="BaseResultMap">
    select  *  from user_user_t   where user_name = #{0} and user_area=#{1}
</select>
其中，#{0}代表接收的是dao层中的第一个参数，#{1}代表dao层中第二参数，更多参数一致往后加即可
2、利用map传多个参数
dao层的函数方法
Public User selectUser(Map paramMap);
对应的mapper
<select id=" selectUser" resultMap="BaseResultMap">
   select  *  from user_user_t   where user_name = #{userName，jdbcType=VARCHAR} and user_area=#{userArea,jdbcType=VARCHAR}
</select>
service层调用
Private User xxxSelectUser(){
Map paramMap=new hashMap();
paramMap.put(“userName”,”对应具体的参数值”);
paramMap.put(“userArea”,”对应具体的参数值”);
User user=xxx. selectUser(paramMap);}
3、注解方式传值
dao层的函数方法
Public User selectUser(@param(“userName”)Stringname,@param(“userArea”)String area);
对应的mapper
<select id=" selectUser" resultMap="BaseResultMap">
   select  *  from user_user_t   where user_name = #{userName，jdbcType=VARCHAR} and user_area=#{userArea,jdbcType=VARCHAR}
</select> 
```

> # spring MVC Framework有这样一些特点

```
1、它是基于组件技术的.全部的应用对象,无论控制器和视图,还是业务对象之类的都是java组件.并且和Spring提供的其他基础结构紧密集成.
2、不依赖于Servlet API(目标虽是如此,但是在实现的时候确实是依赖于Servlet的)
3、可以任意使用各种视图技术,而不仅仅局限于JSP
4、支持各种请求资源的映射策略
5、它应是易于扩展的
```

> # Springmvc工作流程

```
1. 用户发送请求至前端控制器DispatcherServlet
2. DispatcherServlet收到请求调用HandlerMapping处理器映射器。
3. 处理器映射器根据请求url找到具体的处理器，生成处理器对象及处理器拦截器(如果有则生成)一并返回给DispatcherServlet。
4. DispatcherServlet通过HandlerAdapter处理器适配器调用处理器
5. 执行处理器(Controller，也叫后端控制器)。
6. Controller执行完成返回ModelAndView
7. HandlerAdapter将controller执行结果ModelAndView返回给DispatcherServlet
8. DispatcherServlet将ModelAndView传给ViewReslover视图解析器
9. ViewReslover解析后返回具体View
10. DispatcherServlet对View进行渲染视图（即将模型数据填充至视图中）。
11. DispatcherServlet响应用户
```

> # SSM优缺点、使用场景

```
1. Mybatis和hibernate不同，它不完全是一个ORM框架，因为MyBatis需要程序员自己编写Sql语句，不过mybatis可以通过XML或注解方式灵活配置
要运行的sql语句，并将java对象和sql语句映射生成最终执行的sql，最后将sql执行的结果再映射生成java对象。
2. Mybatis学习门槛低，简单易学，程序员直接编写原生态sql，可严格控制sql执行性能，灵活度高，非常适合对关系数据模型要求不高的软件开发，
例如互联网软件、企业运营类软件等，因为这类软件需求变化频繁，一但需求变化要求成果输出迅速。但是灵活的前提是mybatis无法做到数据库无关
性，如果需要实现支持多种数据库的软件则需要自定义多套sql映射文件，工作量大。
3. Hibernate对象/关系映射能力强，数据库无关性好，对于关系模型要求高的软件（例如需求固定的定制化软件）如果用hibernate开发可以节省很多
代码，提高效率。但是Hibernate的学习门槛高，要精通门槛更高，而且怎么设计O/R映射，在性能和对象模型之间如何权衡，以及怎样用好Hibernate
需要具有很强的经验和能力才行。
4. 总之，按照用户的需求在有限的资源环境下只要能做出维护性、扩展性良好的软件架构都是好架构，所以框架只有适合才是最好。
```

> # 如何解决POST、GET请求时中文乱码问题

```
1、post
针对Post方式提交的请求如果出现乱码，可以每次在request解析数据时设置编码格式：
request.setCharacterEncoding("utf-8");
也可以使用编码过滤器来解决，最常用的方法是使用spring提供的编码过滤器：
在Web.xml中增加如下配置（要注意的是它的位置一定要是第一个执行的过滤器）：
<filter>
<filter-name>charsetFilter</filter-name>
<filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
<init-param>
<param-name>encoding</param-name>
<param-value>UTF-8</param-value>
</init-param>
<init-param>
<param-name>forceEncoding</param-name>
<param-value>true</param-value>
</init-param>
</filter>
2、get
针对Get方式的乱码问题，由于参数是通过URL传递的，所以上面通过request设置的编码格式是不起作用的，此时可以在每次发生请求之前
对URL进行编码：例如：Location.href="/encodeURI"("http://localhost/test/s?name=中文&sex=女");
当然也有更简便的方法，那就是在服务器端配置URL编码格式：
修改tomcat的配置文件server.xml：
             <Connector URIEncoding="UTF-8" 
                 port="8080"   maxHttpHeaderSize="8192"
               maxThreads="150" minSpareThreads="25" maxSpareThreads="75"
               connectionTimeout="20000" disableUploadTimeout="true" />

只需增加 URIEncoding="UTF-8"  这一句，然后重启tomcat即可。
```

> # spring支持的bean的作用域

```
1、singleton：单例模式，bean将只有一个实例
2、prototype：原型模式，每次都将产生一个新的实例
3、request：对于每次HTTP请求，使用request定义的bean请求都会产生一个新实例
4、session：对于每次HTTP Session，使用session定义的bean请求都会产生一个新实例
5、globalsession：每个全局的HTTP Session，使用session定义的bean请求都会产生一个新实例
```

> # Spring框架中单例beans是线程安全的吗

```
spring框架并没有对单例bean进行任何多线程的封装处理。关于单例bean的线程安全和并发问题需要开发者自行去搞定。
但实际上，大部分的Spring bean并没有可变的状态(比如Serview类和DAO类)，所以在某种程度上说Spring的单例bean是线程安全的。
如果你的bean有多种状态的话（比如 View Model 对象），就需要自行保证线程安全。
```

> # Spring框架中bean的生命周期

```
1、Spring容器 从XML 文件中读取bean的定义，并实例化bean
2、Spring根据bean的定义填充所有的属性
3、如果bean实现了BeanNameAware 接口，Spring 传递bean 的ID 到 setBeanName方法
4、如果Bean 实现了 BeanFactoryAware 接口， Spring传递beanfactory 给setBeanFactory 方法
5、如果有任何与bean相关联的BeanPostProcessors，Spring会在postProcesserBeforeInitialization()方法内调用它们
6、如果bean实现IntializingBean了，调用它的afterPropertySet方法，如果bean声明了初始化方法，调用此初始化方法
7、如果有BeanPostProcessors 和bean 关联，这些bean的postProcessAfterInitialization() 方法将被调用
8、如果bean实现了 DisposableBean，它将调用destroy()方法
```

> # 哪些是最重要的bean生命周期方法？能重写它们吗

```
有两个重要的bean生命周期方法。第一个是setup方法，该方法在容器加载bean的时候被调用。第二个是teardown方法，
该方法在bean从容器中移除的时候调用
可以重写
bean标签有两个重要的属性(init-method 和 destroy-method)，你可以通过这两个属性定义自己的初始化方法和析构方法。
Spring也有相应的注解：@PostConstruct 和 @PreDestroy
```

> # 什么是Spring的内部bean

```
当一个bean被用作另一个bean的属性时，这个bean可以被声明为内部bean。在基于XML的配置元数据中，可以通过把元素定义在 
或元素内部实现定义内部bean。内部bean总是匿名的并且它们的scope总是prototype
```

> # 如何在Spring中注入Java集合类

```
Spring提供如下几种类型的集合配置元素：

list元素用来注入一系列的值，允许有相同的值。

set元素用来注入一些列的值，不允许有相同的值。

map用来注入一组”键-值”对，键、值可以是任何类型的。

props也可以用来注入一组”键-值”对，这里的键、值都字符串类型
```

> # 什么是bean自动装配，有几种模式

```
Spring容器可以自动配置相互协作beans之间的关联关系。这意味着Spring可以自动配置一个bean和其他协作bean之间的关系，
通过检查BeanFactory 的内容里没有使用和< property>元素
共五种模式：
no：默认的方式是不进行自动装配，通过手工设置ref 属性来进行装配bean。
byName：通过参数名自动装配，Spring容器查找beans的属性，这些beans在XML配置文件中被设置为byName。之后容器试图匹配、装配和
该bean的属性具有相同名字的bean。
byType：通过参数的数据类型自动自动装配，Spring容器查找beans的属性，这些beans在XML配置文件中被设置为byType。之后容器试图
匹配和装配和该bean的属性类型一样的bean。如果有多个bean符合条件，则抛出错误。
constructor：这个同byType类似，不过是应用于构造函数的参数。如果在BeanFactory中不是恰好有一个bean与构造函数参数相同类型，
则抛出一个严重的错误。
autodetect：如果有默认的构造方法，通过 construct的方式自动装配，否则使用 byType的方式自动装配。
```



