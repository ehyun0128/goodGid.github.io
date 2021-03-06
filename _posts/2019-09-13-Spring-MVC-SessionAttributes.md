---
layout: post
title:  " Spring MVC - @SessionAttributes 애노테이션 "
categories: Spring
tags: Spring
author: goodGid
---
* content
{:toc}

> 이 글의 코드 및 정보들은 강의를 들으며 정리한 내용을 토대로 작성하였습니다.

## @SessionAttributes

* Model 정보를 

* **Http Session**에 저장시켜주는 애노테이션이다.

<br>

* 예를 들어

* 여러 화면에서

* 배송 정보를 입력받아야 하는 화면이라면

* Http Session에 

* 각 화면마다 필요한 정보를 저장하고

* 최종적으로 저장한 값들을 가져다 사용할 수 있다.

<br>

* Http Session에 값을 

* 저장시키는 방법은

* HttpSession을 가져와

* 직접 저장할 수 있지만 

> Controller 

``` java
@Controller
@RequestMapping
public class SampleController {

    @GetMapping("/events")
    @ResponseBody
    public String hello(Model model, HttpSession httpSession) {
        Event event = new Event();
        event.setName("goodGid");
        model.addAttribute("event", event);
        httpSession.setAttribute("event", event); // Http Session 직접 저장
        return "hello";
    }
}
```

* @SessionAttributes 애노테이션을 사용하면

* 보다 편리하게 저장할 수 있다.

* @SessionAttributes 애노테이션은

* @SessionAttributes("event")처럼

* 설정한 이름(= "event" )과 동일한 

* Key 값으로

* Model에 저장하는 코드가 있을 시

* 자동으로 Session에도 저장시켜준다.

> Controller

``` java
@Controller
@RequestMapping
@SessionAttributes("event")
public class SampleController {

    @GetMapping("/events")
    @ResponseBody
    public String hello(Model model) {
        Event event = new Event();
        event.setName("goodGid");
        model.addAttribute("event", event); // @SessionAttributes("event") 코드에 의해 
                                            // 동시에 Http Session에도 저장된다.
        return "hello";
    }
}
```

> TC

``` java
@Test
public void helloTest() throws Exception {
    mockMvc.perform(get("/events"))
            .andDo(print())
            .andExpect(status().isOk())
            .andExpect(request().sessionAttribute("event", notNullValue()));
}
```

* @SessionAttributes("event") 코드에 의해 

* Model에 저장하는 동시에

* Http Session에도 저장이 되기 때문에

* Test Code는 성공한다.

<br>

* 참고로 TC에서 사용한 

* **request()**는

* *org.springframework.test.web.servlet.result.MockMvcResultMatchers.request* 를 

* import 하여야한다.


---

## @SessionAttributes 한계

* **@SessionAttributes**를 선언한 클래스 내에서만

* 선언시 명시한 이름에 해당하는

* Model을 Session에 넣어줄 뿐

* 여러 컨트롤러에 걸쳐서는 적용이 안된다.

<br>

* 예를 들면 

``` java
@SessionAttributes("event")
public class A_Controller {
    model.addAttribute("event", event);
    ...
}


public class B_Controller {
    model.addAttribute("event", event);
    ...
}
```

* 이런 구조의 Controller가 있다면

* A_Controller에서는 

* *event* 라는 Key로

* Model에 저장 시

* Http Session에도 저장이 되겠지만

* 그 기능이

* B_Controller에서는 적용이 되지 않는다.

* 뿐만 아니라 Interceptor 혹은 Filter에도 적용이 안된다.

<br>

* 만약 Controller 밖(Interceptor 혹은 Filter 등)에서 

* (= Http Session 전반에 걸쳐 )

* 만들어 준 Session 데이터에 접근하고자 한다면

* [@SessionAttribute]({{site.url}}/Spring-MVC-SessionAttribute)를 사용하면 된다.

* **s**가 붙고 안붙고가 다르다.


---

## Session Clear

* Http Session을 초기화 시키고 싶을 경우엔

* **SessionStatus**를 사용해 

* Session을 초기화 시킬 수 있다.


> Controller 

``` java
@GetMapping("/events")
@ResponseBody
public String hello(Model model, SessionStatus sessionStatus) {
    Event event = new Event();
    event.setName("goodGid");
    sessionStatus.isComplete(); // false
    sessionStatus.setComplete();
    sessionStatus.isComplete(); // true
    return "hello";
}
```

> TC

``` java
@Test
public void helloTest() throws Exception {
    mockMvc.perform(get("/events"))
            .andDo(print())
            .andExpect(status().isOk())
            .andExpect(request().sessionAttribute("event", notNullValue()));
}
```

> Result

``` java
java.lang.AssertionError: Session attribute 'event'
Expected: not null
     but: was null
Expected :not null
Actual   :null
```

* Session을 초기화 하였기 때문에

* Test Code는 실패한다.


---

## 참고

* [스프링 웹 MVC](https://www.inflearn.com/course/%EC%9B%B9-mvc)

