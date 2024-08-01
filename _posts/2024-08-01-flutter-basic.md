---
layout: post
title: Flutter의 기본 개요
subtitle: Flutter의 기본적인 내용에 대해 알아봅니다.
author: 윤영민
categories: Flutter
tags: [Flutter]
---

## Flutter

플러터 위젯은 React 에서 영감을 얻은 최신 프레임워크를 사용하여 빌드됩니다.
핵심 아이디어는 위젯으로 UI를 빌드한다는 것입니다. 
위젯은 현재 구성과 상태를 고려하여 뷰가 어떻게 보여야 하는지 설명합니다.
위젯의 상태가 변경되면 위젯은 설명을 다시 빌드하고, 프레임워크는 기본 렌더 트리에서 한 상태에서 다음 상태로 전환하는 데 필요한 최소한의 변경 사항을 결정하기 위해 이전 설명과 비교합니다.


### 1. Hello World

최소한의 Flutter 앱은 runApp()위젯으로 함수를 호출하기만 합니다.


```groovy

import 'package:flutter/material.dart';

void main() {
  runApp(
    const Center(
      child: Text(
        'Hello, world!',
        textDirection: TextDirection.ltr,
      ),
    ),
  );
}

```

이 runApp()함수는 주어진 것을 가져와 Widget위젯 트리의 루트로 만듭니다.
이 예에서 위젯 트리는 위젯 Center과 그 자식인 Text위젯이라는 두 개의 위젯으로 구성됩니다.
프레임워크는 루트 위젯이 화면을 덮도록 강제합니다.
즉, "Hello, world"라는 텍스트가 화면 중앙에 배치됩니다.

이 경우 텍스트 방향을 지정해야 합니다.
위젯을 MaterialApp사용하면 이 작업이 자동으로 처리됩니다.

앱을 작성할 때 일반적으로 위젯이 상태를 관리하는지 여부에 따라 StatefulWidget또는 StatelessWidget의 하위 클래스인 새 위젯을 작성합니다. 위젯의 주요 작업은 다른 하위 수준 위젯의 관점에서 위젯을 설명하는 함수를 구현하는 것입니다 . 프레임워크는 프로세스가 기본 build()를 나타내는 위젯에서 바닥을 칠 때까지 이러한 위젯을 차례로 빌드합니다. 기본 RenderObject는 위젯의 지오메트리를 계산하고 설명합니다.


### 2 기본 위젯

* Text 
    : 위젯 Text을 사용하면 애플리케이션 내에서 스타일이 적용된 텍스트를 생성할 수 있습니다.
* Row,Column
    : 수평 및 수직 방향 모두에서 유연한 레이아웃을 만들 수 있습니다. 
* Stack
    : 수평 또는 수직방향이 아닌, Stack위젯을 페인트 순서대로 서로 위에 놓을 수 있도록 해줍니다.
    : 그런 다음 위젯을 Positioneda의 자식에 사용 Stack하여 스택의 위쪽, 오른쪽, 아래쪽 또는 왼쪽 가장자리를 기준으로 위젯을 배치할 수 있습니다.
    : 스택은 웹의 절대 위치 지정 레이아웃 모델을 기반으로 합니다.
* Container
    : 위젯 Container을 사용하면 직사각형 시각적 요소를 만들 수 있습니다.
    : 컨테이너는 BoxDecoration배경, 테두리 또는 그림자와 같은 로 장식할 수 있습니다.
    : Container또한 에는 크기에 적용되는 여백, 패딩 및 제약 조건이 있을 수 있습니다.
    : Container행렬을 사용하여 3차원 공간에서 를 변환할 수 있습니다.


```groovy

import 'package:flutter/material.dart';

class MyAppBar extends StatelessWidget {
  const MyAppBar({required this.title, super.key});

  // Fields in a Widget subclass are always marked "final".

  final Widget title;

  @override
  Widget build(BuildContext context) {
    return Container(
      height: 56, // in logical pixels
      padding: const EdgeInsets.symmetric(horizontal: 8),
      decoration: BoxDecoration(color: Colors.blue[500]),
      // Row is a horizontal, linear layout.
      child: Row(
        children: [
          const IconButton(
            icon: Icon(Icons.menu),
            tooltip: 'Navigation menu',
            onPressed: null, // null disables the button
          ),
          // Expanded expands its child
          // to fill the available space.
          Expanded(
            child: title,
          ),
          const IconButton(
            icon: Icon(Icons.search),
            tooltip: 'Search',
            onPressed: null,
          ),
        ],
      ),
    );
  }
}

class MyScaffold extends StatelessWidget {
  const MyScaffold({super.key});

  @override
  Widget build(BuildContext context) {
    // Material is a conceptual piece
    // of paper on which the UI appears.
    return Material(
      // Column is a vertical, linear layout.
      child: Column(
        children: [
          MyAppBar(
            title: Text(
              'Example title',
              style: Theme.of(context) //
                  .primaryTextTheme
                  .titleLarge,
            ),
          ),
          const Expanded(
            child: Center(
              child: Text('Hello, world!'),
            ),
          ),
        ],
      ),
    );
  }
}

void main() {
  runApp(
    const MaterialApp(
      title: 'My app', // used by the OS task switcher
      home: SafeArea(
        child: MyScaffold(),
      ),
    ),
  );
}

```

파일 섹션 uses-material-design: true에 항목이 있는지 확인하세요.
그러면 미리 정의된 Material 아이콘 세트를 사용할 수 있습니다.
Materials 라이브러리를 사용하는 경우 일반적으로 `flutterpubspec.yaml`을 포함하는 것이 좋습니다.

```groovy

name: my_app
flutter:
  uses-material-design: true

```


많은 Material Design 위젯은 테마 데이터를 상속하기 위해 제대로 표시되기 위해 `MaterialApp` 안에 있어야 합니다.
따라서 `MaterialApp`으로 애플리케이션을 실행하세요.

위젯 MyAppBar은 높이가 56개의 장치 독립 픽셀이고 내부 패딩이 8픽셀인 Container를 왼쪽과 오른쪽에 모두 만듭니다.
컨테이너 내부에서 Row 레이아웃을 사용하여 자식을 구성합니다.
가운데 자식인 title위젯은 Expanded로 표시되어 다른 자식이 사용하지 않은 남은 사용 가능한 공간을 채우기 위해 확장됩니다.
여러 자식을 가질 수 있으며 Expanded 인수를 사용하여 사용 가능한 공간을 사용하는 비율을 결정할 수 있습니다.

위젯 MyScaffold은 자식을 세로 열로 구성합니다.
열의 맨 위에는 MyAppBar의 인스턴스를 배치하여 앱 바에 Text제목으로 사용할 위젯을 전달합니다.
위젯을 다른 위젯에 인수로 전달하는 것은 다양한 방식으로 재사용할 수 있는 일반 위젯을 만들 수 있는 강력한 기술입니다.
마지막으로 는 MyScaffold를 사용하여 Expanded나머지 공간을 본문으로 채웁니다.
본문은 가운데 정렬된 메시지로 구성됩니다.



---
