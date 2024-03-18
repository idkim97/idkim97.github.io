---
permalink: /2023-08-15-DAO, DTO, VO, Entity란/
published: true
title: "[Spring] DAO, DTO, VO, Entity란?"
date: 2023-08-15 09:00:00
toc: true
toc_sticky: true
toc_label: "DAO, DTO, VO, Entity란?"
categories:
- Spring
tags:
- Spring
- Java
---

<br><br>

## ✅ DAO ( Data Access Object )

- Data Access Object의 약자로, DB 데이터에 접근하기 위한 객체
- **프로젝트의 Service에 해당하는 부분과 DB를 연결하는 기능 수행**
- CRUD 작업을 수행하는 클래스. 즉, 데이터에 대한 CRUD를 전담하는 오브젝트
- SpringBoot에서는 Repository 클래스가 DAO의 역할을 수행한다.

```java
public class TestDao {
public void add(TestDto dto) throws ClassNotFoundException, SQLException {
        private static final String DRIVER = "com.mysql.jdbc.Driver";
    private static final String URL = "jdbc:mysql://localhost:3306/dao_Db";
    private static final String USER = "root";
    private static final String PASSWORD = "1234";   
        
        String sql = "SELECT * FROM vouchers";

        try {
            con = DriverManager.getConnection(URL, USER, PASSWORD);
            stmt = con.createStatement();
            res = stmt.executeQuery(sql);
            while (res.next()) {
                System.out.println(res.getString("id") + " ");
                System.out.println(res.getString("value") + " ");
            }
        } catch (SQLException throwables) {
            throwables.printStackTrace();
        }

    preparedStatement.setString(1, dto.getName());
    preparedStatement.setInt(2, dto.getValue());
    preparedStatement.setString(3, dto.getData());
    preparedStatement.executeUpdate();
    preparedStatement.close();

    connection.close();

    }
}
```


<br><br><br><br>

## ✅ DTO ( Data Transfer Object )

- Data Transfer Object의 약자로, 계층간(MVC) **데이터 전달**을 위한 객체
- 비즈니스 로직을 가지지 않고 getter, setter 메소드만 가진다.
- 주로 View와 Controller 사이에서 활용된다.

```java
public class personDTO {
    private String name;
    private int age;

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }

    public void setName(String name) {
        this.name = name;
    }

    public void setAge(int age) {
        this.age = age;
    }
}
```


<br><br><br><br>

## ✅ VO ( Value Object )
- Value Object의 약자로, 값 자체를 표현하는 객체
- DTO와 유사하지만, setter 메서드를 가지지 않는다.
- getter 메서드와 이외의 비즈니스 로직을 포함할 수 있다.
- setter 메서드는 없고, getter 메서드만 존재하기 때문에 **오로지 읽기만 가능하다!**
- 객체 간 "필드값"이 같다면 해당 객체들은 같은 객체로 여긴다.
- 때문에 주소값이 달라도 필드값만 같으면 같은 객체로 여긴다.
- [참고 자료](https://tecoble.techcourse.co.kr/post/2020-06-11-value-object/)


<br><br><br><br>

## ✅ Entity
- **실제 DB와 매핑되는 클래스**
- Entity를 기준으로 테이블을 형성하거나, 형성한 테이블을 기준으로 Entity를 설정해줘야한다.
- 한마디로 DB 컬럼과 일치하게끔 변수들을 설정해줘야 한다.
- Entity를 데이터 전달하는 용도로 사용해선 절대 안된다.
- JPA를 활용한다면 @Entity 어노테이션과 함께 사용하고, 기본 생성자를 필수로 생성해야 한다.
- 보통 Builder패턴을 활용해서 생성자를 만드는게 일반적이다.

<br>

- 일반적인 Entity 예제

```java
public class Person {
    private final Long id;
    private final String name;
    private final int value;

    public Person(Long id, String name, int value) {
        this.id = id;
        this.name = name;
        this.value = value;
    }
}
```

<br>

- JPA 활용 Entity 예제

```java

@Entity
@Table(name="Aim")
@Getter
@Setter
@NoArgsConstructor(access = AccessLevel.PROTECTED)
public class Aim {
     @Id
    @Column(name="instance_id", updatable = false, nullable = false)
    private String instanceId;

    @Column(name="user_id", nullable = false)
    private Integer userId;

    @Column(name="aim_state", nullable = false)
    private Integer aimState;

    @Column(name="server_ip", nullable = true)
    private String serverIp;

    @Column(name="short_url", nullable = true)
    private String shortUrl;

    @Column(name="subscribe_state", nullable = false)
    private Integer subscribeState;

    @Column(name="created_date" , nullable = false)
    private LocalDateTime createdDate;

    @Column(name="expired_date", nullable = false)
    private LocalDateTime expiredDate;

    @Builder
    public Aim(String instanceId, Integer userId, Integer aimState, String serverIp,
               String shortUrl, Integer subscribeState,
               LocalDateTime createdDate, LocalDateTime expiredDate){
        this.instanceId = instanceId;
        this.userId = userId;
        this.aimState = aimState;
        this.serverIp = serverIp;
        this.shortUrl = shortUrl;
        this.subscribeState = subscribeState;
        this.createdDate = createdDate;
        this.expiredDate = expiredDate;
    }
}
```