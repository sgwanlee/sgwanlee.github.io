---
title: irb, rails console에서의 한글 encoding 문제 해결
layout: post
category: [dev, rails, ruby]
--- 



- 기본 encoding : UTF-8
- 한글 사용을 위해서는 `readline` 패키지 필요
- Ruby를 설치할 때 마다 readline 경로를 명시해야되나, rbenv plugin을 사용하면 자동으로 경로가 지정됨


<br>

    brew install
    git clone git://github.com/tpope/rbenv-readline.git ~/.rbenv/plugins/rbenv-readline
    rbenv install <ruby_version>



이미 설치되어 있는 경우, 해당 버전의 ruby를 삭제하고 재설치

    rbenv uninstall <ruby_version>
    rbenv install <ruby_version>



---

all credit to (https://blog.iamseapy.com/archives/177)[1]

[1]: https://blog.iamseapy.com/archives/177