---
title: Gem - Factory_girl
layout: post
category: [dev, rails, test]
--- 



FactoryGirl Rails v4.4 기준입니다.

Fixture 쓰지 말고, FactoryGirl 쓰자.

- Fixture는 Active Record를 우회해서 validation이 안된다.
- 데이터가 쉽게 변경되어서 test code만큼 test db 관리에 신경써야 된다.


### Association

association을 만들 때, `create_list`를 사용한다.
`create`와 같이 overwrite할 인자들을 넘겨줄 수 있다.
 
    item1 = create(:item_with_keywords, sub_menu: @sub_menu, nmc: 1)

associated model에 넘길 인자를 받을 때는 `ignore`로 설정해준다.

    factory :item do
      sequence(:name) { |n| "Item_#{n}"}
      brand "Super"
      sub_menu
      factory :item_with_keywords do
        //associated model인 keyword에 넘기는 parameters
        ignore do
          keywords_count 1
          nmc 0
        end
        after(:create) do |item, evaluator|
          create_list(:keyword, evaluator.keywords_count, {item: item, nmc: evaluator.nmc})
        end
      end
    end

    factory :keyword do
        sequence(:name) { |n| "Keyword_#{n}"}

        # keyword factory에 `item`을 적어주지 않으면 `items` method가 생성되지 않는다.
        item 
        
        ignore do
          naver_search_ad_logs_count {1}
          nmc 0
        end
        after(:create) do |keyword, evaluator|
          create_list(:naver_search_ad_log, evaluator.naver_search_ad_logs_count, {keyword: keyword, nmc: evaluator.nmc})
        end
      end


keyword에 `item`을 적어준다.
  `create(:keyword)` 를 실행할 때 item도 생성된다.


### 내가 겪은 Trouble

`has_secure_password` 를 사용했을 때, factory_girl로 만든 `user`로 login하는데 문제가 있었다. 원인은 아직모르겠으나, factory를 사용하지 않고, ActiveRecord를 직접 생성하는 것으로 우회했다.

==> 안된 예
factory :user do

  factory :admin do
  end
end

==> success
factory :user do
end

factory :admin, class: User do
end


- model과 다른 이름의 factory를 만들 경우
{% highlight ruby %}
{% endhighlight %}
https://github.com/thoughtbot/factory_girl/blob/master/GETTING_STARTED.md#defining-factories

cheat sheet
http://ricostacruz.com/cheatsheets/factory_girl.html