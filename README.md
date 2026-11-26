# Fabless Intership Project

TOPST(D3-G, VCP-G, AI-G) 플랫폼을 기반으로 차량용 SoC 개발 환경을 구성하고, FreeRTOS를 적용하여 동작을 검증합니다.

[개인 이력서](https://github.com/REDNeOnSpade/-)

## 활동 내역
- WSL2 및 Ubuntu 기반 Linux 개발 환경 세팅
- 크로스 컴파일 툴체인 및 `FreeRTOS-VCP` 프로젝트 구성
- Git / GitHub을 활용한 환경 세팅 및 실습 기록 관리

<details>
  <summary><b> 가변저항에 따른 ADC 출력 변화</b></summary>
  <br>
  가변 저항 범위 [0~4095Ω]

  
  <img width="799" height="631" alt="ADC_Pot - Mobaxterm" src="https://github.com/user-attachments/assets/adc704fc-bba8-415b-9a26-f7f95b4dfdc6" />




  ```python
  #include <adc.h>
#include <stdint.h>
#include "pot.h"

#define ADC_TYPE        0
#define ADC_MODULE      0 // 사용할 ADC Module 번호
#define ADC_CHANNEL     ADC_CHANNEL_3 // 측정할 채널

void Potentiometer_Run(void)
{
    uint32_t adc_val = 0; // ADC 변환 결과(12bit data)를 저장할 변수

    ADC_Init(ADC_TYPE, ADC_MODULE); // ADC 초기화

    while (1)
    {
        adc_val = ADC_Read(ADC_CHANNEL, ADC_MODULE, 0); // ADC 값 읽기
        adc_val &= 0xFFF; // 상위 상태 비트 제거 -> 하위 12bit(실제 ADC 값)만 추출

        mcu_printf("[ADC] CH%d = %4d\n", ADC_CHANNEL, adc_val); // 결과를 UART 콘솔로 출력

        SAL_TaskSleep(100);
    }
}

  ```
</details>

<details>
  <summary><b>Zonal Architecture 기반 D3-G ↔ VCP-G CAN 통신 및 구동·조향 제어</b></summary>
  <br>

- **개발 환경**: Telechips TOPST (D3-G / VCP-G, TCC70xx MCU BSP V1.0.0), FreeRTOS, C / Python, Qt
- **통신 및 제어 인터페이스**: CAN (Transceiver 연동), PDM/PWM, GPIO, UART (MobaXterm)

#### 1. 시스템 개요
- 상위 제어기(D3-G)에서 CAN 버스로 주행·조향 제어 명령을 송신하고, 하위 존 제어기(VCP-G)가 FreeRTOS 태스크 및 메시지 큐(`xQ_MotorSpeed`, `xQ_MotorWheel`)를 통해 DC 모터와 서보 모터를 실시간 제어하는 프로토타입을 구현했습니다.

#### 2. 구현 내용
| 구성 요소 | 역할 및 구현 사항 |
| :--- | :--- |
| **CAN 송수신 연동** | D3-G 측 `IPC_Example.py` 스크립트로 CAN 프레임 송신 및 트랜시버 경유 VCP-G 연동 구성 |
| **구동 모터 제어** | `MotorSpeedTask`, `ESC_SetAngle256`, `MotorA_Set` / `MotorB_Set` 기반 속도·듀티 및 방향 제어 |
| **조향 서보 제어** | `MotorWheelTask`, `ConfigureServoPWM` 기반 입력값 매핑 및 50Hz(20ms) PWM 펄스 출력 |

</details>

<details>
  <summary><b>CNN 경량화 및 정확도 튜닝 (MNIST)</b></summary>
  <br>

- **실습 환경**: Google Colab / TensorFlow (Keras)
- **실습 자료**: [Colab 환경에서 CNN MNIST 실습 노트북](https://drive.google.com/file/d/1Pohbtm0vZiJonpCf-VilCamU6s-Fud5W/view?usp=sharing)

#### 1. 실험 개요
- 기본 3단 `Conv2D + MaxPool2D` 구조에서 **모델 경량화(파라미터 수 최소화)**를 시도하고, 극단적 축소 시 발생하는 학습 저하 원인을 분석하여 적정 균형점을 도출했습니다.

#### 2. 주요 트러블슈팅 및 실험 결과
| 모델 구성 | 총 파라미터 | 정확도(Acc) | 분석 내용 |
| :--- | :---: | :---: | :--- |
| **원본 모델** (`filters=32, k=3`) | 21,706개 | ~99% | 충분한 특징 추출로 높은 정확도 확보 |
| **극소형 모델** (`filters=1, k=3`) | 130개 | 11.24% | 채널 수 부족으로 **Dying ReLU** 발생 및 특징 표현력 한계로 무작위 예측 수준에 머묾 |
| **최소 유효 모델** (`filters=4, k=3`) | 706개 | 정상 수렴 | 최소한의 다중 필터 확보 및 입력 정규화(`1/255`)로 학습 정상화 |

#### 3. 핵심
- **Dying ReLU 방지**: 단일 필터(`filters=1`) 구조에서는 초기 가중치가 음수로 밀릴 경우 전체 활성값이 `0`이 되어 기울기 전파가 중단됨을 확인
- **정확도 확보 조건**: 다중 채널(`filters ≥ 4~16`)을 통한 특징 분리, 입력 데이터 정규화(`0~1` 스케일링), 라벨 형태에 맞는 손실 함수(`sparse_categorical_crossentropy`) 매칭이 필수적임

</details>

## Location
### PIXELPLUS


경기도 성남시 수정구 금토로 40번길 30
## Term
2026.11.16 - 2026.12.23



