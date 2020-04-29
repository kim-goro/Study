> [목차](index.md)
### HTML
- [환경 준비](#환경-준비)
- [HTML 태그](#HTML-태그)
- [서버와 클라이언트](#서버와-클라이언트)
- [웹 서버와 HTTP](#웹-서버와-HTTP)

<br><br>
<br><br>



# 환경 준비
- [Atom 에디터](https://atom.is/)
- 국제민간표준화기구 [W3C](https://www.w3.org/)
- 저작권 없는 이미지 사이트 [언스플래시](https://unsplash.com/)
- [HTML 태그 통계 순위](https://advancedwebranking.com/)

<br><br>
<br><br>

# HTML 태그
- `<head>` : 본문을 설명하는 태그
- `<body>` : 본문
- `<html>`
- `<title>`
- `<meta>` : 웹페이지 요구사항
- `<div>`
- `<a>`
  - `href>` : HyperText Reference 
  - `target>` : 새 탭에서 열람
  - `title>` : 툴팁
- `<script>`
- `<link>`
- `<img>`
  - `src>` : source
- `<p>`
- `<span>`
- `<li>`
- `<ul>` : `<li>`의 부모태그
- `<br>`
- `<style>`
- `<h1>`
- `<h2>`
- `<input>`
- `<form>`
- `<strong>`
- `<h3>`
- `<table>`
- `<tr>`
- `<td>`
- `<ol>` : ordered list
<br>

> :bulb: 스크린리더 같은 보조장치로를 이용해 정보를 접하게 되는 상황 등 에서, `<br>`보다 `<p>`태그가 단락이 존재한다는 것을 의미론적으로 표현할 수 있기 때문에 검색엔진의 입장에서 더 가치있습니다.
또한 <img>보다는 HTML의 의미에 맞게 설계하는 것이 중요합니다. 이것이 바로 접근성(Acceessibility)라고 합니다.  

<br>

# 서버와 클라이언트
- 웹 서버를 구축하는 방법
  1. 컴퓨터에 직접 웹 서버를 설치하는 방법입니다.
  2. 호스트를 빌려주는 호스팅 업체를 이용한다. (인터넷이 연결된 컴퓨터 하나하나를 호스트(host)라고 한다.)
    - Static Web hosting
      - [GitHub](https://github.com/)
      - Neocities.org
      - Azure Bolb
      - BitBalloon
      - Google Colud Storage
      - Amazon S3
    - Dynamic Web hosting
      - Apache
      - IIS
      - Ngnix
- WAMP(Windows Apache MySQL PHP) 설치하기
  - Bitami 매니저 프로그램을 이용하면 여러 가지 프로그램을 관리하기가 수월합니다.
    - https://bitnami.com/stack/wamp 
  - 리눅스에 설치하기
    - https://www.digitalocean.com/community/tutorials/how-to-install-the-apache-web-server-on-ubuntu-16-04
    - `$ sudo apt-get update` : 'sudo'는 슈퍼관리자의 권한으로 명령을 실행
    - `$ sudo apt-get install apache2` : 'apt-get'은 우분투 등의 리눅스배포판에서 일종의 앱스토어와 같은 것이다.
    - `$ sudo hostname -I`
    - `$ cd /var/www/html`
    - `$ ls -al`
    - `$ sudo mv index.html index2.html` : 이름 변경
    - `$ cd ~/web1`
    - `$ sudo cp*/var/www/html`
    - `$`
  - ( MacOS는 아파치가 설치되어 있다. )

<br><br>
<br><br>


# 웹 서버와 HTTP
- http://localhost/index.html, http://127.0.0.1/index.html
  - `http://` 웹 브라우저와 웹 서버가 서로 통신할 때 사용하는 통신 규약인 HyperText Transfer Protocol을 이용한다.
  - `127.0.0.1` (IPA) Internet Protocol Address : 웹 브라우저가 설치돼 있는 컴퓨터를 가리키는 주소
  - `127.0.0.1`를 입력할 경우 그 웹브라우저가 설치돼 있는 각자의 컴퓨터에 있는 웹 서버를 가리킵니다.
  - `index.html`를 입력하면 웹 브라우저가 자신의 컴퓨터에 설치된 웹서비스에 접속해서 index.html 파일을 요청하게 됩니다.
- file://~/index.html
  - `file`로 시작하는 주소의 경우 컴퓨터 안에 설치된 웹 서버가 이 과정에 개입하지 않습니다. 

<br>

## 웹 서버와 웹 브라우저의 통신
- `127.0.0.1` 자기 자신의 컴퓨터를 나타낸다.
- 웹 브라우저가 웹 서버에 요청하려면, 웹 서버의 IP주소 혹은 도메인이 필요합니다.
- http://127.0.0.1:8080/index.html
  - `:8080` : PORT, 웹 서버를 구분하여 접속하기 위함
  


