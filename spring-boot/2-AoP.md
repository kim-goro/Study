> [목차](index.md) 
### 3. Aspect Oriented Programming
  - [AOP 소개](#aop-소개)  
  - [프록시 패턴](#프록시-패턴)  
  - [스프링 @AOP 예제](#스프링-aop-애노테이션-예제)  
  
<br><br>
<br><br>

# AOP 소개
- **스프링 트라이앵글** : `IoC`, `AoP`, `PSA` 
- Aspect Oriented Programing

```java
class A {
   method a () {
           AAAA -> AAA
           오늘은 7월 4일 미국 독립 기념일이래요.
           BBBB -> BB
   }
 
   method b () {
           AAAA -> AAA
           저는 아침에 운동을 다녀와서 밥먹고 빨래를 했습니다.
           BBBB -> BB
   }
}

class B {
  method c() {
          AAAA -> AAA
          점심은 이거 찍느라 못먹었는데 저녁엔 제육볶음을 먹고 싶네요.
          BBBB -> BB
  }
}
```
<br>

```java
class A {
   method a () {
           오늘은 7월 4일 미국 독립 기념일이래요.
   }
 
   method b () {
           저는 아침에 운동을 다녀와서 밥먹고 빨래를 했습니다.
   }
}

class B {
  method c() {
          점심은 이거 찍느라 못먹었는데 저녁엔 제육볶음을 먹고 싶네요.
  }
}

class AAAABBBB {
    method aaaabbb(JoinPoint point) {
        AAAA
	    point.execute()
        BBBB
    }
}
```
흩어진 코드를 한 곳으로 밀집시킵니다.
<br>

```java
@Query("SELECT ptype FROM PetType ptype ORDER BY ptype.name")
	@Transactional(readOnly = true)
	List<PetType> findPetTypes();
```
`@Transactional`은 대표적인 Spring AOP기반 애노테이션  
<br>

> ### 다양한 AOP 구현 방법
  1. 컴파일한 코드에는 `stopwatch`기능을 넣을 수 있다.  A.java ***----(AOP)--->*** A.class
  2. 바이트코드 조작 A.java -> A.class ***---(AOP)--->*** 메모리 (`AspectJ`) : class Loader 중 조작하는 법
  3. 프록시 패턴 (스프링 AOP가 사용하는 방법 -> '디자인 패턴'사용)



<br><br><br><br>
# 프록시 패턴
- 클라이언트 코드 수정은 없이, Bean객체를 생성된 Proxy코드 `@Transactional`을 거치도록 함
- :page_facing_up: : https://refactoring.guru/design-patterns/proxy  
<br>

> ### 성능측정 AoP
```java
    StopWatch stopWatch = new StopWatch();
    stopwatch.stop();
    System.out.println(stopwatch.pretteyPrint());
```

```java
package org.springframework.samples.petclinic.Proxy;
import org.springframework.util.StopWatch;

public class CashPerf implements Payment{
    Payment cash = new Cash();


    @Override
    public void pay(int amount) {

        StopWatch stopwatch = new StopWatch();
        stopwatch.start();
        if(amount>100){
            System.out.println(amount+"신용카드");
        } else{
            cash.pay(amount);
        }
        stopwatch.stop();
        System.out.println(stopwatch.prettyPrint());
    }
}
```


<br><br><br><br>
# 스프링 AOP 애노테이션 예제
- `@LogExecutionTime` 으로 메소드 처리 시간 로깅하기
  - 스프링이 제공하는 '애노테이션 기반 AoP'
  - Loging하고 싶은 메소드에 붙여두고 `quick fix` `ctrl+1` -> `create Annotaion`하기

```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME) //언제까지 유지할 것인가?
public @interface LogExecutionTime {
 //간단한 마커용 애노테이션 형태(골격)
}
```

```java
실제 Aspect (@LogExecutionTime 애노테이션 달린곳에 적용)

@Component //bean으로 등록
@Aspect //Aspect 선언
public class LogAspect {
   Logger logger = LoggerFactory.getLogger(LogAspect.class);

   @Around("@annotation(LogExecutionTime)") //advice //@Around는 jOIN 포인터 파라미터(=타겟메소드)를 받을 수 있다.
   //타겟메소드를 실행하겠다. 'LogExecutionTime'라고하는 애노테이션 코드에...
   public Object logExecutionTime(ProceedingJoinPoint joinPoint) throws Throwable {
       StopWatch stopWatch = new StopWatch();
       stopWatch.start();

       Object proceed = joinPoint.proceed();

       stopWatch.stop();
       logger.info(stopWatch.prettyPrint());

       return proceed;
   }
}
```

<br><br>
------------------------------------------------------
<br><br>

> ### 기타 



