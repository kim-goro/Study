## OOP
- 객체 지향 프로그래밍(Object Oriented Programming), 데이터를 `추상화`시켜 객체간의 관계를 로직
1) 클래스 + 인스턴스(객체)
2) 추상화
3) 캡슐화
4) 상속
5) 다형성

<br><br>

## 1. 버텍스 쉐이더와 픽셀 쉐이더에 대해 설명하세요
- `버텍스 셰이더 (Vertex Shader)`는 '정점의 위치'에 대한 정보를 입력으로 받고, '화면상의 정점의 위치'를 반환하는 셰이더입니다. 변환 과정에서 행렬연산이 이용됩니다.
- `픽셀 셰이더는 (Pixel Shader)` 레스터라이즈 된 결과물을 입력으로 받고, 픽셀의 색을 반환합니다.

레스터라이져가 어디에 얼마나 많은 픽셀을 그릴것인지 판단하여 픽셀 셰이더로 넘기면, 픽셀 셰이더는 각 픽셀이 어떤 색을 띌지 결정합니다.

<br><br>

## 2. `Time.deltaTime`이 시간에 종속적인 계산시 필요한 이유를 설명하세요

설명을 하면 우선 '게임 루프'에 대해서 알아야합니다.

게임은 '연산(Update)' 부분과 '출력(Render)'부분, '입력처리(Input)' 
실시간 게임에서 프로세스처리 속도가 다름 `두 컴퓨터의 프레임 차이`가 있으면 안됩니다. Update는 프레임이 아니라 실제 시간에 맞춰서 진행되어야합니다.

`이전 프레임이 완료되고 난 후 실제로 흐른 시간이 필요하다.`

<br><br>

## 3. GameObject가 일정한 speed로 target을 향해 움직이고, 둘 사이의 거리가 1.0 유닛 미터 이하일 경우 멈추는 코드를 작성하세요

 ```c#
    void Update () {
        transform.position = ((target - transform.position).normalized * speed * Time.deltaTime);
        if(Vector3.Distance(target,transform.position) < 1.0f)
        {
            Destroy(this);
        }
    }
 ```

<br><br>

 ## 4. 두 GameObject가 SphereCollider를 포함하고, isTrigger가 check되 있을 때 OnTrigger 이벤트가 발생하는가? 설명하여라

- rigidbody가 있어야 OnTrigger 나 OnCollision 이벤트가 발생합니다. 
- `isTrigger`는 충돌시 물리를 적용할지 적용하지 않고 이벤트만 발생시킬지를 판단합니다.

<br><br>

## 5. 어떤게 더 빠른가?

(1) 1천개의 MonoBehaviour를 상속받는 게임오브젝트의 Update 처리

(2) MonoBehaviour 를 상속 받는 게임오브젝트 하나에 천개의 클래스 배열이 있고, 그 클래스 배열요소 각각의 커스텀 Update 콜백을 처리 

- '유니티의 `Update`는 `Reflaction`을 이용하기 때문에 `Direct Call 보다 느리다.` 
    - (Start(), Update()가 오버라이드 한 것이 아님으로..)

<br><br>

## 6. 아래의 코드의 문제점을 설명하고 문제를 고쳐라.
```c#
public class TEST : MonoBehaviour {
    void Start () {
        transform.position.x = 10;
    }
}
```
- transform.position은 Vector3 변수 같지만 변수가 아니라 `Property` 입니다. 
- `Property`는 get, set이 모두 정의되어있는 함수입니다. Vector3형 임시변수를 만듭니다.
```c#
 public Vector3 position
    {
        get
        {
            return Position;
        }
        set
        {
            Position = value;
        }
    }
```