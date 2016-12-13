---
title: Rspec 에서 referrer 설정하기
layout: post
category: [dev, rails, rspec]
--- 

referrer 설정하는 법

1) Mocking

    allow(controller.request).to receive(:referrer).and_return("http://example.com")


---
