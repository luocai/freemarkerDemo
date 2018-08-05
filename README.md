# 什么是freemarker
FreeMarker 是一个用 Java 语言编写的模板引擎，它基于模板来生成文本输出。FreeMarker与 Web 容器无关，即在 Web 运行时，它并不知道 Servlet 或 HTTP。它不仅可以用作表现层的实现技术，而且还可以用于生成 XML，JSP 或 Java 等。
# 如何才能使用
工程引入依赖<br>
```xml
  <dependency>
      <groupId>org.freemarker</groupId>
      <artifactId>freemarker</artifactId>
      <version>2.3.23</version>
  </dependency>  
```
# 语法分类
1. 文本，直接输出的部分
2. 注释，即<#--...-->格式不会输出
3. 插值（Interpolation）：即${..}部分,将使用数据模型中的部分替代输出
4. FTL指令：FreeMarker指令，和HTML标记类似，名字前加#予以区分，不会输出。
# 生成文件
* 第一步：创建一个 Configuration 对象，直接 new 一个对象。构造方法的参数就是 freemarker的版本号。
* 第二步：设置模板文件所在的路径。
* 第三步：设置模板文件使用的字符集。一般就是 utf-8.
* 第四步：加载一个模板，创建一个模板对象。
* 第五步：创建一个模板使用的数据集，可以是 pojo 也可以是 map。一般是 Map。
* 第六步：创建一个 Writer 对象，一般创建一 FileWriter 对象，指定生成的文件名。
* 第七步：调用模板对象的 process 方法输出文件。
* 第八步：关闭流
* 代码如下
```java
//1.创建配置类
Configuration configuration=new Configuration(Configuration.getVersion());
//2.设置模板所在的目录 
configuration.setDirectoryForTemplateLoading(new File("D:/pinyougou_work/freemarkerDemo/src/main/resources/"));
//3.设置字符集
configuration.setDefaultEncoding("utf-8");
//4.加载模板
Template template = configuration.getTemplate("test.ftl");
//5.创建数据模型
Map map=new HashMap();
map.put("name", "张三 ");
map.put("message", "欢迎来到神奇的品优购世界！");
//6.创建Writer对象
Writer out =new FileWriter(new File("d:\\test.html"));
//7.输出
template.process(map, out);
//8.关闭Writer对象
out.close();
```
# 语法详解
* assign指令<br>
此指令用于在页面上定义一个变量 
   + 定义简单类型
   + 定义对象类型
```html
<#assign linkman="周先生">
联系人：${linkman}

<#assign info={"mobile":"13301231212",'address':'北京市昌平区王府街'} >
电话：${info.mobile}  地址：${info.address}
```
 
* include指令<br>
此指令用于模板文件的嵌套
创建模板文件head.ftl
```html
<h1>你好</h1>
```
我们修改test.ftl，在模板文件中使用include指令引入刚才我们建立的模板
```html
<#include "head.ftl">
```
* if指令<br>
在模板文件上添加
```html
<#if success=true>
  你已通过实名认证
<#else>  
  你未通过实名认证
</#if>
在代码中对str变量赋值
map.put("success", true);
在freemarker的判断中，可以使用= 也可以使用== 
```
* list指令<br>
需求，实现商品价格表，如下图：
 
   + 代码中对变量goodsList赋值
   + 在模板文件上添加
```java
  List goodsList=new ArrayList();
  Map goods1=new HashMap();
  goods1.put("name", "苹果");
  goods1.put("price", 5.8);
  Map goods2=new HashMap();
  goods2.put("name", "香蕉");
  goods2.put("price", 2.5);
  Map goods3=new HashMap();
  goods3.put("name", "橘子");
  goods3.put("price", 3.2);
  goodsList.add(goods1);
  goodsList.add(goods2);
  goodsList.add(goods3);
  map.put("goodsList", goodsList);
  
  
  ----商品价格表----<br>
<#list goodsList as goods>
  ${goods_index+1} 商品名称： ${goods.name} 价格：${goods.price}<br>
</#list>
```
如果想在循环中得到索引，使用循环变量+_index就可以得到。
* 内建函数<br>
内建函数语法格式： 变量+?+函数名称 + 获取集合大小
 
* 空值处理运算符<br>
如果你在模板中使用了变量但是在代码中没有对变量赋值，那么运行生成时会抛出异常。但是有些时候，有的变量确实是null，怎么解决这个问题呢？
   + 判断某变量是否存在:“??”
   + 缺失变量默认值:“!”
* 运算符
   + 算数运算符
FreeMarker表达式中完全支持算术运算,FreeMarker支持的算术运算符包括:+, - , * , / , %
   + 逻辑运算符
   + 比较运算符


