# mkigor_BMx280
Lightweight and short library for Bosch sensor BME280 and BMP280 (maybe BME680) for Arduino.
It is not pretende to the most optimal code, but is example of learning C++ OOP.
Classes, Encapsulating metods (functions), Inheritance classes, 

It operate and return values of t,p,h at once in structure variable.
By default, lib focused on one time measuring (forced mode, but normal available too) 
with max oversampling x16 of t,p,h, and filter x16. It's suitable for weather station.

Function => `uint8_t check(uint8_t _lv_i2caddr)`<BR>
It must be call 1st, becouse it checks present sensor!<BR>
check connection with i2c sensor by address, and makes SW reset.
fn Input byte I2C address of sensor BME280 or BMP280 (0x76 or 0x77).<BR>
fn return byte: 0 if not present sensor or CHIP CODE.<BR>
CHIP CODEs: 0x58=>BMP280, 0x60=>BME280, 0x61=>BME680.<BR>
note: i2c address 0x77 possible for BMP280 or BME280 or BME680 or MS5607,MS5611,MS5637 CHECK It.

Function => `void begin()`<BR>
setup bmp280 or bme280 to FORCED MODE, with x16 oversampling for T,P,H and x16 filter.<BR>

Function => `void begin(uint8_t mode, uint8_t t_sb, uint8_t filter, uint8_t osrs_t, uint8_t osrs_p, uint8_t osrs_h)`<BR>
init bme280 with Your parameters:<BR>
mode = NOR_MODE or FOR_MODE; <BR>
t_sb = standby (sleep) time, from SB_500US to SD_1000MS;<BR>
filter  =  value of filterring - FIL_OFF, FIL_x2, FIL_x4, FIL_x8, FIL_x16;<BR>
osrs_t, osrs_p, osrs_h = oversampling value for T,P,H - OS_OFF, OS_x1, OS_x2, OS_x4, OS_x8, OS_x16.<BR>

Function => `void do1Meas(void)`<BR>
makes 1 measurement and goes to sleep. You should make delay for waitung result.
Acording to mode, sleep time, oversamlinhg value it can takes 200 ms. It's possible use vTaskDelay(200) in FreeRTOS.<BR>

Function => `bool isMeas(void)`<BR>
returns TRUE while the bme280 IS MEASuring, otherwise FALSE.<BR>


```c++
struct tph_srtu {
  float temp1;
  float pres1;
  float humi1;
};
```
Function => `tph_stru readTPH(void)`<BR>
It DOES NOT make measurement, only read and decoding value T,P,H for one I2C request and return variable  structure. Functions don't use of delay, it should do it manually for benefits of using FreeRTOS.
Or usuing CPU resources for checkking results. Max measuring time takes about 200 mS, but You can check it.<BR>

I used oficial Bosch datasheet bme280 and examples for making this lib:<BR>
https://github.com/GyverLibs/GyverBME280<BR>
https://github.com/farmerkeith/BMP280-library/<BR>
https://github.com/farmerkeith/I2CScanner/<BR>
