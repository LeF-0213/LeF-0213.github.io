---
layout: post
related_posts:
    - /studylog/spring
title:  "HttpMediaTypeNotAcceptableException 예외 해결"
date:   2025-06-25
categories:
  - studylog
  - spring
description: >
  Generic의 개념과 List, Set, Map에서의 활용
---
* toc
{:toc .large-only}

# HttpMediaTypeNotAcceptableException 발생 코드
```java
  public class MovieDTO {
    Integer id;
    String title;
    Long revenue;	// 매출액이 21억을 넘어가는 경우 존재, null을 위해 wrapper클래스
    Long revenueSum; // 누적 매출액이 21억을 넘어가는 경우 존재
    Integer attendance; // 관객수 데이터를 저장할 변수
    Integer attendanceSum; // 누적 관객수를 저장할 변수
  }
```

```java
  @GetMapping("/study/movie/1")
	public ResponseEntity<MovieDTO> getMovie() {
		// DB가서 id가 1인 영화 정보 조회 
		// 제목 하얼빈, 매출액 1328740400, 누적매출액 38811821330, 관객수 7421, 누적관객수 4062055
		MovieDTO dto = new MovieDTO();
		dto.id = 1;
		dto.title = "하얼빈";
		dto.revenue = 1328740400L;
		dto.revenueSum = 38811821330L;
		dto.attendance = 7421;
		dto.attendanceSum = 4062055;
		
		return ResponseEntity.status(200).body(dto);
	}
```
=> Resolved `[org.springframework.web.HttpMediaTypeNotAcceptableException`: No acceptable representation]

# 발생 원인

# Solution
```java
  public class MovieDTO {
	private Integer id;
	private String title;
	private Long revenue;	// 매출액이 21억을 넘어가는 경우 존재, null을 위해 wrapper클래스
	private Long revenueSum; // 누적 매출액이 21억을 넘어가는 경우 존재
	private Integer attendance; // 관객수 데이터를 저장할 변수
	private Integer attendanceSum; // 누적 관객수를 저장할 변수
	
	public Integer getId() {
		return id;
	}
	public void setId(Integer id) {
		this.id = id;
	}
	public String getTitle() {
		return title;
	}
	public void setTitle(String title) {
		this.title = title;
	}
	public Long getRevenue() {
		return revenue;
	}
	public void setRevenue(Long revenue) {
		this.revenue = revenue;
	}
	public Long getRevenueSum() {
		return revenueSum;
	}
	public void setRevenueSum(Long revenueSum) {
		this.revenueSum = revenueSum;
	}
	public Integer getAttendance() {
		return attendance;
	}
	public void setAttendance(Integer attendance) {
		this.attendance = attendance;
	}
	public Integer getAttendanceSum() {
		return attendanceSum;
	}
	public void setAttendanceSum(Integer attendanceSum) {
		this.attendanceSum = attendanceSum;
	}
}
```

```java
  @GetMapping("/study/movie/1")
	public ResponseEntity<MovieDTO> getMovie() {
		// DB가서 id가 1인 영화 정보 조회 
		// 제목 하얼빈, 매출액 1328740400, 누적매출액 38811821330, 관객수 7421, 누적관객수 4062055
		MovieDTO dto = new MovieDTO();
		dto.setId(1);
		dto.setTitle("하얼빈");
		dto.setRevenue(1328740400L);
		dto.setRevenueSum(38811821330L);
		dto.setAttendance(7421);
		dto.setAttendanceSum(4062055);
		
		return ResponseEntity.status(200).body(dto);
	}
```

{
    "id": 1,
    "title": "하얼빈",
    "revenue": 1328740400,
    "revenueSum": 38811821330,
    "attendance": 7421,
    "attendanceSum": 4062055
}