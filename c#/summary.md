## 1부
- [변수](#변수)
- [지역 변수와 전역 변수](#지역-변수와-전역-변수)
- [static 변수, static 함수](#static-변수,-static-함수)
- [형변환](#형변환)
- [조건문 반복문](#조건문-반복문)
- [컬렉션](#컬렉션)
    - [Hashtable](#Hashtable)
    - [ArrayList](#ArrayList)
- [일반화컬렉션 <T>](#일반화컬렉션-<T>)
    - [List](#List)
    - [Queue](#Queue)
    - [Stack](#Stack)
    - [Dictionary](#Dictionary)

## 2부
- [Vector](#Vector)
- [평행이동과 좌표계, 부모자식 관계](#평행이동과-좌표계,-부모자식-관계)
- [회전과 쿼터니언](#회전과-쿼터니언)
- [오버로드](#오버로드)
- [오버라이딩](#오버라이딩)
- [인스턴스화](#인스턴스화)
- [싱글톤](#싱글톤)
- [코루틴](#코루틴)
- [유니티 이벤트](#유니티-이벤트)
- [제네릭](#제네릭)
- [네임스페이스](#네임스페이스)
- [인터페이스](#인터페이스)
- [추상 클래스](#추상-클래스)
    - [virtual](#virtual)

## 3부
- [델리게이트](#델리게이트)
- [콜백메서드](#콜백메서드)
- [일반화 델리게이트](#일반화-델리게이트)
- [델리게이트 체인](#델리게이트-체인)
- [Func, Action 델리게이트](#Func,-Action-델리게이트)
- [무명 형식_AnnonyMouse Type](#무명-형식_AnnonyMouse-Type)
- [무명 메소드_Anonymouse Method](#무명-메소드_Anonymouse-Method)
- [람다함수](#람다함수)
- [람다로 표현된 메서드](#람다로-표현된-메서드)

## 4부
- [클래스 프로퍼티](#클래스-프로퍼티)
- [T 변수 제약조건 만들기](#T-변수-제약조건-만들기)
- [LINQ 란?](#LINQ-란?)
- [C# 확장 메소드_Extension Method](#C#-확장-메소드_Extension-Method)
- [C# 분할 클래스_partial Class](#C#-분할-클래스_partial-Class)
- [C# 중첩 클래스_Nested Class](#C#-중첩-클래스_Nested-Class)


<br><br>
<br><br>


> # 1부
## 변수
- call by reference : 원본의 주소
- call by value : 값 복사
```c#
//기본변수
void Start(){
    int age = 23;
    float height = 169.123...; 
    double pi = 3.14156... // float의 두배의 메모리를 사용
    bool isBoy = true;
    char grade = 'A';
    string movieTitle = "타이틀";
    var myName = "할당값"; // 기본 내장 타입

    int[] num = new int[5];
    int[] num2 = new int[]{1,2,3};
    Debug.log(pi);
}
```
```c#
//사칙연산, 복합연산자
void Start(){
    int a = 5;
    int b = 7
    Debug.log(a+b);
    Debug.log(a-b);
    Debug.log(a*b);
    Debug.log(a/b);
    Debug.log(a%b);

}
```
```c#
//함수
void Attack(Monster targer, int damage, int point){
    PlayAnimation();
    PlaySound();
    target.hp = target.hp - 10;
    exp = exp + 5;
}

Monster ork = new Monster();
Attack(ork,5, 10);
```
<br>

## 지역 변수와 전역 변수
| 종류 | 저장 장소 | 선언 위치 | 허용 범위 | 파괴 시기 | 초기값 |
| :---- | :---- | :---- | :---- | :---- | :---- |
| `전역변수` | `힙메모리` | 함수외부 | 클래스 내 전체 | 프로그램종료 | 0으로 기본설정 |
| 지역변수 | `스택메모리` | 함수내부 | 함수내부 | 함수가 끝나는시점 | 수동으로 설정 |

<br>

## static 변수, static 함수
- 정적 타입은 모든 오브젝트가 공유
```c#
public class calc : monobehaviour{
    public static int count = 0;
    void Awake(){
        count = count + 1;
    }
    public static void ShowAnimalType(){
        Debug.Log("이것은 개 입니다. " + Dog.count);
    }
}
```

<br>

## 형변환
- `(MyClass)obj`
- `obj as MyClass`

```c#
// 형변환
int a = 5
string s = a.ToString();


String s = "777";
int a = int.Parse(s);

MyClass myClassByCast = (MyClass)obj; // cast사용
MyClass myClassByAs = obj as MyClass; // as 사용 (반드시 변환될 것이라 가정)
```

<br>

## 조건문 반복문
```c#
//if
//switch
//while
//DoWhileLoop  
do
{
    print ("Hello World");
    
}while(shouldContinue == true);


//foreach문
foreach(string item in strings)
{
    print (item);
}

//삼항 연산자, ? 조건문
bool shoulddie = (transform.position.z < 0) ? true : false;
```

<br>

## 컬렉션
- 스택 메모리, 힙 메모리 사이의 박싱과 언박싱과정이 있다.
> ### `Hashtable`
```c#
using System;
using System.Collections;

namespace Test_Cs_Console{
    class program{
        static void Main(string[] args){
            Hashtable ht = new Hashtable();
            ht["Apple"] = "사과";
             Console.WriteLine(ht["Apple"]);
        }
    }
}
```
> ### `ArrayList`
```c#
using System;
using System.Collections;

namespace Test_Cs_Console{
    class program{
        static void Main(string[] args){
            ArrayList list = new ArrayList();
            list.Add(10);
            list.RemoveAt(1);
            list.Insert(1, 2.3f);
            foreach(object obj in list)
                Console.Write("{0}",obj);
        }
    }
}
```
<br>

## 일반화컬렉션 `<T>`
- 박싱 언박싱의 과정의 부담이 적다.
> ### `List`
```c#
using System;
using System.Collections.Generic;

namespace Test_Cs_Console{
    class program{
        static void Main(string[] args){
            List<int> list = new List<int>();
            list.Add(10);
            list.RemoveAt(1);
            list.Insert(1, 2.3f);
            foreach(object obj in list)
                Console.Write("{0}",obj);
        }
    }
}
```
> ### `Queue`
- 선입선출`FIFO`의 자료구조이다.
```c#
using System;
using System.Collections.Generic;

namespace Test_Cs_Console{
    class program{
        static void Main(string[] args){
            Queue<int> que = new Queue<int>();
            que.Enqueue(10);
            que.Dequeue();
            while(que.Count > 0)
                Console.Write("{0}",obj);
        }
    }
}
```
> ### `Stack`
- 후입선출`LIFO`
```c#
using System;
using System.Collections.Generic;

namespace Test_Cs_Console{
    class program{
        static void Main(string[] args){
            Stack<int> stack = new Stack<int>();
            stack.Push(10);
            stack.Pop();
            while(stack.Count > 0)
                Console.WriteLine(stack.Pop());
        }
    }
}
```
> ### `Dictionary<T>`
```c#
using System;
using System.Collections.Generic;

namespace Test_Cs_Console{
    class program{
        static void Main(string[] args){
            Dictionary<string,string> dic = new Dictionary<string, string>();
            dic.add("name", "name");
            dic.add("name", "name");
            dic.add("name", "name");

            var enumerator = dic.GetEnumerator();
            while(enumerator.MoveNext()){
                var pair = enumerator.Current;
                string name = pair.value;
            }

            foreach(KeyValuePair<string, Item> pair in dic){
                string name = pair.value;
            }
        }
    }
}
```

<br><br>
<br><br>



# 2부
## Vector
- 방향, 거리, 속도
- 노말벡터 : 길이가 1인 벡터

<br>

## 평행이동과 좌표계, 부모자식 관계
- Global Space : 절대적인 기준
- Local Space : 부모를 기준
```c#
void Start(){
    Vector3 targetPosition = new Vecotr3(1,0,0);
    transform.position = targetPosition;
    transform.rotation = lossyScale; // 글로벌에서의 스케일
    // `lossy` 왜곡 되지 않음
}
```

<br>

## 회전과 쿼터니언
벡터로 할 시 90도 각도를 회전시 짐벌락현상
```c#
void Start(){
    transform.rotation = Quaternion.Euler(new Vector(30,30,30));
    Quternion newRotation = Quaternion.Euler(new Vector(45,60,80));
    Quternion newRotation2 = Quaternion.LookRotation(new Vecotor.up);

    Vector3 direction = targetTransform.postion = transform.position; // 방향
    Quternion targetRotation = Quaternion.Lerp(aRotation, bRotation, 0.5f); // 사잇값

}
```

<br>

## 오버로드
```c#
public class calc : monobehaviour{
    void Start(){
        Debug.Log(Sum(1,1));
    }

    public int Sum(int a, int b){
        return a + b;
    }
    
    public float Sum(float a, float b){
        return a + b;
    }

    public int Sum(int a, int b, int c){
        return a + b + c;
    }
}
```

<br>

## 오버라이딩
```c#
public class BaseRotator : MonoBehaviour
{

    public float speed = 60f;

    void Update()
    {
        Rotate();
    }

    protected virtual void Rotate()
    {
        transform.Rotate(speed * Time.deltaTime, 0, 0);
    }
}
```
```c#
public class ZRotator : BaseRotator
{
  protected override void Rotate()
    {
         transform.Rotate(0, 0, speed * Time.deltaTime);
    }
}
```

<br>

## 인스턴스화
- `Instantiate()`
```c#
public Transform spawnPosition;
public GameObject target;
public RigidBody rg_target;
void Start(){
    GameObject instance = Instantiate(target, spawnPosition.position, spawnPosition.rotation );

    rg_target = Instantiate(target, spawnPosition.position, spawnPosition.rotation );
    rg_target = instance.GetComponent<RigidBody>();
    rg_target.AddForce(0,1000,0);
}
```

<br>

## 싱글톤
- `private static ScoreManager instance`
- `instance = this`
```c#
public class ScoreManager : monobehaviour{
    private static ScoreManager instance;
    private int score = 0;

    public static ScoreManager GetInstance(){
        if(instance == null){
            instance = FindObjectOfType<ScoreManager>();
            if(instance == null){
                GameObject container = new GameObject("Score Manager");
                instance = container.AddComponent<ScoreManager>();
            }
        }
        return instance;
    }

    void Start(){
        if(instance != null){
            if(instance != this){
                Destroy(gameObject);
            }
        }
    }

    public int GetScore(){
        reutrn score;
    }
    public void AddScore(int newScroe){
        score += newScore;
    }
}
```
```c#
public class ScoreAdder : monobehaviour{

    void Update(){
        if(Input.GetMouseButtonDown(0)){
            ScoreManager.GetInstance.AddScroe(5);
            Debug.Log(ScoreManager.GetInstance.GetScore());
        }
    }
}
```

<br>

## 코루틴
```c#
public class Fade : monobehaviour{
    public Image fadeImage;

    void Start(){
        StartCoroutine(FadeIn);
        StartCoroutine("FadeIn");
        StopCoroutine("HelloUnity");
    }
    IEnumerator FadeIn(){
        Color startColor = fadeImage.color;
        for(int i = 0; i < 100; i++){
            startColor.a -= 0.01f;
            fadeImage.color = startColor;
            yield return new WaitForSeconde(0.01f);
        }
    }
    IEnumerator FadeIn2(){
        Color startColor = fadeImage.color;
        isLooping = true;
        While(isLooping){
            startColor.a -= 0.01f;
            fadeImage.color = startColor;
            if(startColor.a <= 0.0f)
                isLooping = false;
            yield return null; // 한 프레임 텀
        }
    }
}
```
- 코드가 순차적으로 실행 : 동기 방식 (Sync)
- 코드가 개별적으로 시작과 끝 : `비동기 방식 (Async)`
    - one(), two(), three()가 동시에 처리되는 `멀티 쓰레딩 프로그램` 구현가능
```c#
    void Start(){
        StartCoroutine(one());
        StartCoroutine(two());
        StartCoroutine(three());
        // IEnumerato은 개별적으로 실행됨
    }
```

<br>

## 유니티 이벤트
- 에디터 하이라키를 통해서 연결가능 -> 스파게티 코드 가능성
- `public UnityEvent onPlayerDead;`
```c#
using UnityEngine.Events;

public class PlayerHealth : monobehaviour{

    public UnityEvent onPlayerDead;

    void OnTriggerEnter(Collider other){
        Dead();
    }
    
    private void Dead(){
        Debug.Log("Player Dead");
        onPlayerDead.Invoke();
        Destroy(gameobject);
        
    }
}
```
```c#
public class AchivementSystem : monobehaviour{
    public void UnLockAchivement(){
        Debug.Log("도전과제 해제");
    }
}
```
```c#
public class GameManager : monobehaviour{

    private void Restart(){
        SceneManager.LoadScene(0); // 현재 씬 재시작
    }

    private void OnPlayerDead(){
        Invoke("Restart",5f); // 지연시간 후 발동
    }
}
```

> ### 델리게이트 타입으로 이벤트 변수를 생성할 수 있다.
- 오직 변수가 속한 클래스 내부에서만 사용가능하다.
- `delegate void MyDelegate(int a);`
- `public event MyDelegate eventCall;`
- `eventManager.eventCall += new MyDelegate(EvenNumber)`
```c#
namespace Cs_Lecture
{
    delegate void MyDelegate(int a);  // 델리게이트 타입 선언

    class EventManager{
        public event MyDelegate eventCall; // 이벤트 델리게이트 변수 선언
        public void NumberCheck(int num)
        {
            if(num%2==0)
                eventCall(num);
        }
    }

    class MainApp{
        static void EvenNumber(int num){
            Console.WriteLine("{0}는 짝수",num);
        }
        static void Main(string[] args){
            EventManager eventManager = new EventManager();
            eventManager.eventCall += new MyDelegate(EvenNumber); // 메소드 참조
            for(int i = 1; i<10; i++)
                eventManager.NumberCheck(i);
        }
    }
}
```
> ### `EventHandler`를 이용해 쉽게 작성할 수 있다.
위 방법의 문제점은 이벤트를 일으키기 위해 선언해야 하는 과정이 복잡하다. 때문에 c#에서는 이벤트를 좀 더 쉽게 발생시키기 위해 이벤트 핸들러라는 키워드를 제공한다. 델리게이트 타입을 선언하지 않고 `EventHandler`키워드 만으로 이벤트를 발생시킨다. 미리 설정되어있는 키워드인 만큼 인자 값 역시 정해져 있다.
- 이벤트를 발생시키는 호출 객체
- 이벤트 발생에 필요한 정보를 담고 있는 객체
```c#
public static event EventHandler PariclePopUp_EventHandler;

void ParticlePopUp(VisualizingParticle visualizingParticle){
    PariclePopUp_EventHandler(this, EventArgs.Empty);
}
```




<br>

## 제네릭
- `public class Container<T>{}`
- `Container<string> container = new Container<string>();`
```c#
public class Util : Monobehaviour{
    void Start(){
        Print<int>(30);
        Print<string>("30");
    }

    public void Print<T>(T inputMessage){
        Debug.Log(inputMessage)
    }
}
```
```c#
public class Util : Monobehaviour{
    void Start(){
        Container<string> container = new Container<string>();
        container.messages = new String[3];
        container.messages[0] = "Hello";
        container.messages[1] = "World";
        container.messages[2] = "Generic";
        for(int i = 0 ; i < container.massages.Length; i++){
            Debug.Log(container.messages[i]);
        }
    }
}

public class Container<T>{
    public T[] messages;
}
```

<br>

## 네임스페이스
- 네임 스페이스는 클래스 이름에 접두어로 사용되는 클래스의 집합입니다.
```c#
namespace Enemy {
    public class Controller1 : MonoBehaviour {
        ...
    }
    
    public class Controller2 : MonoBehaviour {
        ...
    }
}
```
```c#
// 접두사를 피하기 위해
using Enemy
```

<br>

## 인터페이스
- 인터페이스는 추상클래스와 비슷하지만 인스턴스를 가질수 없고 `다중상속`이 가능하여 클래스가 상속받아 사용하는 뼈대같은 개념이다.
- `메소드, 프로퍼티, 인덱서, 이벤트` 선언만 가능
```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;


abstract public class A : MonoBehaviour
{
    abstract public void Abc();
}

interface IMethod // 인터페이스 메소드
{
    void Bbc();
}

interface IProperty // 인터페이스 프로퍼티
{
    int salaryP { get; set; }
}

interface IIndexer // 인터페이스 인덱서
{
   int this[int index]
    {
        get;
        set;
    }
}

public delegate void EventHandler();
interface IEvent // 인터페이스 이벤트
{
    event EventHandler OnDie;
}









public class Interface : A, IMethod, IProperty, IIndexer, IEvent
{
    private int[] arr = new int[100];
    private int salary;

    public event EventHandler OnDie; // 인터페이스 이벤트 구현

    public int salaryP // 인터페이스 프로퍼티 구현
    {
        get
        {
            return salary;
        }
        set
        {
            salary = value;
        }
    }

    public int this[int index] // 인터페이스 인덱서 구현
    {
        get
        {
            return arr[index];
        }
        set
        {
            arr[index] = value;
        }
    }

   

    public override void Abc() // 추상클래스 구현
    {
        print("Abc");
    }

    public void Bbc()  // 메소드 인터페이스 구현
    {
        print("Bbc");
    }

    private void EndGame() { print("Game End"); }

    void Start()
    {
        Abc();
        Bbc();
        salaryP = 10;
        print(salaryP);
        this[5] = 20;
        print(this[5]);
        OnDie += EndGame;
        OnDie();
    }
}
```
```c#
if (Physics.Raycast(ray, out RaycastHit hit, Mathf.Infinity, layerMask))
{
    // 인터페이스를 호출
    IDamagable IDamagable = (IDamagable)hit.collider.GetComponent(typeof(IDamagable));
    IDamagable.Damaged();
}
```

<br>

## 추상 클래스
- 추상 클래스는 인터페이스처럼 뼈대를 제공할 수 있지만, `다중 상속이 되지 않는다.` 

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

abstract public class A : MonoBehaviour
{
    abstract public void Abc(); //뼈대 제공
}

interface IMethod
{
    //함수 가능
    void Bbc();
}

interface IProperty
{
    //프로퍼티 가능, 변수는 불가능
    int salaryP { get; set; }
}

interface IIndexer
{
    //인덱서 가능
    int this[int index]
    {
        get;
        set;
    }
}

public delegate void EventHandler();

interface IEvent
{
    event EventHandler OnDie;
}









public class Test : A, IMethod, IProperty, IIndexer, IEvent
{
    private int[] arr = new int[100];
    private int salary;

    public int this[int index] { get { return arr[index]; } set { arr[index] = value; } }

    public int salaryP { get { return salary; } set { salary = value; } }

    public event EventHandler OnDie;

    public override void Abc()
    {
        print("abc");
    }

    public void Bbc()
    {
        print("bbc");
    }

    private void EndGame()
    {
        print("Game End");
    }

    // Start is called before the first frame update
    void Start()
    {
        Abc();
        Bbc();

        salaryP = 10;
        print(salaryP);

        this[5] = 20;
        print(this[5]);


        OnDie += EndGame;

        OnDie();
    }
}
```
> ### virtual
```c#
public class Employee()
{
    public virtual void GiveBonus(float amount) // svirtual
    {
        currPay += amount;
    }
}



public class SalesPerson () :  Employee
{
    public override void GiveBonus(float amount) // override
    {
        base.GiveBonus(amount * 0.1);
        currPay += amount * 0.1;
    }
}
```


<br><br>
<br><br>







> # 3부
## 델리게이트
- 메소드를 대신해서 호출하는 역할
- 반환, 매개변수를 참조할 메소드에 일치시킴
    - `delegate 반환타입 델리게이트명(매개변수);`

<br>

## 콜백메서드
- A 메서드의 델리게이트로 B 메서드를 참조하게 함
```c#
namespace Cs_Lecture{
    //선언
    delegate int Calculate(float a, float b);

    class MainApp{

        public static void Caculator(float a, float b, Calculate dele){
            Console.WrieLine(dele(a,b));
        }
        public static int Plus(float a, float b){return a+b;}
        public static int Minus(float a, float b){return a-b;}
        public static int Multiply(float a, float b){return a*b;}

        static void Main(string[] args){
            Calculate Plus; // 생성
            Calculate Minus; // 생성
            Calculate Multiply; // 생성

            Calcuator(11, 22, Plus);
        }
    }
}
```
> ### 이벤트활용
- Publisher와 Subscriber관계, 매개변수까지 전달할 수 있다.
- `event` 키워드가 붙으면 기존 구독자(Subscriber)의 덮어씌우기를 방지할 수 있다.
- `event` 키워드가 붙으면 선언된 클래스 외에서 실행 불가
- `event` 키워드가 붙으면 event의 기능으로만 제한함
- `public delegate void Boost(Character target);`
- `public event Boost playerBoost;`
- `player.playerBoost += HealthBoost;`
```c#
public class Character : Monobehaviour{
    public delegate void Boost(Character target); // 대행할 델리게이트
    public event Boost playerBoost;
    public int hp;
    public inf defense;

    void Start(){
        playerBoost(this);
    }
}
```
```c#
public class Booster : Monobehaviour{

    void Awake(){
        Character player = FindObjectOfType<Character>();
        player.playerBoost += HealthBoost;
        player.playerBoost += ShieldBoost;
    }

    public void HealthBoost(Character target){
        target.hp += 10;
    }

    public void ShieldBoost(Character target){
        target.defense += 10;
    }
    
}
```

<br>

## 일반화 델리게이트
- `delegate T MyDelegate<T>(T a , T b);`
- `public static int Plus(int a, int b){return a+b;}`
- `MyDelegate<int> Plus_int;`
```c#
namespace Cs_Lecture{
    //선언
    delegate T MyDelegate<T>(T a , T b);

    class MainApp{

        public static void Caculator<T>(T a, T b, MyDelegate<T> dele){
            Console.WrieLine(dele(a,b));
        }
        public static int Plus(int a, int b){return a+b;}
        public static int Minus(float a, float b){return a-b;}
        public static int Multiply(double a, double b){return a*b;}

        static void Main(string[] args){
            MyDelegate<int> Plus_int; // 생성
            MyDelegate<float> Minus_float; // 생성
            MyDelegate<double> Multiply_double; // 생성

            Calcuator(11, 22, Plus_int);
            Calcuator(11.0f, 22.0f, Minus_float);
            Calcuator(11.5f, 22.5f, Multiply_double);
        }
    }
}
```
<br>

## 델리게이트 체인
- 하나의 델리게이트가 여러 개의 메소드를 참조할수 있다.
- 해당 델리게이트를 호출하면 참조된 메소드들을 차례대로 호출한다.
```c#
namespace Cs_Lecture{
    //선언
    delegate void MyDelegate();

    class MainApp{
        public static void func0() {Console.Write("첫 번째");}
        public static void func1() {Console.Write("첫 번째");}
        public static void func2() {Console.Write("첫 번째");}

        static void Main(string[] args){
            MyDelegate dele;
            dele = new MyDelegate(func0);
            dele += func1;
            dele += func2;

            dele();
        }
    }
}
```

<br>

## Func, Action 델리게이트
- 선행지식 : 델리게이트, 무명 메소드, 람다식
- Func와 Action은 미리 선언된 델리게이트 변수로써 별도 선언없이 사용 가능
- Func : `반환값이 있는` 메소드를 참조하는 델리게이트 변수
    - `.NET Framework`에는 총 17가지의 Func 델리게이트가 준비되어 있다.
```c#
namespace Cs_Lecture{
    class MainApp{
        static float temp(int a,int b, int c){
            return (a+b+c) * 0.1f;
        }
        static void Main(string[] args){
            Func<float> func0 = () => 0.1f; // 매개변수는 없고 반환값은 float 형
            Func<int, float> fun1 = (a) => a * -.1f; // int형 매개변수를 1개 가지고 반환값은 float형
            Func<int, int, float> func2 = (a,b) => (a+b)*0.1f
            Console.WriteLine("func0 반환값:{0}", func0());
            Console.WriteLine("func1 반환값:{0}", func1(10));
            Console.WriteLine("func2 반환값:{0}", func1(10,10));
        }
    }
}
```
- Action : `반환값이 없는` 메소드를 참조하는 델리게이트 변수
```c#
class MainApp{
    static float temp(string name){
        Console.WriteLine("name:{0}",name);
    }
    static void Main(string[] args){
        int sum = 0;
        Action func0 = () => Console.WriteLine("name: 액트0");
        Action<string> act1 = new Action<string>(temp);
        Action<string, string> act2 = (name, age) => {
            Console.WriteLine("name:{0}", name);
            Console.WriteLine("age:{0}", age);
        };
        Action<int, int, int> act3 = (a, b, c) => sum = a+b+c;


        act0();
        act1("액트1");
        act2("액트2","22");
        act3(100, 20, 3);

        Console.WriteLine("sum:{0}",sum);
    }
}
```
유니티 예제
```c#
public class Worker : Monobehaviour{
    delegate void Work();
    Work work; // 델리게이트 변수 -> 할 일을 스택
}
```
```c#
using System;

public class Worker : Monobehaviour{
    Action work; // 리턴값이 없는 델리게이트 변수

    void MoveBricks(){

    }    
    void DigIn(){

    }

    void Start(){
        work += MoveBricks;
        work += Digin;
    }

    void Update(){
        if(Input.GetKeyDown(KeyCode.Space)){
            work();
        }
    }
}
```

<br>

## 무명 형식_AnnonyMouse Type
- 무명 형식은 반드시 선언과 함께 new 키워드로 인스턴스를 생성해야함
- 읽기전용
```c#
class MainApp{
    static void Main(string[] args){

        var temp = new{Age=11, Name="십일"}; // 무명 형식
        Console.WriteLine("Age:{0}, Name:{1}", temp.Age, temp.Name);

        var tempArr = new{
            Int = new Int[]{11,22,33},
            Float = new float[]{0.1f,0.2f,0.3f} 
            };

        foreach(var element in tempArr.Int)
            Console.Write("{0}", element);
    }
}
```

<br>

## 무명 메소드_Anonymouse Method
- 무명 메소드를 호출하기 위해 델리게이트변수가 필요하다.
- 즉, 델리게이트 변수를 선언하고, 그 변수로 무명 메소드를 참조하게 되는 것이다.
- `delegate(int a, int b){ return a+b;}`
```c#
delegate int MyDelegate(int a,int b); // 델리게이트 타입 선언

public static void main(){

    MyDelegate add; // 델리게이트 변수 선언

    add = delegate(int a, int b){
        return a+b; 
    }; // 무명 메소드 참조

    Console.WriteLine(add(11,22));
}
```

<br>

## 람다함수
- 무명 메소드를 단순한 계산식으로 나타낸 것이다.
- 이름이 없는 함수, `익명함수`
- 오브젝트 이기도 하다.
```c#
public class Messenger : Monobehaviour{
    public delegate void Send(string reciever);

    void Start(){

        Send onSend;

        onSend += SendMail;
        onSend += SendMoney;
        onSend += man => Debug.Log("Assainate"+man); // man 매개변수
        onSend += (string man) => {
            Debug.Log("Assainate"+man); // man 매개변수
            Debug.Log("Hide Body"); // man 매개변수
        };
    }

    void Update(){
        if(Input.GetKeyDown(KeyCode.Space)){
            onSend("reciever");
        }
    }

    void SendMail(String Mail){

    }
    void SendMoney(string reciever){

    }
}
```

<br>

## 람다로 표현된 메서드
```c#
private int health = 0;
public void RestoreHealth(int amount) => health += amount;
public bool IsDead() => (health <= 0);
```
```c#
private int health = 0;
 
public int Health {
	get { return health; }
	set { health = value; }
}
 
public bool IsDead {
	get { return (health <= 0); }
}
```
람다로 표현된 프로퍼티
```c#
private int health = 0;
 
public int Health {
	get => health;
	set => health = value;
}
 
public bool IsDead => (health <= 0);
```

<br><br>
<br><br>







> # 4부
## 클래스 프로퍼티
```c#
class MyClass{
    private int num1 {set; get;};
    private int num2 {set; get;};
    private int sum {get{return num1+num2}}; // 읽기 전용
    public int number;    
}

static void Main(){
    Myclass class = new Myclass(num1 = 10, name="프로퍼티");
}
```

<br>

## T 변수 제약조건 만들기
- `where T : new()` : T는 매개변수가 없는 생성자를 가진 타입이어야한다.
- `where T : 클래스 이름` : T는 지정한 클래스 이거나 이를 상속받는 클래스여야 한다.
- `where T : 인터페이스 이름` : T는 인터페이스를 상속받는 클래스 여야 한다.
- `where T : U` : T는 형식매개변수 U의 타입이거나 이를 상속받는 클래스 여야 한다.
```c#
class List<T> where T : 클래스명
{
    ...
}
```


<br>

## LINQ 란?
- Language Integrated Query
- 데이터에 질문하는 언어, 코드 단순화
1. 여러개의 데이터 범위 지정하기
2. group by로 데이터 분류하기
3. join 두 데이터 합치기 (내부조인, 외부조인)
```c#
using System.Linq;

class Womans{
    public string name {get; set;}
    public int age {get;set;}
}

class MainApp{
    static void Main(string[] args){
        Woman[] womanList={
            new Woman() {name ="아라",age=24},
            new Woman() {name ="민희",age=32},
            new Woman() {name ="현아",age=25},
            new Woman() {name ="수지",age=20}
        }

        var Women = from woman in womanList
                    where woman.age > 20
                    orederby woman.age ascending
                    select new{ 
                        title = "성인여자",
                        name = woman.name
                    }; // 배열 데이터로 추출
        foreach(var woman in Women)
            Console.WriteLine("{0}:{1}", woman.title, woman.name);
    }
}
```
```c#
using System.Linq;

class Student{
    public string name {get; set;}
    public int[] score {get;set;}
}

class MainApp{
    static void Main(string[] args){
        Student[] studentList={
            new Student() {name ="아라",score = new int[]{88,73,66,91}},
            new Student() {name ="민희",score = new int[]{78,95,89,52}},
            new Student() {name ="현아",score = new int[]{88,73,66,91}}
        }

        var Students =  from student in studentList
                            from score in student.score
                            where score > 89 // index
                        select new{ 
                            name = student.name,
                            score = score
                        }; // 배열 데이터로 추출
        foreach(var woman in Women)
            Console.WriteLine("{0}:{1}", woman.title, woman.name);
    }
}
```



<br>

## C# 확장 메소드_Extension Method
- 기존 클래스의 기능을 확장시켜주는 메소드
- 상속도 좋은 방법이지만, 클래스가 `sealed`로 한정되어 있는 경우에는 확장 메소드의 사용을 고려해 볼 수 있습니다.
```c#
using System;
using Extension;

namespace Extension
{
    public static class ExtensionMethod
    {
        public static int Multiplication(this int var, int a, int b) // this 확장대상형식 식별자, 매개변수...
        {
            int result = var;
            for (int i = 0; i < b; i++)
            result *= a;
            return result;
        }
    }
}

namespace Example
{
    class Program
    {
        static void Main(string[] args)
        {
        Console.WriteLine("{0}", 5.Multiplication(2, 3));
        }
    }
}
```
- 여기서 5는 `Multiplication()`의 매개변수 var에 들어가며, 2는 a에, 3은 b에 들어가게 됩니다. 10~15행을 살펴보면 result란 변수에 var의 값을 담아, result에 a를 b번 곱하고 이 결과값을 호출부로 반환하여, 반환된 값을 출력시킵니다.

<br>

## C# 분할 클래스_partial Class
클래스를 분할하려면 `partial` 키워드를 사용하면 됩니다. 클래스 말고도 앞으로 배울 인터페이스, 구조체에도 partial 키워드를 사용할 수 있습니다.
- partial 키워드가 붙은 클래스는 `컴파일 시 컴파일러에 의해 하나로 합쳐집니다.` 분할에는 제한이 없으며, 여러 번 분할해도 상관이 없습니다.

```c#
using System;
namespace Example
{
    partial class Nested
    {
        public void Test() { Console.WriteLine("Test()"); }
    }
    partial class Nested
    {
        public void Test2() { Console.WriteLine("Test2()"); }
    }
    partial class Nested
    {
        public void Test3() { Console.WriteLine("Test3()"); }
    }

    class Program
    {
        static void Main(string[] args)
        {
            Nested nested = new Nested();
            nested.Test();
            nested.Test2();
            nested.Test3();
        }
    }
}
```

<br>

## C# 중첩 클래스_Nested Class
- 중첩 클래스는 클래스 내에 또 클래스가 정의된 클래스를 말합니다.
- 부에 정의하는 것보다 관련있는 클래스를 내부 클래스로 두어 코드를 쉽게 이해하기 위해 사용됩니다.
```c#
using System;
namespace ConsoleApplication21
{
    public class OuterClass
    {
        private int a = 70;

        public class InnerClass{
            OuterClass instance;
            public InnerClass(OuterClass a_instance){instance = a_instance;}
            public void AccessVariable(int num){this.instance.a = num;}
            public void ShowVariable(){Console.WriteLine("a : {0}", this.instance.a);}
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            OuterClass outer = new OuterClass();
            OuterClass.InnerClass inner = new OuterClass.InnerClass(outer);
            inner.ShowVariable();
            inner.AccessVariable(60);
            inner.ShowVariable();
        }
    }
}
```