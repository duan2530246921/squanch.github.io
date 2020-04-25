# 异常
## 74.throw 和 throws 的区别？
---
- throw：
    表示方法内抛出某种异常对象
    如果异常对象是非 RuntimeException 则需要在方法申明时加上该异常的抛出 即需要加上 throws 语句 或者 在方法体内 try catch 处理该异常，否则编译报错
    执行到 throw 语句则后面的语句块不再执行
- throws：
    方法的定义上使用 throws 表示这个方法可能抛出某种异常
    需要由方法的调用者进行异常处理

```
package constxiong.interview;
 
import java.io.IOException;
 
public class TestThrowsThrow {
 
	public static void main(String[] args) {
		testThrows();
		
		Integer i = null;
		testThrow(i);
		
		String filePath = null;
		try {
			testThrow(filePath);
		} catch (IOException e) {
			e.printStackTrace();
		}
	}
	
	/**
	 * 测试 throws 关键字
	 * @throws NullPointerException
	 */
	public static void testThrows() throws NullPointerException {
		Integer i = null;
		System.out.println(i + 1);
	}
	
	/**
	 * 测试 throw 关键字抛出 运行时异常
	 * @param i
	 */
	public static void testThrow(Integer i) {
		if (i == null) {
			throw new NullPointerException();//运行时异常不需要在方法上申明
		}
	}
	
	/**
	 * 测试 throw 关键字抛出 非运行时异常，需要方法体需要加 throws 异常抛出申明
	 * @param i
	 */
	public static void testThrow(String filePath) throws IOException {
		if (filePath == null) {
			throw new IOException();//运行时异常不需要在方法上申明
		}
	}
}
```

## 75.final、finally、finalize 有什么区别？
---
- final
    final修饰类，表示该类不可以被继承
    final修饰变量，表示该变量不可以被修改，只允许赋值一次
    final修饰方法，表示该方法不可以被重写
- finally
    finally是java保证代码一定要被执行的一种机制。
    比如try-finally或try-catch-finally，用来关闭JDBC连接资源，用来解锁等等
- finalize
    finalize是Object的一个方法，它的目的是保证对象在被垃圾收集前完成特定资源的回收。
    不过finalize已经不推荐使用，JDK9已经标记为过时。

## 76.try-catch-finally 中哪个部分可以省略？
---
catch 和 finally 语句块可以省略其中一个。

```
package constxiong.interview;
 
public class TestOmitTryCatchFinally {
 
	public static void main(String[] args) {
		omitFinally();
		omitCatch();
	}
	
	/**
	 * 省略finally 语句块
	 */
	public static void omitFinally() {
		try {
			int i = 0;
			i += 1;
			System.out.println(i);
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
	
	/**
	 * 省略 catch 语句块
	 */
	public static void omitCatch() {
		int i = 0;
		try {
			i += 1;
		} finally {
			i = 10;
		}
		System.out.println(i);
	}
}
```

## 77.try-catch-finally 中，如果 catch 中 return 了，finally 还会执行吗？
---
1. 不管有没有异常，finally中的代码都会执行
2. 当try、catch中有return时，finally中的代码依然会继续执行
3. finally是在return后面的表达式运算之后执行的，此时并没有返回运算之后的值，而是把值保存起来，不管finally对该值做任何的改变，返回的值都不会改变，依然返回保存起来的值。也就是说方法的返回值是在finally运算之前就确定了的。
4. 如果return的数据是引用数据类型，而在finally中对该引用数据类型的属性值的改变起作用，try中的return语句返回的就是在finally中改变后的该属性的值。
5. finally代码中最好不要包含return，程序会提前退出，也就是说返回的值不是try或catch中的值
先执行try中的语句，包括return后面的表达式，
有异常时,先执行catch中的语句，包括return后面的表达式,
然后执行finally中的语句,如果finally里面有return语句，会提前退出，
最后执行try中的return，有异常时执行catch中的return。
在执行try、catch中的return之前一定会执行finally中的代码（如果finally存在），如果finally中有return语句，就会直接执行finally中的return方法，所以finally中的return语句一定会被执行的。编译器把finally中的return语句标识为一个warning.

```
// 当try、catch中有return时，finally中的代码会执行么？
public class TryCatchTest {

	public static void main(String[] args) {
//		System.out.println("return的返回值：" + test());
//		System.out.println("包含异常return的返回值：" + testWithException());
		System.out.println("return的返回值：" + testWithObject().age); // 测试返回值类型是对象时
	}

	// finally是在return后面的表达式运算之后执行的，此时并没有返回运算之后的值
	//，而是把值保存起来，不管finally对该值做任何的改变，返回的值都不会改变，依然返回保存起来的值。
	//也就是说方法的返回值是在finally运算之前就确定了的。
	static int test() {
		int x = 1;
		try {
			return x++;
		} catch(Exception e){
			
		}finally {
			System.out.println("finally:" + x);
			++x;
			System.out.println("++x:" + x);
			// finally代码中最好不要包含return，程序会提前退出，
			// 也就是说返回的值不是try或catch中的值
			// return x;
		}
		return x;
	}
	
	static int testWithException(){
		Integer x = null;
		try {
			x.intValue(); // 造个空指针异常
			return x++;
		} catch(Exception e){
			System.out.println("catch:" + x);
			x = 1;
			return x; // 返回1
//			return ++x; // 返回2
		}finally {
			x = 1;
			System.out.println("finally:" + x);
			++x;
			System.out.println("++x:" + x);
			// finally代码中最好不要包含return，程序会提前退出，
			// 也就是说返回的值不是try或catch中的值
//			 return x;
		}
	}
	
	static Num testWithObject() {
		Num num = new Num();
		try {
			return num;
		} catch(Exception e){
			
		}finally {
			num.age++; // 改变了引用对象的值
			System.out.println("finally:" + num.age);
		}
		return num;
	}
	
	static class Num{
		public int age;
	}

}
```

## 78.常见的异常类有哪些？
---
1. NullPointerException 当应用程序试图访问空对象时，则抛出该异常。
2. SQLException 提供关于数据库访问错误或其他错误信息的异常。
3. IndexOutOfBoundsException指示某排序索引（例如对数组、字符串或向量的排序）超出范围时抛出。 
4. NumberFormatException当应用程序试图将字符串转换成一种数值类型，但该字符串不能转换为适当格式时，抛出该异常。
5. FileNotFoundException当试图打开指定路径名表示的文件失败时，抛出此异常。
6. IOException当发生某种I/O异常时，抛出此异常。此类是失败或中断的I/O操作生成的异常的通用类。
7. ClassCastException当试图将对象强制转换为不是实例的子类时，抛出该异常。
8. ArrayStoreException试图将错误类型的对象存储到一个对象数组时抛出的异常。
9. IllegalArgumentException 抛出的异常表明向方法传递了一个不合法或不正确的参数。
10. ArithmeticException当出现异常的运算条件时，抛出此异常。例如，一个整数“除以零”时，抛出此类的一个实例。 
11. NegativeArraySizeException如果应用程序试图创建大小为负的数组，则抛出该异常。
12. NoSuchMethodException无法找到某一特定方法时，抛出该异常。
13. SecurityException由安全管理器抛出的异常，指示存在安全侵犯。
14. UnsupportedOperationException当不支持请求的操作时，抛出该异常。
15. RuntimeExceptionRuntimeException 是那些可能在Java虚拟机正常运行期间抛出的异常的超类。