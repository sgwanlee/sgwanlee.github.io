---
title: Chrome에서 Timezone 변경하는 방법
layout: post
comments: true
tags: [timezone, js]
category: [dev]
---

<!--more-->

이 방법은 mac에서만 사용가능합니다.

우선 적당한 위치에 `chrome-profile` 폴더를 만듭니다.

```
mkdir ~/chrome-profile
```

아래와 같이 원하는 timezone을 설정한 뒤에 chrome을 실행합니다. `--user-data-dir` 에 앞서 만든 chrome-profile 위치를 넣어주면 됩니ㅏㄷ.

```
TZ='US/Pacific' open -na "Google Chrome" --args "--user-data-dir=$HOME/chrome-profile"
```

![chrome-timezone](/public/2020-04-20-chrome-tz.png)
