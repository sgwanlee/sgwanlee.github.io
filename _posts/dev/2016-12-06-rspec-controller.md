---
title: Rails Controller Test with Rspec
layout: post
category: [dev, rails, test]
--- 

Controller에서 테스트 해야 할 것

- Controller의 CRUD 관련 매소드가 예상한것과 같이 동작한다.

<br>  
`Controller method를 호출 하는 방법`

    <HTTP verb> <controller method name> <params>

    get :index

    get :edit, id: 1

    get :show, id: 1

    post :create, name: 'a', brand: 'b'

    delete :destroy, id: 1

<br>
`assign(:variable)`
Controller method에서 선언된 변수에 접근하는 방법

    expect(assign(:items)).to be [item1, item2]

<br>
`response`
Controller method로 부터 반환되는 값

    expect(response).to redirect_to user_path

    expect(response).to render_template :index


<br>

## GET

1. 원하는 데이터가 instance variable에 제대로 할당되어야 한다.
2. 액션에 맞는 템플릿을 렌더링하여야 한다.

<br>

    describe'GET#show'do
        it "assigns the requested contact to @contact" do
            contact = create(:contact)
            get :show, id: contact
            expect(assigns(:contact)).to eq contact
        end
        it "renders the :show template" do
            contact = create(:contact)
            get :show, id: contact expect(response).to render_template :show
        end 
    end



---
