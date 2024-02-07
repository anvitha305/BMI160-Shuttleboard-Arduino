# BMI160-Shuttleboard-Arduino

This driver is for BMI160, BMM150 9DoF sensor breakout boards which are connected through the Arduino SPI.
Derived from the Intel's CurieIMU driver for the Arduino/Genuino 101 and the original BMI160 library for Arduino.

Intel's driver repository: https://github.com/01org/corelibs-arduino101/tree/master/libraries/CurieIMU
GitHub user hanzayou's BMI160 6DoF board Arduino driver: https://github.com/hanyazou/BMI160-Arduino

BMI160 Shuttleboard: https://www.bosch-sensortec.com/media/boschsensortec/downloads/shuttle_board_flyer/bst-dhw-fl022.pdf

## How to install
Copy all files of this project to the your Arduino IDE library folder.

Example:
```
cp ~/github/BMI160-Arduino ~/Documents/Arduino/libraries/
```
## Wiring

### SPI mode
You should connect some digital out pin to the CSB of the BMI160 and tell the number of the pin to the initialize method, begin().
![Screenshot](files/circuit.png)

### I2C mode
You should connect SDO/SA0 pin of the BMI160 to GND for default I2C address or tell the I2C address to the initialize method, begin().
![Screenshot](files/wiring2.png)

## Code
```
#include <BMI160Gen.h>

const int select_pin = 10;
const int i2c_addr = 0x68;

void setup() {
  Serial.begin(9600); // initialize Serial communication
  while (!Serial);    // wait for the serial port to open

  // initialize device
  BMI160.begin(BMI160GenClass::SPI_MODE, select_pin);
  //BMI160.begin(BMI160GenClass::I2C_MODE, i2c_addr);
}

void loop() {
  int gx, gy, gz;         // raw gyro values

  // read raw gyro measurements from device
  BMI160.readGyro(gx, gy, gz);

  // display tab-separated gyro x/y/z values
  Serial.print("g:\t");
  Serial.print(gx);
  Serial.print("\t");
  Serial.print(gy);
  Serial.print("\t");
  Serial.print(gz);
  Serial.println();

  delay(500);
}
```

## Compatibility

Board           |MCU         |tested works|doesn't work|not tested| Notes
----------------|------------|------------|------------|----------|-----
Arduino UNO     |ATmega328P  | X         |            |          |
Arduino 101     |Intel Curie |           |            | x        |
Arduino Leonardo|ATmega32u4  |           |            | x        | D7 pin for INT
Arduino M0 PRO  |ATSAMD21G   |           |            | x        | D7 pin for INT

Other boards which have the same MCU might be work well.
