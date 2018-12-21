---
title: "2018.12.21 memo"
date: 2018-12-21 18:06:00 -0400
categories: blog memo
---


컨트롤러로 뷰(jsp) 매핑(Mapping)

@WebServlet()쓰려 했다가 실패...


매핑 단계를 적어보자

1) 클라이언트인 web.xml에서 시작
web.xml에는 컨트롤러 역할을 하는 서블릿 정보를 담고 있다.
내가 사용한 서블릿은 DispatcherServlet

/Test(프로젝트명)/src/main/webapp
이게 기본경로인 것 같다

서블릿 태그에 서블릿 정의 정보가 담긴 xml파일 경로를 적어준다
서블릿매핑 태그에는 서블릿이름과 url패턴을 적는다
---> url패턴 공부 할 것!


2) 서블릿을 정의한 servlet-context.xml 파일을 살펴보자

<annotation-driven /> 태그로 어노테이션 사용이 가능하도록 설정

리소스를 사용할 경우 경로 지정해주는 태그(아직은 사용X)
<resources mapping="/resources/**" location="/resources/" />

뷰 리졸버 종류가 다양하다
나는 InternalResourceViewResolver 사용
웹 어플리케이션 내부의 리소스로 뷰를 결정할 때 쓰는 뷰 리졸버라고 한다.
앞으로 웹 구조가 더 복잡해진다면 BeanNameViewResolver, XmlViewResolver도 사용해보자

<beans:bean
		class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<!-- 뷰의 접두어, 접미어 설정: 파일명만 작성할 수 있도록 세팅 -->
		<!-- 접두어: 디렉터리 -->
		<beans:property name="prefix" value="/WEB-INF/views/" />
		<!-- 접미어: 확장자 -->
		<beans:property name="suffix" value=".jsp" />
	</beans:bean>
이런 구조를 가진다

ExceptionResolver를 이용하여 에러를 처리할 수도 있다고 한다 -> 이것도 나중에!


3) 컨트롤러 부분

@Controller 로 컨트롤러임을 알려준다

@RequestMapping을 사용해 경로를 지정한다

나는 컨트롤러 클래스에 여러 메서드를 사용했는데

맨 처음 화면에 보여주고 싶은 home.jsp는
@RequestMapping(value = "/", method=RequestMethod.GET)
	public String home() {
		logger.info("SampleController.java home()");
		return "home";
	}
  
HTTP GET 메서드를 받고, home 이름을 return해서 home.jsp 뷰를 사용하도록 설정


다른 뷰 매핑은?

@RequestMapping(value = "/ex01/doA")
	public void doA(){
		logger.info("SampleController.java doA()");
	}

	//doB.jsp 파일 맵핑
	@RequestMapping(value = "/ex01/doB")
	public void doB(){
		logger.info("SampleController.java doB()");
	}

리턴 type을 주지 않고, method 실행X
해당 url를 입력했을 때 해당 jsp(뷰)가 보이도록 설정했다

음... home메서드도 return 값 없이 value로 경로 다 주고
메서드 안에서 HTTP GET 받으면 될 것 같은데... 해보자!
------>안돼! get 메서드는 @RequestMapping 에서 받기

---
