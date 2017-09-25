---
layout: post
title: "spring in action"
description: "스프링 인 액션 정리"
date: "2017-05-08 17:48"
tags: [spring, spring framework, web,server]
comments: true
---

# 스프링 인 액션

## 1장. 스프링속으로

### 1.2 컨테이너

스프링 기반 어플리케이션에서는 스프링 컨테이너 안에서 객체가 태어나고, 자라고, 소멸한다.
스프링 컨테이너는 스프링 프레임워크의 핵심부에 위치한다. 스프링 컨테이너는 종속객체 주입을 이용해 어플리케이션을 구성하는 컴포넌트를 관리하며, 협력 컴포넌트 간 연관관계의 형성도 여기서 이루어진다.

스프링 컨테이너에는 여러가지가 있으나 통상적으로 2가지로 분류 된다.

1. BeanFactory : DI에 대한 기본적인 지원을 제공하는 단순한 컨테이너이다.
2. ApplicationContext : BeanFactory를 확장함으로써 기능을 추가하여 어플리케이션 프레임워크 서비스를 제공하는 컨테이너이다.

#### 1.2.1 어플리케이션 컨텍스트

* AnnotationConfigApplicationContext - 하나 이상의 자바 기반 설정 클래스에서 스프링 어플리케이션 컨텍스트를 로드한다.
* AnnotationConfigWebApplicationContext - 하나 이상의 자바 기반 설정 클래스에서 스프링 웹 어플리케이션 컨텍스트를 로드한다.
* ClassPathXmlApplicationContext - classpath에 위차한 XML파일에서 context 정의 내용을 로드한다.
* FilSystemXmlApplicationContext - 파일 경로로 지정 된 XML을 읽어서 컨텍스트 정의 내용을 로드한다.
* XmlWebApplicationContext - 웹 어플리케이션에 포함된 XML 파일에서 컨텍스트 정의 내용을 로드한다.

#### 1.2.2 빈의 일생

보통의 자바 어플리케이션에서 빈의 생명 주기는 매우 단순하다. 왜냐, new 연산자로 인스턴스화 하고, 바로 사용이 가능하다.
사용되지 않는 경우에는 결국 가비지 컬렉션에 의해 메모리가 제거 된다.

스프링은 이보다 더욱 정교한 메커니즘을 가지고 있다고 한다.

BeanFactory 컨테이너 내 빈이 갖는 구동 생명주기

1. 인스턴스화 : 스프링이 빈을 인스턴스화한다.
2. 프로퍼티 할당 : 스프링이 값과 빈의 레퍼런스를 빈의 프로퍼티에 주입한다.
3. 빈이 BeanNameAware를 구현하면, BeanNameAware의 setBeanName()을 호출한다.
4. BeanFactoryAware의 setBeanFactory()를 호출하여, BeanFactory 자체를 넘긴다.
5. ApplicationContextAware의 setApplicationContext()를 호출하고, 둘러싼 application context에 대한 참조를 넘긴다.
6. BeanPostProcessors의 사전 초기화 : BeanPostProcessor를 구현하면, postProcessBeforeInitalization() 메소드를 호출한다.
7. InitializingBean의 afterPropertiesSet() 메소드를 호출한다.
8. 커스텀 호출 메서드 초기화
9. BeanPostProcessors의 후속 초기화 : BeanPostProcessor를 구현하면, postProcessAfterInitalization() 메소드를 호출한다.
10. 빈 사용 준비가 완료 된다.

이후 컨테이너가 종료 될 경우

1. DisposableBean의 destroy()
2. 커스텀 소멸 메서드 호출

## 2장 와이어링

### 2.1 스프링 설정 옵션 알아보기

세가지 기본적인 와이어링 메커니즘을 스프링에서는 제공하고 있다.

* XML의 명시적 설정
* Java에서 명시적 설정
* 빈을 찾아 자동으로 와이어링

대부분의 선택은 개인 취향의 문제이며 마음에 드는 것으로 선택한다.

### 2.2 자동으로 빈 와이어링하기

* 컴포넌트 스캐닝
* 오토 와이어링

두 가지 방식으로 빈에 와이어링을 한다. 모두 사용할 경우 강력하고, 명시적인 설정을 최소로 유지 가능.

스프링 어플리케이션 컨텍스트에서 모든 빈에게 ID를 할당한다. 모든 자동으로 ID를 주게 되면, 클래스 명의 첫 글자를 소문자화한 ID를 빈의 이름으로 갖는다.

명시적으로 빈에 이름을 부여하는 방법에는 `@Component`가 있고, 이와 같은 기능을 하는 JSR-330 표준에 해당하는 `@Named` 어노테이션이 있다.

```Java
@Component
public class CDPlayer implements MediaPlayer {
  private CompactDisc cd;

  @Autowired
  public CDPlayer(CompactDisc cd) {
    this.cd = cd;
  }

  public void play() {
    cd.play();
  }

}
```

그러나, 스프링에서는 주로 `@Component` 어노테이션을 주로 사용한다.

컴포넌트 스캐닝을 위해서는 베이스 패키지를 세팅 해야 하는데, 여러가지 방법이 있다.

* XML을 이용한 방법

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns:context="http://www.springframework.org/schema/context"
  xmlns:c="http://www.springframework.org/schema/c"
  xmlns:p="http://www.springframework.org/schema/p"
  xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">

  <context:component-scan base-package="soundsystem" />

</beans>
```

* Annotation 방법
```java
@ComponentScan(basePackages="soundsystem") //array라, 복수개도 가능
public class CDPlayerConfig {
}
```

오토와이어링은 스프링이 빈의 요구 사항과 매칭되는 어플리케이션 컨텍스트상에서 다른 빈을 찾아 빈 간의 의존성을 자동으로 만족시키는 것을 말하는데, 스프링에서는 `@Autowired` 어노테이션을 사용한다. 표준으로는 `@Inject`를 사용하면 된다.

단, 매칭 되는 빈이 없다면 예외가 발생하는데, 이 예외를 피하려면, 어노테이션에 속성을 추가하면 되는데, `@Autowired(required = false)` 이렇게 하면 단점으로는 빈이 null일 가능성이 있어 방어 코딩이 안되면, NPE가 발생한다.

`@bean` 어노테이션이 하는 역할은 무엇일까 ? 모호하긴 한데, 좀 더 찾아 보고 해야 하는데, 일단 예제 코드 기준에서 이해한 바는 이렇다.

```java
@Configuration
public class CDPlayerConfig {

  @Bean
  public CompactDisc compactDisc() {
    return new SgtPeppers();
  }

  @Bean
  public CDPlayer cdPlayer(CompactDisc compactDisc) {
    return new CDPlayer(compactDisc);
  }

}
```
stereo-javaconfig 하위 프로젝트인데, 여기서 `@Bean` 어노테이션을 지우고 실행 시켜보면, 실행 할 수 없다.

왜냐, 의존성을 주입 해야하는데, 둘 중 하나라도 없다면, 의존성을 주입할 객체를 찾을 수가 없다. 물론, 생성도 되지 않는다.

`compactDisc` 메소드의 경우 이 친구가 결국 의존성을 주입 할 대상 객체인데, 이 대상 객체가 없으면, `cdPlayer`에서 객체를 생성할 수 조차 없다.

`@Bean` 대신에 `xml`로도 설정을 할 수 있다.

### 생성자 주입을 사용, 빈 초기화하기

1. `<constructor-arg>`
2.  c-namespace

2가지 방법으로 설정을 할 수 있는데, 사실 그냥 1번으로 하는게 더 깔끔하지 않나 싶기도 하다.


## 3장 고급 와이어링

### 3.3 오토와이어링의 모호성

* 기본 빈 지정
    * `@Primary`로 지정
    * `@Qualifier`를 통한 지정
