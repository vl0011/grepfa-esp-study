<!-- TOC -->
* [esp_chip_info](#espchipinfo)
  * [구성](#구성)
  * [model](#model)
  * [feature flags](#feature-flags)
    * [ex)](#ex-)
      * [기능 정보 얻기](#기능-정보-얻기)
      * [버전 정보 얻기](#버전-정보-얻기)
      * [플래시 메모리 정보 얻기](#플래시-메모리-정보-얻기)
<!-- TOC -->

# esp_chip_info

## 구성

```c++
typedef struct {
    esp_chip_model_t model;  // 칩 모델 정보
    uint32_t features;       // 기능, CHIP_FEATURE_x 플래그랑 비트연산 해서 구하면 됩니다.
    uint16_t revision;       // 리비전 번호
    uint8_t cores;           // CPU 코어 수
} esp_chip_info_t;
```

## model

```c++
typedef enum {
    CHIP_ESP32  = 1, // ESP32
    CHIP_ESP32S2 = 2, // ESP32-S2
    CHIP_ESP32S3 = 9, // ESP32-S3
    CHIP_ESP32C3 = 5, // ESP32-C3
    CHIP_ESP32H2 = 6, // ESP32-H2
    CHIP_ESP32C2 = 12, // ESP32-C2
} esp_chip_model_t;
```

## feature flags

```c++
#define CHIP_FEATURE_EMB_FLASH      BIT(0)      // embedded flash memory
#define CHIP_FEATURE_WIFI_BGN       BIT(1)      // 2.4GHz WiFi
#define CHIP_FEATURE_BLE            BIT(4)      // BLE
#define CHIP_FEATURE_BT             BIT(5)      // Bluetooth Classic
#define CHIP_FEATURE_IEEE802154     BIT(6)      // IEEE 802.15.4
#define CHIP_FEATURE_EMB_PSRAM      BIT(7)      // embedded psram
```

### ex)
#### 기능 정보 얻기
```c++
/*
 * 삼항연산자
 * 클래식 블루투스를 지안하면 "/BT", 아닐경우 "" 가 리턴 됨
 */
(chip_info.features & CHIP_FEATURE_BT) ? "/BT" : ""
```

#### 버전 정보 얻기

```c++
/*
 * 메이저, 마이너 버전을 얻고, 출력하는 예제
 */
unsigned major_rev = chip_info.revision / 100;
unsigned minor_rev = chip_info.revision % 100;
printf("silicon revision v%d.%d, ", major_rev, minor_rev);
```

#### 플래시 메모리 정보 얻기
```c++
/*
 * 플래시메모리 사이즈와 free heap size를 출력 한다.
 */
if(esp_flash_get_size(NULL, &flash_size) != ESP_OK) {
    printf("Get flash size failed");
return;
}

printf("%" PRIu32 "MB %s flash\n", flash_size / (uint32_t)(1024 * 1024),
(chip_info.features & CHIP_FEATURE_EMB_FLASH) ? "embedded" : "external");

printf("Minimum free heap size: %" PRIu32 " bytes\n", esp_get_minimum_free_heap_size());

```