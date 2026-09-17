### 옵저버 패턴
> 옵저버 패턴(observer pattern)은 객체의 상태 변화를 관찰하는 관찰자들,
  즉 옵저버들의 목록을 객체에 등록하여 상태 변화가 있을 때마다 메서드 등을 통해 객체가 직접 목록의 각 옵저버에게 통지하도록 하는 디자인 패턴이다.
  주로 분산 이벤트 핸들링 시스템을 구현하는 데 사용된다.
  발행/구독 모델로 알려져 있기도 하다.

### 구현
> 이 패턴의 핵심은 옵저버 또는 리스너(listener)라 불리는 하나 이상의 객체를 관찰 대상이 되는 객체에 등록시킨다.
그리고 각각의 옵저버들은 관찰 대상인 객체가 발생시키는 이벤트를 받아 처리한다.

### 대표적인 사례
   -  외부에서 발생한 이벤트(사용자 입력)에 대한 응답.
   -  객체의 속성 값 변화에 따른 응답. 종족 콜백은 속성 값 변화를 처리하기 위해 호출 될 뿐 아니라 속성 값 또한 바꾼다.
> 옵저버 패턴은 모델-뷰-컨트롤러 패러다임과 자주 결합된다. 옵저버 패턴은 MVC에서 모델과 뷰 사이를 느슨히 연결하기 위해 사용된다.

### 예제
```
using System;

// 먼저 이벤트 발생에 사용할 델리게이트 형식을 선언한다.
// 이것은 System.EventHandler 형식과 같은 델리게이트이다.
// 이 델리게이트는 추상 옵저버로서의 기능을 제공한다.
// 어떠한 구현도 제공하지 않으면, 단지 규약만 제공한다.
public delegate void EventHandler(object sender, EventArgs e);

// 다음으로, 공개된 이벤트를 선언한다. 이것은 구체적인 서브젝트로서의 기능을 제공한다.
public class Button
{
	// 공개된 이벤트를 선언한다.
	public event EventHandler Clicked;

	// 관습적으로, .NET 이벤트 발생은 가상 메서드로 구현된다.
    // 이는 하위 클래스가 해당 이벤트를 재정의를 통해 사용할 수 있게 하고, 발생 여부도 조정할 수 있게 한다.
    protected virtual void OnClicked(EventArgs e)
    {
        // Clicked 이벤트에 등록된 모든 EventHandler 델리게이트를 호출한다.
        if (Clicked != null)
            Clicked(this, e);
    }
}

// 이제 옵저버 클래스에서 이벤트에 델리게이트를 등록하거나 등록 해제할 수 있다:
public class Window
{
    private Button okButton;

    public Window()
    {
        okButton = new Button();

        // Clicked 이벤트에 델리게이트를 등록하는 부분이다. 등록 해제는 -= 연산자를 사용한다.
        // 다수의 옵서버가 Clicked 이벤트에 델리게이트를 등록하는 것이 가능하다.
        okButton.Clicked += new EventHandler(okButton_Clicked);
    }

    private void okButton_Clicked(object sender, EventArgs e)
    {
        // 이 메서드는 Button 클래스에서 Clicked(this, e)를 호출할 때마다 실행된다.
    }
}