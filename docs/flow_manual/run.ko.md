# 실행(Run)

## 계산 시작 및 중지

계산 시작(Start Calculation)을 누르면 계산을 시작한다. 계산이 시작되면 '계산 중지'(Cancle Calcuation), '저장 후 계산 중지'(Save and Stop Calculation), '설정 변경 바로 적용'(Update Configuration)이 활성화 된다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/run.png"><br>실행</center>

'계산 중지'는 계산을 강제로 중지한다. '저장 후 계산 중지'는 진행중인 반복계산(iteration)을 완료한 다음 데이터를 저장하고 계산을 중지한다.

계산이 시작되면 '수치해석 기법', '모니터', '계산 조건'만 활성화되고 나머지는 비활성화 된다. 활성화 된 항목들에서 설정을 수정하고 '설정 변경 바로 적용'을 누르면 변경된 내용을 적용하여 계산이 계속 진행된다.

## 사용자 정의 변수(User Parameter)

사용자 정의 변수를 만들어서 사용할 수 있다. 일괄계산모드(Batch Running Mode)에서 주로 사용되는 기능이다.

다음 그림은 UX, UY 2개의 변수를 지정한 사례이다. 이름(Name)의 괄호 속에 있는 숫자는 이 변수가 몇 군데서 사용되고 있는지는 나타내는데 여기서는 0이므로 아무 곳에도 사용되고 있지 않음을 나타낸다. 디폴트 값(Default Value)는 일괄계산모드가 아닐 때 이 변수에 할당되는 값이다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/userParameter0.png"><br>사용자 정의 변수 설정 예</center>

변수의 선언, 수정, 삭제는 편집(Edit) 버튼을 누르면 나타나는 다음 그림과 같은 창을 사용한다. 파라미터 값(Parameter Values) 옆에 있는 (+)를 누르면 새로운 변수가 추가된다. **변수의 이름은 영문 대문자와 숫자 그리고 언더스코어(\_)만 사용될 수 있으며 영문 대문자로 시작해야 한다**. 변수 오른쪽의 휴지통 아이콘을 클릭하면 삭제된다. 코드에서 사용되고 있는 변수는 삭제되지 않는다.

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/userParameter.png"><br>사용자 정의 변수 편집 창</center>

경계조건, 초기조건 등 숫자를 입력하는 부분에 달러 부호와 함께 변수를 입력하면 된다. '입구 속도' 경계조건의 X-Velocity 에 UX 변수를 사용하고 싶다면 \$UX 라고 쓰면 된다.

## 일괄계산모드(Batch Running Mode)

'일괄계산모드로 전환'(Switch to Batch Running Mode) 버튼을 누르면 다음 그림과 같이 '일괄계산 케이스'(Batch Cases)라는 부분이 나타나고 버튼은 '단위계산모드로 전환'(Switch to Live Running Mode)로 바뀐다. 단위계산모드(Live Running Mode)는 일괄계산이 아닌 하나의 조건에 대해 계산하는 일반적인 방식이다.

[사용자 정의 변수와 일괄계산모드 예제](https://baramcfd.org/tutorials/2024/03/21/batchRunRAE2822-post/)

<center><img src="https://github.com/nextfoam/baram-pages/raw/main/screenshots/pic/batchCases.png"><br>일괄계산</center>

위의 그림은 불러오기(Import) 버튼을 눌러 조건이 설정된 csv 파일을 읽은 상태이다. 읽을 수 있는 파일 형식은 csv와 xlsx 파일이다. 파일에서 var, case1, case2, case3 등은 임의로 사용자가 적어주면 되지만 변수명인 UX, UY는 User Parameter에서 정의된 것을 사용해야 한다. csv 파일의 예는 다음과 같다.

```
var,UX,UY
case1,1,0
case2,2,1
case3,3,2
...
```

위 그림에서 계산(Calc.)와 결과(Result)라는 열이 있다. 케이스를 선택하고(드래그앤드랍으로 여러개 선택 가능) 마우스 오른쪽 버튼을 눌러 계산목록(Schedule Calculation)을 선택하면 '계산'에 체크 표시가 된다. 목록취소(Cancel Calculation)을 선택하면 체크 표시가 사라진다.

계산 시작(Start Calculation) 버튼을 누르면 '계산'에 체크된 케이스들이 순차적으로 계산된다. 계산이 완료된 것은 '결과'에 초록색으로 표시되고 계산 중 발산한 경우는 빨간색으로 표시된다.

모든 계산이 종료된 후 특정 케이스를 오른쪽 마우스 버튼으로 불러오기(Load)를 선택하면 해당 케이스의 잔차와 모니터링 그래프가 나타나고 ParaView에서 결과를 확인할 수 있다.


