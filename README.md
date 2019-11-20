

# Panasonic H Series Aquarea air-water heat pump protocol



## Connection details:
CN-CNT TTL UART 9600,8,E,1  \
Pin-out (from top to bottom) \
1 - +5V (250mV)  \
2 - 0-5V TX  \
3 - 0-5 RX  \
4 - +12V (250mV) \
5 - GND

CN-CNT female connector PAP-05V-S - JST Female Connector Housing - PA, 2mm Pitch, 5 Way, 1 Row - https://uk.rs-online.com/web/p/pcb-connector-housings/4766798/ \
Pre-made crimp leads 150 mm crimp-free end lead PA 2.0 can be used - https://uk.rs-online.com/web/p/pre-crimped-leads/5128721/ \
The Panasonic CZ-TAW1 appears to use a JST B05B-XASK-1 male header - https://uk.rs-online.com/web/p/pcb-headers/6027679/

Schematic how to connect ESP8266 (Tested with Adafruit HUZZAH ESP8266 as this example):

![](https://github.com/Egyras/Panasonic-H-Aquarea/blob/master/panasonic.jpg)



## Protocol info packet:

To get information from a heat pump, "magic" packet should be send to CN-CNT: 

`71 6c 01 10 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 12`

## Protocol bit decrypt info:


| Byte Number | Possible Value | Value decrypt | Value Description |
| ----- | ---- | ----- | -----:|
| 00 | 71 |   | Header  |
| 01 | c8 | Should be Data packet lenght  |  Header |
| 02 | 01|   | Header  |
| 03 | 10 |   | Header   |
| 04 | 56 | Force DHW status 56=off,96=on, 55 = heat pump off| Force dhw status + Heat pump on/off status|
| 05 | 55 |  Holiday mode status 55=off, 65=on | Holiday mode status |
| 06 | 62 | If 62 Heat+DHW, If 52 Only Heat, If 61 only DHW, If 69 Auto+DHW, If 63 Cool+DHW, If 53 Cool, If 59 Auto   | Mode status   |
| 07 | 49 | 49 = Off, 51 = level 1, 59 = level 2, 61 - level 3 For Power mode 49= Off, 4a - power mode 30min, 4b -60min, 4c-90 min | Quiet Mode status + Powerfull mode status |
| 08 | 00 |   | ? |
| 09 | 05 |   | ? |
| 10 | 00 |   | ? |
| 11 | 00 |   | ? |
| 12 | 00 |   | ? |
| 13 | 00 |   | ? |
| 14 | 00 |   | ? |
| 15 | 00 |   | ? |
| 16 | 00 |   | ? |
| 17 | 00 |   | ? |
| 18 | 00 |   | ? |
| 19 | 00 |   | ? |
| 20 | 19 |   | ? |
| 21 | 15 |   | ? |
| 22 | 11 |   | ? |
| 23 | 55 |   | ? |
| 24 | 16 |   | ? |
| 25 | 5e |   | ? |
| 26 | 55 |   | ? |
| 27 | 05 |   | ? |
| 28 | 09 |   | ? |
| 29 | 00 |   | ? |
| 30 | 00 |   | ? |
| 31 | 00 |   | ? |
| 32 | 00 |   | ? |
| 33 | 00 |   | ? |
| 34 | 00 |   | ? |
| 35 | 00 |   | ? |
| 36 | 00 |   | ? |
| 37 | 00 |   | ? |
| 38 | 80 | Convert to DEC 128-128 = 0  | Zone 1 water shift set Temperature For Heat Mode [°C] |
| 39 | 8f | Convert to DEC 143-128 = 15 in direct mode set temp  | Zone 1 water shift set Temperature For Cool Mode [°C] |
| 40 | 80 | Convert to DEC 128-128 = 0  | Zone 2 water shift set Temperature For Heat Mode [°C] |
| 41 | 8a | Convert to DEC 138-128 = 10  | Zone 2 water shift set Temperature For Cool Mode [°C] |
| 42 | b2 | Convert to DEC 178-128 = 50  | Tank Water Set Temperature [°C] |
| 43 | 71 | Convert to DEC 113-128 =-15   | Heat Shift for Holiday mode |
| 44 | 71 | Convert to DEC 113-128 =-15  | Heat Shift for Tank mode  |
| 45 | 97 |   | ? |
| 46 | 99 |   | ? |
| 47 | 00 |   | ? |
| 48 | 00 |   | ? |
| 49 | 00 |   | ? |
| 50 | 00 |   | ? |
| 51 | 00 |   | ? |
| 52 | 00 |   | ? |
| 53 | 00 |   | ? |
| 54 | 00 |   | ? |
| 55 | 00 |   | ? |
| 56 | 00 |   | ? |
| 57 | 00 |   | ? |
| 58 | 80 |   | ? |
| 59 | 85 |   | ? |
| 60 | 15 |   | ? |
| 61 | 8a |   | ? |
| 62 | 85 |  Convert to DEC | ? |
| 63 | 85 |   | ? |
| 64 | d0 |   | ? |
| 65 | 7b |   | ? |
| 66 | 78 |   | ? |
| 67 | 1f |   | ? |
| 68 | 7e |   | ? |
| 69 | 1f |   | ? |
| 71 | 79 |   | ? |
| 72 | 79 |   | ? |
| 73 | 8d |   =13| ? |
| 74 | 8d |   =13| ? |
| 75 | 9e | Convert to DEC 158-128 =30 | Heating Curve Outlet Water Temperature Highest Set [°C] |
| 76 | 96 | Convert to DEC 150-128 =22 | Heating Curve Outlet Water Temperature Lowest Set [°C] |
| 77 | 71 | Convert to DEC 113-128 =-15 | Heating Curve Outside Temperature Lowest Set [°C] |
| 78 | 8f | Convert to DEC 143-128 =15  | Heating Curve Outside Temperature Highest Set [°C] |
| 79 | b7 | Convert to DEC 183-128 =55  | ? Possible Direct Zone2 water Temperature set [°C] |
| 80 | a3 |   =35| ? |
| 81 | 7b |   =-5| ? |
| 82 | 8f |   =15| ? |
| 83 | 8e |   =14| ? |
| 84 | 85 | Convert to DEC 133-128 =5  | Floor heating set delta [°C] |
| 85 | 80 | Convert to DEC 128-128=0 | ? Possible Outdoor temperature for heater ON [°C]  |
| 86 | 8f |   =15| ? Possible cooling curve |
| 87 | 8a |   =10| ?  Possible cooling curve |
| 88 | 94 |  =20 | ? Possible cooling curve |
| 89 | 9e |  =30 | ? Possible cooling curve  |
| 90 | 8a |  =10 | ? |
| 91 | 8a |  =10| ? |
| 92 | 94 |  =20 | ? |
| 93 | 9e |  =30 | ? |
| 94 | 82 | Convert to DEC 130-128 =2  | Floor cooling set delta [°C] |
| 95 | 90 | Convert to DEC 144-128=16|  | ? Possible Outdoor temperature for (heat to cool)   [°C]  |
| 96 | 8b |  Convert to DEC 139-128=11| ? Possible Outdoor temperature for (cool to heat) [°C] |
| 97 | 05 |   | ? |
| 98 | 65 | Convert to DEC 101-1=100   | ? Possible Tank heat up time (max) [min] |
| 99 | 78 | Convert to DEC 120-128=-8 | DHW delta for re-heat  [°C] |
| 100 | c1 |  Convert to DEC 193-128=65  | ? Possible Sterilization boiling temperature [°C] |
| 101 | 0b |  Convert to DEC 11 - 1 = 10 | ? Possible Sterilization max operation time [min] |


111 Bit 56/55 56=Tank 55=Room 3-Way Valve [1:Room, 2:Tank] \
139 Bit B0 (-128) Zone1: Actual (Water Outlet/Room/Pool) Temperature [°C] \
140 Bit 00 -128 Zone2: Actual (Water Outlet/Room/Pool) Temperature [°C] \
141 Bit Aa -128 Actual Tank Temperature [°C] \
142 Bit 7c -128 Actual Outdoor Temperature [°C] \
143 Bit Ab -128 Inlet Water Temperature [°C] \
144 Bit B0 -128 Outlet Water Temperature [°C] \
145 Bit 32 -128 Zone1: Water Temperature [°C] \
146 Bit 32 -128 Zone2: Water Temperature [°C] \
147 Bit 9c -128 Zone1: Water Temperature (Target) [°C] \
148 Bit B6 -128 Zone2: Water Temperature (Target) [°C] \
149 Bit 32 -128 Buffer Tank: Water Temperature [°C] \
150 Bit 32 -128 Solar: Water Temperature [°C] \
151 Bit 32 -128 Pool: Water Temperature [°C] \
152 Bit 80 ? \
153 Bit B7 -128 Outlet Water Temperature (Target) [°C] \
154 Bit Af -128 Outlet 2 Temperature [°C] \
155 Bit Cd -128 Discharge Temperature [°C] \
156 Bit 9a -128 Room Thermostat Internal Sensor Temperature [°C] \
157 Bit Ac -128 Indoor Piping Temperature [°C] \
158 Bit 79 -128 Outdoor Piping Temperature [°C] \
159 Bit 80 -128 Defrost Temperature [°C] \
160 Bit 77 -128 Eva Outlet Temperature [°C] \
161 Bit 80 -128 Bypass Outlet Temperature [°C] \
162 Bit Ff -128 Ipm Temperature [°C] \
163 Bit 91 (145-1)/5 High Pressure [Kgf/Cm2] \
164 Bit 1 -1 Low Pressure [Kgf/Cm2] \
165 Bit 29 (X-1)/5 Outdoor Current [A] \
166 Bit 59 X-1 Compressor Frequency [Hz] \
167 Bit 00 - ? \
168 Bit 00- ? Releated To Pump Flow \
169 Bit 2nd Value , = (X -1)/5 X 2 = ,08 Pump Flow Rate [L/Min] \
170 Bit 1st Value Pump Flow Rate [L/Min] \
171 Bit 1c (X-1) X 100 /2 - Pump Speed [R/Min] \
172 Bit 51 X-1 - Pump Duty [Duty] \
173 Bit 59 (X-1) X10  - Fan Motor Speed 1 [R/Min] \
174 Bit 01  X-1  - Fan Motor Speed 2 [R/Min] \
175 Bit
180 bit + 179 bit = combine both bytes (180 byte) 08 (179 byte) be = 08be= 2238(DEC) - 1 = 2237  (Number of operations)
183 bit + 182 bit = combine both bytes (183) 0b  (182) 25 = 2853 - 1 = 2852 Operating time in h.
185 bit 05 - 1 = 4h, will be Room Heater operation time in h.



To get decimal values you must convert from hexadecimal and do some calulation depending on value. Most of them need just -128(DEC). \
As example 43 bit value to get DHW tank water set temperature b1 (HEX) = 177(DEC) - 128 = 49 C  \
Panasonic query, answer and commands are using 8-bit Checksum (CRC-8) to verify serial data. Last bit is checksum value.


## Query Examples:

Panasonic query:

`71 6c 01 10 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 12`

Panasonic answer example:

`71 c8 01 10 56 55 62 49 00 05 00 00 00 00 00 00
00 00 00 00 19 15 11 55 16 5e 55 05 09 00 00 00
00 00 00 00 00 00 80 8f 80 8a b2 71 71 97 99 00
00 00 00 00 00 00 00 00 00 00 80 85 15 8a 85 85
d0 7b 78 1f 7e 1f 1f 79 79 8d 8d 9e 96 71 8f b7
a3 7b 8f 8e 85 80 8f 8a 94 9e 8a 8a 94 9e 82 90
8b 05 65 78 c1 0b 00 00 00 00 00 00 00 00 55 56
55 21 53 15 5a 05 12 12 19 00 00 00 00 00 00 00
00 e2 ce 0d 71 81 72 ce 0c 92 81 b0 00 aa 7c ab
b0 32 32 9c b6 32 32 32 80 b7 af cd 9a ac 79 80
77 80 ff 91 01 29 59 00 00 3b 0b 1c 51 59 01 36
79 01 01 c3 02 00 dd 02 00 05 00 00 01 00 00 06
01 01 01 01 01 0a 14 00 00 00 77`

Quiet mode 1

`f1 6c 01 10 00 00 00 10 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 82`

Quiet mode 2

`f1 6c 01 10 00 00 00 18 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 7a`

Quiet mode 3

`f1 6c 01 10 00 00 00 20 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 72`

Quiet off

`f1 6c 01 10 00 00 00 08 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 8a`

Set -2C

`f1 6c 01 10 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 7e 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 14`

set -1C

`f1 6c 01 10 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 7f 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 13`

Set 0C

`f1 6c 01 10 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 80 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 12`

set -3C

`f1 6c 01 10 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 7d 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 15`

set -4C

`f1 6c 01 10 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 7c 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 16`

Set -5C

`f1 6c 01 10 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 7b 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 17`

set +1C

`f1 6c 01 10 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 81 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 11`

set +2C

`f1 6c 01 10 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 82 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 10`

set +3C

`f1 6c 01 10 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 83 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 0f`

set +4C

`f1 6c 01 10 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 84 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 0e`

set +5C

`f1 6c 01 10 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 85 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 0d`

tank off

`f1 6c 01 10 02 00 52 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 3e`

tank on

`f1 6c 01 10 02 00 62 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 2e`

set tank to 48C

`f1 6c 01 10 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 b0 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 e2`

set tank to 47C

`f1 6c 01 10 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 af 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 e3`

set tank to 49C

`f1 6c 01 10 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 b1 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 e1`

set tank to 40C

`f1 6c 01 10 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 a8 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 ea`

set tank to max 75C

`f1 6c 01 10 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 ca 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 c8`

heat off

`f1 6c 01 10 02 00 21 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 6f`

heat on

`f1 6c 01 10 02 00 62 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 2e`

set cool to 19C

`f1 6c 01 10 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 93 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 ff`

set cool to 18C

`f1 6c 01 10 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 92 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00`

set cool to 6C 

`f1 6c 01 10 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 86 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 0c`



cool + dhw

`f1 6c 01 10 00 00 03 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 8f`


auto + dhw

`f1 6c 01 10 00 00 08 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 8a`


heat + dhw

`f1 6c 01 10 00 00 02 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 90`

From service cloud commands:

Tank mode only

`f1 6c 01 10 42 54 21 49 00 05 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 80 94 00 00 b1 71 71 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 e6`


heat only

`f1 6c 01 10 42 54 12 49 00 05 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 80 94 00 00 b1 71 71 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 f5`



cool only

`f1 6c 01 10 42 54 13 49 00 05 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 80 94 00 00 b1 71 71 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 f4`

auto

`f1 6c 01 10 42 54 18 49 00 05 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 80 94 00 00 b1 71 71 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 ef`

heat + dhw

`f1 6c 01 10 42 54 22 49 00 05 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 80 94 00 00 b1 71 71 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 e5`


cool + dhw

`f1 6c 01 10 42 54 23 49 00 05 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 80 94 00 00 b1 71 71 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 e4`


auto + dhw


`f1 6c 01 10 42 54 28 49 00 05 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 80 94 00 00 b1 71 71 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 df`


holliday mode ON

`f1 6c 01 10 42 64 22 49 00 05 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 80 94 00 00 b1 71 71 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 d5`


holyday mode OFF

`f1 6c 01 10 42 54 22 49 00 05 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 80 94 00 00 b1 71 71 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 e5`


powerfull 30 min

`f1 6c 01 10 42 54 22 4a 00 05 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 80 94 00 00 b1 71 71 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 e4`


powerfull 60 min

`f1 6c 01 10 42 54 22 4b 00 05 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 80 94 00 00 b1 71 71 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 e3`



powerfull 90 min


`f1 6c 01 10 42 54 22 4c 00 05 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 80 94 00 00 b1 71 71 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 e2`


powerfull off


`f1 6c 01 10 42 54 22 49 00 05 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 80 94 00 00 b1 71 71 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 e5`



force DHW on

`f1 6c 01 10 82 54 21 49 00 05 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 80 94 00 00 b1 71 71 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 a6`


Force DHW OFF

`f1 6c 01 10 42 54 21 49 00 05 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 80 94 00 00 b1 71 71 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 e6`






