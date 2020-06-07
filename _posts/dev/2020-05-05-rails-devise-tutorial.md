---
title: Rails Devise
layout: post
comments: true
tags: [rails]
category: [dev, rails]
---

<!--more-->

```
bundle add devise
rails generate devise:install
```

여러가지 가이드가 나오는데,

```
  1. Ensure you have defined default url options in your environments files. Here
     is an example of default_url_options appropriate for a development environment
     in config/environments/development.rb:

       config.action_mailer.default_url_options = { host: 'localhost', port: 3000 }

     In production, :host should be set to the actual host of your application.
```

우선은 mail 세팅

각 환경별로 설정해줘야 됩니다.
`config/environments/development.rb`

```ruby
config.action_mailer.default_url_options = { host: 'localhost', port: 3000 }
```

```
  2. Ensure you have defined root_url to *something* in your config/routes.rb.
     For example:

       root to: "home#index"
```

```
rails generate devise User
```

```
      invoke  active_record
      create    db/migrate/20200505042738_devise_create_users.rb
      create    app/models/user.rb
      invoke    test_unit
      create      test/models/user_test.rb
      create      test/fixtures/users.yml
      insert    app/models/user.rb
       route  devise_for :users
```

라우터에 추가된 부분

```
Rails.application.routes.draw do
  devise_for :users
```
