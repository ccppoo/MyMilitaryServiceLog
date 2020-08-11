# Lambda Function vs Closure

이 글을 쓴 이유는 Swift를 공부하다가 클로저라는게 내가 알고 있던 람다(Lambda function)였다는 걸 알게된 것이였다.

람다와 클로저가 혼용되어서 스택오버플로우 답변을 한줄 한줄 번역하면서 정리하기로 했다.

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

> 

Such a symbol is called bound. 

>

But what if there are other symbols in the expression? For example: λx.x/y+2. 

>

In this expression, the symbol x is bound by the lambda abstraction λx. preceding it. 

>

But the other symbol, y, is not bound – it is free. 

>

We don't know what it is and where it comes from, so we don't know what it means and what value it represents, and therefore we cannot evaluate that expression until we figure out what y means.

>

In fact, the same goes with the other two symbols, 2 and +. 

>

It's just that we are so familiar with these two symbols that we usually forget that the computer doesn't know them and we need to tell it what they mean by defining them somewhere, e.g. in a library or the language itself.

>

...

You can think of the free symbols as defined somewhere else, outside the expression, in its "surrounding context", which is called its environment. 

>

The environment might be a bigger expression that this expression is a part of (as Qui-Gon Jinn said: "There's always a bigger fish" ;) ), or in some library, or in the language itself (as a primitive).

>

This lets us divide lambda expressions into two categories:

CLOSED expressions: every symbol that occurs in these expressions is bound by some lambda abstraction. In other words, they are self-contained; they don't require any surrounding context to be evaluated. They are also called combinators.
OPEN expressions: some symbols in these expressions are not bound – that is, some of the symbols occurring in them are free and they require some external information, and thus they cannot be evaluated until you supply the definitions of these symbols.
You can CLOSE an open lambda expression by supplying the environment, which defines all these free symbols by binding them to some values (which may be numbers, strings, anonymous functions aka lambdas, whatever…).

And here comes the closure part:
The closure of a lambda expression is this particular set of symbols defined in the outer context (environment) that give values to the free symbols in this expression, making them non-free anymore. It turns an open lambda expression, which still contains some "undefined" free symbols, into a closed one, which doesn't have any free symbols anymore.

For example, if you have the following lambda expression: λx.x/y+2, the symbol x is bound, while the symbol y is free, therefore the expression is open and cannot be evaluated unless you say what y means (and the same with + and 2, which are also free). But suppose that you also have an environment like this:

{  y: 3,
+: [built-in addition],
2: [built-in number],
q: 42,
w: 5  }
This environment supplies definitions for all the "undefined" (free) symbols from our lambda expression (y, +, 2), and several extra symbols (q, w). The symbols that we need to be defined are this subset of the environment:

{  y: 3,
+: [built-in addition],
2: [built-in number]  }
and this is precisely the closure of our lambda expression :>

In other words, it closes an open lambda expression. This is where the name closure came from in the first place, and this is why so many people's answers in this thread are not quite correct :P

So why are they mistaken? Why do so many of them say that closures are some data structures in memory, or some features of the languages they use, or why do they confuse closures with lambdas? :P
Well, the corporate marketoids of Sun/Oracle, Microsoft, Google etc. are to blame, because that's what they called these constructs in their languages (Java, C#, Go etc.). They often call "closures" what are supposed to be just lambdas. Or they call "closures" a particular technique they used to implement lexical scoping, that is, the fact that a function can access the variables that were defined in its outer scope at the time of its definition. They often say that the function "encloses" these variables, that is, captures them into some data structure to save them from being destroyed after the outer function finishes executing. But this is just made-up post factum "folklore etymology" and marketing, which only makes things more confusing, because every language vendor uses its own terminology.

And it's even worse because of the fact that there's always a bit of truth in what they say, which does not allow you to easily dismiss it as false :P Let me explain:

If you want to implement a language that uses lambdas as first-class citizens, you need to allow them to use symbols defined in their surrounding context (that is, to use free variables in your lambdas). And these symbols must be there even when the surrounding function returns. The problem is that these symbols are bound to some local storage of the function (usually on the call stack), which won't be there anymore when the function returns. Therefore, in order for a lambda to work the way you expect, you need to somehow "capture" all these free variables from its outer context and save them for later, even when the outer context will be gone. That is, you need to find the closure of your lambda (all these external variables it uses) and store it somewhere else (either by making a copy, or by preparing space for them upfront, somewhere else than on the stack). The actual method you use to achieve this goal is an "implementation detail" of your language. What's important here is the closure, which is the set of free variables from the environment of your lambda that need to be saved somewhere.

It didn't took too long for people to start calling the actual data structure they use in their language's implementations to implement closure as the "closure" itself. The structure usually looks something like this:

Closure {
   [pointer to the lambda function's machine code],
   [pointer to the lambda function's environment]
}
and these data structures are being passed around as parameters to other functions, returned from functions, and stored in variables, to represent lambdas, and allowing them to access their enclosing environment as well as the machine code to run in that context. But it's just a way (one of many) to implement closure, not the closure itself.

As I explained above, the closure of a lambda expression is the subset of definitions in its environment that give values to the free variables contained in that lambda expression, effectively closing the expression (turning an open lambda expression, which cannot be evaluated yet, into a closed lambda expression, which can then be evaluated, since all the symbols contained in it are now defined).

Anything else is just a "cargo cult" and "voo-doo magic" of programmers and language vendors unaware of the real roots of these notions.

I hope that answers your questions. But if you had any follow-up questions, feel free to ask them in the comments, and I'll try to explain it better.
