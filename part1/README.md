<!-- TOC -->
* [Part 1. Printf와 칩 정보 얻기](#part-1-printf와-칩-정보-얻기)
  * [Hello World](#hello-world)
    * [`printf`](#printf)
    * [`esp_chip_info_t`](#espchipinfot)
    * [`vTaskDelay`](#vtaskdelay)
    * [`esp_restart`](#esprestart)
    * [`monitor` 출력](#monitor-출력)
<!-- TOC -->

# Part 1. Printf와 칩 정보 얻기

## Hello World

### `printf`

PC 에서 C언어 `printf` 함수는 표준 출력에 문자열을 출력 한다.

`idf-py` 에서의 `printf` 함수는 시리얼 모니터에 값을 출력하는 역할을 한다.

`hello_world_main.c` 에 다음 함수는 `hello world` 를 출력 해준다.
```c++
    printf("Hello world!\n");
```


### `esp_chip_info_t`

다음 코드는 현재 `esp32` 칩의 하드웨어 정보를 얻어 온다.

사용중인 `esp32` 칩 종류에 따라 다른 결과를 보여준다.

```c++
    esp_chip_info_t chip_info;
    esp_chip_info(&chip_info);
```


`esp_chip_info_t` 는 아래와 같이 정의되어 있다.

```c++
typedef struct {
    esp_chip_model_t model;  // 칩 모델 정보
    uint32_t features;       // 기능, CHIP_FEATURE_x 플래그랑 비트연산 해서 구하면 됩니다.
    uint16_t revision;       // 리비전 번호
    uint8_t cores;           // CPU 코어 수
} esp_chip_info_t;
```

[칩 정보를 얻는 방법](./chip_info.md)

### `vTaskDelay`

```c++
    for (int i = 10; i >= 0; i--) {
        printf("Restarting in %d seconds...\n", i);
        vTaskDelay(1000 / portTICK_PERIOD_MS); // DELAY
    }
    printf("Restarting now.\n");
    fflush(stdout);
    esp_restart();
```

현재 테스크를 일시 정지하는 함수.

아래 함수는 1초간 현재 테스크를 일시정지 한다.

지연시간 동안 다른 테스크의 실행을 양보 한다.

`vTaskDelay(1000 / portTICK_PERIOD_MS);`

`millisecond` / `portTICK_PERIOD_MS` 를 해야 의도한 `millisecond` 만큼 테스크를 지연할 수 있다.

### `esp_restart`
`esp32` 를 제부팅 한다.


### `monitor` 출력
```text
Restarting now.
ESP-ROM:esp32s3-20210327
Build:Mar 27 2021

... bootstrap ...

W (263) spi_flash: Detected size(8192k) larger than the size in the binary image header(2048k). Using the size in the binary image header.
I (277) cpu_start: Starting scheduler on PRO CPU.
I (0) cpu_start: Starting scheduler on APP CPU.
Hello world!    # <<<<<<<
This is esp32s3 chip with 2 CPU core(s), WiFi/BLE, silicon revision v0.1, 2MB external flash
Minimum free heap size: 391944 bytes
Restarting in 10 seconds...
Restarting in 9 seconds...
Restarting in 8 seconds...
```

`print` 함수로 인해 `Hello world!` 가 출력됨을 알 수 있다.