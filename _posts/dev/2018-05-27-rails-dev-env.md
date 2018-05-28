---
title: Rails 개발환경 설정 하기
layout: post
category: [dev, rails]
--- 


## PG 관련 에러

    Installing pg 0.17.1 with native extensions
    Gem::Ext::BuildError: ERROR: Failed to build gem native extension.

    /Users/sgwanlee/.rbenv/versions/2.2.5/bin/ruby -r ./siteconf20180527-43505-1g6vboj.rb. extconf.rb
    checking for pg_config... no
    No pg_config... trying anyway. If building fails, please try again with
     --with-pg-config=/path/to/pg_config
    checking for libpq-fe.h... no
    Can't find the 'libpq-fe.h header
    *** extconf.rb failed ***
    Could not create Makefile due to some reason, probably lack of necessary
    libraries and/or headers.  Check the mkmf.log file for more details.  You may
    need configuration options.


[solution](https://stackoverflow.com/questions/19625487/impossible-to-install-pg-gem-on-my-mac-with-mavericks)

1. [postgresql.app](https://postgresapp.com/) 사용하고
2. 아래와 같이 pg gem 설치


    gem install pg -v 0.17.1 -- --with-pg-config=/Applications/Postgres.app/Contents/Versions/latest/bin/pg_config



