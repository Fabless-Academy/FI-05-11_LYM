# Fabless Intership Project

TOPST(D3-G, VCP-G, AI-G) 플랫폼을 기반으로 차량용 SoC 개발 환경을 구성하고, FreeRTOS를 적용하여 동작을 검증합니다.

[개인 이력서](https://github.com/REDNeOnSpade/-)

## 활동 내역
- WSL2 및 Ubuntu 기반 Linux 개발 환경 세팅
- 크로스 컴파일 툴체인 및 `FreeRTOS-VCP` 프로젝트 구성
- Git / GitHub을 활용한 환경 세팅 및 실습 기록 관리

## Location
### PIXELPLUS
<img width="1463" height="941" alt="PixelPlus_Location" src="https://github.com/user-attachments/assets/26618352-f7bb-4c97-b2e3-afb5208f68e4" />

경기도 성남시 수정구 금토로 40번길 30
## Term
2026.11.16 - 2026.12.23

## 상세 내역

가변저항에 따른 ADC 출력 변화
<details>
  <summary><b> 클릭하여 상세 실행 결과 보기</b></summary>
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
