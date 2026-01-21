# I2C CLCD

<img width="706" height="170" alt="129" src="https://github.com/user-attachments/assets/8fdba439-49cd-47d9-8069-223c4ea9305b" />

  * PB8 - SCL
  * PB9 - SDA

# 사용자 문자
<img width="720" height="326" alt="image" src="https://github.com/user-attachments/assets/a5df36a0-d278-4d36-a51c-f67a2567731b" />

# 코드 추가
```c
//User code begin 0에 추가
// 커스텀 문자 패턴 정의 (5x8 도트, 8바이트)
// 하트 모양
const uint8_t heart[8] = {
    0b00000,
    0b01010,
    0b11111,
    0b11111,
    0b11111,
    0b01110,
    0b00100,
    0b00000
};

// 스마일 모양
const uint8_t smiley[8] = {
    0b00000,
    0b00000,
    0b01010,
    0b00000,
    0b10001,
    0b01110,
    0b00000,
    0b00000
};

// 화살표 위
const uint8_t arrow_up[8] = {
    0b00100,
    0b01110,
    0b11111,
    0b00100,
    0b00100,
    0b00100,
    0b00100,
    0b00000
};

// 화살표 아래
const uint8_t arrow_down[8] = {
    0b00000,
    0b00100,
    0b00100,
    0b00100,
    0b00100,
    0b11111,
    0b01110,
    0b00100
};

// 온도계 모양
const uint8_t thermometer[8] = {
    0b00100,
    0b01010,
    0b01010,
    0b01010,
    0b01110,
    0b11111,
    0b11111,
    0b01110
};

// 종 모양
const uint8_t bell[8] = {
    0b00100,
    0b01110,
    0b01110,
    0b01110,
    0b11111,
    0b00000,
    0b00100,
    0b00000
};

// 배터리 모양
const uint8_t battery[8] = {
    0b01110,
    0b11011,
    0b10001,
    0b10001,
    0b10001,
    0b10001,
    0b10001,
    0b11111
};

// 스피커 모양
const uint8_t speaker[8] = {
    0b00001,
    0b00011,
    0b01111,
    0b01111,
    0b01111,
    0b00011,
    0b00001,
    0b00000
};

/**
 * @brief CGRAM에 커스텀 문자 등록
 * @param location: 문자 번호 (0~7)
 * @param pattern: 8바이트 패턴 배열
 */
void LCD_CreateChar(uint8_t location, const uint8_t *pattern) {
    if (location > 7) return;  // 최대 8개 문자만 가능
    
    // CGRAM 주소 설정: 0x40 + (location * 8)
    LCD_CMD(0x40 | (location << 3));
    
    // 8바이트 패턴 데이터 쓰기
    for (int i = 0; i < 8; i++) {
        LCD_DATA(pattern[i]);
    }
    
    // DDRAM 모드로 복귀 (커서를 홈 위치로)
    LCD_CMD(0x80);
}

/**
 * @brief 커스텀 문자 출력
 * @param location: 등록된 문자 번호 (0~7)
 */
void LCD_PutCustomChar(uint8_t location) {
    if (location > 7) return;
    LCD_DATA(location);
}

/**
 * @brief 모든 커스텀 문자 등록
 */
void LCD_CreateAllCustomChars(void) {
    LCD_CreateChar(0, heart);
    LCD_CreateChar(1, smiley);
    LCD_CreateChar(2, arrow_up);
    LCD_CreateChar(3, arrow_down);
    LCD_CreateChar(4, thermometer);
    LCD_CreateChar(5, bell);
    LCD_CreateChar(6, battery);
    LCD_CreateChar(7, speaker);
}
```
```c
// User code begin 2 수정
/* USER CODE BEGIN 2 */
I2C_ScanAddresses();

LCD_INIT();

// 커스텀 문자 등록
LCD_CreateAllCustomChars();

// 예제 1: 커스텀 문자 표시
LCD_XY(0, 0);
LCD_PUTS("Custom Chars:");

LCD_XY(0, 1);
LCD_PutCustomChar(0);  // 하트
LCD_DATA(' ');
LCD_PutCustomChar(1);  // 스마일
LCD_DATA(' ');
LCD_PutCustomChar(2);  // 화살표 위
LCD_DATA(' ');
LCD_PutCustomChar(3);  // 화살표 아래
LCD_DATA(' ');
LCD_PutCustomChar(4);  // 온도계
LCD_DATA(' ');
LCD_PutCustomChar(5);  // 종
LCD_DATA(' ');
LCD_PutCustomChar(6);  // 배터리
LCD_DATA(' ');
LCD_PutCustomChar(7);  // 스피커

/* USER CODE END 2 */
```


