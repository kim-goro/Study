// 변수

```java
public class Main{
    public static void main(String[] args){
        int x = 30;
        final int y = 30;
        long l = 30L;
        short s = 30;
        double dd =30.0;
        float ff = 30.0f;
        ff = (double) dd;
        boolean isMarried = true;
        char c = 'a';
        String str = "홍길동";
        String str2 = String.format("저는 %s입니다.,"김정우");

        System.out.printls(35);
        System.out.printf("저는 %s입니다.,str);
        System.out.printls(Math.max(10, 30)));

    }
}
```
```java
public class Main{
    public static void main(String[] args){
        String str = "100";
        int i = Integer.parseInt(str);
        String str2 = String.valueOf(i);
        Random random = new Random();
        int rand = random.nextInt(10);
    }
}
```
```java
public class Main{
    public static void main(String[] args){
        Scanner scanner = new Scanner(System.in);
        String str = scanner.next();
        int i = scanner.nextInt();
        System.out.println(scanner.next());
    }
}
```

// 반복문

```java
public class Main{
    public static void main(String[] args){
        int x = 30;
        final int y = 30;
        long l = 30L;
        short s = 30;
        double dd =30.0;
        float ff = 30.0f;
        ff = (double) dd;
        boolean isMarried = true;
        char c = 'a';
        String str = "홍길동";
        String str2 = String.format("저는 %s입니다.,"김정우");

        System.out.printls(35);
        System.out.printf("저는 %s입니다.,str);
        System.out.printls(Math.max(10, 30)));

    }
}
```
```java
public class Main{
    public static void main(String[] args){
        String str = "100";
        int i = Integer.parseInt(str);
        String str2 = String.valueOf(i);
        Random random = new Random();
        int rand = random.nextInt(10);
    }
}
```
```java
public class Main{
    public static void main(String[] args){
        Scanner scanner = new Scanner(System.in);
        String str = scanner.next();
        int i = scanner.nextInt();
        System.out.println(scanner.next());
    }
}
```
```java
public class Main{
    public static void main(String[] args){
        int i = 10;
        if(i > 5 && true || false){
            System.out.printls(참);
        }
        else if(!i < 3){
            System.out.printls(거짓);
        }
        else{
            System.out.printls(나머지);
        }
        boolean isMarried = true;
        String str = isMarried ? "결혼 했다" : "결혼 안 했다";

        switch(str){
            case "결혼 했다" :
                break;
            case "결혼 안했다" :
                break
            default:
                System.out.printls(?);
        }
    }
}
```
```java
public class Main{
    public static void main(String[] args){
        for(int i = 0; i < 10; i++){
            System.out.printls(i);
            if( i == 3 )
                break;
        }
        int i = 0;
        while(i<10){
            System.out.printls(?);
            i++;
        }
        do{
            System.out.println(i);
            i++
        } while(i < 10)
    }
}
```