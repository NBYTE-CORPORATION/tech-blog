---
layout: post
title: Custom Widget
subtitle: 위젯을 마음대로 커스텀해보자.
author: 주세환
categories: Flutter
tags: [Flutter, Widget, Custom Widget]
---
## 커스텀 위젯

```dart
  Widget build(BuildContext context) {

    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(),
        body: SizedBox(
          child: Text('안녕'),
        ),
      ),
    );
  }
```  

이렇게 작성하면

![](https://velog.velcdn.com/images/nawhes_joo/post/7037a616-e080-469d-8e35-4b1d3d32ebd9/image.png)

body에 안녕이라는 텍스트가 출력된다.

만약, 레이아웃용 위젯들이 너무 길다면 매우 복잡하고 가독성도 많이 떨어질 것이다.

이 때 `커스텀 위젯`을 만들어서 깔끔하게 축약할 수 있다.

---

### class

빈 공간에 stless를 입력하여 StatelessWidget을 생성하고

```dart
class CustomWidget extends StatelessWidget {
  const CustomWidget({super.key});

  @override
  Widget build(BuildContext context) {
    return const Placeholder(); // 축약할 레이아웃 작성
  }
}
```

위 코드와 같이 class를 작명한다.

그리고 return 뒤에 축약할 레이아웃을 넣으면 된다.

```dart
class CustomWidget extends StatelessWidget {
  const CustomWidget({super.key});

  @override
  Widget build(BuildContext context) {
    return SizedBox(
      child: Text('안녕'),
    );
  }
}
```

처음에 작성한 '안녕' 텍스트가 들어간 SizedBox 레이아웃을 넣었다.

이렇게 CustomWidget이라는 이름의 커스텀 위젯을 만든 것이다.

이제 `CustomWidget()`만 작성해도 CustomWidget에 작성한 레이아웃을 불러올 수 있다.

```dart
class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {

    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(),
        body: CustomWidget(),
      ),
    );
  }
}

class CustomWidget extends StatelessWidget {
  const CustomWidget({super.key});

  @override
  Widget build(BuildContext context) {
    return SizedBox(
      child: Text('안녕'),
    );
  }
}
```

전체 코드의 모습이다.

![](https://velog.velcdn.com/images/nawhes_joo/post/270ac3f9-28dd-42ef-ad36-a44d6253a7ed/image.png)

커스텀 위젯으로 생성하기 전과 같은 결과를 확인할 수 있다.

위 예시의 레이아웃 길이가 짧기 때문에 잘 와닿지 않겠지만, 레이아웃이 길어지면 매우 유용하게 사용될 것이다.

물론 다른 dart 파일에 미리 커스텀 위젯을 작성한 후, import를 통해 작성한 커스텀 위젯을 불러올 수 있다.


---

### 변수

```dart
var a = SizedBox(
	child: Text('안녕')
);

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {

    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(),
        body: a,
      ),
    );
  }
}
```

이런 식으로 변수나 함수에 담아 축약해도 된다. 하지만 성능상 이슈가 발생할 수도 있다. 

그렇다면 변수에는 어떤 것들을 담을 수 있을까?

그것은 바로 `변하지 않는 것들`이다.

예를 들면 로고, 상단바, 하단바 등 변하지 않는 UI들은 변수 함수로 축약해도 상관이 없다.

---

### 주의사항

커스텀 위젯이 편하다고 아무거나 다 커스텀 위젯으로 만드는 것은 좋은 방법이 아니다.

대표적으로 state 관리가 힘들어진다는 단점이 있다.

그러므로 재사용이 많은 UI들, 또는 큰 UI(페이지들)을 커스텀 위젯으로 만들어서 사용하면 좋다.

---

## ListView

```dart
return MaterialApp(
      home: Scaffold(
        appBar: AppBar(),
        body: Column(
          children: [
            Text('안녕'),
            Text('안녕'),
            Text('안녕'),
            Text('안녕'),
            Text('안녕'),
          ],
        ),
      ),
    );
```

세로로 배치할 때 사용하는 Column이다.

![](https://velog.velcdn.com/images/nawhes_joo/post/f2e43b45-d583-48f8-af32-a52e05679fff/image.png)


하지만, 글자가 아무리 많더라도 스크롤바가 자동으로 생기지 않는다.

이럴 때 사용하는 것이 ListView이다.

![](https://velog.velcdn.com/images/nawhes_joo/post/8659c16e-8548-434d-8d3f-f48b070fa380/image.png)

Column 대신 ListView를 사용하면 위 사진과 같이 스크롤바가 생기는 것을 확인할 수 있다.

스크롤바 뿐만 아니라`controller`를 통해 스크롤 위치 감시도 가능하고, 메모리 절약도 가능하다.

> 1~100까지의 글자가 있다면, 유저가 가장 아래쪽을 보고 있을 때, 스크롤해서 지나간 위쪽 부분은 메모리에서 삭제가 가능하다. 이런 이유로 메모리 절약(성능개선)이 가능하다.

쇼핑몰, 인스타 피드 등 전부 ListView라고 보면 된다.

용도에 맞게 알맞은 위젯을 사용하여 개발해보자.

출처 - [코딩애플](https://www.youtube.com/@codingapple)