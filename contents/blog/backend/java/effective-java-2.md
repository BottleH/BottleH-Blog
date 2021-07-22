---
title: 이펙티브자바 3판 2장 정리
thumbnail: ./images/Effective-java.png
date: 2021-03-21 22:10:16
category: Effective-java
tags: [java]
draft: false
---
    
## 2장 객체 생성과 파괴

___

- [아이템1. 생성자 대신 정적 팩터리 메서드를 고려하라](#아이템1-생성자-대신-정적-팩터리-메서드를-고려하라)

- [아이템2. 매개변수가 많다면 빌더를 고려하라](#아이템2-매개변수가-많다면-빌더를-고려하라)

- [아이템3. private 생성자나 열거 타입으로 싱글턴임을 보증하라](#아이템3-private-생성자나-열거-타입으로-싱글턴임을-보증하라)

- [아이템4. 인스턴스화를 막으려거든 private 생성자를 사용하라](#아이템4-인스턴스화를-막으려거든-private-생성자를-사용하라)

- [아이템5. 자원을 직접 명시하지 말고 의존 객체 주입을 사용하라](#아이템5-자원을-직접-명시하지-말고-의존-객체-주입을-사용하라)

- [아이템6. 불필요한 객체 생성을 피하라](#아이템6-불필요한-객체-생성을-피하라)

- [아이템7. 다 쓴 객체 참조를 해제하라](#아이템7-다-쓴-객체-참조를-해제하라)

- [아이템8. finalizer와 cleaner 사용을 피하라](#아이템8.-finalizer와-cleaner-사용을-피하라)

- [아이템9. try finally보다는 try with resources를 사용하라](#아이템9-try-finally보다는-try-with-resources를-사용하라)




### 아이템1. 생성자 대신 정적 팩터리 메서드를 고려하라

___

**1-1. 정적 팩터리 메서드의 장점**

  * 이름을 가질 수 있다.
  * 호출될 때마다 인스턴스를 새로 생성하지는 않아도 된다.  
  * 반환 타입의 하위 타입 객체를 반환할 수 있는 능력이 있다.  
  * 입력 매개변수에 따라 매번 다른 클래스의 객체를 반환할 수 있다.  
  * 정적 팩터리 매서드를 작성하는 시점에는 반환할 객체의 클래스가 존재하지 않아도 된다.  

**1-2. 정적 팩터리 메서드의 단점**

  * 상속을 하려면 public이나 protected 생성자가 필요하니 정적 팩터리 메서드만 제공하면 하위 클래스를 만들 수 없다.
  * 정적 팩터리 메서드는 프로그래머가 찾기 어렵다.
    + from : 매개변수 하나 받아서 인스턴스화 ex. `Date.from()`
    + of : 여러 매개변수를 받아서 인스턴스화 ex. `Enumset.of()`
    + valueOf : from과 of의 자세한 버전 ex. `BigInteger.valueOf()`
    + getInstance : 인스턴스를 반환하지만, 같은 인스턴스임을 보장하지 않는다.
    + create, newInstance : 매번 새로운 인스턴스를 생성해 반환한다.  

> __결론: 정적 팩터리 메서드와 public 생성자는 각자의 쓰임새가 있으니 이해하고 사용하며, 무작정 생성자를 제공하는 습관을 고칠 것.__



### 아이템2. 매개변수가 많다면 빌더를 고려하라

___

**2-1. 점층적 생성자 패턴**

  ```java
NutritionFacts cocaCola = new NutritionFacts(240, 8, 100, 0, 35, 27);
  ```

  매개변수가 많아질 수록 많은 생성자를 작성해야하고 복잡해진다.

**2-2. 자바빈즈 패턴**  

  ```java
NutritionFacts cocaCola = new NutritionFacts();
cocaCola.setServingSize(240);
  ```

  클래스를 불변으로 만들 수 없다.

**2-3. 빌더 패턴**

  ```java
NutritionFacts salmon = new NutritionFacts.Builder()
                             .calories(123)
                             .sodium(2)
                             .carbohydrate(5555)
                             .build();
  ```

  점층적 생성자와 자바빈즈 패턴의 장점만 모아둔 가장 좋은 방법임.
  필요한 객체를 직접 만드는 대신 Builder 객체를 얻은 후 setter 들을 호출한 뒤 build()를 통해 필요한 객체를 얻는다.

> __결론: 생성자나 정적 팩터리가 처리해야 할 매개변수가 많다면 빌더 패턴을 선택하는 게 더 낫다.__



### 아이템3. private 생성자나 열거 타입으로 싱글턴임을 보증하라

___

싱글턴(singleton)이란 인스턴스를 오직 하나만 생성할 수 있는 클래스를 말한다.

**3-1. public static final 필드 방식의 싱글턴**

 ```java
public static final Hwan INSTANCE = new Hwan();
 ```

 static 영역이 로딩될 때 딱 1번만 호출된다.

**3-2. 정적 팩터리 방식의 싱글턴**

 ```java
public class Hwan{
   private static final Hwan INSTANCE = new Hwan(); 

   public static Hwan getInstance() { 
        return INSTANCE; 
   }
}
 ```

**3-3. 열거 타입 방식의 싱글턴 - 바람직한** 

 ```java
public enum Hwan {
   INSTANCE;
   
   public void leaveTheBuilding(){...}
}
 ```

 간결하고, 추가 노력없이 직렬화가 가능하다. 하지만 싱글턴이 Enum 외의 클래스를 상속해야 한다면 열거 타입방식은 사용할 수 없다.



### 아이템4. 인스턴스화를 막으려거든 private 생성자를 사용하라

___

 ```java
public class UtilityClass {
  // 기본 생성자가 만들어지는 것을 막는다(인스턴스화 방지용) /
  private UtilityClass() {
     throw new AssertionError();
  }
  ... // 나머지 코드는 생략
}
 ```

private 생성자를 추가하면 클래스의 인스턴스화를 막을 수 있다.



### 아이템5. 자원을 직접 명시하지 말고 의존 객체 주입을 사용하라

___

많은 클래스가 하나 이상의 자원(bean)에 의존한다. 이럴 때 정적 유틸성 클래스나 싱글턴을 사용하게 되면 유연하지 않고 테스트하기 어렵다.

**5-1. 의존 객체 주입방식**

 ```java
private final Lexicon dictionary;

public SpellChecker(Lexicon dictionary) { 
   this.dictionary = Objects.requireNonNull)dictionary); 
}
 ```

 사용하는 자원에 따라 동작이 달라지는 클래스에는 정적 유틸리티 클래스나 싱글톤 방식이 적합하지 않다! 
 이럴 때는 인스턴스를 생성할 때 생성자, static factory method, builder로 필요한 자원을 넘겨주는 방식이 좋다. 
 이것을 의존객체 주입 패턴 이라 한다.

 > __결론: 클래스가 내부적으로 하나 이상의 자원에 의존하고, 그 자원이 클래스 동작에 영향을 준다면 싱글턴과 정적 유틸리티 클래스는 사용하지 않는 것이 좋다.__



### 아이템6. 불필요한 객체 생성을 피하라

___

 예를들어, `new String("hello");` 보단 `"hello";` 가 좋다. 
 전자는 새로운 인스턴스가 만들어져 heap영역에 올라가고, 후자의 문자열리터럴은 상수풀에 올라가기 때문이다.

**6-1. Pattern과 같은 값비싼 객체는 재사용하라**

**6-2. 오토박싱**

 오토박싱은 기본 타입과 그에 대응하는 박싱된 기본 타입의 구분을 흐려주지만, 완전히 없애주는 것은 아니다.

 ```java
Long sum = 0l; 
for (long i = 0 ; i <= Integer.MAX_VALUE ; i++) {
        sum += i; 
    }
 ```

 이 코드는 매우 느리며, 쓸데없이 Long 객체를 2의 31제곱개나 만든다. 단순히 기본타입 long으로 바꾸기만해도 매우 개선된다. 
 따라서 박싱된 기본 타입보다는 기본 타입을 사용하고, 의도치 않은 오토박싱이 숨어들지 않도록 주의하자.



### 아이템7. 다 쓴 객체 참조를 해제하라

___

java는 GC가 자동으로 다 쓴 객체를 회수해주긴 하지만, 몇몇 경우에 개발자가 직접 메모리를 관리함으로써 GC가 회수를 하지않아 문제가 발생한다.

**7-1. 직접 메모리 관리**

 ```java
public class Stack {    
  private Object[] elements;
  
  public Object pop() {
    if (size == 0)
      throw new EmptyStackException();

  return elements[--size]; // 메모리 누수 원인
  } 
}
 ```

 Size만 줄이고, 값은 그대로 보존하여 메모리 누수 발생

 ```java
// 해결코드
Object result = elements[--size];
elements[size] = null;

return result;
 ```

 **7-2. 캐시**

 직접 구현한 캐시 역시 메모리 누수를 일으키는 주범임.

 > 해결방법: `WeakHashMap`, `LinkedHashMap.removeEldestEntry`, 백그라운드 쓰레드를 돌리며 캐시 해제 등
 >
 > - Weak Reference
 >   - Strong Reference : `Integer value = 1;` GC대상이 아니다.
 >   - Soft Reference : `SoftReference<Integer> key = new SoftReference<Integer>(value);` value 가 null 이 되어 참조되지 않을때 GC대상이 된다. 
 >     그러나 Weak Reference와 다르게 메모리가 부족하지않으면 굳이 GC하지 않는다.
 >   - Weak Reference : `WeakReference<Integer> key = new WeakReference<Integer>(value);` value 가 null 이 되어 참조되지 않을때 GC대상이 된다. 무조건 다음 GC 때 사라진다.

 **7-3. 콜백**

 해지하지 않는다면 계속 쌓인다.

 > 해결방법: weak reference로 저장



### 아이템8. finalizer와 cleaner 사용을 피하라

___

 Finalizer는 예측불가능하고 위험하며, 대부분 불필요하다. 성능도 안좋아진다.
 cleaner 또한 finalizer보다 덜 위험하지만 여전히 여전히 예측할 수 없고, 느리고, 일반적으로 불필요하다.



### 아이템9. try finally보다는 try with resources를 사용하라

___

 전통적으로 자원이 제대로 닫힘을 보장하는 수단으로 `try-finally`가 쓰였다. 그러나 2개 이상의 자원을 사용할 때 이것은 복잡해진다.

 ```java
InputStream in = new FileInputStream(src); 

try {
    OutputStream out = new FileOutputStream(dst);
    
    try {
      ...    
    } finally {
      out.close();
    } 
  } finally {
    in.close();
  }
 ```

 `try-with-resources`는 AutoCloseable를 구현하고 자동으로 닫아준다. 자원이 여러개여도 가능하다.

 ```java
try (InputStream in = new FileInputStream(src); OutputStream out = new FileOutputStream(dst)) {
  ... 
  } catch (..) {}
 ```

 