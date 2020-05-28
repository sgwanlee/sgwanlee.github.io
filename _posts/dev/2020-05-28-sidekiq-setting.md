---
title: AWS Elastic Beanstalk 그리고 Sidekiq
layout: post
comments: true
category: [dev, rails]
---

AWS Elastic beanstalk에서 Sidekiq을 설정하는 방법

<!--more-->

# 프로젝트 설정

AWS Elastic Beanstalk에서는 ruby 2.3을 지원합니다. Sidekiq 6.0부터는 ruby 버전 2.6.3이 필요하기 때문에 Elastic Beanstalk에서 sidekiq은 버전 5를 사용할 수 있습니다.

### Gem 설치

```ruby
gem "sidekiq"
```

```sh
> bundle install
```

### 설정

`sidekiq.yml` 파일을 통해서 sidekiq의 설정을 할 수 있습니다. 이 파일은 나중에 sidekiq을 실행시킬 때 설정파일로 사용됩니다.

`/config/sidekiq.yml`

```yaml
:concurrency: 5
staging:
  :concurrency: 10
production:
  :concurrency: 20
:queues:
  - critical
  - default
  - low
```

### Redis 연결

Sidekiq은 work queue를 redis를 이용해서 관리합니다. intializer를 추가해서 sidekiq의 redis 설정을 진행합니다.

`/config/initializer/sidekiq.rb`

```ruby
Sidekiq.configure_server do |config|
  if Rails.env == 'development'
    config.redis = { url: "redis://localhost:6379/1"}
  else
    config.redis = { url: ENV['REDIS_URL'] || "redis://127.0.0.1:6379/1" }
  end
end

Sidekiq.configure_client do |config|
  if Rails.env == 'development'
    config.redis = { url: "redis://localhost:6379/1"}
  else
    config.redis = { url: ENV['REDIS_URL'] || "redis://127.0.0.1:6379/1" }
  end
end
```

### Dashboard 라우팅 추가

Dashboard를 `route.rb`에 추가합니다.

```ruby
require 'sidekiq/web'

Rails.application.routes.draw do
  mount Sidekiq::Web => '/qiksidekiq'
  ...
end
```

# Elastic Benastalk 설정

sidekiq은 `bundle exec sidekiq`으로 실행시켜야 됩니다. Elastic Beanstalk에는 hook이란게 있습니다. 특정 디렉토리에 파일을 추가하면 특정 시점에 그 디렉토리 안의 파일을 실행시키게 됩니다.

아래 script는 `/opt/elasticbeanstalk/hooks/appdeploy/post/50_restart_sidekiq.sh` 파일과 `/opt/elasticbeanstalk/hooks/appdeploy/pre/03_mute_sidekiq.sh` 을 생성합니다.

`appdeploy` 폴더에 있는 파일들은 Elastic beanstalk에서 새로운 application을 띄울때 실행이 되며 `pre` 폴더에 담긴 `03_mute_sidekiq.sh`가 먼저 실행되서, 기존에 실행중인 sidekiq 프로세스를 제거하고, `post` 폴더에 담긴 `50_restart_sidekiq.sh`에서 다시 sidekiq을 실행하게 됩니다.

```
commands:
  create_post_dir:
    command: "mkdir -p /opt/elasticbeanstalk/hooks/appdeploy/post"
    ignoreErrors: true

files:
  "/opt/elasticbeanstalk/hooks/appdeploy/post/50_restart_sidekiq.sh":
    mode: "000755"
    owner: root
    group: root
    content: |
      #!/usr/bin/env bash
      . /opt/elasticbeanstalk/support/envvars
      EB_APP_DEPLOY_DIR=$(/opt/elasticbeanstalk/bin/get-config container -k app_deploy_dir)
      EB_APP_PID_DIR=$(/opt/elasticbeanstalk/bin/get-config container -k app_pid_dir)
      EB_APP_USER=$(/opt/elasticbeanstalk/bin/get-config container -k app_user)
      EB_SCRIPT_DIR=$(/opt/elasticbeanstalk/bin/get-config container -k script_dir)
      EB_SUPPORT_DIR=$(/opt/elasticbeanstalk/bin/get-config container -k support_dir)
      . $EB_SUPPORT_DIR/envvars
      . $EB_SCRIPT_DIR/use-app-ruby.sh
      SIDEKIQ_PID=$EB_APP_PID_DIR/sidekiq.pid
      SIDEKIQ_CONFIG=$EB_APP_DEPLOY_DIR/config/sidekiq.yml
      SIDEKIQ_LOG=$EB_APP_DEPLOY_DIR/log/sidekiq.log
      cd $EB_APP_DEPLOY_DIR
      if [ -f $SIDEKIQ_PID ]
      then
      su -s /bin/bash -c "kill -TERM `cat $SIDEKIQ_PID`" $EB_APP_USER
      su -s /bin/bash -c "rm -rf $SIDEKIQ_PID" $EB_APP_USER
      fi
      . /opt/elasticbeanstalk/support/envvars.d/sysenv
      sleep 10
      su -s /bin/bash -c "bundle exec sidekiq \
      -e $RACK_ENV \
      -P $SIDEKIQ_PID \
      -C $SIDEKIQ_CONFIG \
      -L $SIDEKIQ_LOG \
      -d" $EB_APP_USER
  "/opt/elasticbeanstalk/hooks/appdeploy/pre/03_mute_sidekiq.sh":
    mode: "000755"
    owner: root
    group: root
    content: |
      #!/usr/bin/env bash
      . /opt/elasticbeanstalk/support/envvars
      EB_APP_USER=$(/opt/elasticbeanstalk/bin/get-config container -k app_user)
      EB_SCRIPT_DIR=$(/opt/elasticbeanstalk/bin/get-config container -k script_dir)
      EB_SUPPORT_DIR=$(/opt/elasticbeanstalk/bin/get-config container -k support_dir)
      . $EB_SUPPORT_DIR/envvars
      . $EB_SCRIPT_DIR/use-app-ruby.sh
      SIDEKIQ_PID=$EB_APP_PID_DIR/sidekiq.pid
      if [ -f $SIDEKIQ_PID ]
      then
      su -s /bin/bash -c "kill -USR1 `cat $SIDEKIQ_PID`" $EB_APP_USER
      fi
```

이 파일을 프로젝트의 root에 `.ebextensions` 폴더를 만들고, 그 폴더안에 `01_sidekiq.config`라는 파일에 추가합니다.

`.ebextensions`의 설정파일들을 배포시에 실행되며, 파일명 순으로 실행되기에 파일명 앞에 숫자로 실행순서를 나타내는게 좋습니다.

https://github.com/mperham/sidekiq
https://openclassrooms.com/en/courses/4567631-deploy-rails-applications/4794886-configure-elastic-beanstalk-to-run-sidekiq
