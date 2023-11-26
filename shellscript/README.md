# shellscript
터미널에서 실행될 수 있는 명령어를 하나의 파일로 묶은 것<br>
모든 shellscript는 반드시 Shebang(사용할 shell 명시)으로 시작 <br>
- Shebang 형식 : #!<사용할 shell>, 일반적으로는 #!/bin/sh 사용<br>
Shebang 이후에 사용할 명령어들을 나열하면 됨

주석 표시 : 줄 맨 앞에 #을 입력

./<shellscript  파일이름> : shellscript 실행하기
- 실행하기 위해서는 실행권한이 필요(chmod로 부여)<br>

shellscript 예시
```shellscript
#!/bin/sh

echo "Hello world!"
echo "Wait for 5 seconds..."
sleep 5
echo "Compileted!"
```

## 변수

- 선언 
    - 변수명=데이터
    - = 앞뒤에 띄어쓰기 허용하지 않음
- 사용
    - $변수명으로 사용됨

shellscript 변수 사용 예시
```shellscript
#!/bin/bash
myname='pimobailess'
mydirectory='/home/pimobailess/Desktop'

echo $myname
echo `ls $mydirectory`
```

## 리스트 변수 (배열)
- 선언
    - 변수명=(데이터1 데이터2 데이터3 ....)
-  사용
    - ${변수명[인덱스번호]}

리스트변수 사용 예시
```shellscript
#! /bin/bash

daemons=("httpd" "mysqld" "vsftpd")
echo ${daemons[1]}        # $daemons 배열의 두 번째 인덱스에 해당하는 mysqld 출력
echo ${daemons[@]}        # $daemons 배열의 모든 데이터 출력
echo ${daemons[*]}        # $daemons 배열의 모든 데이터 출력 
echo ${#daemons[@]}       # $daemons 배열 크기 출력

filelist=( $(ls) )        # 해당 쉘스크립트 실행 디렉토리의 파일 리스트를 배열로 $filelist 변수에 입력
echo ${filelist[*]}       # $filelist 모든 데이터 출력
```

사전 정의된 지역변수


| 변수명 | 의미  |
| :----: | :----: |
| $$ | 쉘의 스크립트 번호 |
| $0 | 쉘스크립트 이름 |
| $1~$9 | 명령줄 인수 |
| $* | 모든 명령줄 인수 리스트 |
| $# | 인수의 갯수 |

## 연산자
- expr : 숫자 계산
- expr 를 사용하는 경우 역작은 따옴표( ` )를 사용해야 함(작은 따옴표가 아님)
- 연산자 *와 괄호() 앞에는 역슬래시()와 같이 사용
- 연산자와 숫자, 변수, 기호 사이에는 space를 넣어야 함

리스트변수 사용 예시
```shellscript
#! /bin/bash

num=`expr \( 3 \* 5 \) / 4 + 7`
echo $num
```

## 조건
- 문자 비교
    - 문자1 == 문자2: 문자1 과 문자2가 일치
    - 문자1 != 문자2: 문자1 과 문자2가 일치하지 않음
    - -z 문자: 문자가 null 이면 참
    - -n 문자: 문자가 null 이 아니면 참
    - 문자 == 패턴: 문자열이 패턴과 일치
    - 문자 != 패턴: 문자열이 패턴과 일치하지 않음

- 수치 비교
    - 값1 -eq 값2: 값이 같음(equal)
    - 값1 -ne 값2: 값이 같지 않음(not equal)
    - 값1 -lt 값2: 값1이 값2보다 작음(less than)
    - 값1 -le 값2: 값1이 값2보다 작거나 같음(less or equal)
    - 값1 -gt 값2: 값1이 값2보다 큼(greater than)
    - 값1 -ge 값2: 값1이 값2보다 크거나 같음(greater or equal)

- 파일 검사
    - -e 파일명  : 파일이 존재하면 참
    - -d 파일명  : 파일이 디렉토리면 참
    - -h 파일명  : 심볼릭 링크파일
    - -f 파일명  : 파일이 일반파일이면 참
    - -r 파일명  : 파일이 읽기 가능이면 참
    - -s 파일명  : 파일 크기가 0이 아니면 참
    - -u 파일명  : 파일이 set-user-id가 설정되면 참
    - -w 파일명  : 파일이 쓰기 가능 상태이면 참
    - -x 파일명  : 파일이 실행 가능 상태이면 참

- 논리 연산
    - 조건1 -a 조건2: AND
    - 조건1 -o 조건2: OR
    - 조건1 && 조건2: 양쪽 다 성립
    - 조건1 || 조건2: 한쪽 또는 양쪽다 성립
    - !조건: 조건이 성립하지 않음
    - true: 조건이 언제나 성립
    - false: 조건이 언제나 성립하지 않음

## 조건문
-  if/else if/ else 구조
- 조건이 두개이상일경우 and로 연결
```
if[ 조건 ]
then
    수행문
else if[ 조건 ]
then
    수행문
else
    수행문
fi
```

shellscript 조건문 사용 예시
```shellscript
#!/bin/bash
ping -c 1 192.168.0.1 1> /dev/null
if [ $? == 0 ]
then
  echo "게이트웨이 핑 성공!"
else
  echo "게이트웨이 핑 실패!"
fi
```

## 반복문
- for / while 

for문 구조
```
for 변수 in 변수값1 변수값2 ...
do
  명령문
done
```

for문 예시
```shellscript
#!/bin/bash
for database in $(ls)
do
  echo ${database[*]}
done
```

while 구조

```
while [ 조건문 ]
do
  명령문
done
```

while 예시
```shellscript
#!/bin/bash
lists=$(ls)
num=${#lists[@]}
index=0
while [ $num -ge 0 ]
do
  echo ${lists[$index]}
  index=`expr $index + 1`
  num=`expr $num - $index`
done
```

## 함수
- 함수이름()을 사용하여 선언
- { }를 이용하여 수행내용을 정의
- 함수의 argument는 parameter와 사용법 동일
    - $1, $2, $3 ....으로 사용

함수 선언 구조
```
# 함수 선언
function 함수명()
{
    함수 내용
}

# 함수 사용
함수명
```

함수 사용 예시
```
#!/bin/bash

func_param_test()
{
    echo "first parameter : "$1
    echo "second parameter : "$2
    echo "third parameter : "$3
}

func_param_test "abc" "def" "ghi"
```