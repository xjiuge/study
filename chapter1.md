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



