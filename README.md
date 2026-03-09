# 아두이노 (Arduino)

* 아두이노: 마이컴 보드, 통합 개발 환경(IDE)을 의미함
  - I2C, SPI, 시리얼 통신 등을 이용해 주변 장치와의 입출력 통신
  - 통합 개발 환경을 통해 마이컴 보드 제어 로직 구현
  - 센서, 구동장치, 카메라, LED, 스피커 등 다양한 장치를 부착할 수 있음
  - 확장보드(실드)를 통해 이더넷, 3G 통신, XBee 등의 통신이 가능함
  - [아두이노 홈페이지](https://www.arduino.cc/)는 여기를 참조

* 마이컴 보드의 종류
  | 아두이노 사양 | 우노 R3 | 레오나르도 | 메가 2560 | 듀에 | 프로
  | ------------- | ------- | ---------- | --------- | ---- | ---- |
  | 마이크로프로세서 | ATmega328 | Atmega32u4 | Atmega2560 | AT91SAM3X8E | Atmega168/328 |
  | 동작 전압 | 5V | 5V | 5V | 3.3V | 3.3V/5V |
  | 권장 입력 전압 | 7-12V | 7-12V | 7-12V | 7-12V | 3.35-12V(3.3V) / 5-12V(5V) |
  | 제한 입력 전압 | 6-20V | 6-20V | 6-20V | 6-20V | 6-20V |
  | 디지털 입출력 포트 개수 | 14 (6번은 PWM 출력) | 20 | 54 (15번은 PWM 출력) | 54 (12번은 PWM 출력) | 14 (6번은 PWM 출력) |
  | PWM 채널 | 6 | 7 | 15 | 12 | 6 |
  | 아날로그 입출력 포트 개수 | 6 | 12 | 16 | 입력: 12, 출력: 2 (DAC) | 6 |
  | 입출력 포트의 전류 | 40mA | 40mA | 40mA | 130mA | 40mA |
  | 공급 가능한 최대 전류 | 50mA | 50mA | 50mA | 800mA | - |
  | 플래시 메모리 | 32KB (0.5KB: 부트로더) | 32KB (4KB: 부트로더) | 256KB (8KB: 부트로더) | 512KB | 16KB (168) / 32KB (328) |
  | SRAM | 2KB | 2.5KB | 8KB | 96KB (2뱅크: 64KB, 32KB) | 1KB (168) / 2KB (328) |
  | EEPROM | 1KB | 1KB | 4KB | - | 512B (168) / 1KB (328) |
  | 클럭 주파수 | 16MHz | 16MHz | 16MHz | 84MHz | 8MHz (3.3V) / 16MHz (5V) |

* 실물 장비가 없어도 시뮬레이션이 가능함
  - [Autodesk Tinkercad](https://www.tinkercad.com/circuits)
  - [wokwi](https://wokwi.com/)
  - ...

* 요즘은 많은 칩들이 아두이노 프레임워크로 표준화되어 있음
  - Atmega, ESP32, STM32 등 많은 칩들이 아두이노 프레임워크로 작동할 수 있도록 되어 있음 (Hardware Abstraction Layer)
  - 아두이노 프레임워크 외에도 MicroPython, CircuitPython도 있음
  - 하지만 칩의 최고의 성능을 끌어내려면 제조사 전용 SDK(ESP-IDF, STM32Cube 등)를 이용해야 함

* 칩에 따른 특성 차이
  | 구분 | Arduino Uno | ESP32 | STM32 (Blue Pill 등) |
  | ---- | ----------- | ----- | -------------------- |
  | 제조사 | Microchip (Atmel) | Espressif Systems | STMicroelectronics |
  | 연산 속도 | 16 MHz (느림) | 240 MHz (매우 빠름) | 72 MHz ~ 400 MHz 이상 |
  | 통신 | 별도 모듈 필요 | Wi-Fi & Bluetooth | 내장산업용 통신(CAN 등) 강점 |
  | 전압 | 5V | 3.3V | 3.3V |
  | 주요 용도 | 간단한 교육용, 입문 | IoT, 무선 제어 | 고성능 드론, 산업용 장비 |

## 아두이노 마이컴 보드 인터페이스

* 다음은 아두이노 우노 R3를 예시로 보여주는 입출력 인터페이스이다.

  - 실물 사진

    <img width="804" height="603" alt="image" src="https://github.com/user-attachments/assets/9abbe6c8-ded4-4778-ab27-0a39173ad5b9" />

  - 핀맵

    <img width="1280" height="905" alt="image" src="https://github.com/user-attachments/assets/aac8a98d-3213-4798-abde-3f4001475345" />

  - __포트 구성은 다음과 같다.__
    * 0(RX), 1(TX): 시리얼 통신 포트 (UART), USB를 통해 PC와 UART 통신을 할 때에는 여기에 연결된 장치를 미리 떼어놓아야 함 (not in sync 오류 발생 원인)
    * 0-13: 디지털 입출력 포트 (단, ~ 마크가 붙은 포트 3, 5, 6, 9, 10, 11는 아날로그 출력(PWM) 포트)
    * A0-A5: 아날로그 입력 포트 (경우에 따라서 디지털 입출력 포트 D14-D19로도 사용 가능)
    * 3.3V/5V: 전원
    * GND: 접지
    * Vin: 전원 입력 포트
    * 보드 내부 LED: L (__디지털 출력 13번과 연결되어 있음__), TX, RX (시리얼 통신을 할 때 점멸됨)
    * 그 외 I2C, SPI 포트

  - 아두이노 전원 공급 방법
    * USB 커넥터로 공급: 5V 500mA
    * DC 전원 플러그로 공급: 7~12V
    * Vin에 DC 전원 공급: 7~12V (건전지 등)

* 아두이노 통신 종류
  - 아날로그 입출력
    * 아날로그 입력: 0~5V (0부터 1023까지의 정수로 표현)
    * 아날로그 출력 (PWM: 펄스 폭 변조): 0~5V (0부터 255까지의 정수로 표현)
  - 디지털 입출력
    * 디지털 입/출력: LOW(정수 0, 0V), HIGH(정수 1, 5V 또는 3.3V)
    * 시리얼 통신
      - UART(Universal Asynchronous Receiver-Transmitter): 비동기식, 1:1 연결
        * SoftwareSerial 라이브러리 사용
        * 시리얼 통신 중에서는 거리가 제일 멀리까지 가능함
      - I2C(Inter-Integrated Circuit): 동기식 저속 통신 (100~400kbps), 마스터-슬레이브 Half-duplex 통신 (Multi-Master)
        * Wire 라이브러리 사용
        * SCL (Serial Clock) 선과 SDA (Serial Data) 선 2개만으로도 통신 가능함
        * 장비 주소 값을 이용하여 원하는 장비와 통신이 가능함
        * 통신 거리는 짧은 편
      - SPI(Serial Peripheral Interface): 동기식 고속 통신 (수십 Mbps), 마스터-슬레이브 Full-duplex 통신 (Multi-Slave)
        * SPI 라이브러리 사용
        * SCK (Serial Clock) 선, MOSI (Master Out Slave In) 선, MISO (Master In Slave Out) 선, SS/CS (Slave Select) 선을 이용함
        * 통신할 장비마다 SS 선을 하나씩 연결해야 하며 그 선을 이용하여 물리적으로 통신할 장비를 선택함
        * 속도가 빠르지만 회로가 복잡해짐
        * 통신 거리는 매우 짧은 편

## 아두이노 코드 문법

* 기본적으로 C/C++ 문법을 사용하며 다음 키워드가 예약되어 있음
  - `DEC`, `BIN`, `HEX`, `OCT`: Serial.print()과 같은 출력 함수에서 데이터를 어떤 진법으로 보여줄지 결정함
  - `PI`(원주율 π), `HALF_PI`(π/2), `TWO_PI`(2π): 원주율 관련 상수
  - `true`, `false`: boolean 상수
  - `boolean` 또는 `bool`, `char`(1바이트 정수/문자), `short`(2바이트 정수), `int`(우노 기준: 2바이트 정수), `long`(4바이트 정수), `float`(4바이트 실수), `double`(우노 기준: 4바이트 실수)
  - `signed`, `unsigned`: 부호 있는/없는 자료형
  - `null`: 널 포인터
  - `String`: 아두이노에서 제공하는 문자열 클래스
  - `struct`: 구조체 선언
  - `class`: 클래스 선언
  - `this`: 객체 자신을 가리키는 포인터
  - `const`: 상수 선언
  - `static`: 정적 변수 선언, 함수가 종료되어도 값이 유지됨
  - `new`: 인스턴스 선언

* 연산 함수는 다음과 같다.
  - `abs`: 절대값
  - `cos`: 코사인
  - `sin`: 사인
  - `tan`: 탄젠트
  - `acos`: 아크코사인
  - `asin`: 아크사인
  - `atan`: 아크탄젠트
  - `degrees`: radian 값을 degree 값으로 변환
  - `radians`: degree 값을 radian 값으로 변환
  - `exp`: 밑이 e인 지수 함수
  - `floor`: 소수점 이하 버림
  - `round`: 소수점 이하 반올림
  - `log`: 밑이 e인 자연로그 값 계산
  - `log10`: 밑이 10인 상용로그 값 계산
  - `max`: 최대값
  - `min`: 최소값
  - `random`: 랜덤값 반환, `randomSeed` 함수를 이용해야만 랜덤값 생성 순서를 바꿀 수 있음
  - `sq`: 제곱 계산
  - `sqrt`: 제곱근 계산
  - `map(value, fromLow, toLow, toHigh)`: 특정 범위의 값을 다른 범위의 값으로 비율에 맞춰 변환하며 long 타입만 가능함, result = (value - fromLow) * (toHigh - toLow) / (fromHigh - fromLow) + toLow

### 아두이노에서 제공하는 특수 함수

* 실행을 하기 위한 가장 기본적인 2가지 함수
  - `void setup()`: 전원을 켤 때, reset 스위치를 눌렀을 때 딱 한 번만 실행됨
  - `void loop()`: setup 이후에 실행되며 동작을 무한 반복함

* 핀 입출력을 위한 함수
  - `pinMode(핀번호, OUTPUT | INPUT | INPUT_PULLUP)`: 특정 핀을 어떤 용도로 쓸지 지정함 (INPUT_PULLUP은 입력핀에 자체 풀업저항(20~50kΩ)을 적용한다는 뜻이며 신호가 들어오면 0, 들어오지 않으면 1)
  - `millis()`: 프로그램 실행한 이후 경과한 시간을 밀리초 단위로 반환 (unsinged long), 약 49.7일이 초과되면 오버플로우 발생함
  - `digitalWrite(핀번호, HIGH | LOW)`: 디지털 핀에 HIGH(5V/3.3V) 또는 LOW(0V) 신호 출력
  - `digitalRead(핀번호)`: 디지털 핀의 상태를 읽음 (int: HIGH 또는 LOW)
  - `analogWrite(핀번호, 정수)`: 아날로그 핀에 전압 출력 (정수 0~255)
  - `analogRead(핀번호)`: 아날로그 핀의 값을 읽음 (정수 0~1023)
  - `analogReference(DEFAULT | INTERNAL | EXTERNAL)`: 아날로그 입력의 기준 값 결정
    * DEFAULT: 보드 동작 전압 (5V 또는 3.3V)
    * INTERNAL: 내장 정밀 전압 (우노 기준: 1.1V). 센서 값이 작을 때 정밀도를 높이기 위해 사용함
    * EXTERNAL: AREF 핀에 직접 공급한 전압을 기준으로 정함
  - `pulseIn(핀번호, value)`: 특정 핀에 지정한 신호(HIGH 또는 LOW)가 들어올 때까지 기다렸다가, 그 신호가 유지된 시간을 마이크로초 단위 값으로 반환 (unsigned long) (주로 초음파 센서에서 사용), 타임아웃(기본 1분) 초과시 0 반환

* 소리 재생을 위한 함수
  - `tone(핀번호, 헤르츠값, 밀리초)`: 지정한 핀에 특정 주파수의 사각형파를 발생시킴 (주의: 아두이노는 한 번에 하나의 핀에서만 소리를 낼 수 있음)
  - `noTone(핀번호)`: tone() 함수로 발생시킨 소리 취소

* 인터럽트 함수
  - `attachInterrupt(인터럽트번호, 함수, 모드)`: 인터럽트 번호에 해당하는 핀에 대해 값에 따라 함수를 실행함
    * 인터럽트: 0번은 D2핀, 1번은 D3핀 (디지털 입력만 가능)
    * 함수: 반환값, 함수 인자가 없는 것만 가능함
    * 모드: LOW, CHANGE, RISING, FALLING, HIGH
  - `detachInterrupt(인터럽트번호)`: 설정된 인터럽트를 해제합니다.
  - `noInterrupts()`: 중요한 연산 중에 방해받지 않도록 모든 인터럽트를 일시적으로 중지합니다. (Critical Section 보호)
  - `interrupts()`: 중지했던 인터럽트를 다시 허용합니다.

* 비트 단위 직렬 통신 (Shift I/O): 통신 핀이 부족할 때 '시프트 레지스터(74HC595 등)' 같은 칩을 제어하기 위해 사용함
  - `shiftOut(dataPin, clockPin, bitOrder, value)`: 데이터를 한 비트씩 밖으로 밀어냅니다. (출력 확장)
    * bitOrder: MSBFIRST(최상위 비트부터) 또는 LSBFIRST(최하위 비트부터)
  - `shiftIn(dataPin, clockPin, bitOrder)`: 외부 장치로부터 데이터를 한 비트씩 읽어옵니다. (입력 확장)

* 기타 유용한 함수
  - `delay(밀리초)`: 지정된 밀리초 동안 프로그램 일시정지

* 시리얼 통신
  - `SoftwareSerial(int rxPin, int txPin)`: 통신 포트를 위한 핀 번호 지정
  - `Serial.begin(baud_rate)`: 시리얼 통신 시작 및 속도 설정
  - `Serial.end()`: 시리얼 통신 종료
  - `Serial.available()`: 시리얼 포트로 도착한 버퍼 바이트 수를 반환
  - `Serial.read()`: 수신 데이터의 1번째 바이트 값 반환, 데이터가 없으면 -1 (함수 실행 후 포인터 이동 O)
  - `Serial.peek()`: 수신 데이터의 1번째 바이트 값 반환, 데이터가 없으면 -1 (함수 실행 후 포인터 이동 X)
  - `Serial.flush()`: 수신 버퍼 지움
  - `Serial.print(val, format)`: 데이터를 사람이 읽을 수 있는 문자로 변환하여 전송합니다. (value: int, char 등, format: DEC, BIN, HEX, OCT)
  - `Serial.println(...)`: print와 같지만 끝에 줄바꿈 문자(\r\n)를 추가함
  - `Serial.write(val)`: 데이터를 변환 없이 원 바이트(Raw Data) 그대로 전송합니다. 이미지나 바이너리 파일 전송 시 사용함
  - `Serial.write(buf, len)`: 배열에서 지정한 길이만큼 바이트 단위로 연속 전송

* LCD 장치의 경우, 제조사에 따라 추가해야 할 헤더 파일, 함수가 다르기 때문에 관련 문서를 참조할 것
