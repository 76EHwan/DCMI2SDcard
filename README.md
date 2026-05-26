# STM32H743VITX DCMI Camera to SD Card & LCD

이 프로젝트는 STM32H743VIT6 마이크로컨트롤러의 **DCMI(Digital Camera Interface)**를 활용하여 카메라 모듈로부터 영상 데이터를 수신하고, 이를 TFT LCD에 실시간으로 출력하며, SD 카드에 이미지 파일로 저장하는 시스템입니다.

## 개요 (Overview)
고성능 ARM Cortex-M7 코어를 탑재한 STM32H743VIT6을 기반으로, 하드웨어 카메라 인터페이스(DCMI)와 DMA를 이용해 영상을 고속으로 획득합니다. 획득한 영상은 SPI 기반의 TFT LCD 화면에 실시간으로 렌더링되며, FatFs 파일 시스템을 통해 SD 카드에 저장할 수 있습니다.

## 주요 하드웨어 구성 (Hardware Components)
* **MCU**: STM32H743VIT6 (WeAct Studio 보드 등 호환)
* **카메라 모듈 (Camera)**: 
  * OV2640, OV5640, OV7670, OV7725 드라이버 지원 (`Drivers/BSP/Camera/`)
* **디스플레이 (Display)**: 
  * ST7735 (0.96인치 / 1.8인치 지원) 또는 ST7789 기반 SPI TFT LCD (`Drivers/BSP/ST7735/`, `Drivers/BSP/ST7789/`)
* **저장장치 (Storage)**: 
  * MicroSD 카드 (SDMMC 하드웨어 인터페이스를 통해 연결)

## 주요 기능 (Key Features)
1. **DCMI 기반 고속 영상 획득 (`dcmi.c`)**:
   * 하드웨어 DCMI와 DMA를 연동하여 CPU의 부하를 최소화하면서 고해상도 영상 데이터를 안정적으로 수신합니다.
2. **실시간 모니터링 (LCD Display)**:
   * 수신된 카메라 데이터를 ST7735 또는 ST7789 LCD 컨트롤러에 전송하여 실시간 영상 스트리밍을 제공합니다.
3. **SD 카드 데이터 저장 (`fatfs.c`, `sd_diskio.c`)**:
   * SDMMC 인터페이스와 FatFs 미들웨어를 결합하여, 카메라로 캡처한 이미지 데이터를 파일 형태로 SD 카드에 안정적으로 기록합니다.

## 프로젝트 구조 (Directory Structure)
* `Src/` & `Inc/`: 메인 어플리케이션 로직, DCMI/SDMMC/SPI 등 주변장치 초기화 및 제어 코드 (`main.c`, `dcmi.c`, `sdmmc.c`, `spi.c` 등)
* `Drivers/BSP/Camera/`: 옴니비전(OmniVision) 사의 다양한 카메라 이미지 센서(OV2640, OV5640, OV7670, OV7725) 제어를 위한 레지스터 맵 및 드라이버.
* `Drivers/BSP/ST7735/` & `ST7789/`: SPI 방식의 TFT LCD 화면 출력을 위한 그래픽 및 초기화 드라이버.
* `Drivers/BSP/SDcard/`: SD 카드 제어를 위한 보드 지원 패키지 드라이버.
* `Middlewares/Third_Party/FatFs/`: 파일 입출력을 위한 FAT 파일 시스템 라이브러리.
