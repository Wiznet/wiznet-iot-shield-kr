# Cat.M1장치의 SKT ThingPlug 연동 가이드

## 목차

-   [시작하기 전에](#Prerequisites)
-   [소개](#Step-1-Overview)
-   [서비스 생성](#Step-2-Create-Service)
-   [디바이스 명세 작성](#Step-3-Device-Descriptor)
-   [디바이스 등록](#Step-4-Device-Create)
-   [Cat.M1 디바이스로 ThingPlug에 접속](#Step-5-Connect-ThingPlug)
-   [AT 명령어](#Step-6-ThingPlug-ATCommand)
-   [더 보기](#ReadMore)

<a name="Prerequisites"></a>
## 시작하기 전에

> * 본 문서에서는 Cat.M1 단말에 UART 인터페이스를 연결하고 AT 명령어 기반에서 WIZnet IoT shield를 이용하여 Cat.M1 단말이 **[ThingPlug][link-thingplug-portal]** 에 연결하고 데이터를 송신하는 방법에 대한 가이드를 제공합니다.
> 
> * Cat.M1과 같은 Cellular IoT 디바이스는 통신 서비스 사업자의 운영 기준 및 규정에 따라 모듈 펌웨어 및 동작 방식에 차이가 있을 수 있습니다. 본 문서는 한국 **[SK Telecom Cat.M1 서비스][skt-iot-portal]** 를 기준으로 작성되었습니다.

### Development Environment
* **[SKT ThingPlug Portal][link-thingplug-portal]**



### Supported Boards

| IoT Shield Interface Board |
|:--------|
| WIoT-QC01 (BG96)<br>WIoT-WM01 (WM-N400MSE)<br>WIoT-AM01 (AMM5918K) |

<a name="Step-1-Overview"></a>
## 소개

**[ThingPlug][link-thingplug-portal]** 는 고객의 IoT 디바이스의 관제/제어와 빅데이터 비즈니스를 제공하는 SKT의 IoT 플랫폼입니다. 사용자가 ThingPlug를 활용하여 IoT 서비스를 손쉽게 구현할 수 있도록 지원하고 있습니다.

![](\imgs\thingplug_main.png)

Cat.M1 모듈 및 외장형 모뎀은 UART 인터페이스를 통해 활용하는 AT 명령어로 제어하는 것이 일반적입니다. 또한 SKT 인증 Cat.M1 모듈에는 ThingPlug와 쉽게 연동할 수 있는 AT 명령어가 구현되어 있고, 이 명령어를 사용해 쉽게 ThingPlug와 연결할 수 있습니다. 

 Cat.M1 모듈에 구현된 AT 명령을 사용하여 ThingPlug와 연동하기 전 ThingPlug에 서비스와 디바이스를 등록 하는 과정이 선행되어야 하는데, 그 과정은 다음과 같은 순서로 진행합니다.

1. ThingPlug에 서비스 생성
2. 디바이스 명세 작성
3. 디바이스 등록
4. Cat.M1 디바이스로 ThingPlug에 접속


<a name="Step-2-Create-Service"></a>
## 서비스 생성

ThingPlug는 사용자가 서비스를 생성하고 서비스에 디바이스를 등록하는 구조입니다.
"서비스 생성/가입" 항목에서 서비스를 생성하거나, 다른 사용자가 생성한 서비스에 가입 신청을 할 수 있습니다.

서비스 생성 이후에 "디바이스"를 등록 하거나 "룰엔진", "대시보드"를 생성할 수 있습니다.

![](\imgs\thingplug_function.png)


<a name="Step-3-Device-Descriptor"></a>
## 디바이스 명세 작성
앞서 생성한 "서비스"에 디바이스를 등록하기 위해서는 "디바이스 명세"를 먼저 등록해야 합니다.
"디바이스 명세"란 등록할 디바이스의 여러가지 속성을 데이터 타입으로 분류하여 사전에 등록하는 것입니다. 하나의 디바이스 명세로 같은 속성의 여러 디바이스들을 등록할 수 있습니다.

디바이스 명세는 크게 두가지 데이터 타입이 있습니다. "Telemetry"와 "Attribute"입니다.
* Telemetry는 디바이스의 데이터 중 시간에 따라 주기적으로 생성하는 데이터를 의미합니다. ThingPlug는 이 데이터를 시간에 따른 변화 값을 모두 저장합니다.
* Attribute는 디바이스의 고유한 속성이나, 이벤트로 인해 변경되는 데이터를 의미합니다. ThingPlug는 이 데이터를 현재 상태인 최종 데이터만 저장하게 됩니다. 


Telemetry는 온도, 습도 등의 측정되는 데이터이며, Attribute는 버전 정보, LED 현재 상태 등의 데이터입니다.
만약 온도, 습도를 측정하고 LED를 제어하는 디바이스의 디바이스 명세는 아래와 같은 예시로 작성됩니다.

```c++
{
   "format": "json",
   "jsonForm": {
   "attributes": [
        {
            "name": "LED",
            "commandable": true
        }
    ],
    "telemetries": [
        {
            "name": "temperature",
            "tag": "temperature"
        },
        {
            "name": "humidity",
            "tag": "humidity"
        }
     ]
   }
}

```


<a name="Step-4-Device-Create"></a>
## 디바이스 등록

서비스와 디바이스 명세 작성이 완료되었다면, 디바이스를 등록할 수 있습니다.
디바이스의 등록은 디바이스 ID를 입력하고 디바이스 명세를 선택하는 것으로 디바이스 등록을 진행할 수 있습니다.

![](\imgs\thingplug_dev_01.png)

디바이스 등록이 완료되면, 디바이스 당 디바이스 Token이 자동 발급 됩니다. 디바이스 Token은 디바이스 관리 페이지에서 확인할 수 있습니다.
Cat.M1 디바이스는 등록된 디바이스의 ID와 Token을 사용하여 ThingPlug에 접속할 수 있습니다.

![](\imgs\thingplug_dev_02.png)


<a name="Step-5-Connect-ThingPlug"></a>
## Cat.M1 디바이스로 ThingPlug 접속하기


예를 들어 서비스 명 smarthome에 등록된 디바이스 smartlight001 가 ThingPlug에 접속하기 위해서는 아래와 같은 [AT 명령](#Step-6-ThingPlug-ATCommand) 절차가 필요합니다.

```
AT+SKTPCON=1,"MQTT","test.sktiot.com",1883,120,1,"simple_v1","a0149f60b---------","smarthome","smartlight001"
OK
+SKTPCON=0
```
만약 디바이스가 등록된 명세 중 temperature 측정값을 전송하고 싶다면 아래와 같은 명령을 사용으로 전송 가능합니다.

```
AT+SKTPDAT=1,"telemetry",0
> {"temperature":30, "humidity":20}
OK
```
AT+SKTPDAT=1,"telemetry",0를 입력하면 '>' 프롬프트가 활성화 되면 데이터를 입력합니다.
데이터 입력이 완료된 이후 'Ctrl+Z'를 입력해 데이터 입력이 완료되었음을 모듈에 알리면 ThingPlug로 데이터가 전송됩니다.



<a name="Step-6-ThingPlug-ATCommand"></a>
## ThingPlug AT 명령어

> 좀 더 상세한 AT 명령어 설명은 SKT Thingplug AT Command Manual에서 확인 하실 수 있습니다.
> * [ThingPlug AT 명령어][link-bg96-atcommand-manual]

### 1. ThingPlug 연동 상태 조회

**AT Command:** AT+SKTP

**Syntax:**

| Type | Syntax | Respones | Example
|:--------|:--------|:--------|:--------|
| Read | AT+SKTP | (version)<br>(status)<br>(protocol)<br>....<br>(mqtt_client_id) | AT+SKTP<br>OK<br>version:1.0.0<br>status:connected<br>protocol:MQTTS<br>thingplug_host:test.sktiot.com<br>thingplug_port:8883<br>keep-alive:300<br>cleansession:true<br>api_version:simple_v1<br>device_token:….b5bb<br>service_id:myservice<br>device_id:mydevice<br>mqtt_client_id:mydevice_ABCD |

**Defined values:**

| Parameter | Type | Description |
|:--------|:--------|:--------|
| (version) | String | ThingPlug AT command version |
| (status) | String | 접속 상태<br>disconnected<br>connected |
| (protocol) | String | MQTT: MQTT 방식으로 연결<br>MQTTS: MQTTS 방식으로 연결 |
| (thingplug_host) | String | ThingPlug 도메인 입력(테스트 서버와 상용사버 별도) |
| (thingplug_port) | Integer | 1883: MQTT 방식 연결<br>8883: MQTTS 방식 연결 |
| (keep-alive) | Integer | MQTT 연결시 keep-alive time(1000초 이하) |
| (cleansession) | Integer | 비접속시간동안 실행된 제어명령 수신 여부<br>1 : 사용<br>0 : 미사용 |
| (api_version) | String | ThingPlug API version |
| (device_token) | String | 디바이스 인증 Token. ThingPlug portal에서 발급 |
| (service_id) | String | 서비스 ID. ThingPlug portal에서 생성 |
| (device_id) | String | 디바이스 ID. ThingPlug portal에서 생성 |
| (mqtt_client_id) | String | MQTT Client ID, 자동생성 |




### 2. ThingPlug와 연결/연결해제

**AT Command:** AT+SKTPCON

**Syntax:**

| Type | Syntax | Respones | Example
|:--------|:--------|:--------|:--------|
| Test | AT+SKTPCON | OK<br>status:disconnected | - |
| Read | AT+QIACT? | +QIACT:<br>(1,(context_state),(context_type)[,(IP_address)]<br>[.....<br>+QIACT:<br>(16,(context_state),(context_type)[,(IP_address)]]<br><br>OK | AT+QIACT?<br><br>+QIACT:<br> 1,1,2,"2001:2D8:13B1:4A65:0:0:A248:8002"<br><br> OK |
| Write | AT+SKTPCON=(flag),(protocol),(thingplug_host),(thingplug_port),(keepalive),<br>(cleansession),(api_version),(device_token),(service_id),(device_id) | ThingPlug와 연결 | +SKTPCON=0 |


**Defined values:**

| Parameter | Type | Description |
|:--------|:--------|:--------|
| (flag) | Integer | ThingPlug에 연결 및 연결해제<br>1 : 연결<br>0 : 연결 해제 |
| (protocol) | String | MQTT: MQTT 방식으로 연결<br>MQTTS: MQTTS 방식으로 연결 |
| (thingplug_host) | String | ThingPlug 도메인 입력(테스트 서버와 상용사버 별도) |
| (thingplug_port) | Integer | 1883: MQTT 방식 연결<br>8883: MQTTS 방식 연결 |
| (keep-alive) | Integer | MQTT 연결시 keep-alive time(1000초 이하) |
| (cleansession) | Integer | 비접속시간동안 실행된 제어명령 수신 여부<br>1 : 사용<br>0 : 미사용 |
| (api_version) | String | ThingPlug API version |
| (device_token) | String | 디바이스 인증 Token. ThingPlug portal에서 발급 |
| (service_id) | String | 서비스 ID. ThingPlug portal에서 생성 |
| (device_id) | String | 디바이스 ID. ThingPlug portal에서 생성 |


### 3. ThingPlug 데이터 전송

**AT Command:** AT+SKTPDAT

**Syntax:**

| Type | Syntax | Respones | Example
|:--------|:--------|:--------|:--------|
| Test | AT+SKTPDAT | AT+SKTPDAT<br>OK<br>thingplug_data_type:telemetry<br>thingplug_format_type:JSON<br>thingplug_data: {"temperature":20,"humidity":46} |  |
| Write | AT+SKTPDAT=(flag),(thingplug_data_type),(thingplug_format_type) | ThingPlug로 데이터 전송 | AT+SKTPDAT=1,"telemetry",0<br>> {"temperature":20}<br>OK |

**Defined values:**

| Parameter | Type | Description |
|:--------|:--------|:--------|
| (flag) | Integer | 1: ThingPlug로 데이터 전송<br>파라메터가 없는경우 최근 데이터 조회 |
| (thingplug_data_type) | String | ThingPlug 에 전송할 data type<br>telemetry: 시계열 또는 이벤트 data 를 전송할 경우<br>attribute: 속성 data 를 갱신할 경우 |
| (thingplug_format_type) | Integer | 데이터 포맷 타입<br>0 : JSON<br>1 : CSV<br>1 : OFFSET |



### 4. ThingPlug 제어명령 결과 전송

**AT Command:** AT+SKTPRES

**Syntax:**

| Type | Syntax | Respones | Example
|:--------|:--------|:--------|:--------|
| Write | AT+SKTPRES=(flag),(cmd_type),<br>(rpc_id),(thingplug_cmd_result),<br>(thingplug_cmd_result_data) | 디바이스가 수신한 RPC를처리 후 결과를 ThingPlug로 전달 | +SKTPCMD=tp_user,674480006,1,[{"LED":"on:}]<br>AT+SKTPRES=1,tp_user,674480006,0<br>+SKTPCMD=tp_user,674480006,3<br>OK |

**Defined values:**

| Parameter | Type | Description |
|:--------|:--------|:--------|
| (flag) | Integer | 1: ThingPlug로 제어 결과를 전송 |
| (cmd_type) | String | 제어 명령 타입 |
| (rpc_id) | Integer | 제어 명령의 ID, 수신한 그대로 전달 |
| (thingplug_cmd_result) | Integer | 제어 명령 수행 결과<br>0: 성공<br>1: 실패 |

**제어 명령(cmd_type) 타입:**

| Parameter |  Description |
|:--------|:--------|
| tp_user | 사용자 정의 제어 |
| tp_remote | 통신모듈 제어 |
| tp_fwupgrade | 단말 펌웨어업그레이드 |
| tp_reset | 단말 초기화 |
| tp_reboot | 단말 재기동 |
| tp_clocksync | 단말 시간동기화 |
| tp_sigstatusreport | 단말 전파강도조회요청 |
| tp_upload | 단말 파일 업로드 |
| tp_download | 단말 파일 다운로드 |
| tp_install | 단말 App 설치요청 |
| tp_reinstall | 단말 App 재설치 요청 |
| tp_uninstall | 단말 App 제거요청 |
| tp_update | 단말 App 업데이트요청 |


### 5. ThingPlug 에러코드 조회

**AT Command:** AT+SKTPERR

**Syntax:**

| Type | Syntax | Respones | Example
|:--------|:--------|:--------|:--------|
| Read | AT+SKTPERR=(flag) | +SKTPE: (Error Code)<br><br>OK | AT+SKTPERR=1<br>+SKTPE: -13<br> OK<br><br>AT+SKTPERR=2<br>+SKTPE: Not Supported<br>OK |


**Defined values:**

| Parameter | Type | Description |
|:--------|:--------|:--------|
| (flag) | Integer | 에러 표시 방법<br>1 : 숫자 error code<br>2 : 문자열 error code |

**Error code:**

| error code | error description |
|:--------|:--------|
| 0 | No Error |
| -1 | Generic Error |
| -2 | MQTT Persistence Error |
| -3 | Disconnected |
| -4 | Max Messages Inflight |
| -5 | Bad UTF8 String |
| -6 | Null Parameter |
| -7 | MQTT Topic Name Truncated |
| -8 | MQTT Bad Structure |
| -9 | MQTT Bad QoS |
| -10 | MQTT No More Message IDs |
| -11 | MQTT Operation Incomplete |
| -12 | MQTT Max Buffered Messages |
| -13 | Not Supported |
| -14 | Invalid Parameter |
| -20 | Connection Timeout |
| -21 | Unacceptable Protocol Version |
| -22 | Identifier Rejected |
| -23 | Server Unavailable |
| -24 | Bad user name or password |
| -25 | Not Authorized |
| -30~-99 | User Defined Error |
| 100~122 | Platform Error |


<a name="ReadMore"></a>
## 더 보기

* MBED 기반의 Cat.M1 ThingPlug 연동 가이드 (준비 중)
* Arduino 기반의 Cat.M1 ThingPlug 연동 가이드 (준비 중)


 [mbed-getting-started]: https://
 [skt-iot-portal]: https://www.sktiot.com/iot/developer/guide/guide/catM1/menu_05/page_01
 [link-mbed-compiler]: https://ide.mbed.com/compiler/
 [link-nucleo-l476rg]: https://os.mbed.com/platforms/ST-Nucleo-L476RG/
 [link-bg96-atcommand-manual]: https://www.quectel.com/UploadImage/Downlad/Quectel_BG96_AT_Commands_Manual_V2.1.pdf
 [link-bg96-tcp-manual]: https://www.quectel.com/UploadImage/Downlad/Quectel_BG96_TCP(IP)_AT_Commands_Manual_V1.0.pdf

 [link-thingplug-portal]: https://portal.sktiot.com/intro 
 
