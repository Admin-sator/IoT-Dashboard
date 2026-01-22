# IoT-Dashboard
Arduino UNO R4 Wifi IoT Dashboard with and modules
Requirements:


Arduino UNO R4 Wifi

A Computer for upload code

A Speaker Without amplifier

1 220Ω Resistor

Robotzade UNOKit


If you don't have Robotzade UNOKit, you will need the following other components:
Soil Moisture Sensor

Rain Sensor

2 220Ω Resistor(1reserve)

Sound Sensor

DHT11

LCD I2C(0x27 16,2)

HC-SR04

SG90 Servo(180° Servo)

2-pin non RGB Led(ong leg is the positive lead, and the short leg is the negative lead)

Potansiometer

Breadboard

Connections:

1)Robotzade UNOKit:

LCD to LCD connection female header

Arduino to Arduino section

HC-SR04 to HC-SR04 female header

Servo's cable to Servo connection male header, align the black cable with the "-" sign in UNOKit

End Module's pins to Soil Moisture module's pins

Rain Module to Rain Module female header

Sound Module to Sound Module female header

DHT11 to DHT11 female header


Road Pulling:

LCD's:
5V to 5V

SDA to Arduino's Analog 4

SCL to Arduino's Analog 5


HC-SR04's:

Trig to Arduino's Digital 4

Echo to Arduino's Digital 5


SG90(Servo):

5V female header to 5V

Signal to Arduino Digital 6


Speaker:

Speaker's black cable to negative line

Speaker's red cable to 220ohm Resistor to Arduino Digital 8


Led:

Led's Red female header to Arduino's Digital 9


Potansiometer:

Potansiometer's female header to Arduino's Analog 0


Soil Moisture:

Soil Moisture's Analog(A) female header to Analog 1


Rain Sensor:

Signal female header to Arduino's Analog 2


Sound:
Signal female header to Arduino's Analog 3

DHT11:
Signal female header to Arduino's Digital 2
