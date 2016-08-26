


## Model ##


What to test
- The model’s create method, when passed valid attributes, should be valid.
- Data that fail validations should not be valid.
- Class and instance methods perform as expected.

- 유효한 attritutes를 넘겼을 때 model의 create method가 유효해야한다.
    {% highlight text %}
        item = Item.new(name: "ABC")
        expect(item).to be_valid 
    {% endhighlight %}
- 유효하지 않은 데이터는 유효성 검사를 통과하지 않아야 한다.
    {% highlight text %}
        item.valid?
        expect(item.errors[:name]).to include("can't be blank")
    {% endhighlight %}
- 클래스 매소드(+scope)와 인스턴스 매소드가 예상한것과 같이 동작한다.
    {% highlight text %}
        item_a = Item.create()
        item_b = Item.create()
        item_c = Item.create(created_at : 1.day.ago)
        expect(Contact.created_today).to eq [item_a, item_b]
    {% endhighlight %}

## FactoryGirl vs. Fixture ##
- Fixture 단점
    - Active Record를 우회 - validation check이 안됨
    - 데이터가 쉽게 변경되어서 test code만큼 test db 관리에 신경서야 된다.
- 복잡한 Assocation
    - FactoryGirl의 [callback](http://robots.thoughtbot.com/post/23039827914/get-your-callbacks-on-with-factory-girl-3-3) method를 이용.



##Controller##
What to test
- 7 CRUD methods
- non CRUD method
- Nested routes
- non-HTML output (i.e. CSV, JSON)