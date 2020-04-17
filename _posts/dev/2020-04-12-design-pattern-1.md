---
title: Design pattern - 기초
layout: post
comments: true
tags: [design-pattern, java]
category: [dev]
---

Encapsulation

<!--more-->

# Encapsulation

아래는 Main class에서 balance를 변경할 수 있는 상황. balanace를 0보다 작은 값을 갖지 못하게 하려면 어떻게 해야 될까? `Main` 에서 balance값을 체크하게 되면 Account를 사용하는 모든 class에서 이 로직이 반복되게 된다.

```java
//Account.java
public class Account {
  public float balance;
}

//main.java
public class Main {
  public static void main(String[] args) {
    var account = new Account();
    account.balance = -1;
  }
}
```

아래와 같이 `Account` class 외부에서 `balance` 값을 바꾸지 못하게 변경하고, method를 이용해서 값을 가져오고, 쓰도록 만들 수 있다.

```java
//Account.java
public class Account {
  private float balance;

  public float getBalance() {
    return balance;
  }

  public void setBalance(balance) {
    this.balance = balance
  }
}
```

그리고 `setBalance`에 우리가 원하는 validation logic을 추가하면 balance가 0보다 작은 경우에 값 쓰기를 막을 수 있다.

```java
//Account.java
public class Account {
  private float balance;

  public float getBalance() {
    return balance;
  }

  public void setBalance(balance) {
    if(balance > 0) {
      this.balance = balance
    }
  }
}
```

`setBalance`를 우리에게 조금더 익숙한 입금/출금 함수로 변경.

```java
//Account.java
public class Account {
  private float balance;

  public float getBalance() {
    return balance;
  }

  public void deposit(balance) {
    if(balance > 0) {
      this.balance += balance
    }
  }

  public void withdraw(balance) {
    if(balance > 0) {
      this.balance -= balance
    }
  }
}
```
