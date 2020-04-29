> [목차](index.md)  
### 1. 프로젝트 환경설정
  - [프로젝트 생성](#프로젝트-생성)
  	- [프로젝트 초기화](#프로젝트-초기화)
  - [라이브러리 살펴보기](#라이브러리-살펴보기)
  - [View 환경 설정](#view-환경-설정)
  - [H2 데이터베이스 설치](#h2-데이터베이스-설치)
  - [JPA와 DB 설정, 동작확인](#jpa와db-설정,-동작확인)

<br><br>
<br><br>


## 선수과목
- Spring
- JPA  
<br>

# 프로젝트 생성
## 프로젝트 초기화
- [스프링 부트 스타터 :page_facing_up:](https://start.spring.io/)
- Dependencies Library
  - Spring Web
  - Thymeleaf
  - JPA
  - h2
  - lombok
- groupId: jpabook
- artifactId: jpashop  
- project : gradle
- Spring boot version : 2.17 ( 설정은 2.26 )
- Packaging : Jar
- Java 8  
<br>

## Gradle 전체 설정
```java
plugins {
  id 'org.springframework.boot' version '2.1.9.RELEASE'
  id 'java'
}

apply plugin: 'io.spring.dependency-management'

group = 'jpabook'
version = '0.0.1-SNAPSHOT'
sourceCompatibility = '1.8'

configurations { // lombok 셋팅블럭
  compileOnly {
    extendsFrom annotationProcessor
  }
}

repositories { // lombok 셋팅블럭
  mavenCentral()
}
dependencies { // 관련된 라이브러리를 자동 추가함
  implementation 'org.springframework.boot:spring-boot-starter-web'
  implementation 'org.springframework.boot:spring-boot-starter-thymeleaf'
  implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
  compileOnly 'org.projectlombok:lombok'
  runtimeOnly 'com.h2database:h2'
  annotationProcessor 'org.projectlombok:lombok'
  testImplementation 'org.springframework.boot:spring-boot-starter-test'
}
```  

<br>

## 동작 확인
- 기본 테스트 케이스 실행
 - `src\main\java\jpgbook.jpashop\JpaShopApplication` 실행 `Cntl+Shift+F10`
 -  `src\test\java\jpgbook.jpashop\JpaShopApplicationTests` 확인 차 실행
- 스프링 부트 메인 실행 후 에러페이지로 간단하게 동작 확인 ( http://localhost:8080 )  

<br>

## 롬복 적용
1. Settings > Plugins > `Lombok` 설치
2. Settings > annotation processors > `Enable annotation processiong` 체크
3. 임의의 테스트 클래스를 만들고 @Getter, @Setter 확인 
```java
 package jpabook.jpashop;
 
import lombok.Getter;
import lombok.Setter;

 
@Getter @Setter
public class Hello {
    private String data;
}
```
```
package jpabook.jpashop;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class JpashopApplication {
	public static void main(String[] args) { // main 메서드 안에서 실행할 것!!
		Hello hello = new Hello();
		hello.setData("data");
		String data = hello.getData();
	}
}
```

<br>

> ### 기타 
***참고: 강의에 이후에 추가된 내용입니다.***  
## IntelliJ Gradle 대신에 자바 직접 실행  

최근 IntelliJ 버전은 Gradle로 실행을 하는 것이 기본 설정이다. 이렇게 하면 실행속도가 느리므로 다음과 같이 변경하면 자바로 바로 실행해서 실행속도가 더 빠르다.  
1. Preferences -> Build, Execution, Deployment Build Tools Gradle  
2. Build and run using: Gradle -> IntelliJ IDEA  
3. Run tests using: Gradle -> IntelliJ IDEA  

<br><br>
<br><br>







# 라이브러리 살펴보기
## gradle 의존관계 보기
- 터미널 : `./gradlew dependencies —configuration compileClasspath`  
- IntelliJ : Gradle > jpashop\Source Sets\main\Dependencies\
- *참고: `스프링 데이터 JPA`는 스프링과 JPA를 먼저 이해하고 사용해야 하는 응용기술이다.*

 <br>

## 스프링 부트 라이브러리 살펴보기
- spring-boot-starter-web
	- spring-boot-starter-tomcat: 톰캣 (웹서버)
	- spring-webmvc: 스프링 웹 MVC
- spring-boot-starter-thymeleaf: 타임리프 템플릿 엔진(View)
- spring-boot-starter-data-jpa
	- spring-boot-starter-aop
	- spring-boot-starter-jdbc
		- HikariCP 커넥션 풀 (부트 2.0 기본)
	- hibernate + JPA: 하이버네이트 + JPA
	- spring-data-jpa: 스프링 데이터 JPA
- spring-boot-starter(공통): 스프링 부트 + 스프링 코어 + 로깅
	- spring-boot
		- spring-core
	- spring-boot-starter-logging
		- logback, slf4j  

<br>

## 테스트 라이브러리
- spring-boot-starter-test
	- junit: 테스트 프레임워크
	- mockito: 목 라이브러리
	- assertj: 테스트 코드를 좀 더 편하게 작성하게 도와주는 라이브러리
	- spring-test: 스프링 통합 테스트 지원
- 핵심 라이브러리
	- 스프링 MVC
	- 스프링 ORM
	- JPA, 하이버네이트
	- 스프링 데이터 JPA
- 기타 라이브러리
	- H2 데이터베이스 클라이언트
	- 커넥션 풀: 부트 기본은 HikariCP
	- WEB(thymeleaf)
	- 로깅 SLF4J & LogBack
	- 테스트   
	
<br>

## spring-boot-devtools 라이브러리
- build.gradle > `implementation 'org.springframework.boot:spring-boot-devtools` 추가 -> `Import Change`
- 캐쉬를 자동으로 없애주거나 등 리로딩을 도와줌
- tml 파일을 컴파일만 해주면 서버 재시작 없이 View 파일 변경이 가능하다.
- 인텔리J 컴파일 방법 : build > Recompile

<br><br>
<br><br>





# thymeleaf 환경 설정
- 템플릿 엔진으로서 리액터, vue.js로 대체가능
- Natural Template : 마크업을 꺠지 않고 그대로 사용가능( JSP에 비해 웹브라우저에 쉽게 열림)
- [thymeleaf 공식 사이트 :page_facing_up:](https://www.thymeleaf.org/)
- [스프링 공식 튜토리얼 :page_facing_up:](https://spring.io/guides/gs/serving-web-content/)
- [스프링부트 메뉴얼 :page_facing_up:](https://docs.spring.io/spring-boot/docs/2.1.6.RELEASE/reference/html/boot-features-developing-web-applications.html#boot-features-spring-mvc-templateengines)  
<br>

## 스프링 부트 thymeleaf viewName 매핑
- 스프링 부트가 환경 설정을 자동으로 해줌
- url 에러 떠서 Languages & Frameworks > Schemas and DTDs > Ignored Schemas and DTDs 에 주소 삽입하였음.
- resources:templates/ +{ViewName}+ .html
- resources/templates/hello.html
- static/index.html
```java
@Controller
public class HelloController {
	 @GetMapping("hello") // Get요청
	 public String hello(Model model) { //view로 넘길 Model data
		 model.addAttribute("data", "hello!!");
		 return "hello"; //view이름 명시 : src\main\resources\templates, Enterprise급이면 레퍼런스로 바로가기 지원 (방법모름)
	 }
}
```
```html
<!DOCTYPE HTML>
<html xmlns:th="http://www.thymeleaf.org">
	<head>
		 <title>Hello</title>
		 <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
	</head>
	<body>
		<p th:text="'안녕하세요. ' + ${data}" >안녕하세요. 손님</p>
	</body>
</html>
```
```html
<!DOCTYPE HTML>
<html xmlns:th="http://www.thymeleaf.org">
	<head>
		 <title>Hello</title>
		 <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
	</head>
	<body>
		Hello
		<a href="/hello">hello</a>
	</body>
</html>
```  
- `Copy path` -> 브라우저로 열기
- 정적페이지는 resources/static 
- 랜더링페이지는 resources/template

<br><br>
<br><br>





# H2 데이터베이스 설치
- 개발이나 테스트 용도로 가볍고 편리한 DB, 웹 화면 제공
- H2 데이터베이스의 MVCC 옵션은 H2 1.4.198 버전부터 제거되었습니다. 
- 주의! Version 1.4.199를 사용해주세요.
	- [h2](https://www.h2database.com)
	- [윈도우 설치 버전 2019-03-13](https://h2database.com/h2-setup-2019-03-13.exe)
	- [윈도우, 맥, 리눅스 실행 버전 2019-03-13](https://h2database.com/h2-2019-03-13.zip)
	
- 데이터베이스 파일 생성 방법
	1. jdbc:h2:~/jpashop (최소 한번)
	2. ~/jpashop.mv.db 파일 생성 확인
	3. 이후 부터는 jdbc:h2:tcp://localhost/~/jpashop 이렇게 접속
<br>

## JPA와 DB 설정, 동작확인
- `spring.jpa.hibernate.ddl-auto: create` 옵션은 애플리케이션 실행 시점에 테이블을 drop 하고, 다시 생성한다.
- Logging
	- `show_sql` : 옵션은 System.out 에 하이버네이트 실행 SQL을 남긴다.
	- `org.hibernate.SQL` : 옵션은 logger를 통해 하이버네이트 실행 SQL을 남긴다.

```
//application.yml
spring:
 	datasource:
		 url: jdbc:h2:tcp://localhost/~/jpashop
		 username: sa
		 password:
		 driver-class-name: org.h2.Driver
 jpa:
 	hibernate:
		 ddl-auto: create
 	properties:
		 hibernate:
			# show_sql: true
			 format_sql: true
logging.level:
 	org.hibernate.SQL: debug
# org.hibernate.type: trace
```  

<br><br>
<br><br>





# JPA와 DB 설정, 동작확인
- 회원 엔티티
```java
@Entity
@Getter @Setter
public class Member {
	 @Id @GeneratedValue
	 private Long id;
	 private String username;
	 ...
}
```
- 회원 리포지토리
```java
@Repository
public class MemberRepository {
	 @PersistenceContext
	 EntityManager em;
	 public Long save(Member member) {
		 em.persist(member);
		 return member.getId();
	 }
	 public Member find(Long id) {
	 	return em.find(Member.class, id);
	 }
}
```
- 테스트
```java
@RunWith(SpringRunner.class)
@SpringBootTest
public class MemberRepositoryTest {
	 @Autowired MemberRepository memberRepository;
	 @Test
	 @Transactional
	 @Rollback(false)
	 public void testMember() {
		Member member = new Member();
		member.setUsername("memberA");
		Long savedId = memberRepository.save(member);
		Member findMember = memberRepository.find(savedId);
		
		Assertions.assertThat(findMember.getId()).isEqualTo(member.getId());
		Assertions.assertThat(findMember.getUsername()).isEqualTo(member.getUsername());
		Assertions.assertThat(findMember).isEqualTo(member); //JPA 엔티티 동일성 보장
	 }
}
```  
<br>

> :bulb: 스프링 부트를 통해 복잡한 설정이 다 자동화 되었다. persistence.xml 도 없고, LocalContainerEntityManagerFactoryBean 도 없다. 스프링 부트를 통한 추가 설정은 스프링 부트 메뉴얼을 참고하고, 스프링 부트를 사용하지 않고 순수 스프링과 JPA 설정 방법은 **자바 ORM 표준 JPA 프로그래밍** 책을 참고하자.  

<br><br>
<br><br>





# 쿼리 파라미터 로그 남기기
- 로그에 다음을 추가하기 org.hibernate.type : SQL 실행 파라미터를 로그로 남긴다.
- [외부 라이브러리 사용 spring-boot-data-source-decorator](https://github.com/gavlyukovskiy/spring-boot-data-source-decorator)

스프링 부트를 사용시 `implementation 'com.github.gavlyukovskiy:p6spy-spring-boot-starter:1.5.6'` 추가  
<br>

> :bulb: 쿼리 파라미터를 로그로 남기는 외부 라이브러리는 시스템 자원을 사용하므로, 개발 단계에서는 편하
게 사용해도 된다. 하지만 운영시스템에 적용하려면 꼭 성능테스트를 하고 사용하는 것이 좋다.
