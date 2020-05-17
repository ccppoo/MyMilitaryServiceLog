* 입력 읽기 : read-host

* 출력 : write-verbose

## 함수만들기

```ps1
Function 함수명 {
  [cmdLetBinding()]
  
  Param(
    [Paramter(Position =0 , Mandatory=$BoolValue, ValueFromPipeline = $BoolValue]
    [DataType]$ParameterName
    [Paramter(Position =1 , Mandatory=$BoolValue, ValueFromPipeline = $BoolValue]
    [DataType]$ParameterName2
  )
  
  Begin {
     <# 함수를 처음 실행할 때 먼저 실행되는 코드블럭이다 #>
     <# 필요한 설정이나 동작들, 초기화 할 것들을 여기서 먼저 처리할 수 있다#>
     <# 생성자와 비슷한 시점에 실행되는 코드블럭#>
  }
  Process {
    <# 여기서 함수 실행할 내용... #>
  }
  End {
    <# 함수가 끝날 때 실행할 내용... #>
    <# C++의 소멸자(destuctor)와 비슷한 시점에 실행되는 코드블럭 #>
  }
}
```

cmdletbinding, Param, Begin, Process, End는 모두 선택사항이다

Param에서 옵션들은 모두 선택사항이다. 그래서 다음과 같이 선언해된다.

```ps1
 Param(
    [DataType]$ParameterName
  )
```
단 이때 Mandatory(값 필수 여부)는 False, ValueFromPipeline(파이프라인 이후 자동으로 파라미터 바인딩)은 False가 된다.

따라서 다음과 같이 아무것도 안하는 함수를 만들 수 있다.
```ps1
Function doNothing{
}
```

함수를 실행할 땐 

doSomeThing -parameter1 "something" -parameter2 123

와 같이 선언한다!!
