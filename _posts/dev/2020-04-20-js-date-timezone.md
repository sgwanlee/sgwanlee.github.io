---
title: Javascript에서 timezone 다루기
layout: post
comments: true
tags: [timezone, js]
category: [dev]
---

<!--more-->

사용자의 생일 정보를 받아와서 KYC(Know Your Customer) 인증을 거치게 하는 과정에서 몇몇 사용자들이 계속 자기는 제대로 넣었는데, 저장하고 나면 날짜가 하루 다르게 들어간다는 CS가 조금씩 늘어났다.

서비스가 아시아권을 벗어나서 다른 국가로 넘어가면서 발생하기 시작했는데, 오늘 자세히 들여다보니 timezone 문제였다.

서버에서는 'YYYY-MM-DD' 와 같이 iso format으로 생일정보를 내려받는데, 프론트단에서 `Date` object로 변환해서 사용자에게 보여주게 된다.

아래처럼 iso format이냐 아니냐에 따라서 기준 timezone이 변경되는걸 몰라서, 하루 날짜 차이가 발생하게 되었는데

```
new Date('1999-6-13');
Sun Jun 13 1999 00:00:00 GMT-0700 (북미 태평양 하계 표준시)
new Date('1999-06-13');
Sat Jun 12 1999 17:00:00 GMT-0700 (북미 태평양 하계 표준시)
```

부랴부랴 mdn을 찾아보니 date

[From mdn](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Date/parse)

ISO-8601

> "March 7, 2014"이라는 날짜 문자열이 주어지면 parse ()는 현지 시간대를 사용하지만 "2014-03-07"과 같은 ISO 형식이 주어지면 UTC (ES5 및 ECMAScript 2015)의 시간대로 가정합니다. 따라서 Date 이러한 문자열을 사용하여 생성 된 객체는 시스템이 현지 표준 시간대 (UTC)로 설정되어 있지 않으면 지원되는 ECMAScript 버전에 따라 다른 순간을 나타낼 수 있습니다. 즉, 변환되는 문자열의 형식에 따라 동등하게 나타나는 두 개의 날짜 문자열이 서로 다른 두 개의 값을 가질 수 있습니다.
