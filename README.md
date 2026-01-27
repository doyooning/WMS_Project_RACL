# **RACL - 의류 창고 관리 시스템(WMS)**

---

## **📑 목차**

- [프로젝트 개요](#프로젝트-개요)
- [사용 기술](#사용-기술)
- [프로젝트 구조](#프로젝트-구조)
- [구현 기능](#구현-기능)
- [기술적 도전과 해결](#기술적-도전과-해결)
- [회고](#회고)

---

## **프로젝트 개요**

| **항목** | **내용** |
| --- | --- |
| **프로젝트 소개** | 의류 창고 WMS 프로젝트 |
| **개발 인원** | 총 6명 |
| **담당 역할** | 서기 및 공용 작업공간 관리 <br> 기본 프론트엔드 작업(부트스트랩 적용, 사이드바 구성) <br> API 개발(로그인+회원가입, 회원 관리, 권한별 접근 제어, 공지사항+문의글, 거래처 조회)  |
| **개발 기간** | 총 4일 (2025-11-11 ~ 2025-11-14) |
| **성과 및 결과** | 기한 내 목표한 기능 구현 완료, 실제 DB 적용 완료 |

---

## **사용 기술**

**Backend**

- **Language:** Java 17
- **Framework:** Spring MVC 5.x
- **ORM:** MyBatis 3.x
- **Build Tool:** Gradle
- **WAS:** Apache Tomcat 9.0
- **Connection Pool:** HikariCP
- **Template Engine:** JSP

**Frontend**

- **Core:** HTML5, CSS3, JavaScript (ES6+)
- **Library:** jQuery 3.x, Bootstrap 5
- **Visualization:** ApexCharts.js 3.x

**Database**

- **DBMS:** MySQL 8.x
- **Design Tool:** ERD Cloud

**Collaboration**

- **Version Control:** Git, GitHub
- **IDE:** IntelliJ IDEA

---

## **프로젝트 구조**

### 1. 전체 구조도

![](https://velog.velcdn.com/images/doyooning/post/cd14acd3-1307-41dd-a2c7-08cd990f2bf6/image.jpg)

### 2. ERD + 와이어프레임

- ERD
    
![](https://velog.velcdn.com/images/doyooning/post/d500f687-7253-435b-a3b1-4d32c5d18326/image.jpg)
    
- 와이어 프레임
    
![](https://velog.velcdn.com/images/doyooning/post/75617390-270c-4eab-b581-7e026f45983b/image.jpg)
    
![](https://velog.velcdn.com/images/doyooning/post/7b83e2a4-bc94-4762-95fd-2d381ed63d07/image.jpg)
    
### 3. 디렉토리 구조
    
    ```java
    src/main/java/com/ssg/wms/ 
    ├─ admin // 총관리자 권한 기능
    ├─ advice // 전역 컨트롤러
    ├─ announcement // 공지사항 관련 기능 
    ├─ common // 공통 사용 패키지
    ├─ config // 설정 정보
    ├─ inquiry // 문의글 관련 기능
    ├─ manager // 창고관리자 권한 기능
    ├─ member // 일반회원 권한 기능
    ├─ partner // 거래처 조회 기능
    └─ reply // 문의글에 달린 답글 관련 기능
    ```
    
---
    

## **구현 기능**

<details>
  <summary>회원 가입</summary>

  - 회원 가입 폼을 작성 후 제출
  - 회원 가입이 완료되면 완료 페이지로 이동하며, 관리자 승인 후 체계 사용 가능
  - 회원 가입 폼 각각에 유효성 검증 구현, 조건에 맞지 않을 경우 메시지 출력 및 폼 제출 불가
  - 사업자등록번호의 경우, WMS와 계약이 되어 있는 거래처의 사업자등록번호만 유효한 번호로 판단  
    → 회원은 등록된 사업자등록번호 중에서 입력해야 가입 가능
</details>

<details>
  <summary>로그인</summary>

  - 가입된 ID와 PW로 로그인
  - 회원 로그인을 진행하면 가입된 ID와 PW를 DB에서 찾아 일치하면 완료
  - 로그인 성공하면 회원 ID, 권한은 세션에도 저장됨
</details>

<details>
  <summary>회원 마이페이지</summary>

  - 자신의 정보 확인 및 수정 가능
  - 회원은 우측 상단 드롭다운 메뉴를 통해 마이페이지에 접근 가능
  - 마이페이지에서는 이메일, 전화번호를 비동기로 수정 가능
</details>

<details>
  <summary>회원 관리 (관리자)</summary>

  - 관리자(admin)는 전체 회원 조회 및 가입 요청 관리 가능
  - 회원 검색 가능, 상세 보기에서 미승인 회원일 경우 승인/거절 버튼 활성화
  - 회원 승인/거절 비동기 처리 → 페이지 전환 없이 즉각 반영
</details>

<details>
  <summary>커뮤니티</summary>

  - 공지사항 게시판, 문의사항 게시판 및 답글
  - 관리자는 공지사항 작성/조회/수정/삭제 가능, 그 외 권한은 조회만 가능
  - 공지사항은 중요도 순으로 정렬, 키워드 검색 가능
  - 문의사항 게시판과 답글은 1:N 관계 → 문의글 상세 조회 시 답글 목록 표시
  - 답글 비동기 처리 : 조회, 작성, 삭제 기능을 비동기로 구현하여 페이지 전환 최소화
</details>

<details>
  <summary>거래처 조회</summary>

  - WMS 체계에 가입된 거래처 조회 가능
  - 기능 전체 비동기 처리 → 단일 페이지에서 모든 정보 조회 가능
  - 검색어 입력 시 즉시 반영 → 신속한 정보 조회 및 UX 개선
</details>

<details>
  <summary>권한에 따른 접근 제어</summary>

  - 계정별 권한에 따라 접근 범위 다르게 구현
  - 권한 종류: ADMIN(총관리자), MANAGER(창고관리자), MEMBER(일반회원)
  - 일반회원이 관리자 전용 메뉴 접근 시 → "관리자만 접근 가능" 메시지와 함께 403 오류 화면 표시
  - 특정 권한이 필요한 메뉴는 경로에 권한 명시 (예: `/admin/members`)
</details>

---

## **기술적 도전과 해결**

<details>
  <summary>1. 권한 검증 구현을 위한 노력</summary>
  <br>
  
  **문제 상황**  
  Spring Security를 사용하지 않고 어떤 방식으로 권한을 검증할 것인가?

  **접근 방법**  
  WebConfig에서 인터셉터를 만들어서 해결하자!

  **결과**  
  오류는 없으나 기능 작동 안함

  **원인**  
  올바른 Bean으로 인식되지 않아서 Spring에 추가적인 환경설정 필요  
  → 설정 파일을 수정해야 하는 작업을 해야 하기에 프로젝트 일정상 불가 판단

  **대안**  
  ControllerAdvice로 전역에서 사용가능한 검증 객체를 만들자!

  **최종 결과**  
  설계한 대로 정상 작동함!

  ### 상세 과정

  <WebConfig 구성>
  ```java
  @Log4j2
  @Configuration
  @RequiredArgsConstructor
  public class WebConfig implements WebMvcConfigurer {

      private final RoleCheckInterceptor roleCheckInterceptor;

      @Override
      public void addInterceptors(InterceptorRegistry registry) {
          log.info("addInterceptors 작동");

          registry.addInterceptor(roleCheckInterceptor)
                  .addPathPatterns("/announcement/save", "/announcement/**/edit", "/announcement/**/delete");
      }
  }
```
<RoleCheckInterceptor 구성>
```java
@Log4j2
@Component
@RequiredArgsConstructor
public class RoleCheckInterceptor implements HandlerInterce
    @Override
    public boolean preHandle(HttpServletRequest request,
                             HttpServletResponse response,
                             Object handler) throws Exception {

        HttpSession session = request.getSession(false);
        log.info("preHandle: " + session);

        if (session == null || session.getAttribute("role") == null) {
            log.info("session == null, /login redirected");
            response.sendRedirect("/login");
            return false;
        }

        Role role = (Role) session.getAttribute("role");
        String uri = request.getRequestURI();
        log.info("요청 URI: {}, 사용자 권한: {}", uri, role);

        if (!role.name().equals(Role.ADMIN.name())) {
            log.info("권한 없음: {}", role);
            response.sendError(HttpServletResponse.SC_FORBIDDEN, "접근 권한이 없습니다.");
            return false;
        }

        return true;
    }
}
```
**개선 방안** <br>
<GlobalRoleChecker 구현>
```java
@ControllerAdvice
public class GlobalRoleChecker {
    @ModelAttribute
    public void checkAdminAccess(HttpServletRequest request, HttpServletResponse response) throws IOException {
        String uri = request.getRequestURI();
        HttpSession session = request.getSession(false);

        if (uri.equals("/admin/login")) return;

        // /admin 경로에 접근하는데 ADMIN이 아닌 경우 차단
        if (uri.contains("/admin") || uri.contains("/dashboard")) {
            Role role = (Role) session.getAttribute("role");

            if (session == null || session.getAttribute("role") == null) {
                response.sendRedirect("/login");
            } else if (role != Role.ADMIN) {
                response.sendError(HttpServletResponse.SC_FORBIDDEN, "관리자만 접근 가능합니다.");
            }
        }
    }
}
```
→ 같은 방식으로 각각의 권한을 확인하는 메서드를 추가로 구현해주었고, 그 결과 권한에 따른 접근 제어를 성공적으로 구현함
</details>
<details><summary>2. 세션(Session)과 모델(Model)에 대한 이해</summary>
<br>
  
**문제 상황** <br>
기능 구현 중 파라미터로 세션과 모델을 받아서 데이터를 주고받는데, 사용하다보니 기능 및 역할에 대한 혼동 발생
  
**접근 방법** <br>
개념적인 이해를 바탕으로 프로젝트에서 올바르게 사용하는 방법까지 이해하고자 함

세션
- 클라이언트에서 서버로 연결될 때 상태 유지를 위해 HttpSession 생성
- 로그인-로그아웃시까지 유지되며 사용자 정보(ID, 권한 등) 저장 가능
모델
- MVC 패턴에서 Controller와 View 사이에서 데이터를 전달하는 객체
- 요청을 내부 로직을 통해 처리하고, 그 결과를 사용자에게 화면을 통해 제공할 때 필요
코드 적용
```java
@GetMapping("/mypage")
public String getUserInfo(HttpSession session, Model model) {
    // 마이페이지 조회
    String id = (String) session.getAttribute("loginId");
    if (id == null) {
        return "redirect:/login";
    }
    ...
    model.addAttribute("loginAdmin", staffDTO);

    return "admin/mypage";
}
```

**결과**
- Session은 로그인 여부 확인 및 권한 검증에 사용
- Model은 DTO 데이터를 View에 전달하는 용도로 사용
- 불필요한 중복 코드 방지 및 효율적인 구조 확립
</details>
<details><summary>3. 데이터 무결성 오류 처리</summary>
<br>

**문제 상황**
<br>
회원 정보 수정 시 이메일에 비정상적 데이터를 입력하면 오류 발생 → 전화번호는 정상적으로 수정됨
<br>
**접근 방법**
<br>
DB와 연관된 오류로 판단하고 문제 진단
<br>
**정보 수정 로직 검토**
<br>
```java
@Override
public void updateMember(long memberId, MemberUpdateDTO memberUpdateDTO) {
    memberUpdateDTO.setMemberId(memberId);
    log.info("Member Update: " + memberUpdateDTO);
    memberMapper.updateMember(memberUpdateDTO);
    log.info("Member Updated: " + memberUpdateDTO);
}
```
→ 이메일 오류 발생 시에도 전화번호는 업데이트됨 → 기대한 결과와 불일치
<br>
**해결 방법**
<br>
 트랜젝션 적용
 ``` java
@Transactional
@Override
public void updateMember(long memberId, MemberUpdateDTO memberUpdateDTO) {
    memberUpdateDTO.setMemberId(memberId);
    log.info("Member Update: " + memberUpdateDTO);
    memberMapper.updateMember(memberUpdateDTO);
    log.info("Member Updated: " + memberUpdateDTO);
}
```
```java
@Transactional
@Override
public void insertMember(MemberDTO memberDTO) {
    memberMapper.insertMember(memberDTO);
}
```
**결과**
<br>
- 오류 발생 시 전체 작업을 취소하도록 트랜젝션 적용
- 데이터 무결성 유지 및 안정적인 DB 처리 가능
</details>





---

## **회고**

**잘한 점 (Keep)**

1. **입력 데이터 검증**: 
    
    회원 정보 입력 시 프론트엔드에서 1차적으로 정규식을 통한 유효성 검증 구현,
    
    사업자등록번호 입력 시 양식(3자리-2자리-5자리)을 숫자만 입력하면 자동으로 작성
    
    → 데이터 무결성 향상 및 사용자 경험 개선
    
2. **코드 재사용성**: 
    
    단일 JSP 페이지를 Header/Content/Footer로 나누고 조립하여 작성
    
    → 페이지에서 반복되는 부분을 재사용하여 유지보수성 및 가독성 향상
    
3. **안정적 권한 제어**: 
    
    특정 권한이 필요한 경로 접근시 권한 검증, 세션을 통한 권한 정보 유지
    
    → 비정상적 접근(GET요청 주소로 강제 접근 등) 차단
    

**아쉬운 점 (Problem)**

1. **공통 기능에 대한 소통 부족**: 
    
    모두가 사용/참조하는 기능 구현 시 구체적인 코드 설명 부족
    
    → 주석에 예시 코드 명시, 팀 협업 공간에 문서화 예정
    
2. **기능 디테일 부족**: 
    
    한정된 시간 내 구현 목표로 라이브러리 활용 제한, 필수적 기능 우선 구현
    
    → 프로젝트 구조 및 설계의 중요성을 체감하는 기회가 됨
    
3. **세션의 한계**: 
    
    세션으로 로그인 및 권한 정보 전달로 재접속시 정보 유지 불가
    
    → 세션을 최대한 활용해보는 계기가 되어 차후 JWT 및 OAuth 구현에 도움이 될 것
    

**개선 방안 (Try)**

1. **Spring Security**: 로그인 및 회원 정보 관리 보안성 강화
2. **일정관리 구체화**: Notion 간트 차트 및 칸반보드 기능으로 팀 협업 개선
