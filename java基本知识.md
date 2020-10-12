##### <T< 和<?>的区别

```
<T> 和<?>的区别
<T>是参数类型，常常用于泛型类或泛型方法的定义；<?>是通配符，一般不能直接用来定义类或泛型方法，因为它不能直接参与操作，常常用于泛型方法的调用代码或泛型方法的型参。

  <T>在List、Set、Map中经常见到，用来限制Class中的参数类型，确保Class中参数的一致性。例如：List<String> list = new ArrayList<>();创建了一个内部参数是String类型的类，list中的操作对象都是String。<?>代表任意java类型，只有在不关心数据的具体类型下才使用通配符表示，但在一些情况下，需要将<?>传入的数据进行强转，但这样不如直接传入<T>。
  public class Test1 {
 
    public <T> void test1(List<T> list){
        System.out.println("== test1 output ==");
        System.out.println("list length: " + list.size());  //
        if (!list.isEmpty()) {
            T t = list.get(0); //list中的元素为T类型
            System.out.println("t = " + t);//
        }
    }
 
    public void test2(List<?> list){
        System.out.println("== test2 output ==");
        System.out.println("list length: " + list.size());//
        if (!list.isEmpty()) {
            Object o = list.get(0);//list中的元素为Object
            System.out.println("o = " + o);//
        }
    }
}
 另外除了<?>，还有<? extends T>上界通配符和<? super T>下界通配符。<? extends T> 表示传入数据值需要是T类型或T的子类，<? suprt T>表示传入数据值需要是T类型或T的超类。详细例子见java泛型知识--博客园

一般来说，<?>主要用于变量上，<T>主要用于类或方法上。下图中，list的元素类型为？，但往里边添加String时，会显示出错，因为list中的类型是一个未知的java类型，不属于任何类，所以往里边添加数据时会出错。但可以从list中取出数据，取出的数据类型为Object。
```

<img src="https://img2020.cnblogs.com/i-beta/1574406/202003/1574406-20200310172716103-773603153.png" alt="img" style="zoom:50%;" />

<img src="https://img2020.cnblogs.com/i-beta/1574406/202003/1574406-20200310173246791-1567948311.png" alt="img" style="zoom:50%;" />

##### <T< T和T的用法

```
<T> T和T的用法
泛型（Generic type 或者 generics）是对 Java 语言的类型系统的一种扩展，以支持创建可以按类型进行参数化的类。可以把类型参数看作是使用参数化类型时指定的类型的一个占位符，就像方法的形式参数是运行时传递的值的占位符一样。
       在集合框架（Collection framework）中泛型的身影随处可见。例如，Map 类允许向一个 Map 类型的实例添加任意类的对象，即使最常见的情况在给定映射（map）中保存一个string键值对
	命名类型参数:
       对于常见的泛型模式，推荐的泛型类型变量：
E：元素（Element），多用于java集合框架
K：关键字（Key）
N：数字（Number）
T：类型（Type）
V：值（Value）
大家都知道，Java的泛型是伪泛型，这是因为Java在编译期间，所有的泛型信息都会被擦除，正确理解泛型概念的首要前提是理解类型擦除。Java 泛型是如何工作的？什么是类型擦除？答：泛型是通过类型擦除来实现的，编译器在编译时擦除了所有泛型类型相关的信息，所以在运行时不存在任何泛型类型相关的信息，譬如 List<Integer> 在运行时仅用一个 List 来表示，这样做的动机是兼容 Java 1.5 之前版本。
       泛型擦除具体来说就是在编译成字节码时首先进行类型检查，然后进行类型擦除（即所有类型参数都用他们的限定类型替换，包括类、变量和方法），最后如果类型擦除和多态性发生冲突，就在子类中使用桥接方法解决；如果调用泛型方法的返回类型被擦除，则在调用该方法时插入强制类型转换。在类型擦除中，编译器确保不会创建额外的类，并且没有运行时开销。
	类型擦除原则：
1.用通用类型的类型参数替换其绑定的有界类型参数；
2.如果使用无界类型参数，则使用Object替换类型参数；
3.插入类型转换以实现类型安全；
4.生成桥接方法以在扩展通用类型中保持多态。
<T> T 和T的区别：T是Type的首字母缩写；<T> T 表示“返回值”是一个泛型，传入什么类型，就返回什么类型；而单独的“T”表示限制传入的参数类型。
<T> T 的用法
这个<T> T 表示返回值T的类型是泛型，T是一个占位符，用来告诉编译器，这个东西是先给我留着， 等我编译的时候再告诉你是什么类型。
import org.springframework.util.CollectionUtils;
import java.util.ArrayList;
import java.util.List;
public class Demo {
    public static void main(String[] args) {
        Demo demo = new Demo();
        //获取string类型
        List<String> array = new ArrayList<String>();
        array.add("test");
        array.add("doub");
        String str = demo.getListFisrt(array);
        System.out.println(str);
        //获取Integer类型
        List<Integer> nums = new ArrayList<Integer>();
        nums.add(31);
        nums.add(32);
        Integer num = demo.getListFisrt(nums);
        System.out.println(num);
    }

    /**
     * 这个<T> T 可以传入任何类型的List
     * 关于参数T
     * 第一个 表示是泛型
     * 第二个 表示返回的是T类型的数据
     * 第三个 限制参数类型为T
     *
     * @param data
     * @return
     */
    private <T> T getListFisrt(List<T> data) {
        if (CollectionUtils.isEmpty(data)) {
            return null;
        }
        return data.get(0);
    }
}
T 的用法
import java.util.ArrayList;
import java.util.List;

public class Demo2<T> {

    public static void main(String[] args) {
        //限制T 为String 类型
        Demo2<String> demo = new Demo2<String>();
        List<String> array = new ArrayList<String>();
        array.add("Tom");
        array.add("河南");
        String str = demo.getListFisrt(array);
        System.out.println(str);

        //获取Integer类型
        Demo2<Integer> demo2 = new Demo2<Integer>();
        List<Integer> nums = new ArrayList<Integer>();
        nums.add(12);
        nums.add(13);
        Integer num = demo2.getListFisrt(nums);
        System.out.println(num);
    }

    /**
     * 这个只能传递T类型的数据
     * 返回值 就是Demo<T> 实例化传递的对象类型
     *
     * @param data
     * @return
     */
    private T getListFisrt(List<T> data) {
        if (data == null || data.size() == 0) {
            return null;
        }
        return data.get(0);
    }
}

public <T> List<T> query(String sql, Class<T> clazz, Object[] params) {//查询所有
        List beans = null;
        Connection conn = null;
        try {
            conn = getConnection();
            QueryRunner qRunner = new QueryRunner();
            beans = (List) qRunner.query(conn, sql, new BeanListHandler<T>(clazz), params);
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            DbUtils.closeQuietly(conn);
        }
        return beans;
    }
    
public <T> T get(String sql, Class<T> clazz, Object[] params) {//根据条件查询一条数据
        T obj = null;
        Connection conn = null;
        try {
            conn = getConnection();
            QueryRunner qRunner = new QueryRunner();//QueryRunner类似Statement
            obj = qRunner.query(conn, sql, new BeanHandler<T>(clazz), params);//BeanHandler封装对象
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            DbUtils.closeQuietly(conn);
        }
        return obj;
    }
```



##### Cs系统：学生管理系统	1. 采用的技术是什么？

```
 JDBD——DBUtils——MySQL——Swing（Frame窗体）
```

##### 2. 架构是什么？

```
 CS架构
```

#####  3.目录结构是什么？

```
三层： 
展示层：窗体 作用：发起请求获取数据调用service  

业务层：service  作用：处理业务

数据访问层:dao、po  

发起请求获取数据等——Service业务逻辑——dao、po连接数据库——数据返回service——数据返回到窗体展示出来

用到事务：service层 原因：如果写在dao层，MySQL数据库默认自动提交，所以得卸载业务层。

 

MVC：view视图：给用户看,jsp、html...

   Controller控制层：控制请求，发起处理请求。

   Model：所对应的是service、dao（业务层和数据访问层）

 String str =”123”;

 String str =”123”

If str==str
```

##### equals和==的区别？

```
前者是比较两个数的数值，后者是比较地址
```

##### For循环for each

```
For循环的条件
1、初始值
2、条件
3、自增
```

##### 重写与重载的区别？

```
重写继承  
抽象类和接口
 Abstract 一个了类中出现了抽象方法就是抽象类，一个抽象类中可以有不同的抽象方法
重写方法名返回值相同参数相同
重载方法名相同返回值相同参数可以不同，个数可以不同
1.8后接口允许出现有方法体的方法，继承重写方法，接口实现
```

##### Java的继承的规则是怎样的？

```
单一继承，一个子类只能继承一个父类，一个父类可以被多个子类继承。

接口可以多重实现，一个子类可以实现多个接口，实现多个用逗号隔开，同时可以去继承一个父类，在继承和实现中的规则是先继承后实现。  extends D implements A，C
```

##### 什么是数组？

```
存储多个数据，只能存储单一类型的数据。
Int t[] a ={1,2,3}
Int [] a = new[5]
长度为5,根据下标取值。
一维数组
二维数组 在一维数组中有添加了新的数组
Int [] [] a ={{1,2,3},{4,5,6}}
Int [] [] a =new int[3] [3]  3行3列
```

##### 集合？

```
在jdk中两个。Connection: List有序集合:ArrayList、LinkList、vector set无序集合：  Map集合无父类：hashMap、treeMap、hashTable

hashMap根据键值对存储 （Put（key,values））获取get  key不允许为空 value 允许为空

Hashtable  key和value 不允许为空，value可以重复
```

##### 什么是线程？

```
多线程：用到哪一个哪一个被唤醒，例如：和多人聊天 sleep 生产者和消费者

进程：程序就是进程，例如：电脑的各个图标

线程和进程的关系：一个进程由多个线程支撑运行，
```

##### Integer类型的重点

```java
Integer a=1000,b=1000;
System.out.println(a==b);
上述代码返回false，因为
```

![image-20200923091652457](C:\Users\杨睿卿\AppData\Roaming\Typora\typora-user-images\image-20200923091652457.png)

```java
IntegerCache.low = -128
IntegerCache.high = 127
    所以上面`Integer a = 1000,b = 1000;`其实都是`new Integer(1000);`所以分配的内存地址肯定不一样,所以`==`比较就成`false`了
```

![image-20200923092035479](C:\Users\杨睿卿\AppData\Roaming\Typora\typora-user-images\image-20200923092035479.png)ja

##### java逻辑编程 流程控制

```
循环语句
```

