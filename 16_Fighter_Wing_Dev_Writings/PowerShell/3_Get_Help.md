### Get-Help

모르는 명령어가 태반이기 때문에 검색하는게 좋다.

```ps1
Get-Help -Name [-detailed | -examples | -full | -online ]
```

위의 추가 옵션 4개 모두 필수 입력값은 아니다.

##### -Name 

찾고자 하는 명령어의 이름 변수 Flag다

##### -examples

예시를 같이 보여달라는 변수 Flag다.

보통 인터넷을 통해서 보여주기 때문에 시간이 좀 걸린다.

##### -full

명령어의 모든 기술적 스펙을 보여주는 변수 Flag다.

이 flag를 사용해서 검색하는 편이 상세하게 나와서 좋다.

##### -online

MS에 있는 도움말을 가져온다.

마치 구글에 검색하는 것과 유사한 효과를 보여준다.

**Get-Help**대신 **Update-Help**을 하면 업데이트가 이루어진다.

--------------------

### 명령어 찾는 방법

Get-command -Verb <동사 정규식> -Noun <명사 정규식>

예시 : Get-Command -Verb get* -Noun Net*

### 아 잠깐만

파워쉘은 강력한 도구이므로 후폭풍이 어마무시할 수 있다.

(1) -Comfirm 옵션 Flag
<br> └ 실행하기 전 확인 여부를 물어본다

(2) -Whatif 옵션 Flag
<br> └ 실행하면 일어날 일을 대략적으로 보여준다.
