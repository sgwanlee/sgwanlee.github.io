---
title: Turbolinks & Google analytics
layout: post
category: [dev, rails]
---

- turbolink는 `body`만 바꿔서 page loading time을 줄여준다.
- 전체 page를 바꾸는게 아니기 때문에, google anlalytics script를 넣어주는 것 만으로는 GA가 제대로 동작하지 않는다.
- `application.html.erb`
   {% highlight ruby %}
      <head>
          <%= render 'layouts/google_analytics' %>
      </head>
    {% endhighlight %}
- `laytouts/_google_analytics.html.erb`
    {% highlight ruby %}
      <% if Rails.env.production? %>
        <script>
          (function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
          (i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
          m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
          })(window,document,'script','https://www.google-analytics.com/analytics.js','ga');

          ga('create', 'XX-XXXXXXXX-X', 'auto');
          ga('send', 'pageview');
        </script>
      <% else %>
        <script>
          function ga () {
            var params = Array.prototype.slice.call(arguments, ga.length);
            console.log("GoogleAnalytics: " + params);
          };
        </script>
      <% end %>

    {% endhighlight %}
- `app/assets/javascripts/google_analytics.js.coffee`
  {% highlight ruby %}
    document.addEventListener "turbolinks:load", (event) ->
      if typeof ga is "function"
      ga("set", "location", event.data.url)
      ga("send", "pageview")
  {% endhighlight %}


[credit: sheharyar](https://sheharyar.me/blog/using-google-analytics-in-rails-4-with-turbolinks/)
[credit: scottwb](http://stackoverflow.com/questions/18945464/rails-4-turbolinks-with-google-analytics/25050377#25050377)