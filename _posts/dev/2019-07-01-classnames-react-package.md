---
title: 리엑트 패키지 소개 - classnames
layout: post
comments: true
category: [dev, react]
---

이번에 소개할 리엑트 패키지는 classnames.
에러가 있을 때만 input box 색을 변경하는 경우처럼, 조건에 따라 css class를 추가/삭제 하고 싶을 때
아래 처럼 쓰는 경우가 있을거야.

```javascript
const className = error ? "form-control error" : "form-control";

return (<input className={className} >)
```

간단한 경우엔 괜찮은데, case가 나눠지는 경우엔 좀 보기 복잡해지는 경우가 많아.

```javascript
const getClassname = (state) => {
  let className = 'default_class_name';
  switch(state){
    case USER_STATE_A:
      className += " user_state_a";
    case USER_STATE_B:
      className += " user_state_b";
    case USER_STATE_C:
      className += " user_state_c";
    ...
  }
  return className;
};

render(
  return(<div className={getClassname(state)}/>)
)

```

classnames는 이런 복잡한걸 깔끔하게 정리해주지.

```javascript
const className = classnames(
  'default_class_name',
  {
  "user_state_a": state === USER_STATE_A,
  "user_state_b": state === USER_STATE_B,
  "user_state_c": state === USER_STATE_C
  }
);
render (
  return(<div className={className}/>)
)
```

state와 className이 같다면 좀 더 깔끔한 코드가 될것같아.

```javascript
const className = classnames( 'default_class_name', state);
render (
  return(<div className={className}/>);
)
```

아래와 같은 경우도 가능해

```javascript
const className = classnames(
  'btn',
  {[`btn-${state}`]: true}
);
render (
  return(<button className={className}>click</button>);
)
```

component에 className을 argument로 받아서 적용하는 경우도 아래처럼

```javascript
// without classnames
const Input = (className, ...props) => {
  return (
    <input
      className={className ? `default ${className}` : "default"}
      {...props}
    />
  );
};
```

```javascript
//with classnames
const Input = (className, ...props) => {
  return <input className={classnames("default", className)} {...props} />;
};
```

### classnames Github

[https://github.com/JedWatson/classnames](https://github.com/JedWatson/classnames)
