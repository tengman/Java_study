# 泛型
Java泛型：Java泛型是通过擦除实现的，类定义中的类型参数如T会被替换为Object，在程序运行过程中，不知道泛型的实际类型参数。（其实Java在编译泛型的时候就是做的类型强制转换）<br>
泛型好处：类型更安全，可读性更好。<br>
泛型是计算机程序中一种重要的思维方式，它将数据结构和算法与数据类型相分离，使得同一套数据结构和算法，能够应用于各种数据类型，而且还可以保证类型安全，提高可读性。<br>
\<T extends E>用于定义类型参数，它声明了一个类型参数T，可放在泛型类定义中类名后面、泛型方法返回值前面。<br>
\<? extends E>用于实例化类型参数，它用于实例化泛型变量中的类型参数，只是这个具体类型是未知的，只知道它是E或E的某个子类型。<br>
### 理解通配符
#### 无限定通配符
形如DynamicArray\<?>，称之为无限定通配符。<br>
例如：public static int indexOf(DynamicArray\<?> arr, Object elm)<br>
可以改为：<br>
public static \<T> int indexOf(DynamicArray\<T> arr, Object elm)<br>
实例<br>
private static \<T> void swapInternal(DynamicArray\<T> arr, int i, int j){<br>
    T tmp = arr.get(i);<br>
    arr.set(i, arr.get(j));<br>
    arr.set(j, tmp);<br>
}<br>

public static void swap(DynamicArray\<?> arr, int i, int j){<br>
    swapInternal(arr, i, j);<br>
}<br>

现在我们再来看，泛型方法，到底应该用通配符的形式，还是加类型参数？两者到底有什么关系？我们总结下：<br>
通配符形式都可以用类型参数的形式来替代，通配符能做的，用类型参数都能做。<br>
通配符形式可以减少类型参数，形式上往往更为简单，可读性也更好，所以，能用通配符的就用通配符。<br>
如果类型参数之间有依赖关系，或者返回值依赖类型参数，或者需要写操作，则只能用类型参数。<br>
通配符形式和类型参数往往配合使用，比如，上面的实例方法，定义必要的类型参数，使用通配符表达依赖，并接受更广泛的数据类型。<br>

#### 超类型通配符
还有一种通配符，与形式\<? extends E>正好相反，它的形式为\<? super E>，称之为超类型通配符，表示E的某个父类型，它有什么用呢？有了它，我们就可以更灵活的写入了。<br>

对于有限定的通配符形式\<? extends E>，可以用类型参数限定替代，但是对于类似上面的超类型通配符，则无法用类型参数替代。<br>
本节介绍了泛型中的三种通配符形式，<?>、<? extends E>和<? super E>，并分析了与类型参数形式的区别和联系。

简单总结来说：<br>
\<?>和\<? extends E>用于实现更为灵活的读取，它们可以用类型参数的形式替代，但通配符形式更为简洁。<br>
\<? super E>用于实现更为灵活的写入和比较，不能被类型参数形式替代。<br>
### 局限性
###### 基本类型不能用于实例化类型参数
###### 运行时类型信息不适用于泛型
一个泛型对象的getClass方法的返回值与原始类型对象也是相同的，比如说，下面代码的输出都是true：<br>
Pair\<Integer> p1 = new Pair\<Integer>(1,100);
Pair\<String> p2 = new Pair\<String>("hello","world");
System.out.println(Pair.class==p1.getClass());
System.out.println(Pair.class==p2.getClass());<br>
下面写法不支持<br>
if(p1 instanceof Pair\<Integer>)<br>
不过，Java支持这么写：<br>
if(p1 instanceof Pair\<?>)<br>
###### 类型擦除可能会引发一些冲突

Java不支持创建泛型数组。<br>
如果要存放泛型对象，可以使用原始类型的数组，或者使用泛型容器。<br>
泛型容器内部使用Object数组，如果要转换泛型容器为对应类型的数组，需要使用反射。<br>
