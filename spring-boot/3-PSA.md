> [목차](index.md)  
## 4. Portable Service Abstraction  
  - [PSA 소개](#PSA-소개)
  - [웹 MVC](#웹-MVC-추상화계층)
  - [스프링 트랜잭션](#스프링-트랜잭션)
  - [캐시](#스프링-캐시)

<br><br>
<br><br>





# PSA 소개
- Portable Service Abstraction
- Portable는 다른 기술 스택(`webflux`, `Netty`, `Servlet`, `Reactive`)등 을 적용해도 동일하게 가능하다.
- `Sevlet` 어플레케이션을 만들고 있지만 Sevlet(doGet, doPost...) 을 쓰고있지않다.
- 추상화 객체로 `@RequestMapping`, @GetMapping, @PostMapping, Url, View경로를 지정할 수 있다.
- :page_facing_up: : https://en.wikipedia.org/wiki/Service_abstraction  

<br><br>
<br><br>




# 웹 MVC 추상화계층
- `@Controller` : 요청(헤더, 파라미터 등)을 맵핑하는 컨트롤러 클래스가 된다.  

```java
@Controller
class OwnerController {

	private static final String VIEWS_OWNER_CREATE_OR_UPDATE_FORM = "owners/createOrUpdateOwnerForm";

	private final OwnerRepository owners;

	private VisitRepository visits;

	public OwnerController(OwnerRepository clinicService, VisitRepository visits) {
		this.owners = clinicService;
		this.visits = visits;
	}

	@InitBinder
	public void setAllowedFields(WebDataBinder dataBinder) {
		dataBinder.setDisallowedFields("id");
	}

	@GetMapping("/owners/new")
	public String initCreationForm(Map<String, Object> model) {
		Owner owner = new Owner();
		model.put("owner", owner);
		return VIEWS_OWNER_CREATE_OR_UPDATE_FORM;
	}

	@PostMapping("/owners/new")
	public String processCreationForm(@Valid Owner owner, BindingResult result) {
		if (result.hasErrors()) {
			return VIEWS_OWNER_CREATE_OR_UPDATE_FORM;
		}
		else {
			this.owners.save(owner);
			return "redirect:/owners/" + owner.getId();
		}
	}
```
  
<br><br>
<br><br>




# 스프링 트랜잭션
- 그룹처리 작업을 모두 완료해야 Commit하는 작업
- `@Transactional` 붙은 클래스는 트랜잭션 작업을 해줌
- 포터블 성격을 지님 : `JpaTransacionManager`, `DatasourceTransactionManager`, `HibernateTransactionManager`
- :page_facing_up: : https://mkyong.com/jdbc/jdbc-transaction-example/
- :page_facing_up: : https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/transaction/PlatformTransactionManager.html

```java
package com.mkyong.jdbc;

import java.math.BigDecimal;
import java.sql.*;
import java.time.LocalDateTime;

public class TransactionExample {

    public static void main(String[] args) {

        try (Connection conn = DriverManager.getConnection(
                "jdbc:postgresql://127.0.0.1:5432/test", "postgres", "password");
             Statement statement = conn.createStatement();
             PreparedStatement psInsert = conn.prepareStatement(SQL_INSERT);
             PreparedStatement psUpdate = conn.prepareStatement(SQL_UPDATE)) {

            statement.execute(SQL_TABLE_DROP);
            statement.execute(SQL_TABLE_CREATE);

            // start transaction block
            conn.setAutoCommit(false); // default true

            // Run list of insert commands
            psInsert.setString(1, "mkyong");
            psInsert.setBigDecimal(2, new BigDecimal(10));
            psInsert.setTimestamp(3, Timestamp.valueOf(LocalDateTime.now()));
            psInsert.execute();

            psInsert.setString(1, "kungfu");
            psInsert.setBigDecimal(2, new BigDecimal(20));
            psInsert.setTimestamp(3, Timestamp.valueOf(LocalDateTime.now()));
            psInsert.execute();

            // Run list of update commands

            // error, test roolback
            // org.postgresql.util.PSQLException: No value specified for parameter 1.
            psUpdate.setBigDecimal(2, new BigDecimal(999.99));
            //psUpdate.setBigDecimal(1, new BigDecimal(999.99));
            psUpdate.setString(2, "mkyong");
            psUpdate.execute();

            // end transaction block, commit changes
            conn.commit();

            // good practice to set it back to default true
            conn.setAutoCommit(true);

        } catch (Exception e) {
            e.printStackTrace();
        }

    }

    //...
}
```
<br><br>
<br><br>




# 스프링 캐시
- CacheManager
  - :page_facing_up: : https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/cache/CacheManager.html
