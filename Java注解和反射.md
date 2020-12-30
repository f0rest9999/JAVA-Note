#注解Annotation和反射Reflection

##注解入门

### 什么是注解

- 不是程序本身，可以对程序作出解释，有一定检查和约束的作用
- ==可以被其他程序（比如编译器等）读取==

- 格式"@注释名"，可以添加一些参数值，比如"@SuppressWarnings(value="unchecked")"

### 内置注解

- @override：重写方法

- @Deprecated ：注释说明这个方法可能存在危险，不推荐使用，但是可以使用

- @SuppressWarnings：镇压警告   @SuppressWarnings("all")等等

### 元注解

- 元注解就是注解其他注解的注解，定义了4个

  ![image-20201207102914510](https://gitee.com/f0rest9999/images/raw/master/20201207102914.png)

- 测试元注解 @Target

  ```java
  // @MyAnnotation如果使用在类上，就会报错，除非写成@Target(value = {ElementType.METHOD, ElementType.TYPE})
  public class Test{
      
      @MyAnnotation
      public  void test(){
          
      }
  }
  //可以使用在方法上，
  @Target(value = ElementType.METHOD)
  @interface MyAnnotation{
      
  }
  ```

- Retention表示注解在什么级别还有效，即源码 < 编译 < 运行时，一半默认的是RUNTIME

### 自定义注解

- 使用@interface 自定义注解，它自动继承了java.lang.annotation.Annotation接口
- ![image-20201207104400336](https://gitee.com/f0rest9999/images/raw/master/20201207104400.png)

- ```java
  public class Test{
      //定义了参数name，如果没有默认值，我们必须给注解赋值
      //注解的参数没有顺序要求
      @MyAnnotation2(name = "Cheng", schools = "HIT")
      public  void test(){
          
      }
  }
  //这两个@基本上属于固定写法了
  @Target(value = {ElementType.METHOD, ElementType.TYPE})
  @Retention(RetentionPolicy.RUNTIME)
  @interface MyAnnotation2{
      //注解的参数:参数类型 + 参数名 ();
      //就是这么定义的，它并不是方法，就是注解的参数
      String name();
      //Stiring name() default""; 如果定义了默认值，那么就可以不写这个参数值
      int age() default 0;
      int id() default -1;//如果默认值为 -1，表示不存在
      String [] schools();
  }
  ```

- 一般自定义注解不会这么长，这里只是测试，一般的注解都很简短

- 注解中一个小细节就是，如果只有一个注解参数，那么默认写value就好

## 反射（Reflection）

### 反射机制概述

- 静态语言vs动态语言

  动态语言可以在运行时可以改变其结构的语言，例如新的函数，对象，甚至代码的引进等等。JavaScript、Python、PHP等等

  静态语言就是运行时结构不可变，比如说，**Java、C、C++**

  **但是Java具有一定的动态性，我们可以用反射机制来获得类似动态语言的特性**

  同时增加了一定的不安全性

- 一个JavaScript的动态的例子：

  ![image-20201208201413750](https://gitee.com/f0rest9999/images/raw/master/20201208201420.png)

- ==正常来说都是new一个对象，反射可以理解为通过一个对象反着找到它class，并且获取大量的信息==

- ![image-20201208204151732](https://gitee.com/f0rest9999/images/raw/master/20201208204151.png)

  ---

  

- ![image-20201208202116527](https://gitee.com/f0rest9999/images/raw/master/20201208202116.png)

  ---

  

- ![image-20201208202321706](https://gitee.com/f0rest9999/images/raw/master/20201208202321.png)

- **反射的优点：可以实现动态的创建对象和编译，体现出很大的灵活性**

- **反射的缺点：对性能有影响**

- ![image-20201208202553229](https://gitee.com/f0rest9999/images/raw/master/20201208202553.png)

  ---

### Class类

- ![image-20201208204442257](https://gitee.com/f0rest9999/images/raw/master/20201208204442.png)

- ![image-20201208204635341](https://gitee.com/f0rest9999/images/raw/master/20201208204635.png)

- ![image-20201208204755621](https://gitee.com/f0rest9999/images/raw/master/20201208204755.png)

- ==**获取class对象的几种方式**==

  ![image-20201208205136507](https://gitee.com/f0rest9999/images/raw/master/20201208205136.png)

###哪些类可以有class对象

![image-20201208214200744](https://gitee.com/f0rest9999/images/raw/master/20201208214200.png)

### Java内存分析

![image-20201208214741411](https://gitee.com/f0rest9999/images/raw/master/20201208214741.png)

![image-20201208214918440](https://gitee.com/f0rest9999/images/raw/master/20201208214918.png)

- ==了解==

![image-20201208215104476](https://gitee.com/f0rest9999/images/raw/master/20201208215104.png)

- ==一个demo==

![image-20201208215342901](https://gitee.com/f0rest9999/images/raw/master/20201208215342.png)

![image-20201208215419768](https://gitee.com/f0rest9999/images/raw/master/20201208215419.png)

- ==**图解，Java内存图，可以多看看别的博客，他这里讲的不算详细**==

### 分析类的初始化

- 什么时候会发生类的初始化

  ![image-20201208220642426](https://gitee.com/f0rest9999/images/raw/master/20201208220642.png)

### 类加载器

- 类加载器的作用（==这一节没看懂==）

  ![image-20201208221120627](https://gitee.com/f0rest9999/images/raw/master/20201208221120.png)

### 获取类运行时结构

`见代码`

### 动态创建对象执行方法（怎么使用）

- 获得了这个对象，如何去使用

  ```java
  public class Test {
      public static void main(String[] args) throws ClassNotFoundException, NoSuchFieldException, NoSuchMethodException, IllegalAccessException, InstantiationException, InvocationTargetException {
          Class c1 = Class.forName("User");
          //构造一个对象
          
          //本质上调用的是无参构造器
          User user = (User)c1.newInstance();
          System.out.println(user);
      
      	//通过构造器创建对象
          Constructor constructors = c1.getDeclaredConstructor(String.class, int.class, int.class);
          User user1 = (User) constructors.newInstance("kkk", 1, 12);
          System.out.println(user1);
          
          //通过反射调用普通方法
          User user3 = (User)c1.newInstance();
          //通过反射获取一个方法
          Method setName = c1.getMethod("setName", String.class);
          //方法已经有了，然后传递一个对象和要传递的参数，这个对象就不一定非要是user3了
          setName.invoke(user3, "c");
          System.out.println(user3);
          
          //通过反射操作属性
          User user4 = (User)c1.newInstance();
          Field name = c1.getDeclaredField("name");
          //如果没有这句，因为属性值是private的，会报错，不能直接操作私有属性
          //需要通过这个方法，相当于打开了属性或者是方法的访问权限
          name.setAccessible(true);
          
          name.set(user4, "ccc");
          System.out.println(user4.getName());
          
      }
  }
  ```

### 性能分析

`普通方式执行 >> 关闭检测的反射执行 > 反射执行 `（速度）

### 反射操作泛型

![image-20201209134602159](https://gitee.com/f0rest9999/images/raw/master/20201209134609.png)

![image-20201209135450428](https://gitee.com/f0rest9999/images/raw/master/20201209135450.png)

![image-20201209135513059](https://gitee.com/f0rest9999/images/raw/master/20201209135513.png)

![image-20201209135555910](https://gitee.com/f0rest9999/images/raw/master/20201209135555.png)

### 反射操作注解

![image-20201209135750680](https://gitee.com/f0rest9999/images/raw/master/20201209135750.png)

- ==练习通过注解来完成类和表结构的映射==，==**其实java的框架基本上都是基于大量的反射和注解的**==

  ```java
  import java.lang.annotation.*;
  import java.lang.reflect.Field;
  
  public class Test {
      public static void main(String[] args) throws ClassNotFoundException, NoSuchFieldException {
          Class c1 = Class.forName("Student");
  
          //通过反射获得注解
          Annotation[] annotations = c1.getAnnotations();
          for (Annotation annotation : annotations) {
              System.out.println(annotation);
          }
  
          //获得注解的value值
          Table table = (Table)c1.getAnnotation(Table.class);
          String value = table.value();
          System.out.println(value);
  
          //获得类指定的注解
          Field f = c1.getDeclaredField("name");
          FailedC failedC = (FailedC)f.getAnnotation(FailedC.class);
          System.out.println(failedC.colName());
          System.out.println(failedC.type());
          System.out.println(failedC.length());
      }
  }
  
  //类名的注解
  @Retention(RetentionPolicy.RUNTIME)
  @Target(ElementType.TYPE)
  @interface Table{
      String value();
  }
  //属性的注解
  @Retention(RetentionPolicy.RUNTIME)
  @Target(ElementType.FIELD)
  @interface FailedC {
      String colName();
      String type();
      int length();
  }
  
  @Table("DB_student")
  class Student{
      @FailedC(colName = "stu_id", type = "int", length = 10)
      private int id;
      @FailedC(colName = "stu_age", type = "int", length = 10)
      private int age;
      @FailedC(colName = "stu_name", type = "varchar", length = 5)
      private  String name;
  
      public Student() {
      }
  
      public Student(int id, int age, String name) {
          this.id = id;
          this.age = age;
          this.name = name;
      }
  
      public int getId() {
          return id;
      }
  
      public void setId(int id) {
          this.id = id;
      }
  
      public int getAge() {
          return age;
      }
  
      public void setAge(int age) {
          this.age = age;
      }
  
      public String getName() {
          return name;
      }
  
      public void setName(String name) {
          this.name = name;
      }
  
      @Override
      public String toString() {
          return "Student{" +
                  "id=" + id +
                  ", age=" + age +
                  ", name='" + name + '\'' +
                  '}';
      }
  }
  
  ```

  结果：![image-20201209143137899](https://gitee.com/f0rest9999/images/raw/master/20201209143137.png)

### 反射简单代码示例

```java
//一个class
class User{
    private String name;
    private int id;
    private int age;

    public User() {
    }

    public User(String name, int id, int age) {
        this.name = name;
        this.id = id;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                ", id=" + id +
                ", age=" + age +
                '}';
    }
}
```

```java
//c1、c2、c3、c4的hasCode值是一样的,即同一个class class对象永远都是一个
public class Test {
    public static void main(String[] args) throws ClassNotFoundException {
        //通过反射来获取类的Class对象
        //包名 + 类名
        Class c1 = Class.forName("User");
        System.out.println(c1);
        Class c2 = Class.forName("User");
        Class c3 = Class.forName("User");
        Class c4 = Class.forName("User");

        //一个类在内存中只有一个Class对象
        //一个类被加载后，类的整个结构都封装在Class对象中
        System.out.println(c2.hashCode());
        System.out.println(c3.hashCode());
        System.out.println(c4.hashCode());
    }
}
```

```java
//获取class对象的几种方法
public class Test {
    public static void main(String[] args) throws ClassNotFoundException {
       	//方式一 类名 + class
        Class c1 = User.class;
       
       	//方式二 通过对象获得
        User user = new User();
        Class c2 = user.getClass();
       
        //方式三 通过forname获得 包名 + 类名
       	Class c3 = Class.forName("User");
        
        
        //方式四 有限制：基本内置类型的包装类都有一个TYPE属性
        Class c4 = Integer.TYPE;
    }
}
```

```java
public class Test {
    public static void main(String[] args) throws ClassNotFoundException {
        //通过反射来获取类的Class对象
        //包名 + 类名
        Class c1 = Class.forName("User");

        //获得类的名字
        System.out.println(c1.getName());//获得包名 + 类名
        System.out.println(c1.getSimpleName());//获得类名

        //获得类的属性
        Field[] fields = c1.getFields();//什么都没有打印出来//只能找到public属性
        for(Field field : fields){
            System.out.println(field);
        }

        Field[] fields2 = c1.getDeclaredFields();//找到全部属性
        //private java.lang.String User.name   private int User.id   private int User.age
        for(Field field : fields2){
            System.out.println(field);
        }
        
        //获得指定属性的值
        Field name = c1.getDeclaredField("name");
        System.out.println(name);
        
       	//获得类的方法
        Method[] methods1 = c1.getMethods();//获得本类及其父类的全部public方法
        Method[] methods2 = c1.getDeclaredMethods();//获得本类的所有方法
        
        //获得指定方法(名字， 参数)
        //重载
        Method getName = c1.getMethod("getName", null);
    	Method setName = c1.getMethod("setName", String.class);
    	
    	//获得构造器
        Constructor[] constructors = c1.getConstructors();
        Constructor[] constructors2 = c1.getDeclaredConstructors();
        
        //获得指定的构造器
        Constructor[] constructors3 = c1.getDeclaredConstructors(String.class, int.class, int.class);
    }
}
```

