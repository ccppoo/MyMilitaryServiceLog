# Lambda Function vs Closure

이 글을 쓴 이유는 Swift를 공부하다가 클로저라는게 내가 알고 있던 람다(Lambda function)였다는 걸 알게된 것이였다.

람다와 클로저가 혼용되어서 스택오버플로우 답변을 한줄 한줄 번역하면서 정리하기로 했다.

키워드 : 람다 vs 클로저, 람다와 클로저의 차이, 클로저, 람다

[What is the difference between a closure and a lambda?](https://stackoverflow.com/questions/220658/what-is-the-difference-between-a-closure-and-a-lambda)

본문의 두 번째 답변

There is a lot of confusion around lambdas and closures, even in the answers to this StackOverflow question here. 

> 람다와 클로저에 많은 혼동이 있습니다, 이 스택오버플로우 답변에서도 말이죠.

Instead of asking random programmers who learned about closures from practice with certain programming languages or other clueless programmers, take a journey to the source (where it all began). 

> 특정 프로그래밍 언어나 다른 프로그래밍 언어를 통해서 클로저를 연습하고 배운 프로그래머에게 직접 물어보기보다는, 그 원천에 대해서 알아봅시다 (모든 것들이 시작하는 곳에서부터요)

And since lambdas and closures come from Lambda Calculus invented by Alonzo Church back in the '30s before first electronic computers even existed, this is the source I'm talking about.

> 람다와 클로저의 개념은 1930년대 알론조 처치(Alonzo Church)가 개발한 람다 대수학(Lambda Calculus)에서 나왔습니다.
전자 컴퓨터가 존재하지 않았을 때 처음 탄생하기 전에도 말이죠. 그래서 이 개념을 기반으로 설명드릴 겁니다.


Lambda Calculus is the simplest programming language in the world. The only things you can do in it:►

> 람다 대수학은 세상에서 가장 간단한 프로그래밍 언어입니다. 당신이 할 수 있는 유일한 기능에 대해서 설명드리겠습니다 :►

(탭)

APPLICATION: Applying one expression to another, denoted f x.
(Think of it as a function call, where f is the function and x is its only parameter)

> **애플리케이션** : 하나의 표현(식)을 다른것에 적용 시키는 것, 이를 'f x'로 나타냅니다.
> (이것을 하나의 함수 호출이라고 생각하세요, 'f'가 함수면 'x'는 그 유일한 파라미터(변수) 입니다)

ABSTRACTION: Binds a symbol occurring in an expression to mark that this symbol is just a "slot", a blank box waiting to be filled with value, a "variable" as it were. 

> **추상화** : 표현(식)을 심볼(symbol)에 바인딩함으로써 이 심볼이 하나의 "슬롯(slot)"임을 표시합니다, 마치 값이 채워지길 기다리는 하나의 빈 상자처럼, 하나의 "변수(variable)"처럼 말이죠.


It is done by prepending a Greek letter λ (lambda), then the symbolic name (e.g. x), then a dot('.') before the expression 

> 위 두 가지 기능과 절차는 앞에 그리스 문자 λ(람다)를 먼저 쓰고, 심볼릭 이름 (예 : x), 그리고 식을 쓰기 전에 점('.')을 붙여 완성됩니다(예 : λx.x+2).

This then converts the expression into a function expecting one parameter.

> 그럼 이것은 표현식을 하나의 파라미터를 받는 함수로 변환시킵니다.

For example: λx.x+2 takes the expression x+2 and tells that the symbol x in this expression is a bound variable – it can be substituted with a value you supply as a parameter.

> 에를 들어 'λx.x+2'는 'x+2'라는 표현식을 갖고, 표현식 내부의 심볼 'x'는 경계 변수(bound variable)입니다. 경계 변수는 당신이 파라미터에 넣는 값으로 대신할 수 있습니다.

Note that the function defined this way is anonymous – it doesn't have a name, so you can't refer to it yet, but you can immediately call it (remember application?) by supplying it the parameter it is waiting for, like this: (λx.x+2) 7. 

> 이 방식으로 정의한 함수는 **익명**인 것을 주목하세요 - 이름을 갖고 있지 않습니다, 그래서 당신은 참조(refer)할 수 없습니다, 대신 파라미터를 공급함으로써 즉시 호출할 수 있습니다(**애플리케이션** 기억하시죠?)
> 다음과 같은 방식으로요 : (λx.x+2) 7

Then the expression (in this case a literal value) 7 is substituted as x in the subexpression x+2 of the applied lambda, so you get 7+2, which then reduces to 9 by common arithmetics rules.

> 그러면 표현식 (이 경우 문자 그대로) '7'은 람다에 적용된 하위 표현식 'x+2'에서 7이 'x'로 대체되어, '7 + 2' 결과를 받습니다. 그리고 상식적인 사칙연산을 통해 9를 내놓죠.

So we've solved one of the mysteries:
lambda is the anonymous function from the example above, λx.x+2.

> 그럼 다음과 같은 결론이 나오겠네요 : 
> 람다는 위에서 예시를 들었듯이(λx.x+2) 익명의 함수입니다

(탭 끝)

-------

In different programming languages, the syntax for functional abstraction (lambda) may differ. 

> 프로그래밍 언어마다 기능적 추상화(functional abstraction)는, 람다식은, 다를 수 있습니다.

For example, in JavaScript it looks like this:
function(x) { return x+2; }

> 예를 들어, JavaScript는 다음과 같이 생겼습니다 : function(x){ return x+2; }

and you can immediately apply it to some parameter like this:
(function(x) { return x+2; })(7)

> 그리고 이렇게 파라미터를 바로 적용해서 사용할 수도 있죠 : (function(x) { return x+2; })(7)

or you can store this anonymous function (lambda) into some variable:
var f = function(x) { return x+2; }

> 또는 당신은 익명의 함수(람다)를 변수에 저장할 수 있습니다 : var f = function(x) { return x+2; }

which effectively gives it a name f, allowing you to refer to it and call it multiple times later, e.g.:
alert(  f(7) + f(10)  );   // should print 21 in the message box

> 그러면 'f'라는 이름을 받게되어 참조를 할 수 있고 여러번 호출할 수 도 있습니다 : alert( f(7) + f(10) );

But you didn't have to name it. You could call it immediately:
alert(  function(x) { return x+2; } (7)  );  // should print 9 in the message box

> 이름을 붙일 필요없이 바로 호출할 수 있습니다 : alert( function(x) { return x+2; } (7) );

In LISP, lambdas are made like this: (lambda (x) (+ x 2))

> 리스프(LISP)에서는 람다를 다음과 같이 만듭니다 : (lambda (x) (+ x 2))

and you can call such a lambda by applying it immediately to a parameter:
((lambda (x) (+ x 2)) 7 )

> 그리고 람다를 다음과 같이 파라미터에 적용하여 사용할 수 있습니다 : (lambda (x)( + x 2 )) 7 )

------

OK, now it's time to solve the other mystery: what is a closure. 

> 그럼 이제 남은 질문을 해결할 떄가 되었습니다 : 클로저(closure)는 무엇인가요

In order to do that, let's talk about symbols (variables) in lambda expressions.

> 우선, 람다 표현식에서의 심볼(변수)에 대해서 다뤄봅시다.

As I said, what the lambda abstraction does is binding a symbol in its subexpression, so that it becomes a substitutible parameter. 

> 제가 말했듯이, 람다 추상화가 하는 것은 하위 표현식에 심볼을 바인딩하는 것입니다. 하위 표현식의 파라미터가 되도록 말이죠.

Such a symbol is called bound. 

> 이런 심볼을 'bound' 라고 부릅니다.

But what if there are other symbols in the expression? For example: λx.x/y+2. 

> 근데 만약 다음 예시와 같이 표현식에 다른 심볼이 있는 경우는 어떻게 될까요? : λx.x/y+2

In this expression, the symbol x is bound by the lambda abstraction 'λx.' preceding it. 

> 이 표현식에서는 심볼 'x'는 'λx'에 바인딩된 상태입니다. 

But the other symbol, y, is not bound – it is free. 

> 그러나 다른 심볼 'y'는 바인딩되어있지 않습니다 - 자유로운 상태입니다(바인딩 X)

We don't know what it is and where it comes from, so we don't know what it means and what value it represents, and therefore we cannot evaluate that expression until we figure out what y means.

> 이 식만 봐서는 이게 어디서 왔는지 모르므로, 어떤 의미를 갖는지 어떤 값을 나타내는지 모르는 상태입니다. 그래서 우리는 'y'가 어떤 의미인지 알기 전까지 이 표현식을 평가(evaluate)할 수 없습니다.

In fact, the same goes with the other two symbols, 2 and +. 

> 사실, '2'와 '+'도 마찬가지인 상황입니다.

It's just that we are so familiar with these two symbols that we usually forget that the computer doesn't know them and we need to tell it what they mean by defining them somewhere, e.g. in a library or the language itself.

> 이 두 심볼(2, '+')은 우리에게 너무나 익숙한 기호(심볼)이기 때문에 컴퓨터에게 매번 말하는 것을 까먹고 합니다. 대신 프로그래밍 언어 또는 라이브러리에서 알아서 그 의미를 가져오기를 기대하고 있죠.

...

You can think of the free symbols as defined somewhere else, outside the expression, in its "surrounding context", which is called its environment. 

>  당신은 자유로운(바인딩 되지 않은) 심볼들이 다른 곳에서 정의되었다고 생각할 수 있습니다, 예를 들어 표현식 밖에서 말이죠. 이 곳을  주위 문맥(surrounding context), 즉 환경(environment)라고 부르기도 합니다.

The environment might be a bigger expression that this expression is a part of (as Qui-Gon Jinn said: "There's always a bigger fish" ;) ), or in some library, or in the language itself (as a primitive).

>  환경은 표현식(λx.x/y+2)보다 더 큰 하나의 표현식에 포함된 문맥을 의미하는 것일 수도, 다른 라이브러리를 의미하는 것일 수도, 언어 그 자체를 의미하는 것일 수도 있습니다(원초적으로요).

...

This lets us divide lambda expressions into two categories:

> 그럼 다음과 같이 람다 표현식을 두 가지 카테고리로 나눠봅시다.

(탭)

CLOSED expressions: every symbol that occurs in these expressions is bound by some lambda abstraction. 

> 닫힌 표현식(CLOSED expressions) : 이 표현식에 해당되는 모든 심볼은 람다 추상화에 의해 바인딩되어 있습니다.

In other words, they are self-contained; they don't require any surrounding context to be evaluated. 

> 쉽게 말하자면, 모든 심볼에 대한 정보를 직접 갖고 있는 상태입니다; 표현식을 평가하기 위해서 주변 문맥에 정보를 요구하지 않습니다.

They are also called combinators.

> 닫힌 표현식을 컴비네이터(combinator)라고 부르기도 합니다.

OPEN expressions: some symbols in these expressions are not bound – that is, some of the symbols occurring in them are free and they require some external information, and thus they cannot be evaluated until you supply the definitions of these symbols.

> 열린 표현식(OPEN expressions) : 바인딩되어 있지 않은 심볼들이 존재하는 표현식입니다. 즉, 어떤 심볼들은 자유로운 상태이며, 표현식을 평가하기 위해 외부로부터 정보(예: 심볼의 값)를 제공되어야 하고, 제공되지 않으면 평가받을 수 없는 것을 의미합니다.

(탭 끝)

You can CLOSE an open lambda expression by supplying the environment, which defines all these free symbols by binding them to some values (which may be numbers, strings, anonymous functions aka lambdas, whatever…).

> 당신은 '열린' 람다식을 '닫힐 수'(CLOSE)있습니다. 그러기 위해서 모든 자유로운 심볼들을 어떤 값으로 바인딩을 한 후 정의를 하면 가능합니다. 심볼들은 숫자, 문자열, 함수(람다 함수), 등 모든 포함될 수 있습니다.

...

And here comes the closure part:

> 그리고 드디어 클로저(closure) 부분이 나옵니다:

The closure of a lambda expression is this particular set of symbols defined in the outer context (environment) that give values to the free symbols in this expression, making them non-free anymore. 

> 람다 표현식의 클로저는 특정한 세트(set)의 심볼입니다. 외부에 정의된 문맥(환경)에 따라 표현식에 있는 자유로운 심볼들에 값을 줌으로써 더이상 자유롭게 하지 않습니다(non-free)

It turns an open lambda expression, which still contains some "undefined" free symbols, into a closed one, which doesn't have any free symbols anymore.

> 이는 아직 정의되지 않은 자유로운 심볼들을 보유하지만, 열린 람다 표현식(open lambda expression)을 더이상 자유로운 심볼이 없는 닫힌 것(close lambda expression)으로 만듦니다.

For example, if you have the following lambda expression: λx.x/y+2, the symbol x is bound, while the symbol y is free, therefore the expression is open and cannot be evaluated unless you say what y means (and the same with + and 2, which are also free). 

> 예를 들어 'λx.x/y+2'와 같은 람다 표현식에서, 'x'는 바인딩 된 심볼이며 'y'는 자유로운 심볼입니다. 그러므로 표현식은 열린(OPEN) 상태이며 'y'가 어떤 의미인지 정의하기 전까지는 평가될 수 없습니다 (당연히 의미가 있다고 여겼던 '+'와 '2' 또한 마찬가지입니다)

But suppose that you also have an environment like this:

> 이때, 다음과 같은 환경을 갖고 있다고 가정해봅시다.

{  
   y: 3,
   +: [built-in addition],
   2: [built-in number],
   q: 42,
   w: 5  
}

This environment supplies definitions for all the "undefined" (free) symbols from our lambda expression (y, +, 2), and several extra symbols (q, w). 

> 이 환경은 람다 표현식에서 '정의되지 않은'(자유로운) 심볼들('y', '+', '2')에 정의를 제공하고, 다른 심볼의 정의 또한 제공합니다('q', 'w')

The symbols that we need to be defined are this subset of the environment:

> 우리가 필요한 심볼들은 그 환경 중 일부입니다:

{  
   y: 3,
   +: [built-in addition],
   2: [built-in number]  
}

and this is precisely the closure of our lambda expression :>

> 그리고 이것은 람다 표현식(λx.x/y+2)의 클로저에 가깝습니다.

In other words, it closes an open lambda expression. 

> 다르게 이야기 하자면, 열린 람다 표현식(Open lambda expression)을 닫는다는 것입니다.

This is where the name closure came from in the first place, and this is why so many people's answers in this thread are not quite correct :P

> 이렇게 해서 단어 클로저(close -> closure)가 처음 나온 기원이고, 이 이유가 다른 사람들의 답변이 꽤나 정확하지 않은 이유이기도 하죠 (웃음)

-------

So why are they mistaken? 

> 그럼 무엇이 그들을 햇갈리게 했을까요?

Why do so many of them say that closures are some data structures in memory, or some features of the languages they use, or why do they confuse closures with lambdas? :P

> 왜 많은 사람들이 클로저를 단순히 메모리에 있는 데이터 구조(data structures), 본인이 쓰고 있는 언어의 특징이라고 혼동하고 클로저와 람다를 이해하지 못했을까요? (웃음)

Well, the corporate marketoids of Sun/Oracle, Microsoft, Google etc. are to blame, because that's what they called these constructs in their languages (Java, C#, Go etc.). 

> 음... 썬/오라클, 마이크로소프트, 구글 등의 무책임한 마케팅 담당자들에게 잘못이 있습니다, 왜냐하면 그들이 만드는 언어에서 '클로저'와 '람다'라는 단어를 무분별하게 사용했기 때문이죠 (Java, C#, Golang, 등)

They often call "closures" what are supposed to be just lambdas. 

> 그들 대부분 그냥 '람다'인 것을 '클로저'라고 부릅니다.

Or they call "closures" a particular technique they used to implement lexical scoping, that is, the fact that a function can access the variables that were defined in its outer scope at the time of its definition. 

> 또는 내장된 '사전적 변수 영역(Lexical Scoping)'을 '클로저'라는 기술이라고 부릅니다. 그 당시 '사전적 변수 영역'은 함수 외부에서 정의된 변수들에 접근 할 수 있는 의미로 쓰였습니다.

They often say that the function "encloses" these variables, that is, captures them into some data structure to save them from being destroyed after the outer function finishes executing. 

> 그들(언어를 만드는 사람들)은 정의된 함수(2) 외부에서 사용된 변수들을 함수 내부의 데이터 구조에 동봉(enclose) 함으로써 함수(1)이 실행이 끝났을 때 파괴되지(스택에서 해제) 않도록 하는 과정을 함수(2)가 동봉('enclose')했다고 말합니다

> 역자 : 파괴는 메모리에서 해제, 즉 참조 개수가 0 (reference count == 0) 인 것을 말하는 겁니다.

```python
# 번역자 추가

# 함수(1)
def getCounter():

   # 함수(2) 입장에서 '외부'에 존재하는 변수
   cnt = 0
   
   # 함수(2)
   def counter():
      while true:
         # 외부의 변수를 참조함으로써 계속 갖고(동봉 - enclose)있음
         cnt += 1
         yield cnt
   
   return counter
```

But this is just made-up post factum "folklore etymology" and marketing, which only makes things more confusing, because every language vendor uses its own terminology.

> 그러나 이렇게 본인의 입맛에 정의된 "프로그래밍 언어만의 정의(folklore etymology)"와 홍보된 내용은 기존의 의미에서 벗어남과 동시에 혼동을 주었고, 각각 프로그래밍 언어마다 쓰이는 의미가 달라지게되었습니다 

And it's even worse because of the fact that there's always a bit of truth in what they say, which does not allow you to easily dismiss it as false :P 

> 더 최악인 사실은 완전히 틀린 정의는 아니여서 사용자들이 의심없이 그대로 사용한다는 것입니다 (웃음)

...

> 역자 : 위에 예시를 든 파이썬 코드의 함수 'counter'는 일상적으로 클로저(closure)를 말하는 것이며, 이 글이 계속 말했듯이 클로저는 람다의 일부임을 인지하세요! 
> 이해하기 어렵다면, 위 함수(2)는 반환이 될 때, 함수를 사용하는 입장에서 이 함수가 'counter'라는 이름을 가졌다는 사실을 모르는 것을 인지하세요!
> free symbol == undefined symbol == undefined(unknown) variable
> unknwon == free == undefined
> 자유 심볼 == 자유 변수

Let me explain:

> 그래서 내가 제대로 설명을 할게요 :

If you want to implement a language that uses lambdas as first-class citizens, you need to allow them to use symbols defined in their surrounding context (that is, to use free variables in your lambdas). 

> 당신이 람다식을 일등 객체로 사용가능한 언어를 만들고 싶다면, 람다식이 주변 환경 문맥에 존재하는 심볼들을 찾고 정의하는데 쓸 수 있도록 해야하는 겁니다 (즉, 람다식에 자유 심볼(변수)를 사용할 수 있다는 것).

And these symbols must be there even when the surrounding function returns.

> 그리고 이 심볼들은 람다식 주변을 감싸는 함수가 반환(return) 된 상태여도 그대로 찾을 수 있는 상태를 유지해야합니다 (자유 심볼을 바인딩하기 위해 탐색하기 위해서).

The problem is that these symbols are bound to some local storage of the function (usually on the call stack), which won't be there anymore when the function returns. 

> 여기서 문제는 이 심볼들은 함수(1) 내부의 변수(cnt)에 바인딩되어 있다는 것입니다, 함수(2)가 반환되면 그 심볼들은 없어지는 것인데 말이죠.

Therefore, in order for a lambda to work the way you expect, you need to somehow "capture" all these free variables from its outer context and save them for later, even when the outer context will be gone. 

> 그래서, 당신이 바라는 방식대로 람다식이 작동하기 위해서는, 람다식 내부(함수 (2))의 모든 자유 변수(cnt)가 무엇인지를 파악하기 위해서 외부 맥락(함수 (1) 내부)으로부터 정의를 가져와 저장을 해야합니다. 외부 맥락(함수 (1))이 끝나고 나서도요.

That is, you need to find the closure of your lambda (all these external variables it uses) and store it somewhere else (either by making a copy, or by preparing space for them upfront, somewhere else than on the stack). 

> 즉, 당신은 람다식의 클로저(외부 맥락에서 가져와 쓰는 변수들 - cnt)를 찾고 어딘가에는 저장해야합니다 (복사를 하거나, 스택에 쌓거나, 다른 별도에 공간에 넣거나, 등등)

The actual method you use to achieve this goal is an "implementation detail" of your language.

> 이 목적을 달성하기 위한 방법의 실체는 당신이 만드는 언어의 "디테일의 구현"입니다.

What's important here is the closure, which is the set of free variables from the environment of your lambda that need to be saved somewhere.

> 여기서 중요한 것은 클로저입니다. 외부 맥락(환경)에서 가져온 자유 변수들의 정보에 대한 세트(set)를 어딘가에는 저장해야하는 것 말이죠.

It didn't took too long for people to start calling the actual data structure they use in their language's implementations to implement closure as the "closure" itself. 

> 이렇게 프로그래밍 언어를 설계하는 사람들이 프로그래밍 언어의 구현부에서 '실제 데이터 구조' 그 자체( { "cnt" : 0x1aeff00 } )를 **'클로저' 그 자체**라고 부르는 것은 자연스러운 현상이었습니다

> 역자 : A를 달성(closure)하기 위해 수많은 방법(B1, B2, B3, ...)이 존재하는데, 하나의 방법(프로그래밍 언어마다 구현하는 방식)을  모두 통틀어 B라고 말하는 것
> 추가 : 클로저는 실체가 있는 결과물이지만, 내부 구현 방식은 언어마다 다를 수 있다!

The structure usually looks something like this:

> 실제 구조는 주로 다음과 같이 생겼습니다

Closure {
   [pointer to the lambda function's machine code],
   [pointer to the lambda function's environment]
}

> 클로저 { [람다 함수의 포인터 ] , [람다 함수의 환경 포인터] } 

and these data structures are being passed around as parameters to other functions, returned from functions, and stored in variables, to represent lambdas, and allowing them to access their enclosing environment as well as the machine code to run in that context. 

> 그리고 이 데이터 구조들은 다른 함수의 파라미터간에 주고 받아지며, 함수로부터 반환되기도 하고, 변수에 저장되기도 하며, 람다를 의미하기도 합니다. 고로 '닫힌'(closure 된) 람다 함수의 환경에 접근 할 수 있게되어 다른 문맥에서도 쓰일 수 있게하죠 (위 파이썬 코드에서 함수(1)이 반환되고 나서의 문맥을 말함)

But it's just a way (one of many) to implement closure, not the closure itself.

> 그러나 이것은 단지 클로저를 구현하기 위한 (수많은 방법 중) 하나의 방법에 불과합니다, 클로저 그 자체를 말하는 것이 아니라요.

As I explained above, the closure of a lambda expression is the subset of definitions in its environment that give values to the free variables contained in that lambda expression, effectively closing the expression (turning an open lambda expression, which cannot be evaluated yet, into a closed lambda expression, which can then be evaluated, since all the symbols contained in it are now defined).

> 위에서 제가 설명했듯이, 람다 표현식의 클로저는 자유 변수들의 의미를 담고있기 위한(평가할 수 없는 열린 람다식을 '닫게(Close)'하는) 정의의 부분집합이다.

Anything else is just a "cargo cult" and "voo-doo magic" of programmers and language vendors unaware of the real roots of these notions.

> 다른 설명들은 언어의 기원을 알지도 모르는, 그저 제대로 된 설명이 없는 프로그래머와 유행어 제조기의 (특정 프로그래밍 언어에 대한)"신앙"과 "부두술"에 불과하다.

I hope that answers your questions. But if you had any follow-up questions, feel free to ask them in the comments, and I'll try to explain it better.

> 이것으로 당신의 질문에 답변이 되었기를 바란다. 더 추가로 묻고 싶은 질문이 있다면 자유롭게 답글로 해주길 바란다, 최대한 더 자세히 설명해주겠다.


끝!