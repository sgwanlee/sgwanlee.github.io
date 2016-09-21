---
layout: post
title: 빠른 테스트 - Guard & Zeus
category: [dev, rails]
---

rails app을 pre-loading해서 빠르게 실행시켜주는 [zeus][zeus]를 사용하면 test에 걸리는 시간을 줄일 수 있다. 

[Guard][Guard]는 spec 파일이 변경되는 것을 인지해서, 변경된 spec 파일의 test suits만 실행시켜준다.

Gemfile

    group :test do
      gem 'guard-rspec'
    end

Guard 초기화

    bundle install
    bundle exec guard init

Zeus 설치 & 시작

    gem install zeus
    zeus start

Guardfile

{% highlight ruby %}
guard :rspec, cmd: "zeus rspec" do
  require "guard/rspec/dsl"
  dsl = Guard::RSpec::Dsl.new(self)

  # Feel free to open issues for suggestions and improvements

  # RSpec files
  rspec = dsl.rspec
  watch(rspec.spec_helper) { rspec.spec_dir }
  watch(rspec.spec_support) { rspec.spec_dir }
  watch(rspec.spec_files)

  # Ruby files
  ruby = dsl.ruby
  dsl.watch_spec_files_for(ruby.lib_files)

  # Rails files
  rails = dsl.rails(view_extensions: %w(erb haml slim))
  dsl.watch_spec_files_for(rails.app_files)
  dsl.watch_spec_files_for(rails.views)

  watch(rails.controllers) do |m|
    [
      rspec.spec.call("routing/#{m[1]}_routing"),
      rspec.spec.call("controllers/#{m[1]}_controller"),
      rspec.spec.call("acceptance/#{m[1]}")
    ]
  end

  # Rails config changes
  watch(rails.spec_helper)     { rspec.spec_dir }
  watch(rails.routes)          { "#{rspec.spec_dir}/routing" }
  watch(rails.app_controller)  { "#{rspec.spec_dir}/controllers" }

  # Capybara features specs
  watch(rails.view_dirs)     { |m| rspec.spec.call("features/#{m[1]}") }
  watch(rails.layouts)       { |m| rspec.spec.call("features/#{m[1]}") }

  # Turnip features and steps
  watch(%r{^spec/acceptance/(.+)\.feature$})
  watch(%r{^spec/acceptance/steps/(.+)_steps\.rb$}) do |m|
    Dir[File.join("**/#{m[1]}.feature")][0] || "spec/acceptance"
  end
end
{% endhighlight %}

Guard 시작

    guard


----
참고링크

Rails tutorial: [https://www.railstutorial.org/book/_single-page#sec-guard](https://www.railstutorial.org/book/_single-page#sec-guard)

RailsCasts : [http://railscasts.com/episodes/413-fast-tests](http://railscasts.com/episodes/413-fast-tests)

[zeus]: https://github.com/burke/zeus
[Guard]: https://github.com/guard/guard