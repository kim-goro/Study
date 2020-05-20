## 1. int형 배열이 주어졌을때, 배열안의 모든 짝수를 더하는 함수를 작성하세요
```c#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace _170508PrerapringInterview
{
    class Program
    {
        static void Main(string[] args)
        {
            int[] arr = { 10, 20, 30, 31 };
            Program program = new Program();
            program.TotalEvenNumbers(new int[]{10,20,30,40,31});
        }

        void TotalEvenNumbers(int[] arr)
        { 
            Console.WriteLine(Array.FindAll(arr, (e) => { return e % 2 == 0; }).Sum());
        }
    }
}
```

<br><br>

## 2. 아래 코드의 결과를 설명하세요
```c#
class Program {
  static String location; // 클래스 String
  static DateTime time; // 구조체 DateTime
 
  static void Main() {
    Console.WriteLine(location == null ? "location is null" : location);
    Console.WriteLine(time == null ? "time is null" : time.ToString());
  }
}
```
- `String`은 대표적인 `reference Type`입니다. `null`로 초기화 됩니다.
- `구조체`는 `value Type`으로 구분됩니다. 초기화 되지 않았을 때 그 안의 값들이 모두 `default값`으로 초기화 됩니다.

<br><br>

## 3. time과 null을 비교하는것이 타당한가? (Valid한가) 왜 타당한가?
```c#
static DateTime time;
/* ... */
if (time == null)
{
	/* do something */
}
```
- `Nullable<int> A` 라고 선언하게 되면 A는 null값을 가질 수 있게 됩니다.
- 결론적으로 DateTime 은 null과 비교될 수 있습니다. `Nullable<DateTime>` 형태로 둘 다 바꿔서 비교하면 되니까요.

<br><br>

## 4. 주어진 circle 클래스의 인스턴스로 원주를 구하는 코드를 작성하시오.
```c#
public sealed class Circle {
  private double radius;
  
  public double Calculate(Func<double, double> op) {
    return op(radius);
  }
}
```
```c#
    static void Main(string[] args)
    {
        new Circle().Calculate((rad) => rad * 2 * Math.PI);
    }
```

<br><br>

## 6. 아래 프로그램의 결과를 설명하시오.
```c#
delegate void Printer();

static void Main()
{
      List<Printer> printers = new List<Printer>();
      for (int i = 0; i < 10; i++)
      {
           printers.Add(delegate { Console.WriteLine(i); });
      }

      foreach (var printer in printers)
      {
           printer();
      }
}
```
- `delegate`를 넘길 때 i는 값 자체로 넘어가는게 아니라 `주소로 넘어가게 됩니다. `
- 따라서 i가 10이 되고나서 foreach가 돌아가므로, 10이 10번 출력됩니다.

