# MotorDriver_RaspberryPi_Python
GitHub -> [MotorDriver_RaspberryPi_Python](https://github.com/ntrexlab/MotorDriver_RaspberryPi_Python)
***
## **Manual**
***
* ### 1.HardWare
    * #### 사용모델
         - MDC24D100D-v2 - [MW_MDC24D100D-v2](http://www.devicemart.co.kr/goods/view?no=1077424)
         - Raspberry Pi - [Raspberry Pi 4 Model B 8GB](http://www.devicemart.co.kr/goods/view?no=12553062)
         

        ![Pi to 100D](https://user-images.githubusercontent.com/85467544/120950931-bc137600-c782-11eb-9efa-0ca270e94458.png)

        

***

* ### 2.Tool
    * #### 사용버전
        -Raspberry Pi OS - Raspberry Pi Imager 이용 설치 [Imager 설치 경로](https://www.raspberrypi.com/software/)
   
***

* ### 3. 개발 순서
    #### 1. Pi to 100D 하드웨어를 완성한다. 
    #### 2. Raspberry Pi Imager를 이용하여 Pi OS 기본 이미지를 설치한다.
    #### 3. 키보드 값을 받기 위한 설치를 한다.
    ```c
    pip install keyboard
    
    ```
    #### 4. python 코드를 작성한다.
     ```python
    import keyboard
    import serial
    import tty, sys, termios

    from time import sleep  #time 라이브러리의 sleep함수 사용

    ser = serial.Serial("/dev/ttyUSB0", 115200,timeout=1)
    filedescriptors = termios.tcgetattr(sys.stdin)
    tty.setcbreak(sys.stdin)
    x = 0

    while 1:

        x=sys.stdin.read(1)[0]

        print("..",x)

        delimiter = '\r\n'

        if x == "w" :
            sertext="mvc=200,200"
            sendtext = sertext+delimiter
            sleep(1)

        elif x == "s":
            sertext="mvc=0,0"
            sendtext = sertext+delimiter
            sleep(1)
        elif x == "x" :
            sertext="mvc=-200,-200"
            sendtext = sertext+delimiter
            sleep(1)
        elif x == "a" :
            sertext="mvc=100,-100"
            sendtext = sertext+delimiter
            sleep(1)
        elif x == "d" :
            sertext="mvc=-100,100"
            sendtext = sertext+delimiter
            sleep(1)
        else:
            sertext="mvc=0,0"
            sendtext = sertext+delimiter
            sleep(1)

        ser.write(sendtext.encode())


        print('\n')

    
    ```
***

  * ### Test
    #### 테스트 환경은 DC 모터 두개로 전후진, 좌우 까지 해야된다는 가정하에 코딩.
    ![Ahrs_Test](https://user-images.githubusercontent.com/85467544/121108866-74a1ee00-c845-11eb-8a9d-585ce49a05cf.gif)
    #### 1. w -> 가속도 데이터 수신 설정.
    #### 2. s -> 데이터 수신 속도 1000ms로 설정.(3000ms에서)
    #### 3. a -> 각도 데이터 수신 설정.
    #### 4. d -> 가속도, 각속도, 각도, 자기 데이터 수신 설정

***
