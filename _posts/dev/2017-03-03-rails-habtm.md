---
title: Rails - has_and_belongs_to_many
layout: post
category: [dev, rails]
--- 



## Option

[distinct][2]
중복 assocation 생성을 금지

    class Ingredient < ActiveRecord::Base
        has_and_belongs_to_many :items, -> {distinct}

ingredient.items.size       // 1
ingredient.items << new_item
ingredient.items << new_item
ingredient.items            // 2



## Method

[collection.delete(object)][1]
- assocation관계만 제거. object가 제거되지는 않음


## [Factory_girl][3]

    FactoryGirl.define do
      factory :company do
        #company attributes
      end

      factory :user do
       companies {[FactoryGirl.create(:company)]}
       #user attributes
      end

    end


    company = FactoryGirl.create(:company, #{company attributes})
    user = FactoryGirl.create(:user, :companies => [company])

[1]:http://guides.rubyonrails.org/association_basics.html#methods-added-by-has-and-belongs-to-many-collection-delete-object
[2]: http://guides.rubyonrails.org/association_basics.html#scopes-for-has-and-belongs-to-many-distinct
[3]: http://stackoverflow.com/questions/1484374/how-to-create-has-and-belongs-to-many-associations-in-factory-girl
