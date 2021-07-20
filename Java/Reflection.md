# Reflection 이란?

자바의 리플랙션(Reflection)은 클래스, 인터페이스, 메소드들을 찾을 수 있고, 객체를 생성하거나 변수를 변경할 수 있고
메소드를 호출할 수도 있다. ```Reflection```은 자바에서 기본적으로 제공하는 API 이다.

안드로이드에서 Hidden method를 호출할 때 ```Reflection```을 사용할 수 있다. SDK에
API가 공개되지 않은 경우 Android Studio에서 참조할 수 없어 호출할 수 없지만, 실제로 hidden API가
존재하기 때문에 리플랙션을 이용해서 호출할 수 있다.

또는 테스트 코드 작성을 위해 private 변수를 변경할 때 리플렉션을 사용할 수 있다.
3rd party 라이브러리를 사용하고 이것의 private 변수를 변경하고 싶을 때 리플렉션을 사용하면 라이브러리
코드 변경없이 값을 변경할 수 있다.

```Reflection```은 다음과 같은 정보를 가져올 수 있다. 이 정보를 가져와서 객체를 생성하거나 메소드를 호출하거나 변수의 값을 변경할 수 있다.
- Class
- Constructor
- Method
- Field

### 사용법

#### 준비

Parent.java
```
package test;

public class Parent {
    private String str1 = "1";
    public String str2 = "2";
    
    public Parent(){
    }
    
    private void method1() {
        System.out.println("method1");
    }
    
    public void method2(int n) {
        System.out.println("method2: " + n);
    }
    
    private void method3() {
        System.out.println("method3");
    }
```

Child.java
```
package test;

public class Child extends Parent {
    public String cstr1 = "1";
    private String cstr2 = "2";
    
    public Child(){
    }
    
    private Child(String str) {
        cstr1 = str;
    }
    
    public int method4(int n) {
        System.out.println("method4: " + n);
        return n;
    }
    
    private int method5(int n) {
        System.out.println("method5: " + n);
        return n;
    }
}
```

#### Class 찾기
클래스 ```Class``` 객체는 클래스 또는 인터페이스를 가리킨다. ```java.lang.Class``` 이며 import 허지 않고 사용할 수 있다.

```getName()```은 클래스의 이름을 리턴한다.

```
class Test {
    public static void main(String[] args) { 
        Class clazz = Child.class;
        System.out.println("Class name: " + clazz.getName());
    }
}
```

Output
```
Class name: test.Child
```

```Class.forName()```에 클래스 이름을 인자로 전달하여 클래스 정보를 가져올수 있다.
패키지 네이임이 포함된 클래스 이름으로 써줘야 한다.

```
class Test {
    public static void main(String[] args) {
        Class clazz = Class.forName("test.Child");
        System.out.println("Class name: " + clazz.getName());    
    }
}
```

Output
```
Class name: test.Child
```

#### Constructor 찾기

```getDeclaredConstructor()```는 인자 없는 생성자를 가져온다.

```
class Test {
    public static void main(String[] args) {
        Class clazz = Class.forName("test.Child");
        Constructor constructor = clazz.getDeclaredConstructor();
        System.out.println("Constructor: " + constructor.getName());    
    }
}
```

Output
```
Constructor: test.Child
```

```getDeclaredConstructor(Param)```에 인자를 넣으면 그 타입과 일치하는 생성자를 찾는다.

```
class Test {
    public static void main(String[] args) {
        Class clazz = Class.forName("test.Child");
        Constructor constructor = clazz.getDeclaredConstructor(String.class);
        System.out.println("Constructor(String): " + constructor.getName());    
    }
}
```

Output
```
Constructor(String): test.Child
```

위 두개의 예제는 어떤 인자를 받는 생성자만 찾는 코드라면
```getDeclaredConstructors()```는 private, public 등의 모든 생성자를 리턴해 준다.

```
class Test {
    public static void main(String[] args) {
        Class clazz = Class.forName("test.Child");
        Constructor[] constructors = clazz.getDeclaredConstructors();
        for (Constructor cons : constructors) {
            System.out.println("Get constructors in Child: " + cons);    
        }
    }
}
```

Output
```
Get constructors in Child: private test.Child(java.lang.String)
Get constructors in Child: public test.Child()
```

```getDeclaredConstructors()```는 모든 생성자를 리턴해 줬다면 ```getConstructors()```는
public 생성자만 리턴해준다.

```
class Test {
    public static void main(String[] args) {
        Class clazz = Class.forName("test.Child");
        Constructor[] constructors = clazz.getConstructors();
        for (Constructor cons : constructors) {
            System.out.println("Get public constructors in Child: " + cons);    
        }
    }
}
```

Output
```
Get constructors in Child: public test.Child()
```

### Method 찾기

```getDeclaredMethod()```의 인자로 메소드의 파라미터 정보를 넘겨주면 일치하는 것을 찾아준다.

```
class Test {
    public static void main(String[] args) {
        Class clazz = Class.forName("test.Child");
        Method method = clazz.getDeclaredMethod("method4", int.class);
        System.out.println("Find out method4 method in Child: " + method);    
    }
}
```

Output
```
Find out mehtod4 method in Child: public int test.Child.method4(int)
```

인자가 없는 메소드라면 null을 전달하면 된다.

```
class Test {
    public static void main(String[] args) {
        Class clazz = Class.forName("test.Child");
        Method method = clazz.getDeclaredMethod("method4", null);    
    }
}
```

만약 인자가 여러개라면 클래스 배열을 만들어서 인자를 넣어주면 된다.

```
class Test {
    public static void main(String[] args) {
        Class clazz = Class.forName("test.Child");
        Class[] partypes = new Class[1];
        partypes[0] = int.class;
        Method method = clazz.getDeclaredMethod("method4", partypes);    
    }
}
```

모든 메소드를 찾고 싶다면 ```getDeclaredMethods()```를 사용하면 된다.
공통적으로 함수 이름에 ```Declared```가 들어가면 Super 클래스의 정보는 가져오지 않는다.

```
class Test {
    public static void main(String[] args) {
        Class clazz = Class.forName("test.Child");
        Method[] methods = clazz.getDeclaredMethods();
        for (Method method : methods) {
            System.out.println("Get methods in Child: " + method);
        }    
    }
}
```

Output
```
Get methods in Child: public int test.Child.method4(int)
Get methods in Child: private int test.Child.method5(int)
```

class와 마찬가지로 declared 를 빼면 ```getMethods()```를 사용하면 public 메소드를 리턴해주며, 상속받은 메소드들도 모두 찾아준다.

```
class Test {
    public static void main(String[] args) {
        Class clazz = Class.forName("test.Child");
        Method[] methods = clazz.getMethods();
        for (Method method : methods) {
            System.out.println("Get public methods both Parent in Child: " + method);
        }    
    }
}
```

Output
```
Get public methods in both Parent and Child: public int test.Child.method4(int)
Get public methods in both Parent and Child: public void test.Parent.method2(int)
Get public methods in both Parent and Child: public final void java.lang.Object.wait(long,int) throws java.lang.InterruptedException
Get public methods in both Parent and Child: public final native void java.lang.Object.wait(long) throws java.lang.InterruptedException
Get public methods in both Parent and Child: public final void java.lang.Object.wait() throws java.lang.InterruptedException
Get public methods in both Parent and Child: public boolean java.lang.Object.equals(java.lang.Object)
Get public methods in both Parent and Child: public java.lang.String java.lang.Object.toString()
Get public methods in both Parent and Child: public native int java.lang.Object.hashCode()
Get public methods in both Parent and Child: public final native java.lang.Class java.lang.Object.getClass()
Get public methods in both Parent and Child: public final native void java.lang.Object.notify()
Get public methods in both Parent and Child: public final native void java.lang.Object.notifyAll()
```

### ref
 - [오라클 공식문서 - java.lang.relfect](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/package-summary.html#a8)
 - [Java - Reflection 쉽고 빠르게 이해하기](https://codechacha.com/ko/reflection/)