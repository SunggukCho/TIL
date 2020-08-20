# Python requests 패키지

requests를 활용할 일이 생겼는데 어떻게 돌아가는지에 대해 궁금해서 구글링과 공식문서를 통해 공부한 내용을 정리하고자 본 파일을 만들었다. 작성 내용은 본인이 읽어보고 실행해본 결과에 따른 생각을 정리하는 것이기 때문에 사실과는 다소 차이가 있을 수 있다.

### requests?

requests는 python에서 HTTP 요청을 보내주는 패키지이다.

### requests 패키지 다운(CLI환경에서 pip를 통해 다운)

```bash
$ pip insatll requests
```

`requests`패키지가 제대로 다운되었는지 확인하기 위해 `list`명령어를 실행시킨다

```bash
$ pip list
Package            Version
------------------ ---------
attrs              19.3.0   
backcall           0.2.0    
bleach             3.1.5    
...
jupyter            1.0.0
jupyter-client     6.1.6
jupyter-console    6.1.0
jupyter-core       4.6.3
...
pip                20.1.1
...
requests           2.24.0
```

Package 목록에 requests가 있다면 설치가 완료된 것이다. 본인은 `visual studio code`를 사용하는데, 가끔 `pycharm`을 사용하여 프로젝트를 만들면 설정에 따라 가상 환경을 구축하여 프로젝트를 생성할 수도 있기 때문에 해당 가상환경에 `requests`패키지가 설치되지 않을 수 있다.

pycharm에서 해당 프로젝트로 들어간 후 `Alt`+`F12`키를 통해 Terminal을 열어 `pip list`명령어로 `requests`패키지가 있는지 확인하고, 없으면 `pip install requests`를 통해 설치하면 된다.



### requests 요청하고 response 받기

모든 웹서비스는 사용자가 원하는 정보를 알리는 `request`, 그리고 웹 서버에서 우리에게 보여주는 `response`로 이루어져 있다. python에서 requests 패키지를 통해 직접 html 요청을 보내는 두가지 코드를 살펴보도록 하자 .

- 네이버 영화 검색 결과 크롤링

  ```python
  import requests
  
  name = '겨울왕국2'
  url = 'https://movie.naver.com/movie/search/result.nhn?query=' + name + '&section=all&ie=utf8'
  response = requests.get(url)
  ```
  

다양한 영화정보를 공개하고 있는 네이버 영화(https://movie.naver.com/)사이트에서 `겨울왕국2`를 검색한 결과창을 요청하여 응답을 받는 예시이다.



   ```python
  name = '겨울왕국2'
  url = 'https://movie.naver.com/movie/search/result.nhn?query=' + name + '&section=all&ie=utf8'
   ```

원하는 `url`을 만드는 코드이다. 기본적으로 `url`은 `?`를 기준으로 왼쪽은 기본 주소를 나타내고, 오른쪽은 옵션을 나타낸다. 검색 옵션도 들어가고 요청사항 중 하나인 query도 `?` 이후에 옵션으로 들어간다. 이러한 옵션들은 `&`기호로 구분하여 나열하면 여러가지 옵션을 부여할 수 있다.

 `url`을 하나의 str로 검색이름을 직접 입력하여 만들어도 되지만, 실제로 활용하는 단계에서 다양한 제목으로 검색을 하는 경우가 많기 때문에 name을 따로 설정하여 `url` string에 합쳐주는 방법을 활용하였다.

  해당 방법 말고 직접 url string을 만들어주는 클래스를 정의할 수도 있다.

  실제로 네이버 영화사이트에서 겨울왕국2를 검색했을 때 나오는 url과 비교해보자.


    - 만들어낸 url
      https://movie.naver.com/movie/search/result.nhn?query=겨울왕국2&section=all&ie=utf8
    - 실제 검색해서 나온 url
      https://movie.naver.com/movie/search/result.nhn?query=%B0%DC%BF%EF%BF%D5%B1%B92&section=all

  기본적인 주소를 나타내는 부분인`https://movie.naver.com/movie/search/result.nhn`와 옵션을 나타내는 부분에서`section=all`옵션은 일치한다는 것을 알 수 있다. 사실 `section=all`옵션은 아직 무슨 옵션인지 모르지만 본인이 직접 사이트에서 검색해본 결과 기본 옵션으로 들어가 있어서 `url` string을 만들 때 넣어준것 뿐이다. 공통적인 부분을 제외하고 어떤 차이점이 있을까?

  - 만들어낸 url은 `query=겨울왕국2`옵션과 `ie=utf8`옵션이 추가로 들어가있다
  - 실제로 검색해서 나온 url은 `query=%B0%DC%BF%EF%BF%D5%B1%B92`옵션이 추가로 들어가있다.

  이 차이점은 어디에서 온 것일까? 기본적으로 네이버 검색 url을 서버로 보내는 형식은 아스키코드형태로 들어간다(맞나...?). `query`값으로 `겨울왕국2`를 직접 치게되면, 한글을 인코딩해야 하는데 `ie=utf8`이라는 옵션이 바로 네이버에서 기본적으로 사용하는 `utf-8` 방식으로 인코딩을 하라는 옵션인 것이다.

  실제로 `ie=utf8`옵션을 제거한 url인 ([https://movie.naver.com/movie/search/result.nhn?query=%EA%B2%A8%EC%9A%B8%EC%99%95%EA%B5%AD2&section=all](https://movie.naver.com/movie/search/result.nhn?query=겨울왕국2&section=all))을 직접 입력하여 들어갔을 때, 검색은 `겨울왕국2`가 아닌 `寃⑥슱?뺢뎅2`로 검색되는 것을 확인할 수 있다. `utf-8`에 대해서는 나중에 문자 표현 방식을 정리하면서 다시 살펴보기로 하고 다음 코드로 넘어가보자.

  

  ```python
  response = requests.get(url)
  ```

  `requests.get(url)` : requests 패키지의 `.get`메서드를 통해 해당 url에 대한 HTTP 요청을 보낸다. `.get`메서드의 실행 결과로 HTTP 응답 결과를 반환하며, 이를 `response`라는 변수에 저장한다.

  `response`에 저장된 응답 코드를 출력했을 때 코드가 200이면 요청이 성공적으로 되었다는 것을 의미한다. 다른 응답들에 대한 정보는 경험이 없어서 나중에 알아보도록 한다.

  응답 받은 `response`의 type을 print해보면 `<class 'requests.models.Response'>`라는 내용이 출력된다. 즉 requests패키지에서 정의된 class형태를 반환한다는 것을 알 수 있다.

  가장 중요한 response의 내용을 확인하고 싶다면 `response.text`를 출력해보면 된다.

  ```python
  print(response.text)
  ```

  아마 터미널을 확인하면 엄청나게 긴 알 수 없는 글자들이 나오는 것을 알 수 있다. 이 글자들은 `str` 타입을 가지고 있다는 것을 알 수 있는데, 우리는 이렇게 복잡한 문자열에 대해 어떻게 접근할 수 있을까?

  사실 이 복잡한 문자열은 일정한 규칙이 있다. 바로 html 형식이라는 것이다. 잠깐 다른 소리를 하자면 웹사이트를 구성하는 `Front End`, 즉 우리가 직접 보는 페이지는 `html`+`css`+`js`로 이루어져 있다. 이러한 내용은 나중에 웹을 공부하면서 정리할 것이다.

  

- 영화진흥위원회 오픈 API 활용

  영화진흥위원회 오픈 API(http://www.kobis.or.kr/kobisopenapi)사이트에서는 박스오피스, 영화목록, 영화사, 영화인, 등 영화와 관련된 대부분의 DB를 가지고 있고, 이러한 DB를 오픈 API형태로 제공하고 있다. 해당 사이트에 들어가서 OPEN API > 제공 서비스를 들어가면 각 정보에 어떻게 접근하는지에 대한 설명이 자세하게 나와있다.

  우선, 오픈 API를 활용하기 위해 영화진흥위원회 사이트에 회원가입 후, key를 발급받는다. 오픈 API는 보통 무료로 제공되는 API서비스이지만 key를 발급받아야 서비스를 이용할 수 있다. 해당 오픈API 이용약관을 살펴보면 다음과 같이 나와있다.

  ```
  제 3 조 (서비스 제공절차 및 이용 방법)
  3. 회원은 서비스 이용 확인용 API Key(이하, "발급키")를 발급 받아야 하며, 서비스 이용 시 발급 받은 발급키를 함께 전송 하여야만 서비스를 이용할 수 있습니다.
  4. 회원은 발급 받은 발급키를 타인에게 제공, 공개하거나 공유할 수 없으며 발급 받은 회원 본인에 한하여 이를 사용할 수 있습니다
  
  제 8 조 (회원의 의무)
  3. 회원은 아래 각 호에 해당하는 행위를 하여서는 아니 됩니다.
  e. 위원회 회원의 아이디, API키, 비밀번호 등의 개인정보를 해킹, 도용, 거래하는 행위
  ```

  이를 통해 오픈 API로 제공하는 서비스 개발자들에게 유용하게 쓰일 수 있도록 무료로 제공하지만 이를 악용할 수 있는 특정 IP나 앱/웹 환경을 미리 차단하기 위한 역할로 활용된다는 점을 알 수 있다.

  위의 `네이버 영화 검색 결과 크롤링`은 누구나 접근 가능한 단순 검색의 결과를 크롤링 하기 때문에 key가 필요 없지만, 해당 API를 활용하는 것은 내부적인 소프트웨어나 서비스에 접근하는 것이기 때문에 나름 절차가 있다고 생각한다.

  영화진흥위원회 오픈API 사이트 > OPEN API > 제공 서비스에 들어가면 key를 활용해 원하는 서비스에 접근하는 방법을 알려준다. 그 중에서 영화목록 조회 API 서비스를 이용해보도록 하자.

  ```python
  import requests
  
  mykey = '<제공받은 키를 입력>'
  url = f'http://www.kobis.or.kr/kobisopenapi/webservice/rest/movie/searchMovieList.json?key={mykey}'
  response = requests.get(url)
  ```

  아까와 마찬가지로 url을 만들기 위해 `?`를 기준으로 왼쪽에 기본 주소인 `http://www.kobis.or.kr/kobisopenapi/webservice/rest/movie/searchMovieList.json?`를 입력하였다. 마지막의 `.json`은 response의 내용을 `json`형식으로 받겠다는 뜻이고 이를 `xml`파일로 받고 싶다면 `.xml`을 입력하면 된다.

  `?`를 기준으로 우측에는 key를 string interpolation을 통해 입력하였는데, 해당 API서비스를 이용하기 위해 안내되는 요청 인터페이스 중 요청변수 `key`는 필수라고 나와있다. 즉 해당 API를 이용하기 위해서는 `key`변수를 무조건 입력해주어야 한다.

  해당 url로 request를 보낸 반환값인 `response`를 print해보면 주소와 `key`를 제대로 입력하였을 때 `<Response [200]>`, 즉 제대로 된 응답이 왔다는 것을 알 수 있고 `response`의 type을 print해보면 아까와 같이 `<class 'requests.models.Response'>`라는 내용이 반환된다. response의 내용을 보기 위해 response.text을 출력해보자.  엄청난 string이 출력되는 것을 확인할 수 있다. 이 또한 규칙이 없어 보이지만 아까 주소에 요청한 대로 `json`파일 형식으로 반환된 것이다.

  다른 변수를 더 활용하여 검색을 하고 싶다면 어떻게 해야할까? 물론 `&`기호로 구분하여 요청변수를 추가해주면 된다. `겨울왕국2`라는 제목으로 검색하고자 하면 url은 다음과 같아진다.

  ```python
  import requests
  
  mykey = 'f423e5053f302416a1f16264dfb431dc'
  url = f'http://www.kobis.or.kr/kobisopenapi/webservice/rest/movie/searchMovieList.json?key={mykey}&movieNm=겨울왕국2'
  response = requests.get(url)
  ```

  간단하다! 하지만 우리가 이런 API를 활용할 때 수 많은 영화 데이터에 접근을 하고 싶을텐데, 코드를 실행할 때마다 movieNm에 영화 제목을 치려면 불편하다. 따라서 우리는 이를 위해 movieNm을 따로 변수로 만들어서 미리 입력된 영화 이름들을 변수에 적용하여 interpolation을 할 수 있다.

  ```python
  import requests
  
  mykey = 'f423e5053f302416a1f16264dfb431dc'
  movieNm = '겨울왕국2'
  url = f'http://www.kobis.or.kr/kobisopenapi/webservice/rest/movie/searchMovieList.json?key={mykey}&movieNm={movieNm}'
  response = requests.get(url)
  ```

  이렇게 string interpolation을 통해 우리는 조금 더 편하게 활용할 수 있다. 하지만 이마저도 귀찮다면? `request.get()` 메서드에 설정된 params 인자를 활용하면 된다. params 인자는 무엇이고 어떻게 활용하는 것일까? 이는 `requests` 패키지의 `api.py` 모듈을 통해 알 수 있다.

  설치된 `requests` 패키지의 위치를 보고 싶다면 CLI창에서 `show`명령어를 활용하면 된다

  ```bash
  >> pip show requests
  ```

  패키지 requests의 정보가 출력이 되는데, 그 중에서 Location을 따라 가면 여러가지 모듈을 확인할 수 있다.

  ```python
  def get(url, params=None, **kwargs):
      r"""Sends a GET request.
  
      :param url: URL for the new :class:`Request` object.
      :param params: (optional) Dictionary, list of tuples or bytes to send
          in the query string for the :class:`Request`.
      :param \*\*kwargs: Optional arguments that ``request`` takes.
      :return: :class:`Response <Response>` object
      :rtype: requests.Response
      """
  
      kwargs.setdefault('allow_redirects', True)
      return request('get', url, params=params, **kwargs)
  ```

  우리가 사용하고 있는 get 메서드를 정의하는 부분이다. 메서드를 정의할 때 첫번째 인자로 `self`가 들어간다고 배웠으나 여기서는 `url`을 인자로 받고 있다. 이부분은 패키지와 모듈에 대한 공부를 해야지 알 수 있을것같다. 지금은 우선 넘어가고 `params=None`을 통해 params를 받지만 인자로 들어오지 않을 경우 default로 `None`을 설정한 다는 것을 볼 수 있다. 밑의 주석을 보면, `param params: (optional) Dictionary, list of tuples or bytes to send in the query string for the :class:Request.`라고 나와있는데, params가 필수는 아니고 Dictionary형태로 주어저야 한다는 것을 알 수 있다. 또 `Request` 클래스에서 `query string`을 보내기 위해 사용된다는 것을 알 수 있다. 따라서 우리는 `params` 요청 변수에 `dictionary`형태로 우리들의 query를 넣어주면 활용할 수 있다.

  ```python
  import requests
  
  mykey = 'f423e5053f302416a1f16264dfb431dc'
  movieNm = '겨울왕국2'
  url = 'http://www.kobis.or.kr/kobisopenapi/webservice/rest/movie/searchMovieList.json?'
  
  my_params = {
      'key':'f423e5053f302416a1f16264dfb431dc',
      'movieNm':'겨울왕국2'
  }
  
  response = requests.get(url, params = my_params)
  ```

  즉 `url` string에 `?`이전의 주소까지만 넣고, 필요한 모든 요청변수들을 `my_params`라는 `dictionary`형태로 만들어서 한번에 사용할 수 있다는 것이다. 이 때 주의할 점은 `params`는 `키워드 인지`이기 때문에 

  ```pytohn
  requests.get(url, my_params)
  ```

  가 아니라

  ```python
  requests.get(url, params = my_params)
  ```

  이렇게 넣어주어야 한다는 것이다. 실험 결과 위에처럼 넣어도 실행은 되나 다양한 메서드 및 함수를 활용할 때 키워드 인자를 제대로 새용하기 위한 습관을 들여주는 것이 좋다.



### response 형식에 따른 정보 접근하기

- 영화진흥위원회 오픈 API --> XML 혹은 JSON

  영화진흥위원회 오픈 API > OPEN API > 제공 서비스에 따르면, `REST 방식`(우리가 사용한 방식)의 응답 형식은 `XML`과 `JSON`을 지원한다고 나와있다. 우리는 기본 주소를 `.json`으로 받았기 때문에 우리의 `response`에 들어있는 정보인 `response.text`는 `json` 형식의 규칙에 따라 작성되었다고 할 수 있다. 하지만 아까 `response.text`의 type은 `str`이었는데 어떻게 `json`이라고 할 수 있는가?

  위키백과에 따르면 `json`은 다음과 같다.

  >  `json`이란, 속성-값 쌍 또는 "키-값 쌍"으로 이루어진 데이터 오브젝트를 전달하기 위해 인간이 읽을 수 있는 텍스트를 사용하는 개방형 표준 포맷이다.

  즉 , 일정한 규칙을 가진 텍스트 형태의 문서라는 것이다. 이러한 `str`형태의 `response.text`를 `key`-`value`로 편하게 접근하기 위해 우리는 어떻게 json 형태로 바꿀 수 있을까? 이는 패키지의 `models.py`모듈 중 `json`메서드를 확인해보면 알 수 있다.

  ```python
      def json(self, **kwargs):
          r"""Returns the json-encoded content of a response, if any.
          ...
  ```

  우리는 이를 통해 `.json`메서드가 `response`를 `json`형식으로 `encode`한 내용을 반환한다는 메서드라는 것을 알 수 있다. 메서드의 함수 내용을 굳이 분석하지 않아도 주석으로 된 doc만 읽어보아도 무엇을 위해 해당 메서드가 존재하는지 쉽게 알 수 있다!

  ```python
  my_json = response.json()
  ```

  메서드 하나만으로 우리는 `str`형태를 `dict`형태로 파싱할 수 있는 것이다.

  > `파싱`은 일련의 문자열을 의미있는 토큰으로 분해하고 이들로 이루어진 파스 트리를 만드는 과정이다.

  파싱은 구문분석이라고도 하며, 문법적인 규칙들이 맞는지 확인을 하고, 규칙이 맞다면 규칙대로 구조를 만드는 행위이다. 파싱은 이러한 문서에도 적용될 수 있고 우리가 프로그래밍 언어를 작성하여 실행할 때에도 파싱이 활용된다. 파싱 과정 중 문법적인 규칙들이 틀리다면 `Syntex Error`를 발생시킨다. 

  이제 `my_json`이라는 `dictionary`를 통해 우리가 원하는 정보에 접근하면 되는 것이다.