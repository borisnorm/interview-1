Java 反射是Java语言的一个很重要的特征，它使得Java具体了“动态性”。
 
在Java运行时环境中，对于任意一个类，能否知道这个类有哪些属性和方法？对于任意一个对象，能否调用它的任意一个方法？答案是肯定的。这种动态获取类的信息以及动态调用对象的方法的功能来自于Java 语言的反射（Reflection）机制。
 
Java 反射机制主要提供了以下功能：
在运行时判断任意一个对象所属的类。
在运行时构造任意一个类的对象。
在运行时判断任意一个类所具有的成员变量和方法。
在运行时调用任意一个对象的方法。
 
Reflection 是Java被视为动态（或准动态）语言的一个关键性质。这个机制允许程序在运行时透过Reflection APIs取得任何一个已知名称的class的内部信息，包括其modifiers（诸如public, static 等等）、superclass（例如Object）、实现之interfaces（例如Serializable），也包括fields和methods的所有信息，并可于运行时改变fields内容或调用methods。
 
一般而言，开发者社群说到动态语言，大致认同的一个定义是：“程序运行时，允许改变程序结构或变量类型，这种语言称为动态语言”。从这个观点看，Perl，Python，Ruby是动态语言，C++，Java，C#不是动态语言。
 
尽管在这样的定义与分类下Java不是动态语言，它却有着一个非常突出的动态相关机制：Reflection。这个字的意思是“反射、映象、倒影”，用在Java身上指的是我们可以于运行时加载、探知、使用编译期间完全未知的classes。换句话说，Java程序可以加载一个运行时才得知名称的class，获悉其完整构造（但不包括methods定义），并生成其对象实体、或对其fields设值、或唤起其methods。这种“看透class”的能力（the ability of the program to examine itself）被称为introspection（内省、内观、反省）。Reflection和introspection是常被并提的两个术语。
 
在JDK中，主要由以下类来实现Java反射机制，这些类都位于java.lang.reflect包中：
Class类：代表一个类。
Field 类：代表类的成员变量（成员变量也称为类的属性）。
Method类：代表类的方法。
Constructor 类：代表类的构造方法。
Array类：提供了动态创建数组，以及访问数组的元素的静态方法。
 
下面给出几个例子看看Reflection API的实际运用：
 
一、通过Class类获取成员变量、成员方法、接口、超类、构造方法等
 
在java.lang.Object 类中定义了getClass()方法，因此对于任意一个Java对象，都可以通过此方法获得对象的类型。Class类是Reflection API 中的核心类，它有以下方法
getName()：获得类的完整名字。
getFields()：获得类的public类型的属性。
getDeclaredFields()：获得类的所有属性。
getMethods()：获得类的public类型的方法。
getDeclaredMethods()：获得类的所有方法。
getMethod(String name, Class[] parameterTypes)：获得类的特定方法，name参数指定方法的名字，parameterTypes 参数指定方法的参数类型。
getConstructors()：获得类的public类型的构造方法。
getConstructor(Class[] parameterTypes)：获得类的特定构造方法，parameterTypes 参数指定构造方法的参数类型。
newInstance()：通过类的不带参数的构造方法创建这个类的一个对象。
 
下面给出一个综合运用的例子：
```
public class RefConstructor {
 
    public static void main(String args[]) throws Exception {
        RefConstructor ref = new RefConstructor();
        ref.getConstructor();
 
    }
 
    public void getConstructor() throws Exception {
        Class c = null;
        c = Class.forName("java.lang.Long");
        Class cs[] = {java.lang.String.class};
 
        System.out.println("\n-------------------------------\n");
 
        Constructor cst1 = c.getConstructor(cs);
        System.out.println("1、通过参数获取指定Class对象的构造方法：");
        System.out.println(cst1.toString());
 
        Constructor cst2 = c.getDeclaredConstructor(cs);
        System.out.println("2、通过参数获取指定Class对象所表示的类或接口的构造方法：");
        System.out.println(cst2.toString());
 
        Constructor cst3 = c.getEnclosingConstructor();
        System.out.println("3、获取本地或匿名类Constructor 对象，它表示基础类的立即封闭构造方法。");
        if (cst3 != null) System.out.println(cst3.toString());
        else System.out.println("-- 没有获取到任何构造方法！");
 
        Constructor[] csts = c.getConstructors();
        System.out.println("4、获取指定Class对象的所有构造方法：");
        for (int i = 0; i < csts.length; i++) {
            System.out.println(csts[i].toString());
        }
 
        System.out.println("\n-------------------------------\n");
 
        Type types1[] = c.getGenericInterfaces();
        System.out.println("1、返回直接实现的接口：");
        for (int i = 0; i < types1.length; i++) {
            System.out.println(types1[i].toString());
        }
 
        Type type1 = c.getGenericSuperclass();
        System.out.println("2、返回直接超类：");
        System.out.println(type1.toString());
 
        Class[] cis = c.getClasses();
        System.out.println("3、返回超类和所有实现的接口：");
        for (int i = 0; i < cis.length; i++) {
            System.out.println(cis[i].toString());
        }
 
        Class cs1[] = c.getInterfaces();
        System.out.println("4、实现的接口");
        for (int i = 0; i < cs1.length; i++) {
            System.out.println(cs1[i].toString());
        }
 
        System.out.println("\n-------------------------------\n");
 
        Field fs1[] = c.getFields();
        System.out.println("1、类或接口的所有可访问公共字段：");
        for (int i = 0; i < fs1.length; i++) {
            System.out.println(fs1[i].toString());
        }
 
        Field f1 = c.getField("MIN_VALUE");
        System.out.println("2、类或接口的指定已声明指定公共成员字段：");
        System.out.println(f1.toString());
 
        Field fs2[] = c.getDeclaredFields();
        System.out.println("3、类或接口所声明的所有字段：");
        for (int i = 0; i < fs2.length; i++) {
            System.out.println(fs2[i].toString());
        }
 
        Field f2 = c.getDeclaredField("serialVersionUID");
        System.out.println("4、类或接口的指定已声明指定字段：");
        System.out.println(f2.toString());
 
        System.out.println("\n-------------------------------\n");
 
        Method m1[] = c.getMethods();
        System.out.println("1、返回类所有的公共成员方法：");
        for (int i = 0; i < m1.length; i++) {
            System.out.println(m1[i].toString());
        }
 
        Method m2 = c.getMethod("longValue", new Class[]{});
        System.out.println("2、返回指定公共成员方法：");
        System.out.println(m2.toString());
 
    }
}
```
输出结果：输出结果很长，这里不再给出。
 
 
二、运行时复制对象
 
例程ReflectTester 类进一步演示了Reflection API的基本使用方法。ReflectTester类有一个copy(Object object)方法，这个方法能够创建一个和参数object 同样类型的对象，然后把object对象中的所有属性拷贝到新建的对象中，并将它返回
这个例子只能复制简单的JavaBean，假定JavaBean 的每个属性都有public 类型的getXXX()和setXXX()方法。
```
public class ReflectTester {
    public Object copy(Object object) throws Exception {
        // 获得对象的类型
        Class<?> classType = object.getClass();
        System.out.println("Class:" + classType.getName());
 
        // 通过默认构造方法创建一个新的对象
        Object objectCopy = classType.getConstructor(new Class[]{}).newInstance(new Object[]{});
 
        // 获得对象的所有属性
        Field fields[] = classType.getDeclaredFields();
 
        for (int i = 0; i < fields.length; i++) {
            Field field = fields[i];
 
            String fieldName = field.getName();
            String firstLetter = fieldName.substring(0, 1).toUpperCase();
            // 获得和属性对应的getXXX()方法的名字
            String getMethodName = "get" + firstLetter + fieldName.substring(1);
            // 获得和属性对应的setXXX()方法的名字
            String setMethodName = "set" + firstLetter + fieldName.substring(1);
 
            // 获得和属性对应的getXXX()方法
            Method getMethod = classType.getMethod(getMethodName, new Class[]{});
            // 获得和属性对应的setXXX()方法
            Method setMethod = classType.getMethod(setMethodName, new Class[]{field.getType()});
 
            // 调用原对象的getXXX()方法
            Object value = getMethod.invoke(object, new Object[]{});
            System.out.println(fieldName + ":" + value);
            // 调用拷贝对象的setXXX()方法
            setMethod.invoke(objectCopy, new Object[]{value});
        }
        return objectCopy;
    }
 
    public static void main(String[] args) throws Exception {
        Customer customer = new Customer("Tom", 21);
        customer.setId(new Long(1));
 
        Customer customerCopy = (Customer) new ReflectTester().copy(customer);
        System.out.println("Copy information:" + customerCopy.getId() + " " + customerCopy.getName() + " "
                + customerCopy.getAge());
    }
}
 
class Customer {
    private Long id;
 
    private String name;
 
    private int age;
 
    public Customer() {
    }
 
    public Customer(String name, int age) {
        this.name = name;
        this.age = age;
    }
 
    public Long getId() {
        return id;
    }
 
    public void setId(Long id) {
        this.id = id;
    }
 
    public String getName() {
        return name;
    }
 
    public void setName(String name) {
        this.name = name;
    }
 
    public int getAge() {
        return age;
    }
 
    public void setAge(int age) {
        this.age = age;
    }
}
```
输出结果：
 
Class:com.langsin.reflection.Customer
id:1
name:Tom
age:21
Copy information:1 Tom 21
 
Process finished with exit code 0
 
解说：
ReflectTester 类的copy(Object object)方法依次执行以下步骤
（1）获得对象的类型：
Class classType=object.getClass();
System.out.println("Class:"+classType.getName());
 
（2）通过默认构造方法创建一个新对象：
Object objectCopy=classType.getConstructor(new Class[]{}).newInstance(new Object[]{});
以上代码先调用Class类的getConstructor()方法获得一个Constructor 对象，它代表默认的构造方法，然后调用Constructor对象的newInstance()方法构造一个实例。
 
3）获得对象的所有属性：
Field fields[]=classType.getDeclaredFields();
Class 类的getDeclaredFields()方法返回类的所有属性，包括public、protected、默认和private访问级别的属性
 
（4）获得每个属性相应的getXXX()和setXXX()方法，然后执行这些方法，把原来对象的属性拷贝到新的对象中
 
 
三、用反射机制调用对象的方法
```
public class InvokeTester {
    public int add(int param1, int param2) {
        return param1 + param2;
    }
 
    public String echo(String msg) {
        return "echo: " + msg;
    }
 
    public static void main(String[] args) throws Exception {
        Class<?> classType = InvokeTester.class;
        Object invokeTester = classType.newInstance();
 
        // Object invokeTester = classType.getConstructor(new
        // Class[]{}).newInstance(new Object[]{});
 
 
        //获取InvokeTester类的add()方法
        Method addMethod = classType.getMethod("add", new Class[]{int.class, int.class});
        //调用invokeTester对象上的add()方法
        Object result = addMethod.invoke(invokeTester, new Object[]{new Integer(100), new Integer(200)});
        System.out.println((Integer) result);
 
 
        //获取InvokeTester类的echo()方法
        Method echoMethod = classType.getMethod("echo", new Class[]{String.class});
        //调用invokeTester对象的echo()方法
        result = echoMethod.invoke(invokeTester, new Object[]{"Hello"});
        System.out.println((String) result);
    }
}
```
在例程InvokeTester类的main()方法中，运用反射机制调用一个InvokeTester对象的add()和echo()方法
 
add()方法的两个参数为int 类型，获得表示add()方法的Method对象的代码如下：
Method addMethod=classType.getMethod("add",new Class[]{int.class,int.class});
Method类的invoke(Object obj,Object args[])方法接收的参数必须为对象，如果参数为基本类型数据，必须转换为相应的包装类型的对象。invoke()方法的返回值总是对象，如果实际被调用的方法的返回类型是基本类型数据，那么invoke()方法会把它转换为相应的包装类型的对象，再将其返回。
 
在本例中，尽管InvokeTester 类的add()方法的两个参数以及返回值都是int类型，调用add Method 对象的invoke()方法时，只能传递Integer 类型的参数，并且invoke()方法的返回类型也是Integer 类型，Integer 类是int 基本类型的包装类：
 
Object result=addMethod.invoke(invokeTester,
new Object[]{new Integer(100),new Integer(200)});
System.out.println((Integer)result); //result 为Integer类型
 
 
四、动态创建和访问数组
 
java.lang.Array 类提供了动态创建和访问数组元素的各种静态方法。
 
例程ArrayTester1 类的main()方法创建了一个长度为10 的字符串数组，接着把索引位置为5 的元素设为“hello”，然后再读取索引位置为5 的元素的值
```
public class ArrayTester1 {
    public static void main(String args[]) throws Exception {
        Class<?> classType = Class.forName("java.lang.String");
        // 创建一个长度为10的字符串数组
        Object array = Array.newInstance(classType, 10);
        // 把索引位置为5的元素设为"hello"
        Array.set(array, 5, "hello");
        // 获得索引位置为5的元素的值
        String s = (String) Array.get(array, 5);
        System.out.println(s);
    }
}
```
 
例程ArrayTester2 类的main()方法创建了一个 5 x 10 x 15 的整型数组，并把索引位置为[3][5][10] 的元素的值为设37。
```
public class ArrayTester2 {
    public static void main(String args[]) {
        int[] dims = new int[]{5, 10, 15};
        //创建一个具有指定的组件类型和维度的新数组。
        Object array = Array.newInstance(Integer.TYPE, dims);
       
        Object arrayObj = Array.get(array, 3);
        Class<?> cls = arrayObj.getClass().getComponentType();
        System.out.println(cls);
 
        arrayObj = Array.get(arrayObj, 5);
        Array.setInt(arrayObj, 10, 37);
        int arrayCast[][][] = (int[][][]) array;
        System.out.println(arrayCast[3][5][10]);
    }
}
```
 
深入认识Class类
 
众所周知Java有个Object类，是所有Java类的继承根源，其内声明了数个应该在所有Java类中被改写的方法：hashCode()、equals()、clone()、toString()、getClass()等。其中getClass()返回一个Class类的对象。
 
Class类十分特殊。它和一般classes一样继承自Object，其实体用以表达Java程序运行时的classes和interfaces，也用来表达enum、array、primitive Java types
（boolean, byte, char, short, int, long, float, double）以及关键词void。当一个class被加载，或当加载器（class loader）的defineClass()被JVM调用，JVM 便自动产生一个Class object。如果您想借由“修改Java标准库源码”来观察Class object的实际生成时机（例如在Class的constructor内添加一个println()），不能够！因为Class并没有public constructor
 
Class是Reflection起源。针对任何您想探勘的class，唯有先为它产生一个Class object，接下来才能经由后者唤起为数十多个的Reflection APIs
 
Java允许我们从多种途径为一个class生成对应的Class对象。参看本人的《 深入研究java.long.Class类 》一文。
 
欲生成对象实体，在Reflection 动态机制中有两种作法，一个针对“无自变量ctor”，一个针对“带参数ctor”。如果欲调用的是“带参数ctor“就比较麻烦些，不再调用Class的newInstance()，而是调用Constructor 的newInstance()。首先准备一个Class[]做为ctor的参数类型（本例指定
为一个double和一个int），然后以此为自变量调用getConstructor()，获得一个专属ctor。接下来再准备一个Object[] 做为ctor实参值（本例指定3.14159和125），调用上述专属ctor的newInstance()。
 
动态生成“Class object 所对应之class”的对象实体；无自变量。
 
这个动作和上述调用“带参数之ctor”相当类似。首先准备一个Class[]做为参数类型（本例指定其中一个是String，另一个是Hashtable），然后以此为自变量调用getMethod()，获得特定的Method object。接下来准备一个Object[]放置自变量，然后调用上述所得之特定Method object的invoke()。
为什么获得Method object时不需指定回返类型？
 
因为method overloading机制要求signature必须唯一，而回返类型并非signature的一个成份。换句话说，只要指定了method名称和参数列，就一定指出了一个独一无二的method。
 
 
四、运行时变更field内容
 
与先前两个动作相比，“变更field内容”轻松多了，因为它不需要参数和自变量。首先调用Class的getField()并指定field名称。获得特定的Field object之后便可直接调用Field的get()和set()。
```
public class RefFiled {
    public double x;
    public Double y;
 
    public static void main(String args[]) throws NoSuchFieldException, IllegalAccessException {
        Class c = RefFiled.class;
        Field xf = c.getField("x");
        Field yf = c.getField("y");
 
        RefFiled obj = new RefFiled();
 
        System.out.println("变更前x=" + xf.get(obj));
        //变更成员x值
        xf.set(obj, 1.1);
        System.out.println("变更后x=" + xf.get(obj));
 
        System.out.println("变更前y=" + yf.get(obj));
        //变更成员y值
        yf.set(obj, 2.1);
        System.out.println("变更后y=" + yf.get(obj));
    }
}
```
运行结果：
 
变更前x=0.0
变更后x=1.1
变更前y=null
变更后y=2.1
 
Process finished with exit code 0
