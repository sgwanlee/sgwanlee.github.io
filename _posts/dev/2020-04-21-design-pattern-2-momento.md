---
title: Design pattern - 2. 모멘토 패턴
layout: post
comments: true
tags: [design-pattern, java]
category: [dev]
---

<!--more-->

모멘토 패턴은 Undo 기능을 구현하는데 사용하는 패턴입니다.

```java
public class Editor {
    private String content;

    public String getContent() {
        return content;
    }

    public void setContent(String content) {
        this.content = content;
    }
}
```

Editor는 getter와 setter로 content를 변경할 수 있음.
Undo 기능을 만드려면 어떻게 해야될까?

```java
public class Editor {
    private String content;
    private String prevContent;

    public void setContent(String content) {
        prevContent = this.content;
        this.content = content;
    }
```

이렇게 구현하게 되면 한번 밖에 되돌리지 못한다는 것과 `content`외에 `title`, `description` 등의 attribute가 늘어나게 되면 `prevTitle`, `prevDescription`등 계속해서 attribute를 늘려야 된다는 담점이 있습니다.

여러번 되돌릴 수 있으려면 state list를 가지고 있어야 됩니다. 확장성 있게 attribute를 추가할 수 있도록 하려면 별도의 class 형태로 가져가는게 좋겠죠.

```java
public class Editor {
    private String content;
    private EditorState prevState;
    ...
```

```java
  public class EditorState {
    private String content;
}
```

## Single Responsibility Principle

이런 구현도 가능하지만 Editor 클래스가 하는일이 많아집니다. Single Responsibility Principle은 클래스마다 한가지만 잘하게 만들어라는건데, Editor의 state관리 기능을 별도의 `History` class로 구현해보면 이렇습니다.

```java
public class Editor {
  private String content;
  private EditorState prevState;
  public String getContent() {
      return content;
  }

  public void setContent(String content) {
      this.content = content;
  }
```

```java
public class EditorState {
    private final String content;
}
```

```java
public class History {
  private List<EditorState> state = new ArrayList();
}
```

## 구현방법

이제 어떤식으로 되돌리기를 구현하는지를 간단히 적어봅시다.

컨텐츠 업데이트

1. Editor의 컨텐츠를 업데이트한다.
2. 현재 EditorState를 생성한다.
3. History에 EditorState를 추가한다.

되될리기

1. History에서 가장 최근에 추가된 EditorState를 가져온다.
2. 가져온 EditorState는 History에서 삭제한다.
3. 가져온 EditorState로 Editor의 컨텐츠를 업데이트 한다.

## 컨텐츠 업데이트

컨텐츠 업데이트시 동작되는 코드를 최대한 실행순서에 맞춰서 적어보았습니다. 코드를 읽는다 라기 보다는 영어로 된 글을 읽는다 라는 생각으로 읽어보세요.

```java
public class Main {
  public static void main(String[] args) {
    Editor editor = new Editor();
    History history = new History();

    editor.setContent("Hi")
    history.push(editor.createState())
  }
}
```

`editor`와 `history`를 만들고, editor의 컨텐츠를 `Hi`로 업데이트합니다.

```java
public class Editor {
    private String content;
    public EditorState createState() {
        return new EditorState(content);
    }

    public void setContent(String content) {
        this.content = content;
    }
    ...
```

```java
public class History {
  private List<EditorState> states = new ArrayList();
  public void push(EditorState state) {
      states.add(state);
  }
}
```

`createState`를 통해서 현재 attributes를 이용해 `EditorState` object를 만듭니다. 그 object를 history에게 넘겨주면, history를 List의 최상단에 추가하게 됩니다.

## 되돌리기

```java
public class Main {
  public static void main(String[] args) {
    ...
    editor.restore(history.pop());
  }
}
```

```java
public class History {
  private List<EditorState> states = new ArrayList();
  public EditorState pop() {
      int lastIndex = states.size() - 1;
      EditorState lastState = states.get(lastIndex);
      states.remove(lastState);

      return lastState;
  }
}
```

```java
public class Editor {
    public void restore(EditorState state) {
        content = state.getContent();
    }
}
```

public EditorState(String content) {
this.content = content;
}

    public String getContent() {
        return content;
    }

EditorState는 Editor의 상태를 나타냅니다. constructor에서 content를 입력하고, `getContent`에서 content를 가져올 수 있
