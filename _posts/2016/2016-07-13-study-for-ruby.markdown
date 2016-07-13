---
layout: "post"
title: "study for ruby"
description: "루비를 배워보자 - 루비 이야기"
date: "2016-07-13 22:10"
tags: [ruby,programming,language]
comments: true
---

### Ruby를 공부해보자

Ruby 기초

Ruby언어는 객체지향을 완벽하게 지원하는 언어이자 `Meta Programming`을 지원하는 언어이다. 그리고 동적타이핑을 사용하며, 루비의 자료형 검사는 **실행시간**에 수행된다는 특징을 가진다. 어떤 코드를 실행하기 전까지는 **형 검사**를 수행하지 않는다.

#### 변수

```ruby
x=4
p false.class
#ruby에서는 거의 대부분이 객체로 처리됨
puts 'This is Ruby' unless x == 4
puts 'This is Ruby' if x == 4
```

루비는 _인터프리터 언어_이기에 '그냥' 할당하면 된다. Type은 알아서 매겨준다.


#### 출력

```ruby
puts 'hell, world'
language= 'ruby'
puts "hello, #{language}"
```

`'`따옴표의 경우 문자열 그대로 의미하지만, `"`따옴표의 경우 문자열을 해석한다.


#### .rb 파일 실행

아래처럼 `ruby [실행 파일 명]` 물론 확장자는 `.rb`여야 **ruby**가 인식한다.

```bash
$ ruby duck_Typing.rb
```

#### 함수 정의

모든 함수는 어떤 값을 리턴하게 되는데, 리턴 값을 명시하지 않으면, 함수 안에 존재하는 **마지막 표현**이 나타내는 값을 리턴한다.(자동으로…)

```ruby
def tell_the_truth
  puts 'true'
end
```

#### 배열

배열은 다른 언어와 비슷하고, `[]` 를 이용해 생성하고, `-1`과 같은 첨자도 사용 할 수 있다.

```ruby
animals= ['lions', 'tigers', 'bears']
puts animals
puts animals[0]
puts animals[-1]
puts animals[0..1]
puts (0..1).class
#'[]'는 array 클래스의 메소드 이름이다.
a = [[1,2,3],['a','b','c'],['ser','ss',10]]
puts a[0][0]
puts a.push(1)
puts a.pop
```

#### Hash

쉽게 말해 `연관 배열`이라고 말 할 수 있는데, 배열은 정수만 `인덱스`로 사용 되지만, 해쉬의 경우에는 Key-Value 쌍이기에 `Key`를 _사용자가 정의_ 할 수 있다.

```ruby
#hash is key - value pair
numbers = {1 =>'one',2 =>'two'}
puts numbers[1]
test = {:array =>[1,2,3], :string => 'strinng'}
puts test[:array]
# :symbol 콜론 뒤에 따라오는 식별자를 의미함.
#사물이나 생각에 이름을 붙일때 유용하다.
def tell_the_truth(options={})
  if options[:profession] == :lawyer
    'it could be believed that this is almost certainly not false'
  else
    true
  end
end
tell_the_truth
tell_the_truth(:profession => :lawyer)
```

#### 코드 블록(Code Block)

루비에서 **가장 강력한 기능중에 하나**인데, 이름이 없는 함수라고 할 수 있다. 그리고 이 것은 어떤 함수나 메서드에 **매개변수**로 전달 할 수 있다. 중괄호 사이에 있는 부분은 '코드 블록'이라고 한다.

```ruby
3.times {puts 'times times times'}
# {} 중괄호로 표시된 것이 바로 코드 블록이다.
# 코드 블록은 이름이 없는 함수이며, 이 것을 함수나 메서드에 매개변수로 전달 할 수 있다.


# 결과


times times times
times times times
times times times
```

`{}` 혹은 `do/end`를 이용해서 코드 블록을 만들 수 있다.

```ruby
animals = ['lions and ', 'tigers and', 'bears' , 'oh my']
animals.each {|a| puts a}


# 결과


lions and
tigers and
bears
oh my
```

코드 블록은 하나 혹은 그 이상의 매개변수를 받아들일 수 있다. `number.times`는 Fixnum에 정의된 메서드로 _무언가 주어진 내용을 number의 횟수만큼_ 반복한다. 이 것을 실제로 구현해보자면 아래와 같다.

```ruby
class Fixnum
  def my_times
    i = self
    while i> 0
      i = i - 1
      yield
    end
  end
end
3.my_times {puts 'define times'}
```

#### 믹스인 Mixin

쉽게 설명하자면, 단일 상속만 가능한 부분에서는 구현 하기 어려운 점이 Class에서 `각 장점만 가져오는 방법`은 불가능하다. 즉, Java언어에서는 두 클래스의 장점을 가져오는 것은 불가능한데, Ruby에서는 Mixin이라는 기법으로 활용하면 가능하다. Ruby에서는 Module이라는 것이 있기 때문에 가능한 것인데, 이 기능을 예시로 살펴보자.

```ruby
module ToFile
  def filename
    "object_#{self.object_id}.txt"
  end
  def to_f
    File.open(filename, 'w'){|f| f.write(to_s)}
  end
end
class Person
  include ToFile
  attr_accessor :name

  def initialize(name)
    @name = name
  end
  def to_s
    name
  end
end

puts Person.new("Seungdols").to_f
```

코드상에서 파일에 어떤 내용을 저장하는 부분을 Mixin방식으로 전달하는 것을 볼 수 있다. 즉, 믹스인 기법을 지원하는 언어라면 **코드의 중복성을 훨씬 더 많이 제거하도록 가능케하고, 모듈을 이용하여 레고 블록을 조립하듯이 프로그래밍**을 할 수 있을 것이다.
