> [목차](index.md)

## 9. java.lang패키지와 유용한 클래스

- [java.lang패키지와 유용한 클래스](#javalang%ed%8c%a8%ed%82%a4%ec%a7%80%ec%99%80-%ec%9c%a0%ec%9a%a9%ed%95%9c-%ed%81%b4%eb%9e%98%ec%8a%a4) 
	- [equals](#equals) 
	- [toString()](#tostring)
- [hashCode](#hashcode)
- [String클래스](#string%ed%81%b4%eb%9e%98%ec%8a%a4) 
	- [String 비교](#string-%eb%b9%84%ea%b5%90) 
	- [String 클래스의 생성자와 메서드](#string-%ed%81%b4%eb%9e%98%ec%8a%a4%ec%9d%98-%ec%83%9d%ec%84%b1%ec%9e%90%ec%99%80-%eb%a9%94%ec%84%9c%eb%93%9c)
- [Join과 StringJoiner](#join%ea%b3%bc-stringjoiner)
- [문자열 형변환](#%eb%ac%b8%ec%9e%90%ec%97%b4-%ed%98%95%eb%b3%80%ed%99%98)
- [StringBuffer클래스](#stringbuffer%ed%81%b4%eb%9e%98%ec%8a%a4) 
	- [관련 메서드](#%ea%b4%80%eb%a0%a8-%eb%a9%94%ec%84%9c%eb%93%9c)
- [StringBuilder](#stringbuilder)
- [Math클래스](#math%ed%81%b4%eb%9e%98%ec%8a%a4) 
	- [관련 메서드](#%ea%b4%80%eb%a0%a8-%eb%a9%94%ec%84%9c%eb%93%9c-1)
- [Wrapper class](#wrapper-class)
- [Number class](#number-class) 
	- [관련 메서드](#%ea%b4%80%eb%a0%a8-%eb%a9%94%ec%84%9c%eb%93%9c-2)
- [Autoboxing, unboxing](#autoboxing-unboxing)

<br><br>
<br><br>

# java.lang패키지와 유용한 클래스

- java.lang패키지는 자바프로그래밍에 가장 기본이 되는 클래스들을 포함하고 있다.

| Object클래스의 메서드            | 설명                                                                    |
| :------------------------------- | :---------------------------------------------------------------------- |
| protected Obejct Clone()         | 객체 자신의 복사본을 반환한다.                                          |
| public boolean equals(Object ob) | 객체 자신과 객체 obj가 같은 객체인지 알려준다.                          |
| protected void finalize()        | (거의 사용안함)                                                         |
| public Class getClass()          | 객체 자신의 클래스 정보를 담고 있는 Class인스턴스를 반환한다.           |
| public int hashCode()            | 각체 자신의 해시코드를 반환한다.                                        |
| public String toString()         | 객체 자신의 정보를 문자열로 반환한다.                                   |
| public void notify()             | 객체 자신을 사용하려고 기다리는 쓰레드를 하나만 깨운다.                 |
| public void notifyAll()          | 객체 자신을 사용하려고 기다리는 모든 쓰레드를 깨운다.                   |
| public void wait()               | 다음 쓰레드가 notify()호출할 때까지 현재 쓰레드를 무한히 기다리게 한다. |

<br>

## equals

- 객체를 생성할 때 메모리의 비어있는 공간을 찾아 생성하므로 서로 다른 객체가 같은 주소를 가질순 없다.
- 그러나 두 개 이상의 참조변수가 같은 주소값을 갖는 것은(한 객체를 참조하는 것)은 가능하다.
- `equals메서드`는 주소값으로 비교한다.

```java
class Person {
	long id;

	public boolean equals(Object obj) { // value값을 비교할 수 있도록 equals()을 오버라이딩
		if(obj instanceof Person)
			return id ==((Person)obj).id;
		else
			return false;
	}

	Person(long id) {
		this.id = id;
	}
}
```

<br>

## toString()

- 인스턴스에 대한 정보를 문자열(String)로 제공할 목적으로 정의한 것

```java
public string toString(){
	return getClass().getName()+"@"+Integer.toHexString(hashCode());
}
```

<br><br>
<br><br>

# hashCode

- 해싱(hashing)기법에 사용되는 해시함수(hash function)을 구현한 것이다.
- 해싱은 데이터관리기법 중의 하나인데, 다량의 데이터를 저장하고 검색하는 데 유용하다.
- 해시함수는 찾고자하는 값을 입력하면 그 값이 지정된 위치를 알려주는 해시코드(hash code)를 반환한다.
- 객체의 주소값을 이용해서 해시코드를 만들어 반환하기 때문에 서로 다른 두 객체는 결코 같은 해시코드를 가질 수 없다.

```java
class Ex9_3 {
	public static void main(String[] args) {
		String str1 = new String("abc");
		String str2 = new String("abc");

		System.out.println(str1.equals(str2)); // 출력 : true
		System.out.println(str1.hashCode()); // 출력 : 96354
		System.out.println(str2.hashCode()); // 출력 : 96354
		System.out.println(System.identityHashCode(str1)); // 출력 : 27134973
		System.out.println(System.identityHashCode(str2)); // 출력 : 1284693
	}
}
```

- String클래스는 문자열의 내용이 같으면 동일한 해시코드를 반환하도록 `hashCode메서드`가 오버라이딩되어 있기 때문에 동일한 해시코드를 얻는다.
- 반면에 `System.identityHashCode`는 Object클래스의 `hashCode`메서드처럼 객체의 주소값으로 해시코드를 생성하기 때문에 모든 객체에 대해 항상 다른 해시코드 값을 반환한다.  
  <br>

<br><br>
<br><br>

# String클래스

- 변경 불가능한(Immutable) 클래스
- String에는 문자열을 저장하기 위한 문자형 배열 참조변수`char[]`를 인스턴스 변수로 정의해놓고 있다.

```java
public final class String implements java.io.Serializable, Comparable{
	private char[] value;
}
```

- 덧셈연산자`+` 결합 시 마다 새로운 문자열을 가진 String인스턴스가 생성되어 메모리공간을 차지하게 되므로 가능한 한 결합횟수를 줄이는 것이 좋다.
- 문자열간의 결합이나 추출을 자주 다루는 경우 변경 가능한 `StringBuffer클래스`를 사용하는 것이 좋다.

<br>

## String 비교

- `String str3 = new String("abc")` new 연산자 => 항상 새로운 String인스턴스 생성
- `String str1 = "abc"` 문자열 리터럴 => 이미 존재하는 것을 재사용 - 클래스 파일이 클래스 로더에 의해 메모리에 올라갈 때, 클래스 파일의 리터럴들이 JVM내에 있는 상수 저장소(Constant pool)에 저장된다. - 참조변수는 이 String인스턴스를 참조하게 된다.

```java
class Ex9_6 {
	public static void main(String[] args) {
		String str1 = "abc";
		String str2 = "abc";
		System.out.println("String str1 = \"abc\";"); // 출력 : String str1 = "abc";
		System.out.println("String str2 = \"abc\";"); // 출력 : String str2 = "abc"
		System.out.println("str1 == str2 ?  " + (str1 == str2)); // 출력 : str1 ==str2 ? true
		System.out.println("str1.equals(str2) ? " + str1.equals(str2)); // 출력 : str1.equals(str2) ? true

		String str3 = new String("abc");
		String str4 = new String("abc");
		System.out.println("String str3 = new String(\"abc\");"); // 출력 : String str3 = new String("abc");
		System.out.println("String str4 = new String(\"abc\");"); // 출력 : String str4 = new String("abc");
		System.out.println("str3 == str4 ? " + (str3 == str4)); // 출력 : *str3 == str4 ? false
		System.out.println("str3.equals(str4) ? " + str3.equals(str4)); // 출력 : str3.equals(str4) ? true
	}
}
```

<br>

## String 클래스의 생성자와 메서드

| 메서드                                                | 설명                                                                                         |
| :---------------------------------------------------- | :------------------------------------------------------------------------------------------- |
| String(String s)                                      | 주어진 문자열(s)를 갖는 String인스턴스를 생성                                                |
| String(char[] value)                                  | 주어진 문자열(value)을 갖는 String인스턴스를 생성                                            |
| String(StringBuffer buf)                              | StringBuffer인스턴스가 갖고 있는 문자열과 같은 내용의 String인스턴스를 생성                  |
| Char charAt(int index)                                | 지정된 위치(index)에 있는 문자를 알려준다.                                                   |
| int compareTo(String str)                             | 문자열(str)과 사전순서로 비교한다. 같으면 0, 이전이면 음수, 이후면 양수르 반환               |
| String concat(String str)                             | 문자열(str)을 덧붙인다.                                                                      |
| boolean contains(CharSequence)                        | 지정된 문자열(s)이 포함되었는지 검사한다.                                                    |
| boolean endsWith(String suffix)                       | 지정된 문자열(suffix)로 끝나는지 검사한다.                                                   |
| boolean equals(Obejct obj)                            | 매개변수로 받은 문자열(obj)과 String인스턴스의 문자열을 비교한다.                            |
| boolean equalsIgnoreCase(String str)                  | -                                                                                            |
| int indexOf(int ch, int pos)                          | 주어진 문자(ch)가 문자열에 존재하는지 지정된 위치(pos)부터 확인하여 위치(index)를 반환       |
| String intern()                                       | 문자열을 상수풀(constant pool)에 등록한다. 이미 같은 내용의 문자열이 있을 경우 주소값을 반환 |
| int lastIndexOf(int ch)                               | 지정된 문자 또는 문자코드를 문자열의 오른쪽 끝에서부터 찾아서 위치를 알려준다.               |
| int lastIndexOf(String str)                           | -                                                                                            |
| int lenght()                                          | 문자열의 길이를 알려준다.                                                                    |
| String replace(char old, char nw)                     | 문자열에서 old를 새로운 문자 nw로 바꾼 문자열을 반환                                         |
| String replace(CharSequence old, CharSequence nw)     | -                                                                                            |
| String replaceAll(String regex, String replacement)   | -                                                                                            |
| String replaceFirst(String regex, String replacement) | -                                                                                            |
| String[] split(String regex)                          | 문자열을 지정된 분리자(regex)로 나누어 문자열 배열에 담아 반환한다.                          |
| String[] split(String regex, int limit)               | -                                                                                            |
| boolean startsWith(String Prefix)                     | 주어진 문자열(prefix)로 시작하는지 검사한다.                                                 |
| String substring(int begin, int end)                  | 주어진 시작위치(begin)부터 끝 위치(end)범위에 포함된 문자열을 얻는다.                        |
| String toLowerCase()                                  | String인스턴스에 저장되어있는 모든 문자열을 소문자로 변환하여 반환한다.                      |
| String toUpperCase()                                  | =                                                                                            |
| String trim()                                         | 문자열의 원쪽 끝과 오른쪽 끝에 있는 공백을 없앤 결과를 반환                                  |
| static String valueOf(boolean b)                      | 지정된 값을 문자열로 변환하여 반환한다. 참조변수의 경우 toString()을 호출한 결과를 반환한다. |

<br><br>
<br><br>

# Join과 StringJoiner

- `Join()`은 여러 문자열 사이에 구분자를 넣어서 결합한다.
- 구분자로 문자열을 자르는 `split()`과 반대다.

```java
String animals = "dog,cat,bear";
String[] arr = animals.split(","); // ','를 구분자로 나눠서 배열에 저장
String str = String.join("-",arr); // 배열의 문자열을 '-'로 구분해서 결합
System.out.println(str); // 출력 : dog-cat-bear
```

<br><br>
<br><br>

# 문자열 형변환

- `parseInt()`나 `parseFlaot()`같은 메서드는 문자열에 공백 또는 문자가 포함되어 있는 경우 변환시 `NumberFormatException`예외가 발생할 수 있으니 `trim()`을 사용해야 한다.

|          기본형 -> 문자열 | 문자열 -> 기본형               |
| ------------------------: | :----------------------------- |
| String.valueOf(boolean b) | Boolean.parseBoolean(String e) |
|    String.valueOf(char c) | Byte.parseByte(String s)       |
|     String.valueOf(int i) | Short.parseByte(String s)      |
|    String.valueOf(long l) | Interger..parseByte(String s)  |
|   String.valueOf(float f) | Float..parseByte(String s)     |
|  String.valueOf(double d) | Double.parseByte(String s)     |

<br><br>
<br><br>

# StringBuffer클래스

- 내부적으로 문자열 편집을 위한 버퍼(buffer)를 가지고 있으며, StringBuffer인스턴스를 생성할 때 그 크기를 지정할 수 있다.
- 생성 시 적절한 길이의 char형 배열이 생성되고, 이 배열은 문자열을 저장하고 편집하기 위한 공간(buffer)로 사용된다.

```java
public final class StringBuffer implements java.io.Serializable{
	private char[] value;

	public StringBuffer(int length){
		value = new char[length];
		shared = false;
	}

	public StringBuffer(){
		this(16);
	}

	public StringBuffer(String str){
		this(str.length()+16);
		append(str);
	}
}
```

- `append()`는 반환타입이 자신의 `StringBuffer`주소이므로 연속적으로 호출하는 것이 가능하다.

```java
StringBuffer sb = new StringBuffer("abc");
sb.append("123").append("ZZ");
```

- `StringBuffer`는 `equal메서드`를 오버라이딩하지 않아서 등가비교연산자(==)와 같은 결과를 얻느느다.
- 반면에 `toString()`은 오버라이딩 되어있어서 String인스턴스를 반환한다.

```java
class Ex9_11 {
	public static void main(String[] args) {
		StringBuffer sb  = new StringBuffer("abc");
		StringBuffer sb2 = new StringBuffer("abc");

		System.out.println("sb == sb2 ? " + (sb == sb2));
		System.out.println("sb.equals(sb2) ? " + sb.equals(sb2));

		String s  = sb.toString();	// String s = new String(sb);와 같다.
		String s2 = sb2.toString();

		System.out.println("s.equals(s2) ? " + s.equals(s2));
	}
}
```

<br>

## 관련 메서드

| 메서드                                               | 설명                                                                                                   |
| :--------------------------------------------------- | :----------------------------------------------------------------------------------------------------- |
| StringBuffer()                                       | 16문자를 담을 수 있는 버퍼를 가진 StringBuffer인스턴스를 생성한다.                                     |
| StringBuffer(int length)                             | 지정도니 개수와 문자를 담을 수 있는 버퍼를 가진 StringBuffer인스턴스를 생성한다.                       |
| StringBuffer(String str)                             | -                                                                                                      |
| StringBuffer append(boolean b)                       | 매개변수로 입력된 값을 문자열로 변환하여 StringBuffer인스턴스가 저장하고 있는 문자열의 뒤에 덧붙인다.  |
| StringBuffer append(Object obj)                      | -                                                                                                      |
| int capacity()                                       | StringBuffer인스턴스의 버퍼크기를 알려준다. length()는 버퍼에 담긴 문자열의 길이를 알려준다.           |
| char charAt(int index)                               | 지정된 위치(index)에 있는 문자를 반환한다.                                                             |
| StringBuffer delete(int start, int red)              | 시작위치(start)부터 끝 위치(end) 사이에 있는 문자를 제거한다.                                          |
| StringBuffer insert(int pos, boolean b)              | 두 번째 매개변수로 받은 값을 문자열로 변환하여 저장된 위치(pos)에 추가한다.                            |
| StringBuffer insert(int pos, Obejct obj)             | -                                                                                                      |
| int length()                                         | StringBuffer인스턴스에 저장되어 있는 문자열의 길이를 반환한다.                                         |
| StringBuffer replace(int start, int end, String str) | 지정된 범위의 문자들을 주어진 문자열로 바꾼다.                                                         |
| StringBuffer reverse()                               | StringBuffer인스턴스에 저장되어 있는 문자열의 순서를 거꾸로 나열한다.                                  |
| void setCharAt(int index, char ch)                   | 지정된 위치의 문자를 주어진 문자(ch)로 바꾼다.                                                         |
| void setLength(int newLength)                        | 지정된 길이로 문자열의 길이를 변경한다. 길이를 늘리는 경우에 나머지 빈 공간을 널문자`\u0000`로 채운다. |
| String toString()                                    | StringBuffer인스턴스의 문자열을 String으로 변환                                                        |
| String substring(int start, int end)                 | 지정된 범위 내의 문자열을 String으로 뽑아서 반환한다.                                                  |

<br><br>
<br><br>

# StringBuilder

- `StringBuilder`는 멀티쓰레드에 안전(thread safe)하도록 동기화되어 있다.
- 멀티쓰레드로 작성된 프로그램이 아닌 경우, StringBuffer의 동기화는 불필요하게 성능만 떨어뜨린다.
- 동기화가 StringBuffer의 성능을 떨어뜨리므로 StringBuffer에서 쓰레드의 동기화만 뺸 `StringBuilder`가 추가되었다.
- (현재까지는 싱글쓰레드로 작성하였다.)

```java
StringBuffer = sb;
sb = new StringBuffer();
sb.append("abc");
```

```java
StringBuilder sb;
sb = new StringBuilder();
sb.append("abc");
```

<br><br>
<br><br>

# Math클래스

- Math클래스의 생성자는 private이기 때문에 Math인스턴스를 생성할 수 없다.
- Math의 클래스의 메서드는 모두 static이며 2개의 상수만 정의해 놓았다. 
	- `public static final double E = 2.718...` // 자연로그의 밑 
	- `public static final double PI = 3.14159...` // 원주율

<br>

## 관련 메서드

| 메서드                                | 설명                                                          |
| :------------------------------------ | :------------------------------------------------------------ |
| static double abs(double a)           | 주어진 값의 절대값을 반환한다.                                |
| static double ceil(double a)          | 주어진 값을 올림하여 반환한다.                                |
| static double floor(double a)         | 주어진 값을 버림하여 반환한다.                                |
| static double max(double a, double b) | 주어진 두 값을 비교하여 큰 쪽을 반환한다.                     |
| static double min(double a, double b) | 주어진 두 값을 비교하여 작은 쪽을 반환한다.                   |
| static double random()                | 0.0~1.0 사이 임의의 double 값을 반환(1.0은 포함되지 않는다.)  |
| static double rint(double a)          | 주어진 double값과 가장 가까운 정수값을 double형으로 반환한다. |
| static long round(double a)           | 소수점 첫째자리에서 반올림한 정수값(long)을 반환한다.         |

```java
import static java.lang.Math.*;
import static java.lang.System.*;

class Ex9_13 {
	public static void main(String args[]) {
		double val = 90.7552;
		out.println("round("+ val +")="+round(val));// 반올림

		val *= 100;
		out.println("round("+ val +")="+round(val));// 반올림

		out.println("round("+ val +")/100  =" + round(val)/100);   // 반올림
		out.println("round("+ val +")/100.0=" + round(val)/100.0); // 반올림
		out.println();
		out.printf("ceil(%3.1f)=%3.1f%n",  1.1, ceil(1.1));    // 올림
		out.printf("floor(%3.1f)=%3.1f%n", 1.5, floor(1.5));   // 버림
		out.printf("round(%3.1f)=%d%n",    1.1, round(1.1));   // 반올림
		out.printf("round(%3.1f)=%d%n",    1.5, round(1.5));   // 반올림
		out.printf("rint(%3.1f)=%f%n",     1.5, rint(1.5));    // 반올림
		out.printf("round(%3.1f)=%d%n",   -1.5, round(-1.5));  // 반올림
		out.printf("rint(%3.1f)=%f%n",    -1.5, rint(-1.5));   // 반올림
		out.printf("ceil(%3.1f)=%f%n",    -1.5, ceil(-1.5));   // 올림
		out.printf("floor(%3.1f)=%f%n",   -1.5, floor(-1.5));  // 버림
	}
}
```

- 음수에서는 양수와 달리 -1.5를 버림(floor)하면 -2.0이 된다.

```java
	out.printf("ceil(%3.1f)=%f%n", -1.5, ceil(-1.5));
	out.printf("floor(%3.1f)=%f%n", -1.5, floor(-1.5));
```

<br><br>
<br><br>

# Wrapper class

- 기본형(primitive type)변수를 객체로 다뤄야할때 - 매개변수로 객체를 요구할 때 
	- 기본형 값이 아닌 객체로 저장해야할 때 
	- 객체간의 비교가 필요할 때 등
- `equals()`가 오버라이딩되어 있어서 주소값이 아닌 객체가 가지고 있는 값을 비교한다.
- `toString()`도 오버라이딩되어 있어서 객체가 가지고 있는 값을 문자열로 변환하여 반환한다.
- `compareTo()`로 객체주소를 비교할 수 있다.
- `MAX_VALUE`, `MIN_VALUE`, `SIZE_BYTES`, `TYPE`등의 static상수를 공통적으로 가자고 있다.

| 기본형  | 활용예                            |
| :-----: | :-------------------------------- |
| Boolean | Boolean b = new Boolean(true);    |
|  char   | Character c = new Character('a'); |
|  byte   | Byte b = new Byte(10);            |
|  short  | Short s = new Short(10);          |
|   int   | Integer 1 = new Integer(100);     |
|  long   | Long l = new Long(100);           |
|  float  | Float f = new Float(1.0);         |
| double  | Double d = new Double(1.0);       |

<br><br>
<br><br>

# Number class

- 추상클래스로, 내부적으로 숫자를 멤버변수로 갖는 wrapper class들의 조상이다.
- Object class 
	- Boolean 
	- Character 
	- Number - Byte 
	- Short 
	- Integer 
	- Long 
	- Float 
	- Double 
	- Biginterger : long으로 다룰 수 없는 큰 범위의 정수 
	- BingDecimal : double로도 다룰 수 없는 큰 범위의 부동 소수점수를 처리하기 위함

```java
// Number class안에는 객체가 가지고 있는 값을 숫자와 관련된 기본형으로 변환하여 반환하는 메서드들을 정의하고 있다.
public abstact class Number implements java.io.Serializable{
	public abstract int intValue();
	public abstract long longValue();
	public abstract float floatValue();
	public abstract double doubleValue();

	public byte byteValue(){
		return (byte)intValue();
	}
	public short shortValue(){
		return (Short)intValue();
	}

}
```

<br>

## 관련 메서드

| 자료형 |        문자열 -> 기본형         | 문자열 -> 래퍼클래스        |
| :----: | :-----------------------------: | :-------------------------- |
|  Byte  |   b = Byte.parseByte("100");    | b = Byte.valueOf("100");    |
| Short  |  s = Short.parseShort("100");   | s = Short.valueOf("100");   |
|  Int   |  i = Integer.parseInt("100");   | i = Integer.valueOf("100"); |
|  Long  |   l = Long.parseLong("100");    | l = Long.valueOf("100");    |
| Float  |  f = Float.parseFloat("3.14");  | f = Float.valueOf("3.14");  |
| Double | d = Double.parseDouble("3.14"); | d = Double.valueOf("3.14"); |

```java
// 문자열이 10진수가 아닌 경우 다른 진법(radix)의 숫자일 때도 변환이 가능하도록 다음과 같은 메서드가 제공된다.
static int parseInt(String s, int radix)
static Integer valueOf(String s, int radix)
```

<br><br>
<br><br>

# Autoboxing, unboxing

- 컴파일러가 자동으로 변환하는 코드를 넣어주기 때문에 기본형과 참조형 간의 덧셈을 가능하게 한다.
- `Vector클래스`나 `Array클래스`에 기본형 값을 저장해야할 때 자동으로 형변환코드 추가
- Autoboxing : 기본형 값을 wrapper class의 객체로 자동 변환해주는 것
- unboxing : 반대로 변환하는 것

```java
// 컴파일 전의 코드
int i = 5;
Integer iObj = new Integer(7);
int = sum = i + iObj;

// 컴파일 후의 코드
int i = 5;
Integer iObj = new Integer(7);
int = sum = i + iObj.intValue();
```

```java
// 컴파일 전의 코드
Integer intg = (Integer)i;
Object obj = (Object)i;
Long lng = 100L;

// 컴파일 후의 코드
Integer intg = Integer.valueOf(i);
Object obj = (Object)Integer.valueOf(i);
Long lng = new Long(100L);
```

```java
ArrayList<Integer> list = new ArrayList<Integer>();
list.add(10); // 오토박싱 10 -> new Integer(10)
int value = list.get(0); // 언박싱 new Integer(10) -> 10
```
