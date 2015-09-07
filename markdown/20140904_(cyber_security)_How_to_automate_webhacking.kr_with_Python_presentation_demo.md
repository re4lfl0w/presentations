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

![110%](images/realize.jpg)

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
2. jsbeautifier: indent 됨, tag 사이에 space 들어가는 문제점. 
  - 온라인 [Online JavaScript beautifier](http://jsbeautifier.org/)는 이런 문제점이 없는데 뭐가 문제일까? [issue](https://github.com/beautify-web/js-beautify/issues/771) 올림
3. beautifulsoup: script 안의 소스가 indent가 안됨

---

# 그만 타협하자...

---

# 그만 타협하자...
# [fit] 그나마 html은 제대로 정렬이 되는걸 택하자
## [fit] 3번 beautifulsoup 을 선택하고 문제 풀자!!

---

# No.15 Source에서
# [fit] password is off_script

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

# python2
# 999780950
# python3
# 999780930.7
```

### python2와 python3는 division 결과가 다르다.
### python2에서 python3와 동일한 결과를 얻기 위해서 추가하자

```python
from __future__ import division
```

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

# 떡밥 투척

![](images/paste_bait.jpg)

![inline](images/toz.png)

- 시간: 9월 4일 금요일 저녁7시~9시
- 장소: 강남토즈타워 2층

