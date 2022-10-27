# [05] - API를 작성하는 다양한 방법

# 프로젝트 설정

이번 장에서 실습할 프로젝트는 [**API작성방법**](https://github.com/Alisherka7/Spring-Boot/tree/main/05.%20API%EB%A5%BC%20%EC%9E%91%EC%84%B1%ED%95%98%EB%8A%94%20%EB%8B%A4%EC%96%91%ED%95%9C%20%EB%B0%A9%EB%B2%95) 링크로 다운받아서 실행하시면 됍니다. 이번에는 groupId는 ‘com.springboot’로 설정하고 name과 artifactId는 ‘api’로 설정합니다.

# GET Api 만들기

GET API는 웹 애플리케이션 서버에서 값을 가져올 때 사용하는 **API입니다**. 실무서에서는 HTTP메서드에 따라 컴트롤러 클래스를 구분하지 않습니다. 근데 여기서는 메서드별로 클래스를 생성합니다. 다음 그림과 같이 ********************controller******************** 패키지를 생성하고 GetController 클래스를 생성합니다.

![Screen Shot 2022-10-27 at 14.06.07.png](https://user-images.githubusercontent.com/38793933/198244014-e971b32e-2017-4599-9ec2-0790106d6599.png)

다음으로 예제 5.1과 같이 컨트롤러에  @RestController와 @RequestMapping을 붙여 내부에 선언되는 메서드에서 사용할 공통 URL을 설정합니다.

*5.1 컨트럴러 클래스에 @RestController와 @RequestMapping을 설정*

```java
@RestController
@RequestMapping("/api/v1/get-api")
public class GetController{

}
```

클래스 수준에서 @RequestMapping을 설정하면 내부에 선언한 메서드의 URL 리소스 앞에 @RequestMapping의 값이 공통 값으로 추가됩니다.

# 5.2.1  @RequestMapping으로 구현하기

@RequestMapping 어노테이션을 별다른 설정 없이 선언하면 HTTP의 모든 요청을 받습니다. 그러니 GET 형식의 요청만 받기 위해서는 어노테이션에 별도 설정이 필요합니다. 

예제 5.2와 같이 @RequestMapping 어노테이션의 method 요소의 값을 RequestMethod.GET으로 설정하면 요청 형식을 GET으로만 설정할 수 있습니다.

![Screen Shot 2022-10-27 at 14.18.00.png](https://user-images.githubusercontent.com/38793933/198244019-6df7c0b8-6ee0-4041-8880-0af58c7b2733.png)

Spring4.3 버전 이후로는 새로 나온 아래의 어노테이션을 사용하기 때문에 @RequestMapping 어노테이션은 더 이상 사용되지 않습니다.  이 블로그에서도 이후 예제에서는 특별히 @RequestMapping을 활용해야 하는 니용이 아리나면 아래의 각 HTTP 메서드에 맞는 어노테이션을 사용할 예정입니다.

- @GetMapping
- @PostMapping
- @PutMapping
- @DeleteMapping

예제에서 작성했던 메서드를 호출하고 싶다면 PostMan 에 다음 그림과 같이 설정하고 “Send”를 눌러주면 됩니다.

![Screen Shot 2022-10-27 at 14.23.30.png](https://user-images.githubusercontent.com/38793933/198244026-cccdf8fd-c107-40ea-9e9c-e3eb5eaf3120.png)

다음 그림과 같이 Response가 나오는 것을 볼수 있습니다.

![Screen Shot 2022-10-27 at 14.23.51.png](https://user-images.githubusercontent.com/38793933/198244028-bf20f54b-8c31-439b-b2a8-9b349ce11653.png)

# 5.2.2 매개변수가 없는 GET 메서드 구현

별도의 매개변수 없이 GET API를 구현하는 경우 5.3과 같이 코드를 작성할 수 있습니다.

![Screen Shot 2022-10-27 at 14.39.19.png](https://user-images.githubusercontent.com/38793933/198244030-157f1f06-ccd3-45fa-839f-e174cda08f27.png)

매개변수가 없는 요청은 위 예제 코드의 1번 줄에 나와 있는 URL을 그대로 입력하고 요청할 때 스프링부트 애플리케이션이 정해진 응답을 반환합니다.

![Screen Shot 2022-10-27 at 14.40.59.png](https://user-images.githubusercontent.com/38793933/198244034-1aefd962-ebfb-4d33-8b76-9b577d5e4e41.png)

Response →

![Screen Shot 2022-10-27 at 14.41.25.png](https://user-images.githubusercontent.com/38793933/198244037-5a8ae83c-14a5-4c4a-b6eb-11f794439577.png)

# 5.2.3 @PathVariable을 활용한 GET 메서드 구현

실무 환경에서는 매개변수를 받지 않는 메서드가 꺼의 쓰이지 않습니다. 웹 통신의 기본 목적은 데이터를 주고받는 것이기 때문에 대부분 매개변수를 받는 메서드를 작성하게 됩니다. 매개변수를 받을 때 자주 쓰이는 방법 중 하나는 URL 자체에 값을 담아 요청하는 것입니다. 다음 예제와 같은 URL에 값을 담아 전달되는 요청을 처리하는 방법을 보여줍니다.

![Screen Shot 2022-10-27 at 16.22.07.png](https://user-images.githubusercontent.com/38793933/198244042-e219027b-ea3b-4770-a117-824e9411b1e9.png)

1번 줄에 있는 요청 예시 URL을 보면 이 메서드는 중괄호{}로 표시된 위치의 값을 받아 요청하는 것을 알 수 있습니다.(실제로 요청 시 중괄호는 들어가지 않으면 값만 존재합니다.). 값을 간단히 전달할 때 주로 사용하는 방법이며, GET 요청에서 많이 사용됩니다.

> 이러한 방식으로 코드를 작성할때는 몇 가지 지켜야 할 규칙이 있습니다. @GetMapping 어노테이션의 값으로 URL을 입력할 때 중괄호를 사용해 어느 위치에서 값을 받을지 지정해야 합니다. 또한 메서드의 매개변수와 그 값을  연결하기 위해 3번 줄과 같이 @PathVariable을 명시하며, @GetMapping 어노테이션과 @PathVariable에 지정된 변수의 이름을 동일하게 맞춰야 합니다.
> 

![Screen Shot 2022-10-27 at 16.29.40.png](https://user-images.githubusercontent.com/38793933/198244046-88068c21-08b7-4e50-a457-e8451fa6f108.png)

![Screen Shot 2022-10-27 at 16.29.50.png](https://user-images.githubusercontent.com/38793933/198244050-2f6bc10a-5330-4afb-97db-f89ed4d3bb31.png)

만약 @GetMapping 어노테이션에 지정한 변수의 이름과 메서드 매개변수의 이름을 동일하게 맞추기 어렵다면 @PathVariable 뒤에 괄호를 열어 @GetMapping 어노테이션의 변수명을 지정합니다.

![Screen Shot 2022-10-27 at 16.33.58.png](https://user-images.githubusercontent.com/38793933/198244053-1b6095cc-51eb-4353-a5ba-58cc9a4e01ee.png)

![Screen Shot 2022-10-27 at 16.35.46.png](https://user-images.githubusercontent.com/38793933/198244057-06b9715d-61a4-4741-a16c-389d4ea8d5fa.png)

![Screen Shot 2022-10-27 at 16.35.57.png](https://user-images.githubusercontent.com/38793933/198244060-0dcd0d60-4066-4fa1-8bbf-2425594e081e.png)

2번 줄에 적혀 있는 변수명인 variable과 3번 줄에 적힌 매개변수명인 var가 서로일치하지 않는 상황에서 두 값을 매핑하는 방법을 보여줍니다. @PathVariable에는 변수의 이름을 특정할 수 있는 value 요소가 존재하며, 이 위치에 변수 이름을 정의하면 매개변수와 매핑할 수 있습니다. 3번 줄의 @PathVariable 사용법을 좀 더 풀어쓰면 다음과 같습니다.

![Screen Shot 2022-10-27 at 16.40.16.png](https://user-images.githubusercontent.com/38793933/198244061-99a7cf35-3e35-4fc4-8247-9d93eb690109.png)

# 5.2.1 @RequestParam을 활용한 GET 메서드 구현

GET 요청을 구현할 때 앞에서 살펴본 방법처럼 URL 경로에 값을 담아 요청을 보내는 방법 외에도 쿼리 형식으로 값을 전달할 수 도 있습니다. 즉, URI에서 ‘?’를 기준으로 우측에 ‘{키}={값}’ 형태로 구성된 요청을 전송하는 방법입니다. 애플리케이션에서 이 같은 형식을 처리하려면 @RequestParam을 활용하면 되는데, 다음 예제와 같이 매개변수 부분에 @RequestParam 어노테이션을 명시해 쿼리 값과 매핑하면 됩니다.

![Screen Shot 2022-10-27 at 16.51.00.png](https://user-images.githubusercontent.com/38793933/198244064-dc322d2e-494b-4eed-8c93-1e446a99a8ac.png)

![Screen Shot 2022-10-27 at 16.51.45.png](https://user-images.githubusercontent.com/38793933/198244066-2a3ca776-ef55-438a-9ad7-4d852bdb5219.png)

getRequestParam() 메서드 호출 결과

![Screen Shot 2022-10-27 at 16.51.57.png](https://user-images.githubusercontent.com/38793933/198244069-2db00800-e1ac-4135-b267-9b0f8f55fd06.png)

1번 줄을 보며 ‘?’ 오른쪽에 쿼리스트링(queryString)이 명시돼 있습니다. 쿼리스트링에는 키(변수의 이름)가 모두 적혀 있기 때문에 이 값을 기준으로 메서드의 매개변수에 이름을 매핑하면 값을 가져올 수 있습니다. 카와 @RequestParam 뒤에 적는 이름을 동일하게 설정하기 어렵다면 @PathVariable 예제에서 사용한 방법처럼 value 요소로 매핑합니다.

만약 쿼리스트링에 어떤 값이 들어올지 모른다면 다음과 같이 Map 객체를 활용할 수도 있습니다.

5.7예제

![Screen Shot 2022-10-27 at 16.59.16.png](https://user-images.githubusercontent.com/38793933/198244073-ccc3d5e8-544e-42b3-99ba-2af6c5744ff1.png)

Request 주소

![Screen Shot 2022-10-27 at 16.59.04.png](https://user-images.githubusercontent.com/38793933/198244075-bbb7d843-fb83-4441-8f81-9f2066346ac7.png)

getRequestParam2() 메서드의 호출 결과

![Screen Shot 2022-10-27 at 17.00.00.png](https://user-images.githubusercontent.com/38793933/198244080-09ad4169-2efd-43b3-9dbf-1a6ca64df2c7.png)

예제 5.7의 형태로 코드를 작성하면 값에 상관없이 요청을 받을 수 있습니다. 예를 들어, 회원 가입 관련 API에서 사용자는 회원 가입을 하면서 ID같은 필수 항목이 아닌 취미 같은 선택 항목에 대해서는 값을 기업하지 않는 경우가 있습니다. 이러한 경우에는 매개변수의 항목이 일정하지 않을 수 있어 Map 객체로 받는 것이 효율적입니다.

### URI와 URL의 차이

> URI은 우리가 흔히 말하는 웹 주소를 의미하며,  리소스 어디에 있는지 알려주기 위한 경로를 의미합니다. 반면 URL는 특정 리소스를 식별할 수 있는 식별자를 의미합니다.
웹에서는 URL을 통해 리소스가 어느 서버에 위치해 있는지 알 수 있으며, 그 서버에 접근해서 리소스에 접근하기 위해서는 대부분 URI가 필요합니다.
>
