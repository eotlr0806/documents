

# Servlet 

___

​          

HttpServlet 클래스를 상속받아 구현하여 개발하며, HttpServlet 를 상속받게 되면 doGet, doPost, doDelete, doPut 등과 같은 HTTP method에 따른 처리를 구현할 수 있는 메서드를 override 할 수 있다.

이때 파라미터로 HttpServletRequest, HttpServletResponse 를 전달받는데, 해당 객체들을 이용하여 Client 로 부터 전달 받은 값 혹은 전달 해줄 값을 셋팅 할 수 있다.

또한 Servlet은 코드 내에 HTML 을 작성하여 Client에 전달하는데, 이러한 기능이 왜 필요한지 생각해볼 필요가 있다.

​       

우리가 사용하는 웹 서비스들은 Client <--> Server 구조인데, 이때 Client 에 항상 동일한 정보만 전달해야 된다면 웹 서버만으로도 충분하다.

즉, 우리가 흔히 아는 Apache , Ngnix 는 웹 서버 역할을 하며 Client 로 요청이 오면 html, css, image 등과 같은 정적인 즉 변하지 않는 자료를 전달해 준다.

하지만 우리가 쓰는 웹 서비스들은 모든 사용자에게 항상 동일한 페이지를 제공하는 정적 페이지만을 사용하진 않는다.
한 가지 예를 들면 특정 페이지를 접속 했을 때, 로그인 여부에 따라서도 보여주는 메뉴들이 다를 수 있다.

이렇게 상황에 따라 동적으로 변해야 하는 동적인 페이지를 제공해야 하는데, 웹 서버가 정적인 페이지 외에도 동적인 페이지를 만들어 제공하기에는 역할이 너무 많아져, 해당 역할은 WAS 에게 부여하게 된다. 우리가 흔히 아는 Tomcat 이 대표적인 WAS 이다.

​      

이렇게 동적으로 페이지를 만들어야 할 경우, Client에서 어떤 정보를 주느냐에 따라 페이지가 변해야 하며, 그 결과를 Client에 전달해야 하기에 Client의 요청과 응답의 데이터에 대한 핸들링이 필요하며, 이러한 것을 규격화 한 Java API 가 Servlet 이다.

위에서 말한것처럼 HttpServlet 클래스를 상속하는 것만으로도, 각 HTTP method 에 맞게 메서드를 구현할 수 있으며, 우리가 직접 웹 서버와 통신을 하여 HTTP 정보를 받아 객체로 파싱 후 사용할 걱정도 없이 HttpServletRequest / HttpServletResponse 를 사용할 수 있다.

Client 에서 브라우저를 통해 GET /home 으로 요청을 하게 되면 Server 에서는 /home 으로 URL 이 파싱된 Servlet을 찾아 doGet 메서드를 호출하게 되며, doGet 메서드를 구현한 값에 따라 html 을 만들어 Client로 전달하게 된다.

이렇게 Servlet은 동적인 페이지를 제공하기 위해 사용한다.

​    

그렇다면 이러한 Servlet을 관리해주는 녀석이 필요한데, 이러한 녀석들이 Servlet Container이다.

우리가 위에서 적은 WAS는 대표적인 Servlet Container로, Servlet의 생성 주기를 관리하고, 우리가 웹 서버에게 요청을 받고, 해당 요청을 Http Servlet Request 객체에 어떻게 담을지에 대한 걱정과 같은 것을 덜어 준다.

​           

​           

# JSP

___

​      

Servlet 을 이용해서 동적인 페이지를 만들 수 있다고 위에 말했다.

단, java 파일에 View 딴에서 사용되는 html 코드를 넣는것 자체가 유지보수에 엄청난 어려움이 있다...

그렇기에, JSP(Java Server Pages) 를 이용해 View딴을 분리한것이라고 생각한다.

또한 JSP는 html 코드 안에 Java 코드를 넣을 수가 있다. 이러한게 가능한 이유는, WAS 가 내부적으로 JSP 파일을 Servlet 으로 변경하기 때문이다.



하지만, View 영역인 JSP 안에 비즈니스 로직이 들어가는 것 또한, 나중에 유지보수를 하면서 어려움이 생길 수 있다.
유지보수를 좀 더 편하게 하려면 굳이 찾지 않아도 "이러한 로직은 여기 있어!" 라고 딱 말할 수 있는 약속이 필요하다. 그렇기에 어떤 로직은 Java에서 어떤로직은 JSP 에서 처리하는건 좋지 않은 방식이지 않나 생각이 든다..