---
title: "2018.12.22 memo"
date: 2018-12-22 15:36:00 -0400
categories: blog memo
---

클라이언트에 사용할 서블릿 언급? -> 어떤 xml파일을 참고하지 알려줌
-> 해당 xml파일은 뷰리졸버를 이용한다. 어떤 타입의 뷰를 사용할지 명시해줌
-> 컴트롤러에서 해당 뷰 경로 지정해줌

----> 이제 경로를 이용해서 원하는 페이지를 볼 수 있다!

MVC 패턴을 이해하고 어떻게 스는지 대충 감이 왔다
갈 길이 멀지만... 파이팅이야

아직 감 못잡음

	@RequestMapping(value = "/", method = RequestMethod.GET)
	public String home() {
		logger.info("SampleController.java home()");
		return "home";
	}
	
	@RequestMapping(value = "/ex01/doA")
	public void doA(){
		logger.info("SampleController.java doA()");
	}

	//doB.jsp 파일 맵핑
	@RequestMapping(value = "/ex01/doB")
	public void doB(){
		logger.info("SampleController.java doB()");
	}
  
이런 형식으로 하나의 컨트롤러에 여러 경로 설정합  

그런데


	@RequestMapping(value = "/", method = RequestMethod.GET)
	public String home() {
		logger.info("SampleController.java home()");
		return "home";
	}
	
	@RequestMapping(value = "/ex01/")
	public String doA(){
		logger.info("SampleController.java doA()");
		return "doA";
	}

	//doB.jsp 파일 맵핑
	@RequestMapping(value = "/ex01/")
	public String doB(){
		logger.info("SampleController.java doB()");
		return "doB";
	}


이렇게 다 return type을 정했더니 500error
500에러는 뭘까?

Servlet.init() for servlet appServlet threw exception

으음 검색해보니까 의존성 문제라는 것 같다
어떻게해야 파일 구성을 예쁘고 효율적으로 할 수 있을지 생각해봐야겠다



----> 음.. 의문점

서버를 실행했을 때 왜


@RequestMapping(value = "/", method = RequestMethod.GET)
	public String home(Locale locale, Model model) {
		logger.info("SampleController.java home()");

		//ko_KR라는 이름이 담긴다. -> 특정 나라의 시간을 담기 위해 사용됨
		logger.info("Welcome home! The client locale is {}.", locale);
		num++;
		logger.info("몇 전 접근하는가? {} ", num);

		Date date = new Date();
		
		DateFormat dateFormat = DateFormat.getDateTimeInstance(DateFormat.LONG, DateFormat.LONG, locale);

		String formattedDate = dateFormat.format(date);

		//jsp에 값을 전달하기 위해 model사용 어트리뷰트 명을 정하고 값을 담아 보내면 jsp에선 attribute명으로 값 사용 가능!
		model.addAttribute("serverTime", formattedDate );

		//브라우저에 보여줄 뷰의 이름을 전달 즉 home.jsp뷰를 뜻하는 것
		return "home";
	}


에 두 번 접근하는가?
왜... 왜지ㅠㅠㅠㅠ
맨 처음 서버 접속할 때만 두 번 접근되는걸로봐서 코드 문제는 아닌듯


@RequestMapping(value = "/", method = RequestMethod.GET)
이거 쓸 때 value = "/"요거 빼먹으면
경로 이상하게 막 적어도 무조건 다 된다고 뜸
음 매핑이 요상하게 되는듯..?
value는 	URL 값으로 매핑 조건을 부여 (default) 역항을 한다고 한다


출처: https://sarc.io/index.php/development/1139-requestmapping

3. @RequestMapping 이 사용하는 속성
 
이름		타입		설명
value		String[]	URL 값으로 매핑 조건을 부여 (default)
method		RequetMethod[]	HTTP Request 메소드 값을 매핑 조건으로 부여 GET, POST, HEAD, OPTIONS, PUT, DELETE, TRACE (7개)
params		String[]	HTTP Request 파라미터를 매핑 조건으로 부여
consumes	String[]	설정과 Content-Type request 헤더가 일치할 경우에만 URL이 호출됨
produces	String[]	설정과 Accept request 헤더가 일치할 경우에만 URL이 호출됨

이것도 참고하자
https://www.journaldev.com/3358/spring-requestmapping-requestparam-pathvariable-example

이것도!
https://stackoverflow.com/questions/13213061/springmvc-requestmapping-for-get-parameters

params 다루는 것도 

------> 으악!

나는 

	@RequestMapping(value = "/", method = RequestMethod.GET)
	public String home() {
		logger.info("SampleController.java home()");
		return "home";
	}
	
	@RequestMapping(value = "/ex01/doA")
	public void doA(){
		logger.info("SampleController.java doA()");
	}

	//doB.jsp 파일 맵핑
	@RequestMapping(value = "/ex01/doB")
	public void doB(){
		logger.info("SampleController.java doB()");
	}

이렇게 했을 때 home()가
@RequestMapping(value = "/", method = RequestMethod.GET)를 받기 때문에
home메서드가 가장 먼저 실행되는구나 라고 생각했었는데
아니었따

아아아 어떤 식으로 서블릿을 받고 실행되는거야아아아ㅏㅏㅏㅏㅏㅏ

굳이 method = RequestMethod.GET를 쓰지 않아도 실행된ㄴ다... 무슨일이야!

일단 web.xml엔

	<servlet-mapping>
		<servlet-name>appServlet</servlet-name>
		<url-pattern>/</url-pattern>
	</servlet-mapping>

이렇게 적혀있다

그리고 
@RequestMapping(value = "/")로 매핑된 것을 첫번째로 실행시킴

서블릿 매핑 url 패턴 공부

https://lng1982.tistory.com/97

"/"로 시작하고 "/*"로 끝나는 패턴은 path로 인식
"*."으로 시작하는 경우 확장자 매칭
"/"만 정의한 경우 디폴트 서블릿 의미
그 외의 경우 동치 매칭



으으으으으음

servlet-context.xml을 이렇게 설정해서

	<beans:bean
		class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<!-- 뷰의 접두어, 접미어 설정: 파일명만 작성할 수 있도록 세팅 -->
		<!-- 접두어: 디렉터리 -->
		<beans:property name="prefix" value="/WEB-INF/views/ex01/" />
		<!-- 접미어: 확장자 -->
		<beans:property name="suffix" value=".jsp" />
	</beans:bean>

/WEB-INF/views/ex01/위치에 있는 파일을 불러오고

나는 void 타입 메서드를 사용해서 doA를 가장 먼저 불러오고 싶다

	//주소창에 msg 파라미터 값을 가져와 변수에 저장?
	@RequestMapping(value = "/doA")
	public void doA(@ModelAttribute("msg") String msg){
		logger.info("SampleController.java doA()");
		//num2++;
		//logger.info("몇 번 접근하는가? {}, {}", num, num2);
		logger.info("어떤 값이 출력될까? {} ",msg);
		//return "test";
	}

이렇게 메서드를 설정하면 doA.jsp 매핑은 됨
하지만 첫 화면에 뜨진 않는다
String으로 return된 값을 jsp로 생각하나봐!



	//주소창에 msg 파라미터 값을 가져와 변수에 저장?
	@RequestMapping(value = "/doA")
	public String doA(@ModelAttribute("msg") String msg){
		logger.info("SampleController.java doA()");
		//num2++;
		//logger.info("몇 번 접근하는가? {}, {}", num, num2);
		logger.info("어떤 값이 출력될까? {} ",msg);
		return "test";
	}

	
이렇게 쓰면 
주소에 http://localhost:8080/doA
파일은 test.jsp가 뜬다


내가 이해한 것

	@RequestMapping(value = "/doA")
	public void doA(@ModelAttribute("msg") String msg){
		logger.info("SampleController.java doA()");
	}

이렇게 쓰면 주소에 doA라고 뜨고 doA.jsp를 가져온다

@RequestMapping는 경로를 지정해주고
return에서 jsp 명을 받는 것

return type이 void일 경우엔 @RequestMapping의 value 값으로 인식하게 되어있는 듯

---> 앞으론 @RequestMapping엔 경로만 설정해주고
retrun을 꼭꼭 해서 jsp는 받아오도록 하자

근데 또 의문 생김
아 

	<!-- servlet-context.xml 경로 -->
	<beans:property name="prefix" value="/WEB-INF/views/" />


	//controller.java
	@RequestMapping(value = "/ex01")
	public String doA(@ModelAttribute("msg") String msg){
		logger.info("SampleController.java doA()");
		return "test";
	}

이건 안됨!

입력 주소:	http://localhost:8080/ex01
인식 경로:	/WEB-INF/views/test.jsp

servlet-context.xml에 써있는 기본 경로로 생각!




오늘 공부 결론 이거다!!!!!

:star2: :star2: :star2: :star2: :star2: :star2: :star2:

1) servlet-context.xml 에 기본 경로 잘 지정하고
2) @RequestMapping()으로 경로 설정
3) return으로 정확한 jsp명 입력
+) jsp이름이 주소에 노출되기 싫은 경우 @RequestMapping()로 숨길 수 있다
단, servlet-context.xml 기본 경로에 있는 파일만!

----->
	//controller.java
	@RequestMapping(value = "/ex01/doA")
	public String doA(@ModelAttribute("msg") String msg){
		logger.info("SampleController.java doA()");
		return "test";
	}

이렇게 하면

입력 주소:	http://localhost:8080/ex01/doA
인식 경로:	/WEB-INF/views/test.jsp
이렇게!
---->

++) 위에서 헷갈렸던 것 다시1
여러개 메서드를 return하면 스프링은 어떤 jsp를 보여줄지 알 수 없다
그래서 HTTP Status 500 – Internal Server Error 가 뜨는 것
만약 여러개 뷰를 한 컨트롤러에 정의하고 싶으면
맨 처음에 보여주고 싶은 jsp만 return type을 주고
url로 접속하고 싶은 다른 뷰는
void type을 쓰고 @RequestMapping() value에
경로, 파일명 모두 포함해서 쓰자
그러면 url로 경로 입력해서 접근할 수 있음


:star2: :star2: :star2: :star2: :star2: :star2: :star2:

---
