---
title: Design pattern - 3. State 패턴
layout: post
comments: true
tags: [design-pattern, java]
category: [dev]
---

<!--more-->

포토샵의 여러개의 툴을 생각해보자. 연필을 선택하면 연필로 그려져야되고, 지우개를 선택하면 지워져야 된다. 이처럼 어떤 툴을 선택했느냐에 따라서 같은 사용자 행동이 다른 결과를 만들어내야될 때가 있다.

```java
  public void mouseUp() {
    if(type == "erase") {
      ...
    } else if (type == "pencil") {
      ...
    }
  }
```

이렇게 타입별로 if/else 형태로 나눠서 구현을 할 수도 있다. 그런데 새로운 툴을 추가하려면 다시 else if를 추가해야 되고, `mouseUp`과 같은 매소드가 여러개 있다면 일일이 다 고쳐야 될거다.

앞에서 배웠던 polymorphism을 이용해서, 확장성있는 형태로 바꿔보자.

우선 툴타입을 정의해두자.

```java
// ToolType.jsva
public enum ToolType {
    SELECTION,
    BRUSH,
    ERAGER,
}
```

`Tool` absctract class를 만들자. interface로도 구현할 수 있다.

```java
public class abstract Tool {
  public void abstract mouseUp()
  public void abstract mouseDown()
}
```

```java
public interface Tool {
  void mouseUp()
  void mouseDown()
}
```

이제 새로운 도구가 추가될때는, 이 `Tool` interface를 구현한 class를 정의해주는 형태로 추가해 나갈 수 있다.

```java
public class BrushTool implements Tool {
    @Override
    public void mouseUp() {
        System.out.println("Brush icon");
    }

    @Override
    public void mouseDown() {
        System.out.println("Draw a line");
    }
}
```

```java
public class EraserTool implements Tool {
    @Override
    public void mouseUp() {
        System.out.println("Eraser icon");
    }

    @Override
    public void mouseDown() {
        System.out.println("Erase");
    }
}

```

`BrushTool`과 `EraserTool`이 있는 상태에서 새로운 도구, `SelectionTool`을 추가하려면 어떻게 해야 될까? 앞서 말했듯이 새롭게 class를 정의하고, `Tool` interface의 `mouseUp`과 `mouseDown`을 구현하면 된다.

```java
public class SelectionTool implements Tool {
    @Override
    public void mouseUp() {
        System.out.println("Selection icon");
    }

    @Override
    public void mouseDown() {
        System.out.println("Draw dashed rectangle");
    }
}
```

자 이제 이 도구를 사용하는 `Canvas` 클래스를 만들어 보자.

```java
public class Canvas {
    private Tool currentTool;

    public void mouseDown() {
        // delegating
        currentTool.mouseDown();
    }

    public void mouseUp() {
        currentTool.mouseUp();
    }

    public Tool getCurrentTool() {
        return currentTool;
    }

    public void setCurrentTool(Tool currentTool) {
        this.currentTool = currentTool;
    }
}
```

`Cavas` 클래스는, 현재 선택된 도구의 `state`를 `currentTool` attribute로 가지고 있게 된다. state패턴은 이 state가 변경될 때 마다 다른 행동을 만들수 있게 해준다.

```java
//Main.java
  public static void main(String[] args) {
    Canvas canvas = new Canvas();
    canvas.setCurrentTool(new EraserTool());
    canvas.mouseDown();
    canvas.mouseUp();
  }
```

`EraserTool`을 선택하고 나면, 이제부터 `mouseDown`, `mouseUp`은 `EraserTool` 클래스에 정의된 함수가 실행되게 된다.

`BrushTool`로 변경하고 싶으면 아래와 같이 `currentTool` state를 변경해주면 된다.

```java
canvas.setCurrentTool(new BrushTool())
```
