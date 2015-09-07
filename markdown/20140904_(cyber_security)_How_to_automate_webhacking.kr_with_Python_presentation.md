footer: How to automate webhacking.kr with Python © 조근영 2015
slidenumbers: true

# [fit] How to automate webhacking.kr
# [fit] with python

---

# Who are you?

![original right 100%](https://avatars3.githubusercontent.com/u/6144782?v=3&s=460)

- 조근영, 남자사람
- **Python** 좋아함
- **Data Analysis**, **TDD**, **Penetration Testing**, **DevOps**, Machine Learning, Natural Language Processing 관심
- 보안쪽 약간 발 담그고 있음
- [github](https://github.com/re4lfl0w/)[^github]

[^github]: https://github.com/re4lfl0w/

- [파으리썬 운영자](euripy.github.io)[^euripy]

[^euripy]: http://euripy.github.io

---

# Casting

![85%](images/casting.png)

- 리뷰[^1] 

[^1]: [Hardware Hacking Training Epilogue](http://nbviewer.ipython.org/github/re4lfl0w/ipython/blob/master/hardware/hardware_hacking/eplilogue.ipynb)

^
presente notes. 오 안 바뀌는군..

---

# Why?

![inline](images/search_webhacking_solve.png)

---

# 헐...

---

## [fit] 5000 개...

---

## [fit] 일반적인 풀이는 너무 많다!!

### [fit] 좋아? 그렇다면 차별화를 하기 위해서는??

---

### Python

---

## Python

---

# Python

---

# [fit] Python!!

![](images/python.jpg)

--- 

## 좋아... Webhacking.kr 사냥하러 가보자
## [fit] 일단 목차를 한 번 봐볼까

![](images/hunting.jpg)

---

### 1일차

- 이론 및 실습 준비
- 난이도 하 문제 풀이(자바스크립트, 파라미터 변조)

---

### 2일차

- 난이도 중 문제 풀이(파라미터 변조, XSS 등)

---

### 3일차

- 난이도 중 ~ 상 문제 풀이(SQL Injection)

---

# 자바스크립트
# 파라미터 변조
# XSS
# SQL Injection

![300%](images/sql_injection.jpg)

---

# [fit] 책으로는 많이 봤는데 문제 풀이는 많이 해보지 않음
# 좋아! 도전이다

![](images/hunting.jpg)

---

# [fit] Key Point

![](images/key.jpg)

---

# Key Point

- 내가 말하고자 하는것은 문제를 마주쳤을 때 어떻게 해결하는지?

![](images/key.jpg)

---

# Key Point

- 내가 말하고자 하는것은 문제를 마주쳤을 때 어떻게 해결하는지?
- [암묵지](http://agile.egloos.com/5853241)에 있는 사고의 과정을 보여주는데 초점

![](images/key.jpg)

---

# Key Point

- 내가 말하고자 하는것은 문제를 마주쳤을 때 어떻게 해결하는지?
- [암묵지](http://agile.egloos.com/5853241)에 있는 사고의 과정을 보여주는데 초점
- 결과를 만들어서 대단하지? 라는게 이 슬라이드에서 원하는게 아님

![](images/key.jpg)

---

# Key Point

- 내가 말하고자 하는것은 문제를 마주쳤을 때 어떻게 해결하는지?
- [암묵지](http://agile.egloos.com/5853241)에 있는 사고의 과정을 보여주는데 초점
- 결과를 만들어서 대단하지? 라는게 이 슬라이드에서 원하는게 아님
- 직관, 의사결정, 상황파악 등을 어떻게 했는지 보여주는게 Point

![](images/key.jpg)

---

# 자동화에 필요한 순서

1. 로그인
2. 문제 보기
  - Highlight 문제
3. 문제 소스 보기
4. 인증하기

---

# 1. 로그인

---

## [fit] webhacking.kr은 로그인이 되어있지 않으면
## 로그인 페이지로 돌려보냄

---

# [fit] 자동화를 하기 위해서는 로그인 정보가 필요함

---

## 로그인 정보를 유지하기 위한 파이썬 라이브러리가 뭐가 있지?

---

## Violent Python 에 나온 
## [fit] mechanize를 사용하자[^2]

![120%](images/violent_python_mechanize.png)


[^2]: (http://www.yes24.com/24/goods/8433461?scode=032&OzSrank=1)

---

# 로그인

![inline](images/login1.png)

---

# 파라미터 확인

![right](images/breakdown.png)

![inline](images/login2.png)

---

# 좋아,
# [fit] ![](images/wireshark.png) 너로 정했다!

![](images/choose_you.png)

---

# [fit] Packet capture login info with wireshark

![inline](images/register.png)

---

# 아하!
# POST method로 id, pw를 인자로 넘기는구나.

---

# Login Mechanize Source

```python
import mechanize
import urllib
import urlparse
from custom_source.login import id_, pw

login_url = 'http://webhacking.kr/index.html?enter=1'
data = urllib.urlencode({'id':id_, 'pw':pw})
browser = mechanize.Browser()
resp = browser.open(login_url, data).read()
```
---

# 2. 문제 보기



---

# No. 15

- 목적: 자바스크립트 소스 확인

---

# Print source

```python
resp = browser.open(index_url).read()
resp = browser.open(challenge_url).read()

def join_url(url, base_url='http://webhacking.kr'):
    if 'view-source:' in url:
        url = url.replace('view-source:', '')
    if 'webhacking' in url and 'http' not in url:
        return '{0}{1}'.format('http://', url)
    return urlparse.urljoin(base_url, url)

def print_source(url):
    resp = browser.open(join_url(url)).read()
    print(resp)
    return resp

print_source('challenge/javascript/js2.html')
```

---

# print_source output

![inline](images/print_source_output.png)

---

# [fit] Colorful 하지도 않고
# [fit] Syntax Highlight도 안되어 있고
# [fit] Python 유저에게는 그저 고난과 역경

![](images/breakdown.png)

---

# 좋아, Syntax Highlight가 되는 것을 찾아보자!

![](images/hunting.jpg)

---

# Googling!

![inline](images/google_pygments.png)

---

# Pygments? 뭐지?1

This is the home of Pygments. It is a **generic syntax highlighter** suitable for use in code hosting, forums, wikis or other applications that need to **prettify source code**. Highlights are:

- a wide range of over **300 languages** and other text formats is supported
- special attention is paid to details that increase **highlighting quality**

---

# Pygments? 뭐지?2

- support for new languages and formats are added easily; most languages use a simple regex-based lexing mechanism
- a number of output formats is available, among them **HTML**, **RTF**, **LaTeX** and ANSI sequences
- it is usable as a **command-line** tool and as a library
... and it highlights even Perl 6!

---

## Pygments Demo

![inline](images/pygments.png)

- [Pygments Demo](http://pygments.org/demo/2618435/)

---

# 깔끔한데..?
# [fit] 근데 이걸 어떻게 내 프로젝트에 적용하지?

---

# 처음에는 이해가 안되었다..

![](images/breakdown.png)

---

# [fit] 여기저기 구글링 하면서 찾아다니다
# [fit] 어디서 봤는지는 기억이 나지 않지만
# [fit] 예제를 찾았다. 심 봤다!

![](images/happy.gif)

---

# [fit] 적용하기 위한 사전 개념 필요

```python
pygments.highlight(code, lexer, formatter, outfile=None)
```
- code: 적용하고자 하는 code
- lexer: 어떤 language를 highlight 할 것인지?(ex: Python, C)
- formatter: 어떤 스타일을 사용할 것인지?(ex: default, friendly)
- highlight: 최종 적용할 code, lexer, formatter 구해서 넣어주자!


![right 60%](images/pocket_monster_union.png)

---

# Pygments original source

```python
lexer = get_lexer_by_name('html')
formatter = HtmlFormatter(style='default', linenos=False, full=True)
data = highlight(response, lexer, formatter)
HTML(data=data)
```
- HTML: IPython Notebook에서 HTML을 뿌려주는 역할

---

# Pygments original output

![inline](images/pygments_original.png)

---

# 오오오... 
# [fit] highlight가 된다.

![](images/impress.png)

---

# 근데 아직 정렬이 안됐다.

---

# 근데 아직 정렬이 안됐다.
# 이제 소스에 
# [fit] 정렬해주는 beautifier를 붙여보자

---

# [fit] beautifier 후보군

1. original
2. [jsbeautifier](http://jsbeautifier.org/)
3. [beautifulsoup](http://www.crummy.com/software/BeautifulSoup/)

---

# [fit] jsbeautifier & beautifulsoup

![inline left](images/pygments_jsbeautifier.png)![inline right](images/pygments_beautifulsoup.png)

---

# [fit] beautifier 후보군 문제점

1. original: 소스 정렬 안됨
2. jsbeautifier: indent 됨, tag 사이에 space 들어가는 문제점
3. beautifulsoup: script 안의 소스가 indent가 안됨

---

# 그만 타협하자...

## [fit] 3번 beautifulsoup 을 선택하고 문제 풀자!!

---

# No.15 Auth

![inline](images/auth.png)

- 문제점: 인증받기 위해 일일이 입력해야 됨..**체크 포인트**

---

# No. 17

- 목적: 자바스크립트 변수 값 확인

---

# Print Source No. 17 

![inline](images/js_calc.png)

---

# 두둥...
## [fit] mechanize에서 javascript를 실행할 수 있는가?

![](images/breakdown.png)

---

# 일단 Python 으로 해결해 보자!

```python
unlock = 100*10*10+100/10-10+10+50-9*8+7-6+5-4*3-2*1*10*100*10*10+100/10-10+10+...
print(unlock/10)
```

# 999780950
## [fit] 어..근데 연산 에러가 발생한것 같다.
## 자바스크립트와 결과가 다름

---

# [fit] IPython의 node.js를 실행해서 풀어보자

![inline](images/ipython_node.png)

---

# 헥헥...
## [fit] 어쨌든 Python 으로 해결하긴 했지만 
## [fit] 다음에는 어떻게 해결해야 할지...

---

# No. 14

- 목적: 변수와 함수, onclick() 사용법

---

# Print Source No.14

```python
resp = print_source('webhacking.kr/challenge/javascript/js1.html')
```

```html
<html>
 ...
  <form name="pw">
   <input type="text" name="input_pwd" />
   <input type="button" value="check" onclick="ck()" />
  </form>
  <script>
   function ck()
{
var ul=document.URL;
ul=ul.indexOf(".kr");
ul=ul*30;
if(ul==pw.input_pwd.value) { alert("Password is "+ul*pw.input_pwd.value); }
else { alert("Wrong"); }
}
  </script>
 </body>
</html>
```

---

# [fit] Chrome Development Tool & IPython

![inline left](images/document_url.png)![inline right](images/ipython_node2.png)

## [fit] 문제점: DOM에 의해 생성되는 document.URL을 일일이 복붙해야 한다.
## [fit] 즉, DOM을 제어해야 한다.

---

# 털썩...
## [fit] 드디어 DOM이 나왔구나[^4]
## [fit] 어떻게 해결해야 하지?

[^4]: [DOM(Document Object Model)](https://developer.mozilla.org/ko/docs/DOM)

![](images/breakdown.png)

---

# [fit] 자... 현재까지 오면서 어떤 문제점을 느끼셨나요?

---

# [fit] 자... 현재까지 오면서 어떤 문제점을 느끼셨나요?
# [fit] 자동화하기 위한 구간이 보이시나요?

---

# 현재까지 나타난 문제점

1. **소스 주소**를 한땀한땀 브라우저에서 **복붙**을 해야한다. 
2. **javascript**를 실행할 수 있는가? 
  - 실행할 수 있다고 해도 브라우저에 **종속**적인 상황에서는 어떻게 할 것인가?(ex: DOM)
3. 브라우저에 한땀한땀 복붙을 해서 **인증**을 한다.

---

![](images/last_pikachu.jpg)


---

# [fit] Browser Controller
# [fit] Selenium

![](images/last_pikachu.jpg)

![inline](images/selenium_logo.png)

---

# [fit] 넌 무슨 듣보잡이야?!

---

![](images/choose_you.png)

# [fit] 구글이 선택한 Test Framework

![inline left 30%](images/google_logo.png)![inline right 150%](images/selenium_logo.png)



---

# [fit] 천마디의 말보다 한 번 보는 게 더 낫다
# [fit] 뭐하는 놈인지는 먼저 보고나서 고민

---

# [fit] Demo Time

---

# Selenium Simple Source

```python
from urllib import quote
from urlparse import urljoin
from time import sleep
from selenium import webdriver

driver = webdriver.Firefox()
google_url = 'https://google.com/'

sleep(5)
driver.get(google_url)
sleep(5)

query = 'python'
search_url = urljoin(google_url, 'search?q={}'.format(quote(query)))
driver.get(search_url)
sleep(10)

driver.quit()
```

---

# Why Selenium?1

- Frequent regression testing(자주하는 회귀 테스팅)
- Rapid feedback to developers(개발자에게 빠른 피드백)
- Virtually unlimited iterations of test case execution(가상으로 제한없이 테스트 케이스 실행)
- Support for Agile and extreme development methodologies(빠른 개발 방법론을 지원)

---

# Why Selenium?2

- Disciplined documentation of test cases(규격화 된 테스트 케이스의 문서화)
- Customized defect reporting(개개인의 요구에 맞춘 리포팅)
- Finding defects missed by manual testing(수동 테스트로 생기는 결함을 찾기)

---

# [fit] 말이 굉장히 어렵다...
# [fit] UI 버그를 빠른 시간내에 잡기 위해서 테스트 한다는 개념으로 보면 됨
# [fit] 사용자 스토리에 따라서 테스트하는 Function Testing에도 사용 됨
# [fit] 난 '그저 자동화 도구'로서의 시각으로 바라봄

---

# [fit] 문제점을 다시 한 번 살펴보자.

---

# 현재까지 나타난 문제점

1. **소스 주소**를 한땀한땀 브라우저에서 **복붙**을 해야한다. 
2. **javascript**를 실행할 수 있는가? 
  - 실행할 수 있다고 해도 브라우저에 **종속**적인 상황에서는 어떻게 할 것인가?(ex: DOM)
3. 브라우저에 한땀한땀 복붙을 해서 **인증**을 한다.

---

# 1. 소스 주소 복붙 문제

---

# [fit] 브라우저는 어차피 source를 받아와서 

---

# [fit] 브라우저는 어차피 source를 받아와서 
# [fit] rendering 해주는 것밖에 없잖아?

---

# [fit] 브라우저는 어차피 source를 받아와서 
# [fit] rendering 해주는 것밖에 없잖아?
# [fit] 그렇다면 내가 source에서 주소를 얻어오면 되지않나?!

---

# 좋아 고고씽!

![](images/go_pikhachu.jpg)

---

# [fit] Challenge Page Source

![100%](images/webhacking_challenge.png)

---

# [fit] 보이는가?

---

# [fit] 보이는가?
# [fit] onclick event로 location.href 함수가 호출된다.

---

# [onclick event](http://www.w3schools.com/jsref/event_onclick.asp)

## [fit] Excute a JavaScript when a button is clicked

```html
<script>
function myFunction() {
    document.getElementById("demo").innerHTML = "Hello World";
}
</script>
<button onclick="myFunction()">Click me</button>
```

## [fit] Hello World가 출력된다.

---

# [location.href](http://www.w3schools.com/jsref/prop_loc_href.asp)

## [fit] Return the entire URL(of the current page)

```javascript
location.href='http://google.com'
```

## [fit] 이러면 페이지가 구글로 이동한다.

---

# 해석하자면

```javascript
onclick="location.href='challenge/web/web-01/'"
```

## [fit] click event가 발생했을 때 
## [fit] http://webhacking.kr/challenge/web/web-01/ 페이지로 이동한다.
## [fit] You got it?

---

# 그렇다면...
## [fit] 저 onclick의 속성을 추출한 후에 location.href의 속성을 추출하면
## [fit] challenge/web/web-01/ 만 얻어진다는 말씀?

---

# [fit] 상대주소를 추출하기 위해서는 DOM을 제어해야 되는데
# [fit] DOM을 어떻게 제어해야 되는거냐?

---

# [XPath](https://ko.wikipedia.org/wiki/XPath)

XPath(XML Path Language)는 W3C의 표준으로 **확장 생성 언어 문서의 구조**를 통해 경로 위에 지정한 구문을 사용하여 항목을 **배치**하고 **처리**하는 방법을 기술하는 언어이다. **XML 표현보다 더 쉽고 약어**로 되어 있으며, XSL 변환(XSLT)과 XML 지시자 언어(XPointer)에 쓰이는 언어이다. XPath는 XML 문서의 노드를 정의하기 위하여 **경로식**을 사용하며, 수학 함수와 기타 확장 가능한 표현들이 있다.

---

# [fit] 역시 언제나 딱딱한 정의는 어려워..
## 이해하기 쉽게 example을 보자

---

# [fit] XPath를 활용한 title 추출

![inline left](images/xpath_title.png) ![inline right](images/xpath_title_text.png)

---

# [fit] title은 잘 추출이 됐다.
## [fit] //title은 title tag를
## [fit] //title/text()는 title의 text만(우리가 원하던 것!)

![](images/xpath_title_text.png)

---

# 마우스로 찍은 XPath

![inline 65%](images/webhacking_challenge2.png)

---

# [fit] 마우스로 찍은 XPath를 보니 굉장히 어렵게 나타나 있다.

```html
html/body/table/tbody/tr[2]/td/center/center/form/table/tbody/tr[1]/td[1]/input
```

---

# [fit] 마우스로 찍은 XPath를 보니 굉장히 어렵게 나타나 있다.

```html
html/body/table/tbody/tr[2]/td/center/center/form/table/tbody/tr[1]/td[1]/input
```

## [fit] 이거 가지고 뭔가 추출하기란 굉장히 어려울 것 같다. 너무 specific 해.
# [fit] 큰 틀만 파악하고 변형해보자!

---

# [fit] 그렇다면 이제 본격적으로 
# [fit] onclick의 속성을 추출해 보자

![right 80%](images/webhacking_challenge2.png)

---

# challenge list 추출

![](images/webhacking_challenge3.png)

---

![right 80%](images/webhacking_challenge4.png)

# XPath 활용해서 onclick 속성 추출

![inline left](images/webhacking_challenge_xpath.png)

## [fit] 의도치않게 ID가 선택이 된다. 이런것을 잘 처리해 주자

---

![right 75%](images/webhacking_challenge5.png)

# 문제들의 input tag만 선택됨

![inline left](images/webhacking_challenge_xpath.png)

## [fit] webhacking.kr의 총 문제수는 66문제다.
하지만 **ID값**이 제일 처음에 **포함**되기 때문에 **67개**의 노드가 추출된 것을 확인 가능

---

# [fit] 문제의 tag들을 추출했다..!!

![](images/impress.png)

---

# [fit] 다들 알고 있겠지만 그래도 확인하는 차원에서
# [fit] Tag와 Attribute와의 차이점을 알고가자

---

# Tag & Attribute 차이점

![inline](images/webhacking_challenge_xpath_tag_attribute.png)

**Tag**: form, table, tbody, tr, td
**Attribute**: type, onclick, style, background, color, onmouseout, onmouseover

---

# @를 붙여주면 Attribute 접근 가능

![right 80%](images/webhacking_challenge_xpath_attribute.png)

```html
//form/table/tbody/tr/td/input/@onclick
```

## 실은 나도 Attribute 추출하는 것을 발표 준비하면서 깨달았다.
## 역시 발표하는건 발표자에게 더 도움이 되는 일

---

# [fit] 그럼 이제 source단에서 Parsing이 
# [fit] 가능하므로 복붙을 하지 않아도 된다.

---

# 현재까지 나타난 문제점

1. ~~**소스 주소**를 한땀한땀 브라우저에서 **복붙**을 해야한다.~~
2. **javascript**를 실행할 수 있는가? 
  - 실행할 수 있다고 해도 브라우저에 **종속**적인 상황에서는 어떻게 할 것인가?(ex: DOM)
3. 브라우저에 한땀한땀 복붙을 해서 **인증**을 한다.

---

# [fit] 그럼 이제 어느 정도 준비가 끝난것 같다.
# [fit] Selenium 으로 로그인부터 다시!

![](images/hunting.jpg)

---

# [fit] 로그인 구현은 mechanize로 해봤는데
# [fit] Selenium은 다른 방식이다.

---

# [fit] Selenium은 우리가 일반적으로 Browser를 
# [fit] 사용하는 방식과 똑같이 사용하면 된다.

---

![](images/webhacking_login.png)

# [fit] Login Logic

1. Connect Login Webpage

---

![](images/webhacking_login.png)

# [fit] Login Logic

1. Connect Login Webpage
2. Input ID

---

![](images/webhacking_login.png)

# [fit] Login Logic

1. Connect Login Webpage
2. Input ID
3. Input PW

---

![](images/webhacking_login.png)

# [fit] Login Logic

1. Connect Login Webpage
2. Input ID
3. Input PW
4. Click Login button

---

![right 120%](images/webhacking_login.png)

# [fit] webhacking.kr Login Analysis

```html
<form method="post" action="index.html?enter=1" 
name="lf" onkeypress="if(event.keyCode==13)go();">
</form>
```

```javascript
function go()
{
    if(lf.id.value=="") { lf.id.focus(); return; }
    if(lf.pw.value=="") { lf.pw.focus(); return; }
    lf.submit();
}
```

---

![right 120%](images/webhacking_login.png)

# [fit] webhacking.kr Login Analysis

```html
<form method="post" action="index.html?enter=1" 
name="lf" onkeypress="if(event.keyCode==13)go();">
</form>
```

```javascript
function go()
{
    if(lf.id.value=="") { lf.id.focus(); return; }
    if(lf.pw.value=="") { lf.pw.focus(); return; }
    lf.submit();
}
```

## [fit] 로그인 하려면 javascript를 써야되네?!



---

# 필요한 함수들 먼저 Import

```python
# built-in
import urllib
import urlparse
import re
import time

# third-party
import jsbeautifier
import mechanize
from selenium import webdriver
from BeautifulSoup import BeautifulSoup as bs
from pygments import highlight
from pygments.lexers import get_lexer_by_name
from pygments.formatters.html import HtmlFormatter
from IPython.display import HTML

# custom
from custom_source.login import id_, pw


login_url = 'http://webhacking.kr/index.html?enter=1'
index_url = 'http://webhacking.kr/index.php'
challenge_url = 'http://webhacking.kr/index.php?mode=challenge'
auth_url = 'http://webhacking.kr/index.php?mode=auth'
```

---

# Login 구현 Source

```python
from urllib import quote
from urlparse import urljoin
from time import sleep
from selenium import webdriver

WAIT = 1
driver = webdriver.Firefox()

sleep(WAIT)
driver.get(login_url)
sleep(WAIT)

sleep(WAIT)
driver.find_element_by_name('id').send_keys(id_)
driver.find_element_by_name('pw').send_keys(pw)
driver.execute_script('go();') # javascript 실행해서 로그인!
sleep(10)

driver.quit()
```

---

# [fit] Demo Time

---

# [fit] Login 구현하면서 javascript 문제까지 해결!

![](images/happy.gif)

---

# 현재까지 나타난 문제점

1. ~~**소스 주소**를 한땀한땀 브라우저에서 **복붙**을 해야한다.~~
2. ~~**javascript**를 실행할 수 있는가?~~
  - ~~실행할 수 있다고 해도 브라우저에 **종속**적인 상황에서는 어떻게 할 것인가?(ex: DOM)~~
3. 브라우저에 한땀한땀 복붙을 해서 **인증**을 한다.

---

# [fit] 이제 인증만 해결하면 된다!

![](images/go_pikhachu.jpg)

---

# Auth Analysis

![right 65%](images/webhacking_auth.png)

```html
<form method="post" action="?mode=auth_go">
    <table>
        <tbody>
            <tr>
                <td>Flag</td>
                <td>
                    <input type="text" name="answer" size="100">
                </td>
            </tr>
            <tr>
                <td colspan="2" align="center">
                    <input type="submit" value="Submit">
                    <br><br>
                Do not brute-force
                </td>
            </tr>
        </tbody>
    </table>
</form>
```

---

# Auth 구현 Source

```python
sleep(WAIT)
driver.get(auth_url)
sleep(WAIT)

sleep(WAIT)
answer = 'off_script'
driver.find_element_by_name('answer').send_keys(answer) # name으로도 선택 가능
# css selector로도 선택 가능
driver.find_elements_by_css_selector('form table tbody tr td input')[-1].click()
sleep(WAIT)

sleep(10)
driver.switch_to.alert.accept() # 중요! alert창 없애줘야 한다!
sleep(10)
```

![100%](images/webhacking_auth_alert.png)

---

# [fit] Demo Time

---

# 현재까지 나타난 문제점

1. ~~**소스 주소**를 한땀한땀 브라우저에서 **복붙**을 해야한다.~~
2. ~~**javascript**를 실행할 수 있는가?~~
  - ~~실행할 수 있다고 해도 브라우저에 **종속**적인 상황에서는 어떻게 할 것인가?(ex: DOM)~~
3. ~~브라우저에 한땀한땀 복붙을 해서 **인증**을 한다.~~

---

# 후...
# 드디어 문제점 모두 해결했다.

![](images/winner.jpg)

---

# [fit] 일단 문제점은 해결됐지만

---

# [fit] 일단 문제점은 해결됐지만
# [fit] 재사용하기 편한 클래스로 변환해야 한다.

---

# [fit] 일단 문제점은 해결됐지만
# [fit] 재사용하기 편한 클래스로 변환해야 한다.
# [fit] Refactoring의 시간

---

# Refactoring

1. login: login 구현. 사용자 id, pw 입력

![150%](images/pikachu_revolution.jpg)

---

# Refactoring

1. login: login 구현. 사용자 id, pw 입력
2. view\_challenge: challenge.html 을 파싱해서 문제들 주소 추출

![150%](images/pikachu_revolution.jpg)

---

# Refactoring

1. login: login 구현. 사용자 id, pw 입력
2. view\_challenge: challenge.html 을 파싱해서 문제들 주소 추출
3. print\_problem\_source: 소스 출력

![150%](images/pikachu_revolution.jpg)

---

# Refactoring

1. login: login 구현. 사용자 id, pw 입력
2. view\_challenge: challenge.html 을 파싱해서 문제들 주소 추출
3. print\_problem\_source: 소스 출력
4. print\_index\_phps: 문제 페이지 안의 소스 출력

![150%](images/pikachu_revolution.jpg)

---

# Refactoring

1. login: login 구현. 사용자 id, pw 입력
2. view\_challenge: challenge.html 을 파싱해서 문제들 주소 추출
3. print\_problem\_source: 소스 출력
4. print\_index\_phps: 문제 페이지 안의 소스 출력
5. auth: 인증 페이지

![150%](images/pikachu_revolution.jpg)

---

# Refactoring

1. login: login 구현. 사용자 id, pw 입력
2. view\_challenge: challenge.html 을 파싱해서 문제들 주소 추출
3. print\_problem\_source: 소스 출력
4. print\_index\_phps: 문제 페이지 안의 소스 출력
5. auth: 인증 페이지
6. accept\_alert: 인증 페이지에서 확인 버튼 클릭하기

![150%](images/pikachu_revolution.jpg)

---

# Class & Methods

```python
class webHacking(object):
    def __init__(self):
    def __del__(self):
    def login(self):
    def view_challenge(self):
    def print_problem_source(self, num):
    def print_index_phps(self, src='index.phps'):
    def auth(self, answer):
    def accept_alert():
```

![150%](images/pikachu_revolution.jpg)

---

# [fit] Mechanize 푹 쉬어!

![](images/rest.jpg)

---

# [fit] 근데 느꼈나..?
# [fit] 처음부터 저렇게 짜임새있는 구조가 나온건 아니야.
# [fit] 삽질 하다보니까 저렇게 하면 편할것 같아서 나온 구조..
# [fit] 즉, 생각의 산물

---

# [fit] Demo Time

---

# 내가 공부한 Resources

1. [Selenium with Python](https://selenium-python.readthedocs.org/)
2. [Selenium 책](http://www.yes24.com/24/goods/12190846?scode=032&OzSrank=1)

![original 120%](http://ecx.images-amazon.com/images/I/51-X8BBhbBL.jpg)

---

# 포켓몬 사진 출처

[포켓몬스터 베스트위시 시즌2 23화 사토시 대 코테츠! 비밀병기 사잔드라!! 리뷰](http://m.blog.naver.com/tjrdl552/130154691876)
[포켓몬스터 베스트위시 시즌2 24화 결착 잇슈리그!](http://m.blog.naver.com/tjrdl552/130156471180)

![](images/pikachu_volleyball.gif)

---

# 사전 지식

- 이 지식이 없으면 엄청난 삽질 동반함!
- virtualenv
- pip

---

# 나중에 추가할 내용들

- print\_problem\_source, print\_index_phps 설명(ppt 만드는 시간이 꽤 많이 든다. 40시간 정도 쓴듯. **암 걸리겠다.**)
- Proxy 적용해서 파라미터 변조(이게 제일 감이 안 잡힙)
  - 어쩔 수 없이 [Fiddler](http://www.telerik.com/fiddler)나 [Burp Suite](https://portswigger.net/burp/)를 써야할듯
- XSS
- SQL Injection
- sqlmap

--- 

# [fit] 감사합니다

---

# [fit] Q&A

---

# 마지막 피카츄걸 짤방

![original 80%](images/pikachu_girl.jpg)


<!-- 
## 뒤에 배경을 다른걸로 바꿔야겠다. 여기가 경계선이라는걸 알려줘야 되니까?
## 아니면 배경색만 바꾸는게 있나?
## background image만 적용하는 것도 좋아보임. -->