---
permalink: /2023-09-12-NamedParameterJdbcTemplate 이란/
published: true
title: "[Spring] NamedParameterJdbcTemplate 이란?"
date: 2023-09-12 09:00:00
toc: true
toc_sticky: true
toc_label: "NamedParameterJdbcTemplate 이란?"
categories:
- Spring
tags:
- Spring
---

<br><br>






## ✅ NamedParameterJdbcTemplate 이란?

기존에 사용하던 JdbcTemplate 같은 경우는 우리가 데이터를 넣을 부분에 ?를 이용해서 처리를 했는데 이러한 방식은 인자 위치에 따라 값을 넣는 순서가 강제되기 떄문에 가독성을 떨어트리는 문제가 있다. 그래서 이를 보완하기 위해 NamedParameterJdbcTemplate가 나왔다.

NamedParameterJdbcTemplate은 `?`대신 `:변수명`을 사용해서 인자에 대한 값을 넣어주기 때문에 가독성이 높고 순서에 강제되지 않는 이점이 있다.

```java
private NamedParameterJdbcTemplate namedParameterJdbcTemplate;

public void setDataSource(DataSource dataSource) {
	this.namedParameterJdbcTemplate = new NamedParameterJdbcTemplate(dataSource);
}

public int countOfActorsByFirstName(String firstName) {
	String sql = "select count(*) from t_actor where first_name = :first_name";
	SqlParameterSource namedParameters = new MapSqlParameterSource("first_name", firstName);
	return this.namedParameterJdbcTemplate.queryForObject(sql, namedParameters, Integer.class);
}
```

<br><br><Br>

**Map**을 사용해서 처리도 가능하다.

```java
private NamedParameterJdbcTemplate namedParameterJdbcTemplate;

public void setDataSource(DataSource dataSource) {
	this.namedParameterJdbcTemplate = new NamedParameterJdbcTemplate(dataSource);
}

public int countOfActorsByFirstName(String firstName) {
	String sql = "select count(*) from t_actor where first_name = :first_name";
	Map<String, String> namedParameters = Collections.singletonMap("first_name", firstName);
	return this.namedParameterJdbcTemplate.queryForObject(sql, namedParameters, Integer.class);
}
```

<br><br><br>

**객체와 매핑**해서 사용도 가능하다.
```java
public class Actor {
	private Long id;
	private String firstName;
	private String lastName;

	public String getFirstName(){
		return this.firstName;
	}

	public String getLastName(){
		return this.lastName;
	}

	public Long getId(){
		return this.id;
	}
}
```

<br>

위와 같은 Actor 객체가 존재한다고 할때 `BeanPropertySqlParameterSource`를 이용하면 아래와 같이 편하게 처리가 가능하다.

```java
private NamedParameterJdbcTemplate namedParameterJdbcTemplate;

public void setDataSource(DataSource dataSource) {
	this.namedParameterJdbcTemplate = new NamedParameterJdbcTemplate(dataSource);
}

public int countOfActors(Actor exampleActor) {
	// notice how the named parameters match the properties of the above 'Actor' class
	String sql = "select count(*) from t_actor where first_name = :firstName and last_name = :lastName";
	SqlParameterSource namedParameters = new BeanPropertySqlParameterSource(exampleActor);
	return this.namedParameterJdbcTemplate.queryForObject(sql, namedParameters, Integer.class);
}
```

<br><br><br>






