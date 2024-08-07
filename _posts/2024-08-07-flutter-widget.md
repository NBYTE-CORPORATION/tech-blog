---
layout: post
title: Flutter의 기본 위젯
subtitle: Flutter의 기본 위젯에 대해 알아봅니다.
author: 윤영민
categories: Flutter
tags: [Flutter]
---

### 1. Material Components 사용

Flutter는 Material 디자인을 따르는 앱을 빌드하는 데 도움이 되는 여러 위젯을 제공합니다. 
앱은 Material 위젯으로 시작하며 MaterialApp 위젯은 앱 루트에서 여러 유용한 위젯을 빌드합니다.
네비게이터를 사용하면 응용프로그램 화면 간에 원활하게 전환할 수 있습니다.

```groovy

import 'package:flutter/material.dart';

void main() {
  runApp(
    const MaterialApp(
      title: 'Flutter Tutorial',
      home: TutorialHome(),
    ),
  );
}

class TutorialHome extends StatelessWidget {
  const TutorialHome({super.key});

  @override
  Widget build(BuildContext context) {
    // Scaffold is a layout for
    // the major Material Components.
    return Scaffold(
      appBar: AppBar(
        leading: const IconButton(
          icon: Icon(Icons.menu),
          tooltip: 'Navigation menu',
          onPressed: null,
        ),
        title: const Text('Example title'),
        actions: const [
          IconButton(
            icon: Icon(Icons.search),
            tooltip: 'Search',
            onPressed: null,
          ),
        ],
      ),
      // body is the majority of the screen.
      body: const Center(
        child: Text('Hello, world!'),
      ),
      floatingActionButton: const FloatingActionButton(
        tooltip: 'Add', // used by assistive technologies
        onPressed: null,
        child: Icon(Icons.add),
      ),
    );
  }
}

```

이제 코드가 MyAppBar 및 MyScaffold에서 AppBar 및 Scaffold 위젯으로, material.dart에서 앱이 조금 더 Material로 보이기 시작했습니다.
예를 들어 앱 바에 그림자가 있고 제목 텍스트가 올바른 스타일링을 자동으로 상속합니다. 플로팅 액션 버튼도 추가됩니다.
위젯은 다른 위젯에 인수로 전달됩니다.
스캐폴드 위젯은 여러 다른 위젯을 명명된 인수로 가져오고, 각 인수는 적절한 위치에 있는 스캐폴드 레이아웃에 배치됩니다.
마찬가지로 AppBar 위젯을 사용하면 선두 위젯과 제목 위젯의 작업을 위젯으로 전달할 수 있습니다. 
이 패턴은 프레임워크 전체에서 반복되며, 위젯을 설계할 때 고려할 수 있습니다.

---

### 2. 제스처 처리

```groovy

import 'package:flutter/material.dart';

class MyButton extends StatelessWidget {
  const MyButton({super.key});

  @override
  Widget build(BuildContext context) {
    return GestureDetector(
      onTap: () {
        print('MyButton was tapped!');
      },
      child: Container(
        height: 50,
        padding: const EdgeInsets.all(8),
        margin: const EdgeInsets.symmetric(horizontal: 8),
        decoration: BoxDecoration(
          borderRadius: BorderRadius.circular(5),
          color: Colors.lightGreen[500],
        ),
        child: const Center(
          child: Text('Engage'),
        ),
      ),
    );
  }
}

void main() {
  runApp(
    const MaterialApp(
      home: Scaffold(
        body: Center(
          child: MyButton(),
        ),
      ),
    ),
  );
}

```

GestureDetector 위젯에는 시각적 표현이 없지만 대신 사용자가 만든 제스처를 감지합니다.
사용자가 컨테이너를 탭하면 GestureDetector가 컨테이너를 호출하여 콘솔에 메시지를 인쇄합니다.
GestureDetector를 사용하여 탭, 드래그 및 스케일을 포함한 다양한 입력 제스처를 감지할 수 있습니다.

많은 위젯에서 GestureDetector를 사용하여 다른 위젯에 대한 선택적 콜백을 제공합니다.
예를 들어, 아이콘버튼, 상승버튼 및 유동액션버튼 위젯에는 사용자가 위젯을 누를 때 트리거되는 OnPressed() 콜백이 있습니다.


---

### 3. 입력에 따른 위젯 변경

지금까지 이 페이지는 상태 비저장 위젯만 사용했습니다.
상태 비저장 위젯은 부모 위젯에서 인수를 받아 최종 구성원 변수에 저장합니다.
위젯을 build()하도록 요청받으면 이러한 저장된 값을 사용하여 위젯이 생성하는 위젯에 대한 새로운 인수를 도출합니다.

예를 들어, 사용자 입력에 더 흥미로운 방식으로 반응하기 위해 응용 프로그램은 일반적으로 일부 상태를 제공합니다.
Flutter는 StatefulWidgets를 사용하여 이 아이디어를 캡처합니다.
StatefulWidgets는 상태 개체를 생성하는 방법을 알고 상태를 유지하는 데 사용되는 특수 위젯입니다.
앞서 언급한 ElegatedButton을 사용하여 다음과 같은 기본 예를 생각해 보십시오

```groovy

import 'package:flutter/material.dart';

class Counter extends StatefulWidget {
  // This class is the configuration for the state.
  // It holds the values (in this case nothing) provided
  // by the parent and used by the build  method of the
  // State. Fields in a Widget subclass are always marked
  // "final".

  const Counter({super.key});

  @override
  State<Counter> createState() => _CounterState();
}

class _CounterState extends State<Counter> {
  int _counter = 0;

  void _increment() {
    setState(() {
      // This call to setState tells the Flutter framework
      // that something has changed in this State, which
      // causes it to rerun the build method below so that
      // the display can reflect the updated values. If you
      // change _counter without calling setState(), then
      // the build method won't be called again, and so
      // nothing would appear to happen.
      _counter++;
    });
  }

  @override
  Widget build(BuildContext context) {
    // This method is rerun every time setState is called,
    // for instance, as done by the _increment method above.
    // The Flutter framework has been optimized to make
    // rerunning build methods fast, so that you can just
    // rebuild anything that needs updating rather than
    // having to individually changes instances of widgets.
    return Row(
      mainAxisAlignment: MainAxisAlignment.center,
      children: <Widget>[
        ElevatedButton(
          onPressed: _increment,
          child: const Text('Increment'),
        ),
        const SizedBox(width: 16),
        Text('Count: $_counter'),
      ],
    );
  }
}

void main() {
  runApp(
    const MaterialApp(
      home: Scaffold(
        body: Center(
          child: Counter(),
        ),
      ),
    ),
  );
}

```

StatefulWidget과 State가 서로 다른 개체인 이유가 궁금할 수 있습니다.
Flutter에서 이 두 가지 유형의 개체는 수명 주기가 다릅니다.
위젯은 현재 상태에서 응용프로그램의 프리젠테이션을 구성하는 데 사용되는 임시 개체입니다.
반면에 상태 개체는 build()할 호출 사이에 영구적이어서 정보를 기억할 수 있습니다.

위의 예제는 사용자 입력을 수락하고 결과를 build() 메서드에 직접 사용합니다.
더 복잡한 응용 프로그램에서는 위젯 계층의 서로 다른 부분이 서로 다른 문제를 일으킬 수 있습니다.
예를 들어, 하나의 위젯은 날짜나 위치와 같은 특정 정보를 수집할 목적으로 복잡한 사용자 인터페이스를 제공하는 반면, 다른 위젯은 해당 정보를 사용하여 전체 프레젠테이션을 변경할 수 있습니다.

Flutter에서 변경 알림은 콜백을 통해 위젯 계층 구조를 "up"하는 반면, 현재 상태는 프레젠테이션을 수행하는 상태 비저장 위젯으로 "down" 이동합니다.
이 흐름을 리디렉션하는 일반적인 상위는 State입니다.
다음의 약간 더 복잡한 예제는 이것이 실제로 어떻게 작동하는지 보여줍니다

```groovy

import 'package:flutter/material.dart';

class CounterDisplay extends StatelessWidget {
  const CounterDisplay({required this.count, super.key});

  final int count;

  @override
  Widget build(BuildContext context) {
    return Text('Count: $count');
  }
}

class CounterIncrementor extends StatelessWidget {
  const CounterIncrementor({required this.onPressed, super.key});

  final VoidCallback onPressed;

  @override
  Widget build(BuildContext context) {
    return ElevatedButton(
      onPressed: onPressed,
      child: const Text('Increment'),
    );
  }
}

class Counter extends StatefulWidget {
  const Counter({super.key});

  @override
  State<Counter> createState() => _CounterState();
}

class _CounterState extends State<Counter> {
  int _counter = 0;

  void _increment() {
    setState(() {
      ++_counter;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Row(
      mainAxisAlignment: MainAxisAlignment.center,
      children: <Widget>[
        CounterIncrementor(onPressed: _increment),
        const SizedBox(width: 16),
        CounterDisplay(count: _counter),
      ],
    );
  }
}

void main() {
  runApp(
    const MaterialApp(
      home: Scaffold(
        body: Center(
          child: Counter(),
        ),
      ),
    ),
  );
}

```

카운터 표시(CounterDisplay)와 카운터 변경(CounterIncrementor)의 문제를 깔끔하게 분리하여 두 개의 새로운 상태 비저장 위젯을 만듭니다.
순 결과는 이전 예와 동일하지만, 책임 분리를 통해 상위 위젯의 단순성을 유지하면서 더 큰 복잡성을 개별 위젯에 캡슐화할 수 있습니다.


---

### 4. 기본 위젯 사용하기

다음은 이러한 개념을 결합한 보다 완벽한 예입니다.
가상 쇼핑 애플리케이션은 판매용으로 제공되는 다양한 제품을 표시하고 의도된 구매를 위해 쇼핑 카트를 유지합니다.
먼저 프레젠테이션 클래스인 ShoppingListItem을 정의합니다

```groovy

import 'package:flutter/material.dart';

class Product {
  const Product({required this.name});

  final String name;
}

typedef CartChangedCallback = Function(Product product, bool inCart);

class ShoppingListItem extends StatelessWidget {
  ShoppingListItem({
    required this.product,
    required this.inCart,
    required this.onCartChanged,
  }) : super(key: ObjectKey(product));

  final Product product;
  final bool inCart;
  final CartChangedCallback onCartChanged;

  Color _getColor(BuildContext context) {
    // The theme depends on the BuildContext because different
    // parts of the tree can have different themes.
    // The BuildContext indicates where the build is
    // taking place and therefore which theme to use.

    return inCart //
        ? Colors.black54
        : Theme.of(context).primaryColor;
  }

  TextStyle? _getTextStyle(BuildContext context) {
    if (!inCart) return null;

    return const TextStyle(
      color: Colors.black54,
      decoration: TextDecoration.lineThrough,
    );
  }

  @override
  Widget build(BuildContext context) {
    return ListTile(
      onTap: () {
        onCartChanged(product, inCart);
      },
      leading: CircleAvatar(
        backgroundColor: _getColor(context),
        child: Text(product.name[0]),
      ),
      title: Text(product.name, style: _getTextStyle(context)),
    );
  }
}

void main() {
  runApp(
    MaterialApp(
      home: Scaffold(
        body: Center(
          child: ShoppingListItem(
            product: const Product(name: 'Chips'),
            inCart: true,
            onCartChanged: (product, inCart) {},
          ),
        ),
      ),
    ),
  );
}


```

ShoppingListItem 위젯은 상태 비저장 위젯에 대한 일반적인 패턴을 따릅니다.
이 위젯은 생성자에서 받은 값을 최종 구성원 변수에 저장한 다음 build() 함수 중에 사용합니다.
예를 들어 inCart 부울은 현재 테마의 기본 색상을 사용하는 것과 회색을 사용하는 두 시각적 모양 사이를 전환합니다.

사용자가 목록 항목을 탭하면 위젯이 직접 inCart 값을 수정하지 않습니다.
대신 위젯은 상위 위젯에서 받은 onCartChanged 함수를 호출합니다.
이 패턴을 사용하면 위젯 계층 구조에 상태를 더 높게 저장할 수 있으며, 이로 인해 상태가 더 오랜 기간 동안 지속됩니다.
극단적으로 runApp()을 실행하기 위해 전달된 위젯에 저장된 상태가 애플리케이션의 수명 동안 지속됩니다.

부모가 onCartChanged 콜백을 받으면 부모는 내부 상태를 업데이트하고 이를 통해 부모가 ShoppingListItem의 새 인스턴스를 재구성하고 inCart 값으로 생성하도록 트리거합니다.
부모가 재구성할 때 ShoppingListItem의 새 인스턴스를 생성하지만 프레임워크는 새로 생성된 위젯을 이전에 생성된 위젯과 비교하고 차이점만 기본 RenderObject에 적용하기 때문에 작업이 저렴합니다.

다음은 변경 가능한 상태를 저장하는 부모 위젯의 예입니다

```groovy

import 'package:flutter/material.dart';

class Product {
  const Product({required this.name});

  final String name;
}

typedef CartChangedCallback = Function(Product product, bool inCart);

class ShoppingListItem extends StatelessWidget {
  ShoppingListItem({
    required this.product,
    required this.inCart,
    required this.onCartChanged,
  }) : super(key: ObjectKey(product));

  final Product product;
  final bool inCart;
  final CartChangedCallback onCartChanged;

  Color _getColor(BuildContext context) {
    // The theme depends on the BuildContext because different
    // parts of the tree can have different themes.
    // The BuildContext indicates where the build is
    // taking place and therefore which theme to use.

    return inCart //
        ? Colors.black54
        : Theme.of(context).primaryColor;
  }

  TextStyle? _getTextStyle(BuildContext context) {
    if (!inCart) return null;

    return const TextStyle(
      color: Colors.black54,
      decoration: TextDecoration.lineThrough,
    );
  }

  @override
  Widget build(BuildContext context) {
    return ListTile(
      onTap: () {
        onCartChanged(product, inCart);
      },
      leading: CircleAvatar(
        backgroundColor: _getColor(context),
        child: Text(product.name[0]),
      ),
      title: Text(
        product.name,
        style: _getTextStyle(context),
      ),
    );
  }
}

class ShoppingList extends StatefulWidget {
  const ShoppingList({required this.products, super.key});

  final List<Product> products;

  // The framework calls createState the first time
  // a widget appears at a given location in the tree.
  // If the parent rebuilds and uses the same type of
  // widget (with the same key), the framework re-uses
  // the State object instead of creating a new State object.

  @override
  State<ShoppingList> createState() => _ShoppingListState();
}

class _ShoppingListState extends State<ShoppingList> {
  final _shoppingCart = <Product>{};

  void _handleCartChanged(Product product, bool inCart) {
    setState(() {
      // When a user changes what's in the cart, you need
      // to change _shoppingCart inside a setState call to
      // trigger a rebuild.
      // The framework then calls build, below,
      // which updates the visual appearance of the app.

      if (!inCart) {
        _shoppingCart.add(product);
      } else {
        _shoppingCart.remove(product);
      }
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Shopping List'),
      ),
      body: ListView(
        padding: const EdgeInsets.symmetric(vertical: 8),
        children: widget.products.map((product) {
          return ShoppingListItem(
            product: product,
            inCart: _shoppingCart.contains(product),
            onCartChanged: _handleCartChanged,
          );
        }).toList(),
      ),
    );
  }
}

void main() {
  runApp(const MaterialApp(
    title: 'Shopping App',
    home: ShoppingList(
      products: [
        Product(name: 'Eggs'),
        Product(name: 'Flour'),
        Product(name: 'Chocolate chips'),
      ],
    ),
  ));
}

```

ShoppingList 클래스는 StatefulWidget을 확장하며, 이는 이 위젯이 변경 가능한 상태를 저장한다는 것을 의미합니다.
ShoppingList 위젯이 트리에 처음 삽입되면 프레임워크는 createState() 함수를 호출하여 트리의 해당 위치와 연결할 _ShoppingListState의 새 인스턴스를 만듭니다.
(State의 하위 클래스는 일반적으로 개인 구현 세부 정보임을 나타내기 위해 밑줄로 이름이 지정됩니다.)
이 위젯의 상위 클래스가 재구성되면 상위는 ShoppingList의 새 인스턴스를 생성하지만 프레임워크는 CreateState를 다시 호출하는 대신 이미 트리에 있는 _ShoppingListState 인스턴스를 재사용합니다.

_ShoppingListState는 현재 ShoppingList의 속성에 액세스하기 위해 해당 위젯 속성을 사용할 수 있습니다.
상위가 새 ShoppingList를 재구성하고 생성하면 _ShoppingListState가 새 위젯 값으로 재구성됩니다.
위젯 속성이 변경될 때 알림을 받으려면 이전 위젯과 현재 위젯을 비교할 수 있도록 이전 위젯을 전달한 didUpdateWidget() 함수를 재정의하십시오.

onCartChanged 콜백을 처리할 때 _ShoppingListState는 _ShoppingCart에서 제품을 추가하거나 제거하여 내부 상태를 변경합니다.
프레임워크가 내부 상태를 변경했다는 신호를 보내기 위해 해당 콜을 setState() 콜로 래핑합니다.
setState를 호출하면 이 위젯이 더티로 표시되고 다음 번에 앱이 화면을 업데이트해야 할 때 다시 빌드하도록 예약됩니다.
위젯의 내부 상태를 수정할 때 setState를 호출하는 것을 잊어버린 경우 프레임워크는 위젯이 dirty임을 알 수 없고 위젯의 build() 함수를 호출하지 않을 수 있으며, 이는 사용자 인터페이스가 변경된 상태를 반영하여 업데이트하지 않을 수 있음을 의미합니다.
이러한 방식으로 상태를 관리하면 하위 위젯을 만들고 업데이트하기 위한 별도의 코드를 작성할 필요가 없습니다.
대신 두 가지 상황을 모두 처리하는 빌드 함수만 구현하면 됩니다.


---

참고사이트 : [Flutter](https://docs.flutter.dev/ui)

---


