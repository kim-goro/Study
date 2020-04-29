> [목차](index.md)  
## 2. Inversion of Control  
  - [IoC 소개](#ioc-소개)
  - [IoC (Inversion of Control) 컨테이너](#ioc-컨테이너)  
  - [빈 (Bean)](#빈-bean)  
  - [의존성 주입 (Dependency Injection)](#의존성-주입)

<br><br>
<br><br>




# IoC 소개
- **Inversion of Control**
- DI(Dependency Injection)이 포함된 개념
- 일반적인 (의존성에 대한) 제어권: “내가 사용할 의존성은 내가 만든다.” 

```
class OwnerController {
   private OwnerRepository repository = new OwnerRepository();
}
```

- IoC: “내가 사용할 의존성을 누군가 알아서 주겠지”
```
class OwnerController {
   private OwnerRepository repo;
   public OwnerController(OwnerRepository repo) {
       this.repo = repo;
   } 
} 
```
```
class OwnerControllerTest {
   @Test
   public void create() {
         OwnerRepository repo = new OwnerRepository();
         OwnerController controller = new OwnerController(repo);
   }
}
```
 - `OwnerController`선언시 `OwnerRepository repo`를 반드시 할당해야하기 때문에 코드가 안전하다
 - `Java/org.springframework.sample.petclinic/owner/OwnerControllerTests`에서 `OwnerRepository repo`를 `Bean`으로 할당
 - 선언만하고 받기만 한다. 객체할당은 밖에서 한다.
 - :page_facing_up: : https://martinfowler.com/articles/injection.html

<br><br>
<br><br>




# IoC 컨테이너
- Inversion of Control
- `ApplicationContext(BeanFactory)` 이 담당
- 빈(bean)을 만들고 의존성을 엮어주며 제공해준다.
- 애노테이션, @bean, 특정 인터페스를 상속하면 Bean으로 등록할 수 있다.
- 컨테이너에 있는 Bean객체에게만 서로 의존성 주입을 등록할 수 있다.
```
  @Autowired
  ApplicationContext applicationContext;

  @Test
  public void getBean(){
    OwnerController bean = applicationContext.getBean(OwnerController.class);
    assertThat(bean).isSNotNull();
  }
 ```
- 객체의 :grey_exclamation: `Single Tone Scope` 성향으로 클래스가 받은 bean와 해쉬코드가 같다.
- 인텔리J 코드 옆 북마크에 초록색 마크:green_apple:가 뜬다.

<br><br>
<br><br>




# 빈 Bean
 - 스프링 IoC 컨테이너가 관리하는 객체를 말한다.
 - `ApplicationContext`이 담고있는 객체
 - IoC 컨테이너가 사용하는 인터페이스를 :grey_exclamation: `LifeSycle Callback`이라고 부름 -> `@Component` 애노테이션붙은 모델클래스를 찾아 Bean으로 등록
<br>

> 어떻게 Bean으로 등록하지?
1. Component Scanning : 아래 디렉토리로 @Component 애노테이션이 있는지 확인
   - @Repository
   - @Service
   - @Controller
   - @Configuration

2. 또는 직접 일일히 XML이나 자바 설정 파일에 등록
```
@Configuration
public class SampleConfig{
  @Bean
  public SampleConfig sampleController(){
    return new SampleController();
  }
}
```
3. `OwnerRepository` 는 특정 인터페이스`Repository`를 상속받음으로 `JPA`에 의해 Bean으로 등록
<br>

## 빈 설정
- 이름 또는 ID
- 타입
- 스코프
- ( 아이러니하게도 컨테이너를 직접 쓸 일은 많지 않다. )
- :page_facing_up: : https://github.com/spring-guides/understanding/tree/master/application-context
- :page_facing_up: : https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/ApplicationContext.html
- :page_facing_up: :https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/BeanFactory.html



<br><br>
<br><br>



# 의존성 주입
- dependencyinjection
<br>

## 어떻게 bean을 꺼내쓰지?
 - `@Autowired` 또는 `@Inject`
 - 또는 `ApplicationContext`에서 `getBean()`으로 직접 꺼낸다 -> 의존성 주입 권장
 <br>

## `@Autowired` / `@Inject`를 어디에 붙일까?

 방법 1 : 생성자(권장하지만 :grey_exclamation: `Circular Dependency`가 일어날 수 있다) 
 - : 스프링 4.3버전부터 생성자에 `@Autowired` 애노테이션은 생략되었다.
 ```
 public OwnerController(OwnerRepository clinicService, VisitRepository visits) {
		this.owners = clinicService;
		this.visits = visits;
	}
  ```
 방법 2 : 필드
 ```
   @Autowired
   private OwnerRepository owners;
 ```
 방법 3 : Setter
 ```
   @Autowired
   public void setOwners(OwnerRepository owner){
     this.owners = owners;
   }
 ```