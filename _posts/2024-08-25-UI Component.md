---
layout: post
title: UI Component
subtitle: SwiftUI가 기본적으로 제공하는 UI Component대해 알아보도록 하자.
author: 박현
categories: SwiftUI
tags: [Swift, UI프레임워크, SwiftUI, AppleWatch]
---

오늘은 SwiftUI가 기본적으로 제공하는 컴포넌트에 대해서 알아보도록 하자.

<br>

## Text
Text는 텍스트를 표시하는 데 사용되는 기본 컴포넌트이다.
폰트, 스타일, 정렬과 같은 다양한 옵션이 있다.

아래의 코드는 
텍스트 라인 수 제한하는것과 텍스트색상과 배경색변경하는 예제이다.

```swift

import SwiftUI

struct BlogView: View {
    
    var body: some View {
        VStack {
            Text("Hello~~") //일반 텍스트
            
            Spacer()
            
            Text("안녕하세요 오늘은 8월25일입니다. 요즘 날씨가 너무 더워요  얼른 여름이 끝나고 겨울이 왔으면 좋겠네용ㅎㅎㅎㅎ")
              .lineLimit(2)
            // 줄의 개수를 제한.
            
            Spacer();
            
            Text("박현")
                .foregroundColor(Color.orange)
                //텍스트 색상변경
            
            
            Spacer();
            
            Text("배경색 변신")
                .background(Color.green)
                //텍스트 배경색 변경
        }
    }
}

struct BlogView_Previews: PreviewProvider {
    static var previews: some View {
        BlogView()
    }
}
```

![Text](/assets/images/posts/박현/20240825/Text.png)



## Button
버튼은 사용자와 상호작용하기 위해 사용되는 컴포넌트이다. 
사용자가 버튼을 클릭하면 특정 동작이 실행되도록 이벤트를 만들수있다.
버튼에 스타일을 줘서 예쁘게 꾸밀수도있고 애니메이션과 같은 효과도 추가할 수 있다.

```swift

import SwiftUI

struct BlogView: View {
    @State var onText = false
    
    var body: some View {
        VStack {

             Button("일반버튼") {
               print("버튼클릭")
             }
             

             Button {
               print("left 버튼클릭")
             } label: {
               Image(systemName: "chevron.left")
             }
             
             Toggle("Toggle", isOn: $onText)
                   .padding()
                   .background(.orange)
                   .cornerRadius(15)
                 
             if onText {
                 Text("on");
             }else{
                 Text("off")
             }
         
           }
    }
}

struct BlogView_Previews: PreviewProvider {
    static var previews: some View {
        BlogView()
    }
}

```
위의 코드를 실행하게 되면 아래와 같은 사진 처럼 3개의 버튼이 만들어지게 된다.

![Button](/assets/images/posts/박현/20240825/button.png)


첫번째 버튼은 일반적으로 사용하는 버튼이다. 버튼을 클릭하게 되면 이벤트로 설정해두었던 "버튼클릭"이라는 텍스트가 출력될것이다.

두번째 버튼은 커스텀한 뷰를 버튼으로 만들고싶을때 사용하는 방법이다. 간단하게 Label를 사용하여 현재 Image를 사용한 부분에 커스텀한 뷰를 넣으면 완성된다.

세번째버튼은 토글버튼이다. 
첫번째 인자값은 토글텍스트값 

두번째 인자값은 isOn이라는 상태값을 넣어줘야한다.(Toggle("",isOn))
onText의 값은 토글 스위치를 클릭할때마다 상태값이 변경된다.


## List
List는 목록(List) 인터페이스를 구현한다. 물론 계층구조의 리스트도 구현할수있다고한다.

List별로 섹션을 나눌수도있고 텍스트와 이미지와 함께 한행에 출력되게 할 수도 있다.

.listStyle()를 사용하여 리스트에 스타일을 줄 수도있다.



```swift

import SwiftUI

struct item: Identifiable {
    let name: String
    let id = UUID() //식별값
}

private var items = [
    item(name: "롯데자이언츠"),
    item(name: "삼성라이온즈"),
    item(name: "기아타이거즈"),
    item(name: "한화이글스"),
    item(name: "엘지트윈스"),
    item(name: "키움히어로즈")
]


struct BlogView: View {
   
    var body: some View {
       
        List{
            Text("박현");
            Text("박현2");
            Text("박현3");
        }.listStyle()//리스트 스타일
        
        
        List(items) {
                        Text($0.name)
                    }.refreshable { //리스트재조회
           
                    }
    }
}

struct BlogView_Previews: PreviewProvider {
    static var previews: some View {
        BlogView()
    }
}

```

![List](/assets/images/posts/박현/20240825/List.png)

## TextField
TextField는 사용자가 텍스트를 입력할때 사용하는 컴포넌트이다.
첫번째 인자 값으로 placeholder 텍스트(placeholer를 주고싶지않아도 빈값이라도 보내야한다.)

두번째 인자 값으로 @State로 선언된 변수를 넣으면 이 변수에 텍스트가 입력된다.

비밀번호와 같은 중요한 데이터는 입력되는 값을 보호해주는 Secure Textfield 컴포넌트를 사용하여 입력하여야한다.

```swift

import SwiftUI

struct BlogView: View {
    @State var id: String = ""
    @State var password: String=""
    
    var body: some View {
       
        VStack{
            TextField("아이디을 입력하세요.", text: $id)
                .textFieldStyle(.plain)
            
            SecureField("비밀번호를 입력하세요", text: $password)
                
        }
    
    }
}

struct BlogView_Previews: PreviewProvider {
    static var previews: some View {
        BlogView()
    }
}

```

![TextField](/assets/images/posts/박현/20240825/TextField.png)


## Picker
Picker는 여러 옵션 중에서 하나를 선택할 수 있는 컴포넌트이다.
크게 wheel, menu(드롭다운형식), radioGroup, segmented, ColorPicker,  DatePicker 으로 PickerStyle이 나누어져있다.

segment로 할경우엔 텍스트와 이미지로만 사용할수있다고한다. 다른뷰를 사용하게되면 빈 세그먼트가 나타난다고...

radioGroup은 Mac에서만 제공한다.

각각 스타일별로 지원하고있는 os버전이 다르니 확인 후 개발하여야 한다. 
ColorPicker를 사용해볼려고했더니 워치os와 맞지않다고 사용못했다....


```swift

import SwiftUI

struct BlogView: View {
    var teams = ["롯데", "엘지", "기아"]
      @State var SelTeam = ""
      
      var body: some View {
        VStack {
          Picker("최애 팀을 고르세요", selection: $SelTeam) {
            ForEach(teams, id: \.self) {
              Text($0)
            }
          }
          .pickerStyle(.wheel)
          .cornerRadius(10)
     
          
          Text("\(SelTeam) 선택")
        }
      }
}

struct BlogView_Previews: PreviewProvider {
    static var previews: some View {
        BlogView()
    }
}

```

![Picker](/assets/images/posts/박현/20240825/Picker.png)

![DatePicker](/assets/images/posts/박현/20240825/DatePicker.png)


Picker를 사용해본 결과 기본적으로 제공하는 Picker는 스타일 조절이 어렵다....

----

이글은 SwiftUI 공식문서를 참고하여 작성하였습니다.