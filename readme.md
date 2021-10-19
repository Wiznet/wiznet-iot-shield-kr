# WIZnet IoT Shield


이 저장소는 WIZnet IoT Shield의 각종 자료들에 접근 할 수 있는 랜딩 페이지입니다.
다양한 하드웨어 플랫폼에서 활용 할 수 있는 예제 코드와 시작 가이드 및 어플리케이션 노트 등의 문서를 제공합니다.


예제 코드와 문서는 플랫폼 별 각각의 Repository에서 개별적으로 관리됩니다.




## WIZnet IoT Shield for Cat.M1

![][wiot-shield-h]

### Overview

WIZnet IoT Shield는 다양한 오픈 하드웨어 플랫폼 및 상용 보드 기반에서 **Cat.M1 응용 제품**을 구현 할 수 있도록 개발 되었습니다.
크게 다음과 같은 특징을 갖고 있습니다.

* **Cat.M1 응용 제품 개발 가속화**를 위해 개발된 연결형 보드
* 대표적인 오픈 하드웨어 플랫폼 **Arduino**와 **Arm MBED** 및 **상용 보드** 지원
	* Arduino Pin Connector 호환 디자인
* 다양한 벤더의 Cat.M1 모듈을 활용 할 수 있는 **인터페이스 보드** 결합형 디자인 (XBee Socket Type)
* 간편한 프로토타이핑 지원을 위한 Sensor 2종 실장 (CDS, Temperature)
* Standalone 모드 지원
* 다양한 예제 코드 및 문서 제공

### Repositories

WIZnet IoT Shield의 각 플랫폼 별 Cat.M1 개발 자료는 아래 저장소에서 확인하실 수 있습니다.
상세한 내용은 각각의 저장소를 참고 하시기 바랍니다.

* **[Quickstart Guide: Standalone 모드를 활용한 Cat.M1 모듈 테스트](https://github.com/Wiznet/wiznet-iot-shield-kr/blob/master/docs/quickstartguide_standalone_mode.md)**


**오픈 하드웨어 플랫폼 보드**
* **[WIZnet IoT Shield for Arduino](https://github.com/Wiznet/wiznet-iot-shield-arduino-kr)**
* **[WIZnet IoT Shield for Arm MBED](https://github.com/Wiznet/wiznet-iot-shield-mbed-kr)**
* **[WIZnet IoT Shield for Raspberry Pi](https://github.com/Wiznet/wiznet-iot-shield-raspberrypi-kr)**



**하드웨어 개발 자료**
* **[WIZnet IoT Shield Hardware Materials](https://github.com/Wiznet/wiznet-iot-shield-hardware-kr)**
  * 하드웨어 개발 자료 및 인터페이스 보드 별 IoT Shield 점퍼 설정 가이드 등이 포함되어 있습니다.

### IoT Shield Board

**WIZnet IoT Shield for Cat.M1의 인터페이스 모듈 결합 전 / 후**
> WIoT-QC01 인터페이스 모듈 결합

![][wiot-shield-compare]


### Interface Boards

**WIZnet IoT Shield for Cat.M1은 다음과 같은 Cat.M1 모듈을 지원합니다.**

| ![][wiot-qc01]             | ![][wiot-wm01]                 | ![][wiot-am01]                 | ![][wiot-tl01]                 |
|:--------------------------:|:------------------------------:|:------------------------------:|:------------------------------:|
| **WIoT-QC01** <br>(BG96, 앰투앰넷) | **WIoT-WM01** <br>(WM-N400MSE, 우리넷) | **WIoT-AM01** <br>(AMM5918K, AM텔레콤) | **WIoT-TL01** <br>(ME910C1-K1, 텔릿) |

> 추후 다양한 벤더의 모듈이 추가 될 수 있습니다.

### Pinout

![][shield-pinout]


## IoT 오픈 하우스

[IoT 오픈 하우스](https://github.com/Wiznet/IoTOpenHouse/)를 방문하시거나 [SK텔레콤 IoT 서포트 신청](http://b2b.tworld.co.kr/cs/counsel/iotOpenHouse.bc/)을 하시면, LoRa, Cellular(Cat M1), WiFi, Ethernet 등을 활용한 IoT 개발에 대한 컨설팅과 상용화를 위한 지원을 무상으로 받으실 수 있습니다.

## Support

[![WIZnet Developer Forum][forum]](https://forum.wiznet.io/c/korean-forum/oshw/)

**[WIZnet Developer Forum](https://forum.wiznet.io/c/korean-forum/oshw/)** 에서 전세계의 WIZnet 기술 전문가들에게 질문하고 의견을 전달할 수 있습니다.<br>지금 방문하세요!


## License

**WIZnet IoT Shield** 저장소의 모든 문서와 예제는 [Apache License, Version 2.0](https://www.apache.org/licenses/LICENSE-2.0)으로 배포됩니다.



[wiot-shield]: ./docs/imgs/hw/wiot-shield_top.png
[wiot-shield-h]: ./docs/imgs/hw/wiot-shield-qc01_top_h.png
[wiot-shield-compare]: ./docs/imgs/hw/wiot-shield-qc01-compare.png
[wiot-qc01]: ./docs/imgs/hw/wiot-qc01_top.png
[wiot-wm01]: ./docs/imgs/hw/wiot-wm01_top.png
[wiot-am01]: ./docs/imgs/hw/wiot-am01_top.png
[wiot-tl01]: ./docs/imgs/hw/wiot-tl01_top.png
[forum]: ./docs/imgs/forum.jpg
 
[shield-layout]: ./docs/imgs/WIoT-Shield_Allparts.png
[shield-pinout]: ./docs/imgs/WIoT-Shield_Pinout.png



