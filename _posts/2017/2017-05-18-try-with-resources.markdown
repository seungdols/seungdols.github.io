---
layout: "post"
title: "try-with-resources"
description: "자원을 자동으로 닫자! - Autocleable"
date: "2017-05-18 22:56"
tags: [java, programming]
comments: true
---

# Try-with-resources 사용하기

JDK7에서 생겨난 자원 자동 종료?라고 생각하면 된다. 물론, 다되는 것은 아니고 `Autocleable`을 구현한 클래스만 사용가능하다.

```java
String readDataFromFile(String filePath) {
    BufferedReader br = null;
    String data = "";
    try {
        br = new BufferedReader(new FileReader(filePath));

        data = br.readLine();

    } catch (FileNotFoundException e) {
        e.printStackTrace();
    } catch (IOException e) {
        e.printStackTrace();
    } finally
    {
        try {
            br.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
    return data;
}
```

보통 파일이나 어떤 자원을 생성하고 읽어야 한다면, 수 많은 `try-catch-finally` 구문을 사용해야 했습니다.

그런데, 아래처럼 바뀐 것이죠.

```java
static String readFirstLineFromFile(String path) throws IOException {
     try (BufferedReader br =
                  new BufferedReader(new FileReader(path))) {
         return br.readLine();
     }
 }
 ```

 사용 방법은 간단하며, `try ()` 구문 안에서 자원을 생성하면, 알아서 `close()`를 호출해줍니다.

 더군다나, JDK7에서부터 Multi-catch를 쓸 수 있죠.

```java
public class ExampleExceptionHandlingNew
{
   public static void main( String[] args )
   {
   	try {
   		URL url = new URL("http://www.yoursimpledate.server/");
   		BufferedReader reader = new BufferedReader(
   			new InputStreamReader(url.openStream()));
   		String line = reader.readLine();
   		SimpleDateFormat format = new SimpleDateFormat("MM/DD/YY");
   		Date date = format.parse(line);
   	}
   	catch(ParseException | IOException exception) {
   		// handle our problems here.
   	}
   }
}
```

 참고

* [java7exceptions-486908](http://www.oracle.com/technetwork/articles/java/java7exceptions-486908.html)
* [tryResourceClose](https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html)
* [자바캔님 블로그](http://javacan.tistory.com/entry/my-interesting-java7-five-features)
