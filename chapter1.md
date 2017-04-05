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
public class Singleton { 
       private Singleton() {}  
       private static Singleton single=null;  
       //静态工厂方法 
       public static Singleton getInstance() {  
       if(single == null) {    
             single = new Singleton();  
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

http://www.cnblogs.com/lzq198754/p/5780331.html



