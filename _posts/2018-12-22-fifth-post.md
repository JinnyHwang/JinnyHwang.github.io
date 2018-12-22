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
:ghost: :ghost: :ghost: :ghost:

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

---
