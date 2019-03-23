Quickstart Guide: Standalone 모드를 활용한 Cat.M1 모듈 테스트
---
## 시작하기 전에

> * 본 가이드는 Cat.M1 모듈의 AT 명령어 동작을 확인하기 위한 테스트 과정으로, MCU 보드 없이 WIZnet IoT Shield 및 인터페이스 보드만을 활용합니다.

> * Cat.M1과 같은 Cellular IoT 디바이스는 통신 서비스 사업자의 운영 기준 및 규정에 따라 모듈 펌웨어 및 동작 방식에 차이가 있을 수 있습니다. 본 문서는 한국 **[SK Telecom Cat.M1 서비스][skt-iot-portal]** 를 기준으로 작성되었습니다.


### Development Environment
* 시리얼 터미널 프로그램 (Token2Shell)
> 사용자 환경에 따라, 다른 시리얼 터미널 프로그램을 활용해도 무방합니다.

### Hardware Requirement

| IoT Shield Interface Board |
|:--------:|
| WIoT-QC01 (BG96) |

> WIZnet IoT Shield는 다양한 Cat.M1 모듈을 활용 할 수 있도록 구성되었습니다. 본 가이드에서는 WIoT-QC01 인터페이스 보드를 활용하지만 다른 인터페이스 보드를 사용하는 경우에도 동일한 방법으로 테스트를 진행할 수 있습니다.


> 각 인터페이스 보드 별로 지원하는 AT 명령어는 차이가 있으므로, 테스트 하려는 Cat.M1 모듈의 AT 명령어 매뉴얼을 참고하시기 바랍니다.

## 하드웨어 설정 및 연결

1. WIoT-QC01 인터페이스 보드의 슬롯에 USIM 카드를 삽입합니다.

2. IoT Shield의 SMA 커넥터에 LTE 안테나를 연결합니다. LTE 안테나는 손가락으로 돌려 조여야 합니다.

3. IoT Shield와 WIoT-QC01 인터페이스 보드를 결합합니다.

4. U.FL 안테나 커넥터 케이블로 인터페이스 보드의 LTE U.FL 안테나 커넥터와 Shield 보드의 U.FL 안테나 커넥터를 연결합니다.

3. 보드 상단에 위치한 `UART SEL` 선택 핀의 TXD / RXD 점퍼를 USB로 설정합니다.

4. USB 케이블로 `P1 MODULE UART` USB 포트와 PC를 연결합니다.

![][usb-port]


## 시리얼 터미널 설정

1. 윈도우 장치 관리자에서 모듈이 연결된 COM 포트를 확인합니다.

2. 시리얼 터미널 프로그램을 실행하여 보드의 COM 포트로 **Baudrate 115200**을 선택하여 연결을 수행합니다.


> 예제 코드의 시리얼 포트 설정은 115200-8-N-1, None입니다.

![][1]

## AT 명령어 테스트

PC와 USB 케이블 연결 후, Shield 보드의 `SW1` PWRKEY 버튼을 2-3초간 눌렀다 떼면 모듈이 동작합니다.
정상 동작 시 Cat.M1 안테나 커넥터 옆의 PWR LED가 점등되고 STATE LED가 점멸하는 것을 볼 수 있습니다.

시리얼 터미널이 연결되어 있는 경우, 모듈은 다음과 같이 출력합니다.
```shell
RDY

+CFUN: 1

APP RDY

+CPIN: READY

+QUSIM: 1

+QIND: SMS DONE
```



시리얼 터미널 프로그램을 통해 다음 AT 명령어를 차례로 입력하여 모듈의 동작을 확인합니다.
```cpp
/* WIZnet IoT Shield Quickstart: BG96 */

// Product Identification Information
ATI

// International Mobile Equipment Identity (IMEI)
AT+GSN

// USIM Status
AT+CPIN?

// Network Status (M2Mnet only)
AT+QCDS

// Operator Status (응답까지 약간의 지연이 있음)
AT+COPS=?
```

망 개통 상태로 정상 동작하는 모듈은 다음과 같이 응답합니다.
세부 응답 항목에는 차이가 있을 수 있습니다.

![][2]


[skt-iot-portal]: https://www.sktiot.com/iot/developer/guide/guide/catM1/menu_05/page_01

[usb-port]: ./imgs/wiot-shield-usbport.png
[1]: ./imgs/quickstartguide_standalone_mode-1.png
[2]: ./imgs/quickstartguide_standalone_mode-2.png
