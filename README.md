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
try - catch - finally
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
1. once using finally, the code in finally part would run eventully. No matter what are in try or catch part, no matter the program is crashed or not.
2. In finally, we normally deal with shuting down database resource, IO flow and socket.  
```

```

