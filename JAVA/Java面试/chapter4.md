# 反射
## 57.什么是反射？
---
JAVA反射机制是在运行状态中，对于任意一个实体类，都能够知道这个类的所有属性和方法；

对于任意一个对象，都能够调用它的任意方法和属性；这种动态获取信息以及动态调用对象方法的功能称为java语言的反射机制。

JAVA反射（放射）机制：“程序运行时，允许改变程序结构或变量类型，这种语言称为动态语言”。从这个观点看，Perl，Python，Ruby是动态语言，C++，Java，C#不是动态语言。但是JAVA有着一个非常突出的动态相关机制：Reflection，用在Java身上指的是我们可以于运行时加载、探知、使用编译期间完全未知的classes。换句话说，Java程序可以加载一个运行时才得知名称的class，获悉其完整构造（但不包括methods定义），并生成其对象实体、或对其fields设值、或唤起其methods。
 
获取String的方法
```
public class DumpMethods {


    public static void main(String[] args) throws Exception{
        
        Class<?> classType = Class.forName("java.lang.String");

        Method[] methods =classType.getDeclaredMethods();

        for (Method method:methods){
            System.out.println(method);
        }

    }


}
```
结果：

<img src="/_markdownimg/反射.png" alt="图片名称" align=center />

通过反射调用方法 
通过反射调用方法。详情见代码及注释：

```
import java.lang.reflect.Method;
public class InvokeTester
{
    public int add(int param1, int param2)
    {
        return param1 + param2;
    }
    public String echo(String message)
    {
        return "Hello: " + message;
    }
    public static void main(String[] args) throws Exception
    {
        // 以前的常规执行手段
        InvokeTester tester = new InvokeTester();
        System.out.println(tester.add(1, 2));
        System.out.println(tester.echo("Tom"));
        System.out.println("---------------------------");

        // 通过反射的方式
        // 第一步，获取Class对象
        // 前面用的方法是：Class.forName()方法获取
        // 这里用第二种方法，类名.class
        Class<?> classType = InvokeTester.class;

        // 生成新的对象：用newInstance()方法
        Object invokeTester = classType.newInstance();
        System.out.println(invokeTester instanceof InvokeTester); // 输出true



        // 通过反射调用方法
        // 首先需要获得与该方法对应的Method对象
        Method addMethod = classType.getMethod("add", new Class[] { int.class,  int.class });

        // 第一个参数是方法名，第二个参数是这个方法所需要的参数的Class对象的数组
        // 调用目标方法
        Object result = addMethod.invoke(invokeTester, new Object[] { 1, 2 });
        System.out.println(result); // 此时result是Integer类型

        //调用第二个方法
        Method echoMethod = classType.getDeclaredMethod("echo", new Class[]{String.class});
        Object result2 = echoMethod.invoke(invokeTester, new Object[]{"Tom"});
        System.out.println(result2);
    }
}
```

## 58.什么是 java 序列化？什么情况下需要序列化？
---
序列化：将 Java 对象转换成字节流的过程。

反序列化：将字节流转换成 Java 对象的过程。

当 Java 对象需要在网络上传输 或者 持久化存储到文件中时，就需要对 Java 对象进行序列化处理。

序列化的实现：类实现 Serializable 接口，这个接口没有需要实现的方法。实现 Serializable 接口是为了告诉 jvm 这个类的对象可以被序列化。

注意事项：

某个类可以被序列化，则其子类也可以被序列化
声明为 static 和 transient 的成员变量，不能被序列化。static 成员变量是描述类级别的属性，transient 表示临时数据
反序列化读取序列化对象的顺序要保持一致
```
package constxiong.interview;
 
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.ObjectInputStream;
import java.io.ObjectOutputStream;
import java.io.Serializable;
 
/**
 * 测试序列化，反序列化
 * @author ConstXiong
 * @date 2019-06-17 09:31:22
 */
public class TestSerializable implements Serializable {
 
	private static final long serialVersionUID = 5887391604554532906L;
	
	private int id;
	
	private String name;
 
	public TestSerializable(int id, String name) {
		this.id = id;
		this.name = name;
	}
	
	@Override
	public String toString() {
		return "TestSerializable [id=" + id + ", name=" + name + "]";
	}
 
	@SuppressWarnings("resource")
	public static void main(String[] args) throws IOException, ClassNotFoundException {
		//序列化
		ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("TestSerializable.obj"));
		oos.writeObject("测试序列化");
		oos.writeObject(618);
		TestSerializable test = new TestSerializable(1, "ConstXiong");
		oos.writeObject(test);
		
		//反序列化
		ObjectInputStream ois = new ObjectInputStream(new FileInputStream("TestSerializable.obj"));
		System.out.println((String)ois.readObject());
		System.out.println((Integer)ois.readObject());
		System.out.println((TestSerializable)ois.readObject());
	}
 
}
```

打印结果

```
测试序列化
618
TestSerializable [id=1, name=ConstXiong]
```

## 59.动态代理是什么？有哪些应用？
---
动态代理：在运行时，创建目标类，可以调用和扩展目标类的方法。

Java 中实现动态的方式：JDK 中的动态代理 和 Java类库 CGLib。

应用场景如：

统计每个 api 的请求耗时
统一的日志输出
校验被调用的 api 是否已经登录和权限鉴定
Spring的 AOP 功能模块就是采用动态代理的机制来实现切面编程

## 60.怎么实现动态代理？
---
之前虽然会用JDK的动态代理，但是有些问题却一直没有搞明白。比如说：InvocationHandler的invoke方法是由谁来调用的，代理对象是怎么生成的，直到前几个星期才把这些问题全部搞明白了。 
废话不多说了，先来看一下JDK的动态是怎么用的。 
```
package dynamic.proxy; 

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

/**
 * 实现自己的InvocationHandler
 * @author zyb
 * @since 2012-8-9
 *
 */
public class MyInvocationHandler implements InvocationHandler {
	
	// 目标对象 
	private Object target;
	
	/**
	 * 构造方法
	 * @param target 目标对象 
	 */
	public MyInvocationHandler(Object target) {
		super();
		this.target = target;
	}


	/**
	 * 执行目标对象的方法
	 */
	public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
		
		// 在目标对象的方法执行之前简单的打印一下
		System.out.println("------------------before------------------");
		
		// 执行目标对象的方法
		Object result = method.invoke(target, args);
		
		// 在目标对象的方法执行之后简单的打印一下
		System.out.println("-------------------after------------------");
		
		return result;
	}

	/**
	 * 获取目标对象的代理对象
	 * @return 代理对象
	 */
	public Object getProxy() {
		return Proxy.newProxyInstance(Thread.currentThread().getContextClassLoader(), 
				target.getClass().getInterfaces(), this);
	}
}

package dynamic.proxy;

/**
 * 目标对象实现的接口，用JDK来生成代理对象一定要实现一个接口
 * @author zyb
 * @since 2012-8-9
 *
 */
public interface UserService {

	/**
	 * 目标方法 
	 */
	public abstract void add();

}

package dynamic.proxy; 

/**
 * 目标对象
 * @author zyb
 * @since 2012-8-9
 *
 */
public class UserServiceImpl implements UserService {

	/* (non-Javadoc)
	 * @see dynamic.proxy.UserService#add()
	 */
	public void add() {
		System.out.println("--------------------add---------------");
	}
}

package dynamic.proxy; 

import org.junit.Test;

/**
 * 动态代理测试类
 * @author zyb
 * @since 2012-8-9
 *
 */
public class ProxyTest {

	@Test
	public void testProxy() throws Throwable {
		// 实例化目标对象
		UserService userService = new UserServiceImpl();
		
		// 实例化InvocationHandler
		MyInvocationHandler invocationHandler = new MyInvocationHandler(userService);
		
		// 根据目标对象生成代理对象
		UserService proxy = (UserService) invocationHandler.getProxy();
		
		// 调用代理对象的方法
		proxy.add();
		
	}
}
```

执行结果如下:

```
-------before----------
---------add--------
---------after--------
```

用起来是很简单吧，其实这里基本上就是AOP的一个简单实现了，在目标对象的方法执行之前和执行之后进行了增强。Spring的AOP实现其实也是用了Proxy和InvocationHandler这两个东西的。 
用起来是比较简单，但是如果能知道它背后做了些什么手脚，那就更好不过了。首先来看一下JDK是怎样生成代理对象的。既然生成代理对象是用的Proxy类的静态方newProxyInstance，那么我们就去它的源码里看一下它到底都做了些什么？

```
/** 
 * loader:类加载器 
 * interfaces:目标对象实现的接口 
 * h:InvocationHandler的实现类 
 */  
public static Object newProxyInstance(ClassLoader loader,  
                      Class<?>[] interfaces,  
                      InvocationHandler h)  
    throws IllegalArgumentException  
    {  
    if (h == null) {  
        throw new NullPointerException();  
    }  
  
    /* 
     * Look up or generate the designated proxy class. 
     */  
    Class cl = getProxyClass(loader, interfaces);  
  
    /* 
     * Invoke its constructor with the designated invocation handler. 
     */  
    try {  
            // 调用代理对象的构造方法（也就是$Proxy0(InvocationHandler h)）  
        Constructor cons = cl.getConstructor(constructorParams);  
            // 生成代理类的实例并把MyInvocationHandler的实例传给它的构造方法  
        return (Object) cons.newInstance(new Object[] { h });  
    } catch (NoSuchMethodException e) {  
        throw new InternalError(e.toString());  
    } catch (IllegalAccessException e) {  
        throw new InternalError(e.toString());  
    } catch (InstantiationException e) {  
        throw new InternalError(e.toString());  
    } catch (InvocationTargetException e) {  
        throw new InternalError(e.toString());  
    }  
    }  
```

我们再进去getProxyClass方法看一下 

```
public static Class<?> getProxyClass(ClassLoader loader, 
                                         Class<?>... interfaces)
	throws IllegalArgumentException
    {
	// 如果目标类实现的接口数大于65535个则抛出异常（我XX，谁会写这么NB的代码啊？）
	if (interfaces.length > 65535) {
	    throw new IllegalArgumentException("interface limit exceeded");
	}

	// 声明代理对象所代表的Class对象（有点拗口）
	Class proxyClass = null;

	String[] interfaceNames = new String[interfaces.length];

	Set interfaceSet = new HashSet();	// for detecting duplicates

	// 遍历目标类所实现的接口
	for (int i = 0; i < interfaces.length; i++) {
	    
		// 拿到目标类实现的接口的名称
	    String interfaceName = interfaces[i].getName();
	    Class interfaceClass = null;
	    try {
		// 加载目标类实现的接口到内存中
		interfaceClass = Class.forName(interfaceName, false, loader);
	    } catch (ClassNotFoundException e) {
	    }
	    if (interfaceClass != interfaces[i]) {
		throw new IllegalArgumentException(
		    interfaces[i] + " is not visible from class loader");
	    }

		// 中间省略了一些无关紧要的代码 .......
		
		// 把目标类实现的接口代表的Class对象放到Set中
	    interfaceSet.add(interfaceClass);

	    interfaceNames[i] = interfaceName;
	}

	// 把目标类实现的接口名称作为缓存（Map）中的key
	Object key = Arrays.asList(interfaceNames);

	Map cache;
	
	synchronized (loaderToCache) {
		// 从缓存中获取cache
	    cache = (Map) loaderToCache.get(loader);
	    if (cache == null) {
		// 如果获取不到，则新建地个HashMap实例
		cache = new HashMap();
		// 把HashMap实例和当前加载器放到缓存中
		loaderToCache.put(loader, cache);
	    }

	}

	synchronized (cache) {

	    do {
		// 根据接口的名称从缓存中获取对象
		Object value = cache.get(key);
		if (value instanceof Reference) {
		    proxyClass = (Class) ((Reference) value).get();
		}
		if (proxyClass != null) {
		    // 如果代理对象的Class实例已经存在，则直接返回
		    return proxyClass;
		} else if (value == pendingGenerationMarker) {
		    try {
			cache.wait();
		    } catch (InterruptedException e) {
		    }
		    continue;
		} else {
		    cache.put(key, pendingGenerationMarker);
		    break;
		}
	    } while (true);
	}

	try {
	    // 中间省略了一些代码 .......
		
		// 这里就是动态生成代理对象的最关键的地方
		byte[] proxyClassFile =	ProxyGenerator.generateProxyClass(
		    proxyName, interfaces);
		try {
			// 根据代理类的字节码生成代理类的实例
		    proxyClass = defineClass0(loader, proxyName,
			proxyClassFile, 0, proxyClassFile.length);
		} catch (ClassFormatError e) {
		    throw new IllegalArgumentException(e.toString());
		}
	    }
	    // add to set of all generated proxy classes, for isProxyClass
	    proxyClasses.put(proxyClass, null);

	} 
	// 中间省略了一些代码 .......
	
	return proxyClass;
    }

```

进去ProxyGenerator类的静态方法generateProxyClass，这里是真正生成代理类class字节码的地方。

```
 public static byte[] generateProxyClass(final String name,
                                            Class[] interfaces)
    {
        ProxyGenerator gen = new ProxyGenerator(name, interfaces);
		// 这里动态生成代理类的字节码，由于比较复杂就不进去看了
        final byte[] classFile = gen.generateClassFile();

		// 如果saveGeneratedFiles的值为true，则会把所生成的代理类的字节码保存到硬盘上
        if (saveGeneratedFiles) {
            java.security.AccessController.doPrivileged(
            new java.security.PrivilegedAction<Void>() {
                public Void run() {
                    try {
                        FileOutputStream file =
                            new FileOutputStream(dotToSlash(name) + ".class");
                        file.write(classFile);
                        file.close();
                        return null;
                    } catch (IOException e) {
                        throw new InternalError(
                            "I/O exception saving generated file: " + e);
                    }
                }
            });
        }

		// 返回代理类的字节码
        return classFile;
    }

```

现在，JDK是怎样动态生成代理类的字节的原理已经一目了然了。 
好了，再来解决另外一个问题，那就是由谁来调用InvocationHandler的invoke方法的。要解决这个问题就要看一下JDK到底为我们生成了一个什么东西。用以下代码可以获取到JDK为我们生成的字节码并写到硬盘中。

```
package dynamic.proxy; 

import java.io.FileOutputStream;
import java.io.IOException;

import sun.misc.ProxyGenerator;

/**
 * 代理类的生成工具
 * @author zyb
 * @since 2012-8-9
 */
public class ProxyGeneratorUtils {

	/**
	 * 把代理类的字节码写到硬盘上
	 * @param path 保存路径
	 */
	public static void writeProxyClassToHardDisk(String path) {
		// 第一种方法，这种方式在刚才分析ProxyGenerator时已经知道了
		// System.getProperties().put("sun.misc.ProxyGenerator.saveGeneratedFiles", true);
		
		// 第二种方法
		
		// 获取代理类的字节码
		byte[] classFile = ProxyGenerator.generateProxyClass("$Proxy11", UserServiceImpl.class.getInterfaces());
		
		FileOutputStream out = null;
		
		try {
			out = new FileOutputStream(path);
			out.write(classFile);
			out.flush();
		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			try {
				out.close();
			} catch (IOException e) {
				e.printStackTrace();
			}
		}
	}
}

package dynamic.proxy; 

import org.junit.Test;

/**
 * 动态代理测试类
 * @author zyb
 * @since 2012-8-9
 *
 */
public class ProxyTest {

	@Test
	public void testProxy() throws Throwable {
		// 实例化目标对象
		UserService userService = new UserServiceImpl();
		
		// 实例化InvocationHandler
		MyInvocationHandler invocationHandler = new MyInvocationHandler(userService);
		
		// 根据目标对象生成代理对象
		UserService proxy = (UserService) invocationHandler.getProxy();
		
		// 调用代理对象的方法
		proxy.add();
		
	}
	
	@Test
	public void testGenerateProxyClass() {
		ProxyGeneratorUtils.writeProxyClassToHardDisk("F:/$Proxy11.class");
	}
}

```

通过以上代码，就可以在F盘上生成一个$Proxy.class文件了，现在用反编译工具来看一下这个class文件里面的内容。 

```
// Decompiled by DJ v3.11.11.95 Copyright 2009 Atanas Neshkov  Date: 2012/8/9 20:11:32
// Home Page: http://members.fortunecity.com/neshkov/dj.html  http://www.neshkov.com/dj.html - Check often for new version!
// Decompiler options: packimports(3) 

import dynamic.proxy.UserService;
import java.lang.reflect.*;

public final class $Proxy11 extends Proxy
    implements UserService
{

	// 构造方法，参数就是刚才传过来的MyInvocationHandler类的实例
    public $Proxy11(InvocationHandler invocationhandler)
    {
        super(invocationhandler);
    }

    public final boolean equals(Object obj)
    {
        try
        {
            return ((Boolean)super.h.invoke(this, m1, new Object[] {
                obj
            })).booleanValue();
        }
        catch(Error _ex) { }
        catch(Throwable throwable)
        {
            throw new UndeclaredThrowableException(throwable);
        }
    }

	/**
	 * 这个方法是关键部分
	 */
    public final void add()
    {
        try
        {
			// 实际上就是调用MyInvocationHandler的public Object invoke(Object proxy, Method method, Object[] args)方法，第二个问题就解决了
            super.h.invoke(this, m3, null);
            return;
        }
        catch(Error _ex) { }
        catch(Throwable throwable)
        {
            throw new UndeclaredThrowableException(throwable);
        }
    }

    public final int hashCode()
    {
        try
        {
            return ((Integer)super.h.invoke(this, m0, null)).intValue();
        }
        catch(Error _ex) { }
        catch(Throwable throwable)
        {
            throw new UndeclaredThrowableException(throwable);
        }
    }

    public final String toString()
    {
        try
        {
            return (String)super.h.invoke(this, m2, null);
        }
        catch(Error _ex) { }
        catch(Throwable throwable)
        {
            throw new UndeclaredThrowableException(throwable);
        }
    }

    private static Method m1;
    private static Method m3;
    private static Method m0;
    private static Method m2;

	// 在静态代码块中获取了4个方法：Object中的equals方法、UserService中的add方法、Object中的hashCode方法、Object中toString方法
    static 
    {
        try
        {
            m1 = Class.forName("java.lang.Object").getMethod("equals", new Class[] {
                Class.forName("java.lang.Object")
            });
            m3 = Class.forName("dynamic.proxy.UserService").getMethod("add", new Class[0]);
            m0 = Class.forName("java.lang.Object").getMethod("hashCode", new Class[0]);
            m2 = Class.forName("java.lang.Object").getMethod("toString", new Class[0]);
        }
        catch(NoSuchMethodException nosuchmethodexception)
        {
            throw new NoSuchMethodError(nosuchmethodexception.getMessage());
        }
        catch(ClassNotFoundException classnotfoundexception)
        {
            throw new NoClassDefFoundError(classnotfoundexception.getMessage());
        }
    }
}

```