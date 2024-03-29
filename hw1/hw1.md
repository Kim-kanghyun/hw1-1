# CRACKME 1~3 문제 풀이

## crackme0x01
1.  gdb crackme0x01으로 gdb로 파일을 실행시켜준다.
![1](https://raw.githubusercontent.com/Kim-kanghyun/hw1-1/master/hw1/img/1.PNG)
2. r로 파일을 실행시켜 파일이 어떤식으로 돌아가는 지 확인한다.
![2](https://raw.githubusercontent.com/Kim-kanghyun/hw1-1/master/hw1/img/2.PNG)
3. 확인해 보니 password를 입력해야 하는 것 같다.
4. disass main으로 main함수의 소스코드를 어셈블리어로 변환한 결과를 본다.
![3](https://raw.githubusercontent.com/Kim-kanghyun/hw1-1/master/hw1/img/3.PNG)
5. 결국 비밀번호를 받는 부분은 scanf부분이므로 b scanf 전 줄을 통해 bp를 설정해준다.
: b* main+59
6. r을 실행해 중단점에서 멈추고 scanf가 실행되었을 때 값을 관찰해보니 scanf는 %d값으로 받고 0xffffd644에 값을 넣는다는 것을 확인할 수 있었다.
![4](https://raw.githubusercontent.com/Kim-kanghyun/hw1-1/master/hw1/img/4.PNG)
7. ni로 다음 줄을 보면 cmp로 ebp-4의 값과 0x149a를 비교한다는 것을 알 수 있다.
![5](https://raw.githubusercontent.com/Kim-kanghyun/hw1-1/master/hw1/img/5.PNG)
8. p $ebp-4로 보니 ebp-4의 주소값이 0xffffd644인 것을 확인할 수 있다. 즉 scanf에서 받은 값과 0x149a를 비교한다는 말이므로 위 값을 10진수로 변환한 값인 5274를 입력하면 다른 결과가 나올 것이다.
![6](https://raw.githubusercontent.com/Kim-kanghyun/hw1-1/master/hw1/img/6.PNG)
9. 확인해보니 이런 메세지가 뜨고 종료된다.
![7](https://raw.githubusercontent.com/Kim-kanghyun/hw1-1/master/hw1/img/7.PNG) 

## crackme0x02
1. gdb와 disass main 으로 어셈블리어 내용을 보는것 까지는 위 문제와 동일
2. scanf 전 줄에 bp를 잡고 ni로 관찰하니 파라미터가 ("%d", 0xffffd644) 이고 0xffffd644 는 p $ebp-4로 ebp-4임을 확인 할 수 있었음
3. 밑에 두 줄을 보면, ebp - 4 주소값에 있는 값을 eax 에 저장하고 이 값을 ebp - 0xc 주소값에 있는 값과 비교하는 것을 확인할 수 있다.
![8](https://raw.githubusercontent.com/Kim-kanghyun/hw1-1/master/hw1/img/8.PNG)
4. 이때 ebp - 0xc 주소값에 있는 값은  0x52b24이다.
![9](https://raw.githubusercontent.com/Kim-kanghyun/hw1-1/master/hw1/img/9.PNG)
5. 그러므로 338724를 입력하고 실행해보면 잘 작동한다.
![10](https://raw.githubusercontent.com/Kim-kanghyun/hw1-1/master/hw1/img/10.PNG)


## crackme0x03
1. gdb와 disass main 으로 어셈블리어 내용을 보는것 까지는 위 문제와 동일
2. scanf 의 파라미터가 ("%d", 0xffffd644) 이고 0xffffd644 는 ebp - 4 이다.
3. test 시작하는 위에 줄에 bp를 찍고 si로 함수 안에 들어가보면 일단 함수 안에 들어가는 값을 보니 내가 입력한 값, 0x52b24을 포함해서 총 4개의 값이 들어가는 것을 확인할 수 있다.
![11](https://raw.githubusercontent.com/Kim-kanghyun/hw1-1/master/hw1/img/11.PNG)
4. 안에 들어가서 확인해보니 ebp+8에 있는 값과 ebp+0xc을 비교하는 것을 확인 할 수 있다.
![12](https://raw.githubusercontent.com/Kim-kanghyun/hw1-1/master/hw1/img/12.PNG)
5. 각 각의 값을 확인해보니, scanf에서 받은 값과, 0x52b24인 것을 확인 할 수 있었다.
![13](https://raw.githubusercontent.com/Kim-kanghyun/hw1-1/master/hw1/img/13.PNG)
6. 그러므로 338724를 입력하고 실행하면 잘 작동되는 것을 확인 할 수 있다.
![14](https://raw.githubusercontent.com/Kim-kanghyun/hw1-1/master/hw1/img/14.PNG)

