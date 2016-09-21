---
title: tip - nil이 아닌경우에 method를 실행
layout: post
category: [dev, ruby]
--- 

object가 nil이 아닌 경우에만 method를 실행하고 싶을 때가 있다.

    self.send(:method) if self.respond_to?(:method)


---

all credit : [http://stackoverflow.com/questions/15787610/call-method-only-if-it-exists](http://stackoverflow.com/questions/15787610/call-method-only-if-it-exists)