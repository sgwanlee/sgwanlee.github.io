---
layout: post
title: EXIF 옵션에 따라 이미지 회전시키기
---

{% highlight ruby %}
def auto_orient
  manipulate! do |image|
    image.tap(&:auto_orient)
  end
end
{% endhighlight %}