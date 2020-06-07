---
title: Javascript에서 timezone 다루기
layout: post
comments: true
tags: [timezone, js]
category: [dev]
---

<!--more-->

오늘은 Javascript Timezone에 대한 얘기입니다.

서버에서는 'YYYY-MM-DD' 와 같이 iso format으로 생일정보를 내려받는데, 프론트단에서 `Date` object로 변환해서 사용자에게 보여주는 케이스였습니다.

주의해야 할 점은 아래처럼 iso format이냐 아니냐에 따라서 기준 timezone이 변경되는걸 몰라서, 하루 날짜 차이가 발생하게됩니다.

```
new Date('1999-6-13');
Sun Jun 13 1999 00:00:00 GMT-0700 (북미 태평양 하계 표준시)
new Date('1999-06-13');
Sat Jun 12 1999 17:00:00 GMT-0700 (북미 태평양 하계 표준시)
```

날짜를 로컬 타임에 맞게 보여주는건 문제가 안되는데, 문제가 될 수 있는건 프로필 수정 등의 기능에서 form data로 미리 string형태로 넣어주게 되면 문제가 발생합니다.

사용자가 생각한 생일 정보 : 1999-06-13
브라우저에 표기되는 로컬 타임존을 반영한 생일 정보 : 1999-06-12

이렇게 로컬 타임존으로 인해서 변경된 것을 사용자가 인지하지 못하고 다시 서버로 put request를 보내게 되면 서버에 있는 정보가 하루 전으로 수정됩니다.

이런식으로 몇번을 반복하게 되면 사용자 생일 정보가 계속 하루씩 빨라지게 되죠.

부랴부랴 mdn을 찾아보니 date

[From mdn](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Date/parse)

ISO-8601

> "March 7, 2014"이라는 날짜 문자열이 주어지면 parse ()는 현지 시간대를 사용하지만 "2014-03-07"과 같은 ISO 형식이 주어지면 UTC (ES5 및 ECMAScript 2015)의 시간대로 가정합니다. 따라서 Date 이러한 문자열을 사용하여 생성 된 객체는 시스템이 현지 표준 시간대 (UTC)로 설정되어 있지 않으면 지원되는 ECMAScript 버전에 따라 다른 순간을 나타낼 수 있습니다. 즉, 변환되는 문자열의 형식에 따라 동등하게 나타나는 두 개의 날짜 문자열이 서로 다른 두 개의 값을 가질 수 있습니다.
