> ## 참고서적

- ### Java의 정석 기초편

  - [github](https://github.com/castello/javajungsuk_basic)
  - [코드초보스터디](https://cafe.naver.com/javachobostudy)
  - 자바의정석기초 `Youtube`

<br>
 
> ## 목차
## 1. 자바를 시작하기 전에
- [자바를 시작하기 전에](1-자바를-시작하기-전에.md#자바의-특징)  
  
## 2. 변수
- [변수](#변수)
- [String](#string)
- [타입 형변환](#타입-형변환)
- [입출력](#입출력)

## 3. 연산자

- [연산자와 피연산자](#연산자와-피연산자)
- [자동 형변환](#자동-형변환)

## 4. 조건문과 반복문

- [조건문과 반복문](#조건문과-반복문)
- [이름 붙은 반복문](#이름-붙은-반복문)

## 5. 배열

- [배열](#배열)
- [다차원 배열의 선언](#다차원-배열의-선언)
- [Arrays로 배열 다루기](#arrays로-배열-다루기)

## 6. 객체지향 프로그래밍1

- [객체지향 프로그래밍](#객체지향-프로그래밍)
- [객체의 생성과 사용](#객체의-생성과-사용)
- [객체 배열](#객체-배열)
- [클래스의 정의](#클래스의-정의)
- [선언위치에 따른 변수의 종류](#선언위치에-따른-변수의-종류)
- [호출스택\_call stack](#호출스택_call-stack)
- [기본형 매개변수](#기본형-매개변수)
- [오버로딩](#오버로딩)
- [static메서드와 instance메서드](#static메서드와-instance메서드)
- [생성자\_contructor](#생성자_contructor)
- [this\_객체 자신을 가리키는 참조변수](#this_객체-자신을-가리키는-참조변수)
- [변수의 초기화](#변수의-초기화)

## 7. 객체지향 프로그래밍2

- [상속](#상속)
- [포함 Composite 관계](#포함-composite-관계)
- [오버라이딩 Overriding](#오버라이딩-overriding)
- [참조변수 super](#참조변수-super)
- [패키지 Package](#패키지-package)
- [Import](#import)
- [제어자 modifier](#제어자-modifier)
- [static](#static)
- [final](#final)
- [abstract](#abstract)
- [다형성 Polymorphism](#다형성-polymorphism)
- [추상 클래스 abstract class](#추상-클래스-abstract-class)
- [추상 메서드 abstract method](#추상-메서드-abstract-method)
- [인터페이스 Interface](#인터페이스-Interface)
- [인터페이스를 이용한 다형성](#인터페이스를-이용한-다형성)
- [디폴트 메서드와 static메서드](#디폴트-메서드와-static메서드)
- [내부클래스 inner class](#내부클래스-inner-class)
- [익명 클래스 anonymous class](#익명-클래스-anonymous-class)

## 8. 예외처리

- [예외처리](#예외처리)
- [예외 처리하기 try_catch](#예외-처리하기-try_catch)
- [예외 발생시키기](#예외-발생시키기)
- [사용자 정의 예외만들기](#사용자-정의-예외만들기)
- [예외 되던지기 exception re_throwing](#예외-되던지기-exception-re_throwing)
- [연결된 예외](#연결된-예외)

## 9. java.lang패키지와 유용한 클래스

- [java.lang패키지와 유용한 클래스](#java.lang패키지와-유용한-클래스)
- [hashCode](#hashcode)
- [String클래스](#string클래스)
- [Join과 StringJoiner](#Join과-StringJoiner)
- [문자열 형변환](#문자열-형변환)
- [StringBuffer클래스](#StringBuffer클래스)
- [StringBuilder](#stringbuilder)
- [Math클래스](#Math클래스)
- [Wrapper class](#wrapper-class)
- [Number class](#number-class)
- [Autoboxing, unboxing](#autoboxing-unboxing)

## 10. 날짜와 시간,형식화

- [Calendar클래스](#Calendar클래스)
- [형식화 클래스](#형식화-클래스)

## 11. 컬렉션 프레임웍

- [Collection framework](#collection-framework) 
	- [Collection framework와 핵심 인터페이스](#collection-framework%ec%99%80-%ed%95%b5%ec%8b%ac-%ec%9d%b8%ed%84%b0%ed%8e%98%ec%9d%b4%ec%8a%a4)
- [Collection인터페이스](#collection%ec%9d%b8%ed%84%b0%ed%8e%98%ec%9d%b4%ec%8a%a4)
- [List인터페이스](#list%ec%9d%b8%ed%84%b0%ed%8e%98%ec%9d%b4%ec%8a%a4)
- [Set인터페이스](#set%ec%9d%b8%ed%84%b0%ed%8e%98%ec%9d%b4%ec%8a%a4)
- [Map인터페이스](#map%ec%9d%b8%ed%84%b0%ed%8e%98%ec%9d%b4%ec%8a%a4) 
	- [Map과 Iterator](#map%ea%b3%bc-iterator)
- [ArrayList](#arraylist) 
	- [관련 메서드](#%ea%b4%80%eb%a0%a8-%eb%a9%94%ec%84%9c%eb%93%9c)
- [Array](#array)
- [LinkedList](#linkedlist)
- [Stack과 Queue](#stack%ea%b3%bc-queue) 
	- [Stack](#stack) 
	- [Queue](#queue) 
	- [Stack과 Queue의 활용](#stack%ea%b3%bc-queue%ec%9d%98-%ed%99%9c%ec%9a%a9)
- [Iterator, ListIterator, Enumeration](#iterator-listiterator-enumeration)
- [Comparator와 Comparable](#comparator%ec%99%80-comparable)
- [HashSet](#hashset)
- [TreeSet](#treeset)
- [HashMap의 Key와 Value](#hashmap%ec%9d%98-key%ec%99%80-value)
- [Collection의 메서드\_동기화](#collection%ec%9d%98-%eb%a9%94%ec%84%9c%eb%93%9c%eb%8f%99%ea%b8%b0%ed%99%94)
- [Collection의 메서드\_변경불가,싱글톤,단일컬렉션](#collection%ec%9d%98-%eb%a9%94%ec%84%9c%eb%93%9c%eb%b3%80%ea%b2%bd%eb%b6%88%ea%b0%80%ec%8b%b1%ea%b8%80%ed%86%a4%eb%8b%a8%ec%9d%bc%ec%bb%ac%eb%a0%89%ec%85%98)
- [컬랙션 클래스 정리요약](#%ec%bb%ac%eb%9e%99%ec%85%98-%ed%81%b4%eb%9e%98%ec%8a%a4-%ec%a0%95%eb%a6%ac%ec%9a%94%ec%95%bd)


## 12. 지네릭스,열거형,애너테이션

- [타입 변수](#타입-변수)
- [Generic Type과 다형성](#Generic-Type과-다형성)
- [Iterator](#Iterator)
- [HashMap<K,V>](#HashMap<K,V>)
- [제한된 지네릭 클래스](#제한된-지네릭-클래스)
- [지네릭스의 제약](#지네릭스의-제약)
- [와일드 카드](#와일드-카드)
- [Generic type의 형변환](#Generic-type의-형변환)
- [Generic Type의 제거](#Generic-Type의-제거)
- [Enum](#Enum)
- [열거형의 조상\_java.lang.Enum](#열거형의-조상_java.lang.Enum)
- [열거형에 멤버 추가하기](#열거형에-멤버-추가하기)
- [Annotation](#Annotation)

## 13. 쓰레드

- [Process와 Thread](#Process와-Thread)
- [쓰레드의 실행](#쓰레드의-실행)
- [싱글쓰레드와 멀티쓰레드](#싱글쓰레드와-멀티쓰레드)
- [쓰레드의 우선순위](#쓰레드의-우선순위)
- [Daemon Thread](#Daemon-Thread)
- [쓰레드의 상태](#쓰레드의-상태)
- [쓰레드의 실행제어](#쓰레드의-실행제어)
- [쓰레드의 동기화 sychronization](#쓰레드의-동기화-sychronization)
- [sychronized를 이용한 동기화](#sychronized를-이용한-동기화)

## 14. 람다식

- [람다식](#람다식)
- [Functinal Interface](#Functinal-Interface)
- [java.util.function패키지](#java.util.function패키지)
- [매개변수가 두 개인 함수형 인터페이스](#매개변수가-두-개인-함수형-인터페이스)
- [Predicate의 결합](#Predicate의-결합)
- [컬렉션 프레임웍과 함수형 인터페이스](#컬렉션-프레임웍과-함수형-인터페이스)
- [메서드 참조](#메서드-참조)
- [Stream](#Stream)
- [스트림 만들기\_파일과 빈 스트림](#스트림-만들기_파일과-빈-스트림)
- [스트림의 연산\_중간 연산](#스트림의-연산_중간-연산)
- [스트림의 연산\_최종 연산](#스트림의-연산_최종-연산)
- [Optional](#Optional)
- [Collect와 Collectors](#Collect와-Collectors)
- [스트림을 컬렉션, 배열로 변환](#스트림을-컬렉션,-배열로-변환)
- [스트림의 그룹화와 분할](#스트림의-그룹화와-분할)
- [스트림의 변환](#스트림의-변환)

## 14. 입출력

- [바이트 기반 스트림_InputStream, OutputStream](#%eb%b0%94%ec%9d%b4%ed%8a%b8-%ea%b8%b0%eb%b0%98-%ec%8a%a4%ed%8a%b8%eb%a6%bcinputstream-outputstream)
- [보조 스트림](#%eb%b3%b4%ec%a1%b0-%ec%8a%a4%ed%8a%b8%eb%a6%bc)
- [문자기반 스트림_Reader, Writer](#%eb%ac%b8%ec%9e%90%ea%b8%b0%eb%b0%98-%ec%8a%a4%ed%8a%b8%eb%a6%bcreader-writer)
	- [문자 기반 스트림_Reader](#%eb%ac%b8%ec%9e%90-%ea%b8%b0%eb%b0%98-%ec%8a%a4%ed%8a%b8%eb%a6%bcreader)
	- [문자 기반 스트림_Writer](#%eb%ac%b8%ec%9e%90-%ea%b8%b0%eb%b0%98-%ec%8a%a4%ed%8a%b8%eb%a6%bcwriter)
	- [StringReader와 StringWriter](#stringreader%ec%99%80-stringwriter)
	- [BufferedReader와 BufferedWriter](#bufferedreader%ec%99%80-bufferedwriter)
	- [InputStreamReader, OutputStreamWriter](#inputstreamreader-outputstreamwriter)
	- [바이트 기반 스트림과 문자 기반 스트림의 비교](#%eb%b0%94%ec%9d%b4%ed%8a%b8-%ea%b8%b0%eb%b0%98-%ec%8a%a4%ed%8a%b8%eb%a6%bc%ea%b3%bc-%eb%ac%b8%ec%9e%90-%ea%b8%b0%eb%b0%98-%ec%8a%a4%ed%8a%b8%eb%a6%bc%ec%9d%98-%eb%b9%84%ea%b5%90)
- [InputStream과 OutputStream](#inputstream%ea%b3%bc-outputstream)
- [FileInputStream과 FileOutputStream](#fileinputstream%ea%b3%bc-fileoutputstream)
- [FilterInputStream과 FilterOutputStream](#filterinputstream%ea%b3%bc-filteroutputstream)
- [BufferedInputStream과 BufferedOutputStream](#bufferedinputstream%ea%b3%bc-bufferedoutputstream)
	- [BufferedInputStream](#bufferedinputstream)
	- [BufferedOutputStream](#bufferedoutputstream)
- [SequenceInputStream](#sequenceinputstream)
- [PrintSteam](#printsteam)
- [표준 입츌력 Standard IO](#%ed%91%9c%ec%a4%80-%ec%9e%85%ec%b8%8c%eb%a0%a5-standard-io)
	- [표준 입출력의 대상변경](#%ed%91%9c%ec%a4%80-%ec%9e%85%ec%b6%9c%eb%a0%a5%ec%9d%98-%eb%8c%80%ec%83%81%eb%b3%80%ea%b2%bd)
- [File클래스](#file%ed%81%b4%eb%9e%98%ec%8a%a4)
- [직렬화 serialization](#%ec%a7%81%eb%a0%ac%ed%99%94-serialization)
	- [ObjectInputStream, ObjectOutputStream](#objectinputstream-objectoutputstream)
- [직렬화가 가능한 클래스 만들기](#%ec%a7%81%eb%a0%ac%ed%99%94%ea%b0%80-%ea%b0%80%eb%8a%a5%ed%95%9c-%ed%81%b4%eb%9e%98%ec%8a%a4-%eb%a7%8c%eb%93%a4%ea%b8%b0)
	- [직렬화와 역직렬화 예제](#%ec%a7%81%eb%a0%ac%ed%99%94%ec%99%80-%ec%97%ad%ec%a7%81%eb%a0%ac%ed%99%94-%ec%98%88%ec%a0%9c)

## 14. 입출력

- [네트워킹이란?](#네트워킹이란?)
- [InetAddress클래스](#InetAddress클래스)
- [URL](#URL)
- [소켓 프로그래밍](#소켓-프로그래밍)
