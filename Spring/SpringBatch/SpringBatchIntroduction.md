# Spring Batch Introduction

엔터프라이즈 도메인 어플리케이션에서는 비지니스 운영에 필수적인 작업을 종종 벌크 프로세싱(대용량 자동 처리 프로세스)으로 개발한다.

**ex) 예시**

- 대용량의 정보를 사용자 개입 없이 자동으로, 효유적이게 처리하는 프로세싱. 주로 주어진 시간 기반 이벤트가 많다.(월말 정산 처리, 통지, 서신)
- 매우 큰 데이터 셋을 주기적으로 처리하는 어플리케이션 (1️⃣ 보험혜택을 정하거나 보험료를 조정하는 일)
- 내/외부 시스템을 통해 생성된 데이터를 하나로 통합하는 일. (형식 처리 , 유효성검사, 트랜잭션처리 등 )

</br>

---

❓**의문**

1️⃣ : **왜 보험혜택이나 보험료 조정하는일에 큰 데이터 셋을 주기적으로 처리할까?**

→ **보험 혜택과 요율 조정은 개인마다 다양한 정보(예: 나이, 건강 상태, 사고 이력 등)에 따라 달라지기 때문에, 이러한 정보를 주기적으로 갱신하여 조정해야한다.**

</br>

## 스프링 배치

- 종합적인 경량 배치 프레임 워크
- Spring Framwork의 특성(생산성, POJO기반 , 보편적인 사용성)을 기반으로 만들어져 쉽게 활용할 수 있다.
- 스케줄링 프레임워크가 아니기에 별도의 스케줄링 라이브러리와 함께 사용된다..(Quartz,Tivoli,Conntrol-M)
  - 스케줄러와 함께 동작하도록 설계되어있다.
- **로깅/추적, DB 트랜잭션 관리, Job 처리 통계, Job 재시작, 스킵 리소스 관리같은 대용량 데이터 처리에 필수적인 기능을 제공한다.**
- 최적화, 파티셔닝을 통해 대용량의 데이터 처리 및 고성능 배치 작업을 구현할 수 있다.
- 단순 대용량 파일을 읽고 DB에 저장하는일 외에도 DB간의 대용량 데이터를 이동시키고 데이터를 변환시키는 일 모두 지원한다.

</br></br>

# 배경 (Background)

오픈 소스 커뮤니티가 웹 및 MSA 아키텍쳐에 집중하는 동안, Java 기반 배치 처리에 대한 재사용 가능한 아키텍처 프레임워크의 필요성이 간과되어왔고,이에 따라 SpringSource(현재 VMware)와 Accenture 는 협력하여 스프링 배치를 개발하였다. 두 회사의 협력으로 표준화된 배치 애플리케이션 개발이 가능해져 기업과 정부 기관은 검증된 솔루션을 활용할 수 있게 되었다.

</br></br>

# 사용처 (Usage Senarios)

**일반적인 배치 프로그램은 보통 :**

- 데이터베이스, 파일, 1️⃣ queue로부터 다량의 자료 읽기
- 특정 방식으로 데이터를 처리하기
  - 데이터 변환, 계산 ,필터링 ,가공 등
- 가공된 형태로 데이터를 다시 작성하기

스프링 배치는 일반적으로 사용자와 상호 작용없이 오프라인 환경에서도 2️⃣유사한 트랜잭션을 하나로 묶어 처리하여 기본적인 배치 반복을 자동화 한다.

</br>

---

❓**의문**

1️⃣ : 일반적으로 말하는 메세지 큐

2️⃣ : 유사한 트랜잭션 ⇒ 같은 종류의 데이터나 비슷한 처리가 필요한 트랜잭션

</br>

**비지니스 Scenarios :**

- 배치 프로세스를 주기적으로 DB에 저장
- 동시에 여러개 일괄 처리 : job의 병렬 처리
- 단계적인 엔터프라이즈 message-driven 처리
- 대규모 병렬 배치 처리
- 실패 후 수동 또는 예약에 의한 재시작
- 의존 단계의 순차적 처리(workflow 기반 배치의 확장) → **특정 작업이 다른 작업의 결과에 의존하는 경우, 그 작업들을 순차적으로 처리하는 과정**
- 부분 처리 : 레코드 스킵하여 처리(ex 롤백)
- 전체 배치 트랜잭션 : 1️⃣ 배치 크기가 작거나 기존의 저장 프로시저 or 스크립트가 있는 경우에는 한번에 배치를 트랜잭션

**기술 목표 (Technical Objectives)**

- 배치 개발자는 Spring 프로그래밍 모델을 사용한다. 비즈니스 로직에 집중하고 프레임워크가 인프라를 관리하도록 해야한다. (ex) Job 과 Step 관리 , 트랜잭션 관리, 청크처리 ,작업 이력관리 등
- 인프라, 배치 실행 환경, 배치 어플리케이션 간의 책임을 명확하게 구분한다.
- 모든 프로젝트가 구현할 수 있는 인터페이스로 공통적인 핵심 실행 서비스를 제공한다.
- 핵심 인터페이스는 "즉시 사용 가능한" 간단한 디폴트 구현체를 제공한다.
- 모든 계층의 스프링 프레임 워크를 활용해 쉽게 서비스를 구성하고 커스텀마이징과 확장이 가능하다.
- 제공하는 모든 핵심 서비스는 인프라 계층에 영향을주지 않고 쉽게 교체하거나 확장 할 수 있어야한다.
- **Maven을 사용하여 빌드된 JAR가 애플리케이션과 완전히 분리하여 간단한 배치 모델을 제공한다.**

</br>

---

❓**의문**

1️⃣ : 왜 전체 배치 트랜잭션을 쓰나?

→ 배치 크기가 작은 경우 한번에 작업을 처리하는 것이 더 안정적이다.

기존의 저장 프로시저 or 스크립트?

→ 한 번 작성한 저장 프로시저를 여러 배치 작업에서 호출하여 사용할 수 도 있다.
