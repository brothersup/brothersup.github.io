---
title: 자바 입출력 - Scanner, BufferedReader, BufferedWriter
author:
  name: 신형섭
  link: https://github.com/brothersup
date: 2022-03-05 17:53:00 +0900
categories: [study, java]
tags: [java, 입출력, Scanner, bufferedReader, bufferedWriter]
image:
  src: /70502054/156830053-7b86a906-95a9-4ff8-bc9c-3ac84091a533.png
  width: 910
  height: auto
---

최근 알고리즘공부를 시작하면서 몰랐거나 안써본 클래스들에 대해 많이 알게되었는데 그중에 하나가 콘솔로 입력값을 받는것이었다.

예전에 학교에서 C언어를 배울때 scanf 함수를 써서 입력값을 받았던게 생각나서 검색해보니 자바에도 Scanner라는 클래스가 있어서 Scanner로 입력을 받다가, BufferedReader 클래스로도 입력을 받을 수 있다는 것을 알게되었다.

성능이 더 좋다고 해서 그 뒤로는 BufferedReader를 사용해서 입력값을 받고있는데, BufferedWriter도 세트처럼 쓰는 것 같아서 출력시 함께 사용중이다. 그래서 쓰고있는 김에 복습도 할겸 정리해두려고 한다.

<hr>

# Scanner
- java.util 패키지에 있는 클래스
- 입력받은 값을 문자나 정수, 실수, 문자열 등 다양한 타입으로 리턴해준다.
- 스페이스바나 엔터로 입력받은 값을 구분해서 나눠준다.

### 사용 예
```java
import java.util.Scanner;

public class ScannerTest {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        String str = scanner.next();
        int number = scanner.nextInt();

        System.out.println(str);
        System.out.println(number);
    }
}
```
- next(), nextInt() 메서드를 사용해서 각각 입력받은 값을 데이터타입에 맞게 받아서 출력하는 코드다.
- 값을 받는 메서드 개수만큼 입력을 받고 스페이스바나 엔터 입력으로 구분한다.

![](/70502054/156818791-3ac8a226-c299-4114-9b6a-eecf2a181459.png)
![](/70502054/156819133-ab1f4504-1950-4bbb-b4ff-dc2e1032b514.png)
>구분은 스페이스, 엔터 모두 가능


![](/70502054/156819619-6698f1c4-4309-45f2-b477-b151c4991213.png)
> 데이터 타입이 다르면 예외가 발생하므로 주의!!
{: .prompt-info}

<hr>

# BufferedReader / BufferedWriter
- java.io 패키지에 있는 클래스
- 사용이 끝나면 close() 메서드를 호출하도록 한다.
- 메서드를 사용하기위해서 예외처리가 필요하다.

## BufferedReader
- readLine() 메서드를 사용해서 입력값을 받음.
- 줄단위로 입력을 받기 때문에 구분은 Scanner 클래스와 달리 엔터로만 구분된다.
- Scanner 처럼 스페이스바로 구분하려면 split() 같은 메서드를 사용하면 된다.
- 입력받은 값은 String 타입으로 반환하기 때문에 별도의 캐스팅이 필요하다.
- Scanner에 비해 훨씬 큰 버퍼의 크기를 갖고있다.
- Scanner는 입력 데이터를 파싱하지만 BufferedReader는 파싱 과정이 없기때문에 Scanner보다 속도가 빠르다.

## BufferedWriter
- 버퍼에 저장된 데이터를 flush()나 close() 메서드가 호출될 때 출력스트림으로 내보낸다.
- 개행문자가 포함되지 않기때문에 줄바꿈을 하려면 입력끝에 개행문자를 넣어주거나 newLine() 메서드를 사용한다.
- 정수형을 출력할 경우 파라미터로 받은 정수를 문자의 코드값으로 인식해서 해당 문자를 출력하므로 String.valueOf() 메서드로 문자로 바꾼다음 출력해야 한다.

### println()과 write() 특징 비교
```java
import java.io.*;

public class BufferedIOTest {
  public static void main(String[] args) {
    BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
    BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
    try {
      // 입력받은 문자열을 그대로 출력
      String str1 = br.readLine();
      System.out.println("println = " + str1);
      bw.write("write = " + str1);
      bw.newLine(); // 줄바꿈 처리

      // 입력받은 문자열을 공백으로 구분
      String[] str2 = br.readLine().split(" ");
      for (String s : str2) {
        System.out.println("println = " + s);
        bw.write("write = " + s);
        bw.newLine();
      }

      // 입력받은 숫자들의 합
      String[] numbers = br.readLine().split(" ");
      int total = 0;
      for (String number : numbers) {
        total += Integer.parseInt(number);
      }
      System.out.println("println = " + total);
      bw.write("write = " + total);

      // bufferedReader 사용 끝
      br.close();
      bw.close(); // close메서드가 호출되면 flush가 먼저 일어난 다음 close된다.
    } catch (IOException e) {
      e.printStackTrace();
    }
  }
}
```
### 출력 결과
![](/70502054/156873046-e145d642-e274-4036-b7e8-1c294071c0d9.png){: width="400" height="auto" }
> - write() 다음줄에 newLine() 메서드를 호출해서 개행처리를 하고있음을 볼 수 있다.
> - 결과를 보면 알수있듯이 println()은 호출되자마자 출력하지만 write()는 모아뒀다가 한번에 출력한다.

### 정수형 출력 예
```java
public class Example {
  public static void main(String[] args) {
    BufferedWriter br = new BufferedWriter(new OutputStreamWriter(System.out));
    br.write(97); // 출력값: a
    br.write(String.valueOf(97)); // 출력값: 97
  }
}
```

<hr>

# 마무리

첫 포스팅으로 입출력 관련 클래스들의 특징과 각 클래스들에서 자주쓰는 메소드에 대해 간단히 알아보았다.

Scanner는 입력값을 편하게 받을 수 있다는 장점과 파싱하고 캐스팅하느라 처리속도가 BufferedReader에 비해 느리다는 단점이 있다.

BufferedReader는 속도는 빠르지만 예외처리, 파싱, 캐스팅하느라 번거롭기 때문에 다수의 캐스팅이 필요하거나 입력값이 적을 땐 그냥 Scanner를 쓰고 그 외엔 BufferedReader/Writer를 사용하면 될 것 같다.(개인적인 생각..)

정리한 내용 외에도 입출력에 관련된 많은 클래스들과 메소드들이 있지만, 아직은 쓰는일이 없기 때문에 나중에 쓰게되면 알아봐야겠다.

<hr>

## 참고
- [자바11 api 문서](https://docs.oracle.com/en/java/javase/11/docs/api/index.html){:target="_blank"} - [Scanner](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Scanner.html){:target="_blank"}, [BufferedReader](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/io/BufferedReader.html){:target="_blank"}, [BufferedWriter](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/io/BufferedWriter.html){:target="_blank"}
- <https://www.geeksforgeeks.org/difference-between-scanner-and-bufferreader-class-in-java/>{:target="_blank"}
- <https://www.quora.com/Why-is-Scanner-so-much-slower-than-BufferedReader>{:target="_blank"}
