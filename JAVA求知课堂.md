1.窗口 + R 输入 cmd，可以打开dos界面
	根据截图进行dos命令操作，对文件如果有错误就加后缀名试试
	且所有标点符号均是英文

2.Java 垃圾回收机制是自动回收，优点是不会忘记回收，缺点是回收不及时
	现代内存较大，可以忍受回收不及时

3.下载jdk 详见截图
	网址https://www.oracle.com/technetwork/java/javase/downloads

4.环境变量的配置 详见截图
	若有不同版本要求，那就是重新配置JAVA_HOME即可

5.jdk jre jvm之间的关系 详见截图(包含关系)

6.cmd 之后 找到.java文件的路径之后 进行“javac”进行编译 再通过“java”运行class文件
	每次更改完都要重新编译

7.文档注释 在类之前写的：
	/**
	*文档注释
	*这是一个打印hello world的类
	*@author
	*version
	*/

	这是java特有的  可以对类和方法进行注释

8.关于起标识符（名字）的规范：
	包名：xxyyzz
	类名、接口名：XxYyZz
	变量名、方法名：xxYyZz
	常量名：XX_YY_ZZ
9.八种基本数据类型以外都是引用数据类型
	注意float 定义时要为 1.22f这样的
10.最常用的引用类型:
	String是一个典型的不可变类，可以在初始化是赋值为null  可以用"+"拼接 
	当有一系列的‘+’运算，从某个部分有其中含有字符串，那么这个字符串之后的所有都按字符串拼接来当作字符串来连接。
	
	且 若有 String str1 = “nn”；
			String str2 = “nn”；内存中只会存在一个“nn”，在赋值时只会让变量引用“nn”的内存地址
			之后再声明变量赋值时，其实是将内存地址赋值给str2

11.集成开发环境的安装
	集成开发环境包含文本编辑工具、自动编译、简化运行、随时进行代码调试
	
	设置workspace的步骤 看截图workspace
	
	New -> Other ->输入java 有一个java project -> 输入工程名字 且点击Use default JRE ->Finish
	
	在src New-> class ->package可以不用  输入Name 注意类的开头要大写->就可以在class中写方法了  例子中写的是main
	
	编写过程中 用 Alt + / 就可以主动显示
	
	想单个运行就鼠标右键 ->Run As -> 点击对应的 Application 
	Console是控制台显示窗口

12.‘A’ 65  'a' 97
	
13.int a[] = new int [4];
   int a[] = new int []{1,2};   常见的错误是数组越界和空指针异常

14.面向对象的三大特征：封装、继承、多态
	类：设计图   对象：实在的设计产品

15.类的写法：
	public class Person{
		//属性,类的成员变量是有默认值的，不用初始化
		String name;
		int age;
		//方法，驼峰命名法
		public void showName(){
			System.out.println(name);
		}	
		public int getAge(){
			return age;
		}
	}

16.对象的创建和使用
	通过 new 来创建  再同一个package下面
	public class Test {
	public static void main(String[] args){
		//实例化Person类，创建Person对象
		Person person = new Person();
		person.name = "zhangsan";
		person.showname();//方法的调用
		int i = person.getAge();
		System.out.print(i);
	}
}
	即操作类的变量和调用类的方法

17.类的成员之一：属性
	格式：修饰符 类型 属性名 = 初值；
									修饰符就是在修饰变量
	说明：修饰符private：该属性只能由该类的方法访问，不可以在类的外部使用
	      修饰符public：该类可以被该类以外的方法访问
		  类型：任何基本类型（类型的初值见截图）

18.类的成员之二：方法
    格式：修饰符 返回值类型 方法名（参数列表）{方法体语句}；
	
	方法内部不能在定义方法  只能调用方法
	且同一个类中的方法可以直接互相调用，不用new实例化对象在调用

19.变量的分类
	成员变量 
		实例变量（不以static修饰，存在于对象的所在的堆内存中）
		类变量（以static修饰，这类的变量是静态的，此变量在类不需要实例化就可以使用，直接通过类名.属性名直接使用）
	局部变量
		形参
		方法局部变量
		代码块局部变量

20.匿名对象：不定义对象  直接调用这个对象的方法 如：new Person().shout();
	使用情况：如果对一个对象只需要进行一次方法调用，那么就可以使用匿名对象
			  经常将匿名对象作为实参传递给一个方法调用。（任何定义的类 再定义的对象 都可以作为参数传递给函数）
			  
21.类的访问机制：
	在同一个类中：类中的方法可以直接访问类中的成员变量（static方法只能访问static成员变量，编译不通过）
	在不同类中：先要创建访问类的对象，再用对象访问类中定义的成员

22.面向对象思想落地法则（一）
	关注于类的设计，成员、属性、方法
	类的实例化
	通过（对象.属性 对象.方法调用）

23.方法的重载（overload）
	重载：在一个类中，允许存在一个以上同名的方法（只要他们参数个数或者参数类型不一样即可，参数顺序不一样也构成重载）

24.关于可变参数的声明（可变参数就是形参个数可以变化，在这里特指方法二的使用）
	（1）用数组的方式来传递可变个数的参数
	（2）用java特有的...来传递可变个数的参数，这种参数在使用时与数组的使用方式相同
	public void printInfo(String... ss){
		for(int i = 0;i < ss.length;i++){
			System.out.print(ss[i]);
		}
	}
	方法二 包含 方法一 实则是差不多的 因此常用方法二
	区别在于方法一如果没有参数要传递必须要定义一个空数组，而方法二可以选择不填
	
	且若有部分可变参数时，声明...可变参数要放在所有形参的最后才正确

25.形参和实参
	Java方法的参数传递方式只有一种：值传递。（即是变量在栈内存中的值）
	
	JVM内存分为三部分 栈、堆、方法区（详见截图）
	
	基本数据类型就是在栈中保存的，而引用对象在栈中保存的是引用对象的地址，那么方法的参数传递是值传递
	
	基本数据类型在方法传递时就是复制值
	
	但是！方法的参数不是基本数据类型时，如对象，其形参和实参传递的是栈内存中的地址，即对同一个对象进行操作

26.软件包package
	类似于文件夹的存在，使项目容易管理
	
	src -> new 一个 package
	可以有层级结构，包里面还可以创建包，用“.”来进行分隔， 包通常是小写单词
	
	注意：保证同一个包下面类不重名即可，且同一个包下面引用其他类是不需要import的，是默认的

27.import关键字
	
	import 包.类
	import 包.*代表的是引用该包下面的所有类
	import 也不是必须的，可以在类里面使用其他类的全名进行区别

28.	java中一些基本的包（截图）

29.面向对象的特征之一：封装和隐藏
	public class Person{
		public int age;//把类的属性开放出来，调用者可以随便使用，这样会有问题
	}
	
	比如其他类调用之后 赋值-100，显然不符合逻辑
	因此，我们需要对这样不能让调用者随意使用属性做封装和隐藏
	
	public class Person{
		private int age;
		
		public void setAge(int a){
			if(a >= 0 && a <= 140){
				age = a;
			}
			else{打印错误信息}
		}
	}
	
	总结：通常将变量设置成private，然后在提供public 的 方法：set()和get()实现对该属性的操作
	好处：对外隐藏细节、方便在方法中加入一定的控制逻辑、方便修改

30.四种访问权限修饰符：
	修饰符类型： private 	缺省 	protected 	public（详见截图）
				 最严格				             最宽松
	限制到		 类内部   同一个包	  子类		任何地方

	对于class 只有两种修饰符，public 和 缺省（同一个包内部的类访问）
	
	补充：在同一个java文件中可以存在多个类，但是只能有有一个是public，其他的都是缺省

31.类的成员之三：构造器（构造方法）（有截图的）
	
	每个类创造了 就会有默认的构造器，才能new，且默认的构造方法前面是什么修饰符，与定义的类的修饰符有关
	这是隐式的构造器，默认的隐式构造器是和类同名的，new 对象实际上就是调用类的构造方法
	
	也可以在类中显式的定义
	pubic class Person{
		public Person{
			age = 1;//这样年龄默认值 就会更改为1
		}
	}
	
	也可以定义显式带参数的形式
	pubic class Person(int a){
		age = a;
	}
	当存在显式的构造方法时，就没有默认的无参构造器了 
	
	一个类可以创造多个重载的构造器，父类的构造器不可以被子类继承
	
	构造器重载：既然构造器也是方法，所以构造器可以进行重载，方便创造不同需要的对象

32.this 关键字
	表示当前对象，可以调用类的属性、方法和构造器
	
	它在方法内部使用，即这个方法所属对象的引用
	
	它在构造器内部使用，表示该构造器正在初始化的对象
	
	当在方法内需要调用该方法的对象时，就用this
	
	p.s. public Person（int age, Srting name）{
			this.age = age ; // 此时不能age = age了 因为区别不了
			this.name = name;
	}
	
	所以这里this.就代表的当前类的变量
	
	还有public Person2（int age, String name）{
		this.Person(age, name);
	}
	在这里是调用当前类的方法
	
	使用this能大幅度提高代码可阅读性，
	其中this.() 等同于调用类中的构造器，并且通过()里面的参数类型决定是调用的哪个构造器，若使用这种，必须把该语句放在首行
	且保证构造器不能通过this关键字变相地自己调用自己

33.JavaBean 
	即符合一定标准的Java类：
		类是公共的
		有一个无参的公用构造器
		有属性，一般是私有的
		每个属性都有自己的 set和 get方法
			一般的  void setName（String name）{ this.name = name;}
					String getName（）{return this.name}
		
	快捷的生成 get和set方法的流程：定义属性完成 -> 鼠标右键 -> Source -> Generate Getters and Setters -> OK


34.高级类特性1：继承  extends 关键字
	p.s.
	public class Student extends Person{
		String school;//直接写新加的属性即可，父类的属性不用再写了
	}

	共性代码写父类 -> 然后通过子类继承来省略共性代码 ->每个子类只写自己的特性代码
	
	作用：提高了代码的复用性、让类与类之间产生联系提供了多态的前提、不要仅为了获取其他类中某个功能而去继承
	
	继承要有逻辑关系在里面，不能随意继承，是对父类的一种“扩展”（有截图的）	
	
	且子类不能直接访问父类中private的成员变量和方法，可以通过set和get等方法间接访问
	
	Java只支持单继承，不支持多重继承，即一个子类只能有一个父类，可以多层次的继承

35.方法的重写（Override）
	在子类中可以根据需要对从父类继承的方法进行改造，在执行程序时，子类的方法将 覆盖 父类的方法
	
	子类重写父类的方法 只是重写其方法体
	子类重写时不能用比父类更严格的访问控制
	两者同时时static，或同时不是
	子类的重写抛出的异常不能大于父类的
	
	在当前子类中 界面光标 Alt + / 可以看到重写方法的模板提示
	
	注意重写要区别于重载

36.四种访问权限修饰符的补充（详见截图）  其中同一个包 指的是在同一个包下面的，包里面的包不算

37.关键字super（用法与this相近）
	访问父类中定义的属性
	调用父类中的成员和方法
	尤其当父子类里面出现同名成员时，可以用super进行区分
	
	super的追溯不仅限于父类，可以调用父类之上的所有层级（父类的父类...等）
	
	在子类构造方法可以用super中调用父类的构造器：
		子类中所有的构造器默认调用父类中空参数的构造器
		
		当父类中没有空参数的构造器时（通常是父类中有显式的构造器）
			子类必须通过this或者super指定调用本类或者父类中相应的构造器，且必须放在构造器的第一行
		//通俗理解为父类中如果有显式的了，子类中要用super调用父类中的构造器
		
		如果子类中既没有显式的调用父类或者本类的构造器，且父类中有没有无参的构造器，则会编译出错

38.this和super的区别（详见两张截图总结）
	
39.补充：简单类对象实例化过程（详见截图）
	Person p = new Person();这句的内存执行
	
40.补充：子类对象的实例化过程（详见截图）

41.面向对象的三大特征之二：多态（详见截图）
	多态是面向对象中最重要的概念
	在java中主要体现在两方：方法的重载和重写、对象的多态性（可以直接应用在抽象类和接口上）
	
	重载体现在相同的名称的方法实现了不同的逻辑
	重写体现在子类中同名方法可以覆盖掉父类方法的逻辑
	
	Java引用变量有两个类型：编译时类型和运行时类型，编译时类型由声明该变量时使用的类型所决定，运行时类型由实际赋给该变量的对象决定
	（截图）
	若两者不一致，则会出现多态，这就是对象的多态
	
	对象的多态（截图）：
		一个变量只能有一种确定的数据类型
		一个引用类型变量可以指向多种类型的变量
		Person p = new Person();
		Student s = new Student();
		//这是正常情况
		
		Person e = new Student();//父类的引用对象可以指向子类的实例，即向上转型
		
		子类可以看做是特殊的父类，所以父类的引用可以指向子类的对象，
		即向上转型
		
		如果是向上转型这种方式，该变量不能使用子类中另外定义的成员变量
		因为变量的属性是在编译时确定的，即成员变量不具备多态性，只看引用对象所属的类
		且即使有同名的变量也不会形成覆盖
		
		但是方法的调用是在运行时确定的（截图）
		Person e = new Student();
		e.getInfo();//此时有动态绑定，假设Person和Student都有getInfo()方法，此时调用的确实是Student中的该方法（有截图）
		
		成员方法是有多态性的（成员方法的动态绑定）（前提是需要存在继承或者实现关系、要有方法重写覆盖操作）：
			编译时：要查看引用变量所属的类是否有所调用的方法
			运行时：调用实际对象中所属的类中的重写过的方法

42.instanceof 操作符
	x instanceof A  x是对象 A是类 返回布尔类型，检验x是否为A类的对象（截图） 

	注意子类的对象也是instanceof父类的
	
	还有在Person e = new Student();
	e 是属于 Student类的

43.Object类：基类，是所有Java类的根父类
	所以在定义一个方法时，不确定参数到底是什么类，但是确定参数是一个类，形参就写成Object类
	
	同理在定义一个方法时，不确定参数到底是哪个类，但是知道他是Person类或者是Person的某个子类，
		形参的定义就写成Person类，这样Person和Student等等的对象都可以传进来了，具体是哪个还要看实参属于哪个类
	
	Object类的一些常用的方法（见截图） 
	
	重点讲equals这个方法（截图） 	equals可以在类中重写（练习是在==和equals 二 这一小节里面）
	
	hashCode是获得哈希值
	
	toString打印对象当前内存地址，也可以重写
	当打印一个对象的时候，默认打印的是这个对象的toString方法返回值

44.对象的类型转换（Casting）
	基本数据类型的Casting
	
	对于Java对象的强制类型转换称为造型
	
	从子类到父类的类型转换可以自动进行
	
	Student s = new Student();
	Person p = s;
	
	从父类到子类的类型转换必须通过强制类型转换
		
	Person p = new Person();
	Student s  = (Student) p;
	
	无继承关系的对象之间类型转换是非法的

45. == 与 equals的区别
	
	== 比较基本类型时，两个变量值相等就为true
	   比较对象类型时，只有指向同一个对象时，==才返回true，即比较的是对象的内存地址
	
	equals只能比较对象类型
		比较普通类型的类时，只有指向同一个对象时，才返回true，即比较的是对象的内存地址
		但是像String、File、Date等特殊类时，比较的就是对象内容，即是否值相同
	

46.String多种创建方式的区别（详见截图）
	
	字面量创建String： String s1 = "abc";	
					   String s2 = "abc";
		按照这种形式创建时，总会先在字符串常量池里面找有没有“abc”，没有的话创建一个字符串常量（s1的情况）
		有的话则不再创建，直接将其地址赋给s2，
		这种的实际上s1和s2指向同一个字符串
		
	new的方创建String：String s3 = new String("def");
					   String s4 = new String("def");
		总会先在字符串常量池里面找有没有“def”，一开始没有所以要在字符串常量池里面创建一个“def”,
		然后再在堆中创建一个“def”的字符串并将地址给s3
		在s4时，字符串常量池不做操作，因为已经存在，但是在堆中要再创建一个“def”的字符串并将地址给s4
		这种的实际上s3和s4不会指向同一个字符串
		
	这样，明显地，字面量的方式比new的方式创建省内存	

47.包装类（Wrapper）
	针对八种基本定义相应的引用类型-包装类（封装类），这样就可以使用类中的方法（有截图）
	现在是自动的拆箱和装箱了
	Integer i1 = 112;//自动装箱
	int i0 = i1;//自动拆箱
	
	字符串转化成其他基本类型
	parseXxx()
	int i = Integer.parseInt("123");
	
	基本类型转化成字符串
	String s = String.valueOf(12);
	
	所以，基本数据类型的包装类主要应用 于，基本数据类型与字符串直接的转换

48.static 关键字（静态）（详见截图）
	static可以应用于创造类变量，在类中static修饰的变量，被称为类变量(静态变量)，直接类名.属性名就可以使用，
	是类的一部分，被所有实例化对象共享
	这时候在类外进行初始化时，只需要进行类变量的初始化，然后该类的对象中该变量便全是这个初始化值
	
	因此想让对象共享数据时，也可以说，希望有些变量不因对象的不同而改变，就可以使用类变量
	
	方法也可以使用static，即方法与调用者无关，这样就用static来创造类方法，从而简化方法的调用
	
	用类名.方法直接访问，其中类方法较为常用
	
	比如在做工具类时//其实主要就是应用于此，注意static方法内部不可以用this和super
		p.s.
			String s = "11";
			if(s != null && !s.equals("")){
				return true;//在未来开发中，可能会大量的使用这个判断，大量使用会带来很多重复，
								//因此要把它抽取到一个工具类，做成一个方法
			}


​			
			public class Utils{
				public static boolean isEmpty(Stirng s){
					boolean flag = flase;
					if(s != null && !s.equals("")){
						flag = true;
					}
					return flag;
				｝
			}
			
			使用时，直接 boolean bool = Utils.isEmpty(s);  //类名.方法名
			
	使用范围和修饰后成员特点详见截图
	
	还比如在计数new了多少对象时：//类变量使用起来要慎重
	public class Chinese{
		public static int count;
		public Chinese｛
			this.count++;
		｝
		public static void showCount{
			输出“总共new了” + Chinese.count + "个对象";
		}
	}
	
	最后直接 Chinese.showCount();//实现了计数

49.单例设计模式（分为饿汉模式和懒汉模式）（详见截图，区别在于对象什么时候new这个对象，
				饿汉式是在类加载之后还没有人调用就先new好，以后不论谁调用Instance,都返回这个对象
				懒汉式是在第一次调用Instance方法时new一个，第二次及以后都是返回这个对象）
	
	设计模式就是编程中逐渐总结解决问题的一些套路
	单例：只有一个实例（实例化对象）
	
	适用情况（截图有例子）：一般是new对象太费劲了、或者频频new对象没有必要
	
	饿汉式：
		public class Single1{
			//私有化的构造，构造方法私有化，调用这个类的人就不能直接使用new来创建对象
			private Single1{
		
			}
			//私有的single类型的类变量，也就是new Single1 只new了一次
			private static Single1 single = new Single1();
			
			public static Single1 getInstance(){
				return single;
			}
		}
		
		这样实例化时只能这样实例化：
		Single1 s = Single1.getInstance();
		
		即使现在这样，由于single是static类型的类变量，整个代码运行过程中只有一份，所以不管多少次getInstance
		都是只有这一个single，即s1、s2、s3都是指向这一个对象
		Single1 s1 = Single1.getInstance();
		Single1 s2 = Single1.getInstance();
		Single1 s3 = Single1.getInstance();
	
	懒汉式：
		public class Single2{
			//私有化的构造，构造方法私有化，调用这个类的人就不能直接使用new来创建对象
			private Single2{
		
			}
			//私有的single类型的类变量，因为是类变量
			private static Single2 single = null;
			
			public static Single2 getInstance(){
				if(single == null){
					single = new Single2();//第一次实例化时，要实例化
				}
				return single;//后面所有的实例化，因为single是类变量，共享一个，此时已经是new完的了，直接返回
			}
		}
		
		Single2 s = Single2.getInstance();//只有这一次将single类变量从null变成了new Single2(),完成实例化
		
		Single1 s1 = Single1.getInstance();//后面所有的实际上都是用的第一次的实例化对象
		Single1 s2 = Single1.getInstance();
		Single1 s3 = Single1.getInstance();

44.再理解main方法的语法（看对应小节视频）

45.类的成员之四：初始化块（详见截图）（第一遍没看太懂）
	//非静态的代码块	
	{
		
	}
	//静态代码块
	static
	{
		//里面只能有static修饰的属性和方法
	}
	静态和非静态之间最大的区别是
	在程序运行过程中，非静态代码块每次new对象都有重新执行，但是静态只会执行一次
	
	在实际开发中，static静态代码块用的会多一些，多用于初始化类的静态属性，
	
	应用于匿名对象的初始化（这块再看一遍，并不是太理解）

46.final 关键字
	表示“最终”
		标记的类不能被继承
		标记的方法不能被子类重写
		标记的变量称为常量，只能被赋值一次，且必须显式赋值一次，不能默认赋值
	
47.抽象类abstract class
		用abstract 关键字修饰，可以修饰类，也可以修饰方法
		
		抽象方法没有方法体， 如 abstract int abstractMethod(int a);
		
		含有抽象方法的类必须声明为抽象类，且抽象类不能被实例化（抽象的意义）
		抽象类是用来作为父类被继承的，抽象类的子类必须重写父类的抽象化方法，并提供方法体，否则仍然为抽象类
		
		不可以用abstract来修饰属性，构造器，静态方法，私有方法，final的方法
	
	p.s.举例看截图
		抽象类应用看截图

48.模板方法设计模式（详见截图）
	
	抽象类就是模板方法设计模式的一种体现
	
	应用和 典型实例 详见截图 【这个后面很有可能会用到】

49.接口（特殊的抽象类方式）（详见截图）
	类可以实现多个接口  public class TestInImpl implements TestIn, TestInN
	
	且接口是可以继承接口的 public interface TestInN extends TestIn{}
	
	如果类没有实现接口的所有方法，这个类就要定义为抽象类
	
	定义类的方式 是先写继承 后写实现
	
	会唱歌的厨子的是个老师的例子，看截图，来区分抽象类和接口
	
	Cooking c = new SCTeacher();// 与继承相似，接口与实现类之间存在多态性
	c.fry();
	
	总结：抽象类是对一类事物的高度抽象，其中既有属性又有方法；接口是对方法的抽象，也就是对一系列动作的抽象。
	当需要对一类事物抽象时，应该使用抽象类，当需要对一系列动作抽象，需要使用接口，并这些类去实现相应的接口即可

50.工厂方法
	这个看视频的例子好理解，在合作开发时可能会用到
	
51.内部类
	具体写法看视频和截图
	成员内部类主要是解决Java不能多重继承的问题
	
	p.s.类A现在想同时获得类B和类C的方法，并且重写
	class A｛
		public void testB(){
			new InnerB().testB();
		}
		
		public void testC(){
			new InnerC().testC();
		}
		
		private class InnerB extends B{
			@override 
			public void testB(){
			}	
		}
		private class InnerC extends C{
			@override 
			public void testB(){
			}
		}
	｝
	
	可以使用内部类变相的实现类的多重继承，可以同时继承多个类

52.面向对象总结（详见截图）
	下面开启新的篇章

53.Java的异常
	异常的定义：截图
	举例：数组越界、空指针异常、错误运算（这三个都属于RuntimeException运行时异常，不容易被发现）
	异常类的层次：截图

	有个IOException，里面常见的有：从一个不存在的文件读取数据、越过文件结尾继续读数据EOFException、连接一个不存在的URL

54.	异常处理
	
	对于由于异常使程序中断，可以通过异常处理防止这种中断
	
	主要思路分为：抓、抛
	
	捕获异常：
		try-catch例子：exception_train
		
		try-catch是为了防止程序可能出现的异常，在捕获异常的代码块中，如果前面的代码有异常的，在整个try中就不会执行后面的
		
		也不一定指明异常类型，可以直接Catch(Exception e) 
		
		e.printStackTrace();//跟踪发生异常中会堆栈中信息
		e.getMessage();//可以知道捕获遗产的信息
	
	抛出异常：
		throws声明抛出异常	
		throws例子：exception_train
		
		父类抛出异常，子类重写时也要抛出异常，子类重写的方法不能抛出比父类方法范围更大的异常
		
		还可以人工的抛出异常（例子Test2）
		
		还可以创建用户自定义的异常类（一般不用这样）
	

55.集合
	集合概述（集合篇有好些个截图）
	
	接口：
		Set：无序、不可重复
		List：有序、可重复
		Map:具有映射关系
	
	想让集合存同样类型的对象，就要使用泛型：Set<String> set1 = new HashSet<String>()
	就是那个<>，一般使用泛型来定义<Integer>等等
	
	常用方法：add、remove、clear、contain(i)、size等等
		
		Set与HashSet:
			Set中最主要的是HashSet，运用实例（截图）
			还有一个是TreeSet，主要是元素有了自然排序状态（截图）
			如果是Person的实例对象进行排序，就要让Person类继承一个Comparator接口
			并且重写compare方法（截图）,正序倒序看截图
			且此时应该是Set<Person> set1 = new HashSet<Person>(new Person());
				
		List与ArrayList（截图）
			List<String> list = new ArrayList<String>();
			默认按照添加顺序，安排索引号，可以通过索引来访问元素值
			允许使用重复元素，还有一些对应方法 ： list.add(1, "k"); 
												  list.get(1);
												  list.addAll(2, list2);//插入一个集合
												  indexOf：第一出现的索引
												  lastIndexOf：第二次出现的索引
												  list.set(1,"ff"):修改该元素
												  list.subList(2, 4):截取一段元素形成新的集合，2到3
	
		Map与HashMap类
			Map<String, Integer> map = new HashMap<String, Integer>();
			
			map.put(key, value);
			map.get(key);//根据key取值
			map.remove(key);//根据key移除
			map.size()
			map.containsKey(key);//是否包含指定的key
			map.containsValue(value);//是否包含指定的value
			map.clear();
			map.valuses());//获得map集合中所有value的集合
			
		Map与KeyMap类
			KeyMap能根据key值进行自然排序，与TreeSet类似
			
	Set遍历分为两种：迭代器遍历、增强for（for each）循环遍历
			
	迭代器遍历：Iterator it = set.iterator();
					whiie(it.hasNext()){
						syso(it.next());
					}
	增强for循环遍历：(一般用这个)
				for(Object o : set){//把set的值一次一次取出来，每次都赋给o，知道遍历完成
					syso（o）;
				}
	
	Map不能直接for each遍历，主要用以下两种：
		
		map.keySet();//获取map的key的集合
			Set<String> keys = map.keySet();
			for(String key :keys){
					
			}
				
		Set<Entry<String, Integer>> entrys = map.entrySet();
		for(Entry<String, Integer>en : entrys){
			syso(en.getKey());
			syso(en.getValue());
		}

56.操作集合的工具类：Collections
	例如 Collections.shuffle(List)
	
	重新看一下

57.I/O流

58.File类 

59.泛型
	加入泛型标志符，只能添加指定类型，保证数据安全
	Java中只在编译阶段检查泛型是否有错误，不会到运行阶段
	
	泛型类的使用：不同的泛型类之间不能转换数据
	
	泛型接口的使用：未传入泛型实参，与定义泛型类相同，在声明时，需要将泛型类也加入类中
					如果确定了泛型实参的类型，就是按这个类型走了
	
	泛型方法：几种类型的泛型方法，泛型方法在调用之前没有固定的数据类型，在调用之后会根据传入的类型更改相应的类型

60.通配符 ？ 
	