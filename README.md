# java

## 1. stringbuffer,stringbuilder

## 2. override equals and hashCode
```package equal;
import java.util.Objects;
public class prove {
	int age;
	String name;
	public prove(int a, String n) {
		this.age =a;
		this .name=n;
	}
	public boolean equals(Object b){
		if (b==null) {
			return false;
		}
		if (this==b) {
			return true;
		}
		if (!(b instanceof prove) ) {
			System.out.print("yes");
			return false;
		}
		prove person=(prove) b;
		
		return (age==person.age)&&(name==person.name);
	}
	@Override
	public int hashCode() {
		return Objects.hash(age,name);
	}	
	public static void main(String[] args) {
		prove person1=new prove(15,"jack");
		prove person2=new prove(15,"jack");
		System.out.println(person1.equals(person1));
		System.out.println(person1.hashCode());
		System.out.println(person2.hashCode());
	}
}
```
## 3. ways to get input from keyboard
method1: Scanner

```java
Scanner input = new Scanner(System.in);
String s  = input.nextLine();
input.close();
```

mathod2: BufferedReader 

```java
BufferedReader input = new BufferedReader(new InputStreamReader(System.in)); 
String s = input.readLine(); 
```

## 4. 异常
### 引入与分类
#### try - catch - finally
* try-catch
  ```
  try{
  codes that probably causes crash;
  }catch(Exception ex){ // Exception ex => polymorphism.
  code for modification or reminding;
  
  ex.printStackTrace();//show all the process to trace the crash.
  throw ex; //throw exception to caller. If caller is main function, then this command will throw ex to JVM and trigle two  effects: (1) print infomation of exception. (2) stop the program.
  }
  ```
 
* try-catch-finally
1. once using finally, the code in finally part would run eventully(except for System.exit(status:1)). No matter what are in try or catch part, no matter the program is crashed or not.
2. In finally, we normally deal with shuting down database resource, IO flow and socket. 
3. return commmand will not conflict with finally. Finally runs before return. 
```
try{
  codes that probably causes crash;
  }catch(ArithmaticException ex){ 
  code for modification or reminding.
  ex.printStackTrace();//show all the process to trace the crash.
  }catch(InputMismatchException ex){
  //some code.
  }catch(Exception ex){
  //put in the last place.
  }finally{
  //some code would definitely run by program normally.
  }
```
#### Exception Category
<p align="center">
  <img src="./pic/Screen Shot 2020-02-23 at 9.31.13 PM.png" width="400">
</p>


* Runtime Exception 

ArrayIndexOutOfBounds... NullPointer... Arithmatic... etc.

* compile Exception

```
Class.forname("Text").newInstance();
//Where has two risks: no Text class or instance exception.
```


WAY TO HANDLE THIS SITUATION
1. Utilize try-catch to capture the Exception
2. Throws Exception

```
Public Static void main(String[] args) throws Exception{

....
throw new Exception("oops");
....
}
or 

Public Static void main(String[] args){

....
throw new RuntimeException("oops");
....


}

```

#### The difference  between throw and throws

1. 方法内部 vs 方法声明处
2. throw 异常对象 vs throws 异常类型
3. throw 异常出现的源头 vs throws 向caller抛出异常，让caller去处理。

## 5. IO Stream
### 字节流
read one byte once， 不能很好的表示汉字，会变成乱码。
```
public class Test {
	public void main (String arg[]) throws IOException{
		File file= new File(pathname: "/user/....");
		FileInputStream fis= new FileInputStream(file);
		int n = fis.read();
		while(n!=-1){
			System.out.println(n);
			n=fis,read();
		}
		fis.close();
	}
}
```
提高效率的方法，改变每次读入对字节，同时返回每次读入对字节数。利用缓存数组。
```
byte[] b=new byte[];
int len=fis.read(b);
while(len!=-1){
len=fis.read(b);
}
```
以上是读取文件，接下来是写入文件。
```
...
File file= new File(pathname: "/user/d.txt");
FileOutputstream fos=new FileOutputStream(file); 
String str="dafhkahdfkhakdfhak";
byte[] b= str.getBytes();
fos.write();
fos.close();


```

