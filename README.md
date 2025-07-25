# AViC mark4

![50118A62-6CF0-418D-B7F5-079604FAB273_4_5005_c.jpeg](AViC%20mark4%202272a69ddfde80479462d8885297603f/50118A62-6CF0-418D-B7F5-079604FAB273_4_5005_c.jpeg)
<img width="966" height="683" alt="shce" src="https://github.com/user-attachments/assets/bb2822fc-bd07-4deb-a91a-c77bd31b0598" />
<img width="693" height="326" alt="muc" src="https://github.com/user-attachments/assets/63895a70-1f34-4c74-b449-2b955dc81b5e" />
<img width="390" height="496" alt="pow" src="https://github.com/user-attachments/assets/85b2e691-adef-49a6-bb5e-b86ac4c6b241" />
<img width="383" height="652" alt="pcb" src="https://github.com/user-attachments/assets/ba18cd69-e0d2-4598-9cff-d39ba7b90d83" /> 


이번에는 기존과는 다르게 완전 하이엔드급 보드를 제작해 보려고 하였다. 이를 위해서 TDK사의 가장 최신 센서와 20cm정확도를 갖는 기압 센서, 그리고 내장 GPS와 지자기 센서, 총 4개의 pyro까지, 필요한 모든 것들을 신용카드 사이즈의 보드 안에 통합하려고 하였다.

# 기능 테스트

- [x]  USB 업로드 테스트
- [x]  LED테스트
- [x]  I2C 스캔
- [x]  SDMMC 테스트
- [x]  wifi
- [x]  Pyro 테스트
- [x]  부저
- [x]  GNSS
- [x]  5v out
- [ ]  외부 배터리 연결시 전원 역류방지

## MPU

이번에는 풀 사이즈 Esp32 S3 wroom 1 가 아니라 esp32 s3 mini를 사용하였다.

주요 사양으로는 

- **프로세서:** Xtensa® 32비트 LX7 듀얼 코어 프로세서
- **클럭 주파수:** 최대 240 MHz
- **무선 통신:**
    - 2.4GHz Wi-Fi (802.11 b/g/n)
    - Bluetooth 5 (LE)
- **메모리:**
    - 내부 SRAM: 512KB

주요 사양은 기존의 [AViCDEsp32](https://www.notion.so/AViCDEsp32-20f2a69ddfde80878ec1d9f6c206afdb?pvs=21) 에 사용되었던 MPU와 같은 성능이다.

## 가속도계

가속도계는 TDK InvenSense사의 ICM-45686 6축 가속도 센서를 사용하였다. 이 센서는 최대 32g까지 측정할 수 있으며, 해상도는 16bit이다. 흔히 아두이노용 가속도계로 알려진 mpu6050를 만든 회사의 최신 플래그쉽 가속도 센서이다.

## 기압계

기압계는 MS5607을 사용하였다. 

이 센서의 분해능은 20cm 정도의 높이를 측정할 수 있다. 이 정도면 꽤 정확한 기압계라고 볼 수 있다.

## 자자기계

이번에는 자이로 센서의 드리프트를 보정하기 위해 LIS2MDL 3축 지자기계를 사용하였다.

## GPS

저번 보드에서는 GPS신호를 받으려면 외장 모듈을 활용해야 했지만, 이번에는 ATGM336h과 U.FL 컨넥터로 안테나만 연결해주면 GPS신호를 바로 보드에서 받을 수 있게 하였다.

또한 LED를 연결해 PPS(1초에 한번 신호를 보내는 핀)을 통해 연결 여부를 시각적으로 볼 수 있도록 해 주었다.

## Micro SD카드

저번과 같이 SDMMC를 위한 SD카드 슬롯도 배치하여 주었다.

## 모스펫 스위치

이번에는 4개의 파이로 체널로 구성되었는데. 듀얼 N타입 모스펫이 두개 들어가 있다. 각각의 배선에 따라 최대 4A의 전류를 흐르게 할 수 있다.

## 3.3V, 5V출력

저번과 같이 레귤레이터로는 AMS1117을 사용했다. 다른 점이라면 이번에는 3.3V뿐 아니라 5V도 지원이 된다는 것이다. 이는 추후 사용할 카메라인 Runcam Split 4 4k가 최소 5V 입력을 지원하기에, 이를 보드 내에서 처리하기 위함이다.

## LED 인디케이터

저번 보드에서의 문제점이 이게 작동 중인지 아닌지 관계 없이 전원이 들어오면 LED가 계속 켜져있는다는 것이였는데, 이를 해결하기 위해 MPU에서 직접 이를 조정하게 해 이 문제를 해결했다.

![웹에서 센서 데이터를 읽어올 때 마다 LED가 깜빡이게 만든 모습](AViC%20mark4%202272a69ddfde80479462d8885297603f/7BAA1B5F-86BD-4FAB-B6B4-121E591A28E2_4_5005_c.jpeg)

웹에서 센서 데이터를 읽어올 때 마다 LED가 깜빡이게 만든 모습

## 부저

이 는 원래 에이비오닉스로 활용될 계획도 갖고 있어서, 로켓 내부에 있을 때에 청각적으로 작동중임을 알 수 있도록 부저를 달아서 알 수 있게 하였다.

또한 부팅시에 부팅음을 넣어서 시리얼 모니터를 보지 않아도 정상적으로 리셋이 되었는지 알 수 있도록 하였다.

## USB-C

ESP32 s3는 풀 스피드 USB2.0를 지원하고 내장 CDC가 있기 때문에 D- D+핀만 정상적으로 연결해 준다면 usb를 이용하여 코딩과 시리얼 모니터(UART0)를 볼 수 있다. 또한 USB에서 나오는 5V전원을 통해 보드 전체의 전원이 들어올 수 있도록 해 주었다. 이를 통해 기존 코드 업로드 방식에서 엄청난 혁신을 이루었는데, 코드를 업로드 할 때마다 리셋 버튼을 누르는 수고와 항상 USB TTL컨버터를 찾아야 하는 번거로움에서 해방되었다. (그래서 USB TTL컨버터를 잃어버렸다;)

또한 이 USB-C에 대한 글을 하나 작성하였다. 

https://kimsj5025.tistory.com/entry/Esp32-S3-USB-OTG-%EC%8B%9C%EB%A6%AC%EC%96%BC-%EB%AA%A8%EB%8B%88%ED%84%B0%EA%B0%80-%EC%95%88%EB%82%98%EC%98%AC%EB%95%8C-%ED%95%B4%EA%B2%B0-%EB%B0%A9%EB%B2%95-ArduinoIDE-PlatformIO

## 더 많은 GPIO

PWM용, UART 통신(+ RX TX -)을 위한 헤더핀 총 8개 그리고 3.3,5V 각각 두개, 4개의 파이로 체널, 9개의 GPIO, 3개의 GND 총 20개의 터미널 블럭을 이용한 단자를 만들어 주었다. 또한 각가그이 M4 나사 규격의 마운트홀도 모두 GND에 연결해 주어 총 7개의 GND단자가 있게 되었다.
