###### tags: `ESPHOME`,`HA`,`ESP8266`

# ESPHome-冷氣/電視/電燈遙控器 

:::info
**軟體：**  
(1) Home Assistant   
(2) ESPHome  

**硬體：**  
(1) Nodemcu(ESP8266)   
(2) 433壁切開關 + 433遙控器  
(3) 紅外線發射器(LED) + 紅外線接收器  
(4) DHT22模組   

**功能：**  
(1) 遠端操作433遙控器，控制433壁切開關  
(2) 遠端操作紅外線發射器，控制冷氣與電視  
(3) 利用紅外線接收器，取得紅外線遙控器指令編碼  
(4) 利用DHT22取得環境溫度與濕度  

**參考：**  
https://davidsword.ca/create-custom-ir-remote-with-home-assistant-esphome/
:::

# HA畫面
![HA](https://raw.githubusercontent.com/Chihhao/Public_Drive/main/C5KeKC5.png)

# 硬體線路
* 線路圖 (PS. 4.7K上拉電阻不用裝)
![線路圖](https://raw.githubusercontent.com/Chihhao/Public_Drive/main/IMG_3503.jpg)
* 433牆壁開關正面
![牆壁開關正面](https://raw.githubusercontent.com/Chihhao/Public_Drive/main/IMG_3470.jpg)
* 433牆壁開關背面
![牆壁開關背面](https://raw.githubusercontent.com/Chihhao/Public_Drive/main/IMG_3469.jpg)

# YAML
```yaml=
esphome:
  name: light-433-8266
  platform: ESP8266
  board: nodemcuv2
    
switch:
  - platform: template
    id: fan_power_btn
    name: FAN Power Button
    turn_on_action:
      - remote_transmitter.transmit_raw:
          carrier_frequency: 38kHz
          code: [8939, -4466, 576, -536, 579, -1660, 570, -1662, 574, -537, 578, -536, 581, -534, 581, -538, 577, -1626, 608, -1654, 576, -1657, 576, -538, 577, -1661, 573, -538, 577, -1629, 606, -1660, 572, -539, 580, -1656, 574, -539, 576, -539, 581, -537, 579, -535, 576, -540, 580, -537, 577, -536, 579, -537, 578, -1658, 576, -1655, 579, -1659, 574, -1658, 572, -1661, 577, -1652, 581, -1658, 575]

  - platform: template
    id: AC_power_btn
    name: AC-On
    turn_on_action:
      - remote_transmitter.transmit_raw:
          carrier_frequency: 38kHz
          code: [3354, -1584, 451, -423, 411, -448, 380, -1204, 452, -377, 450, -1244, 411, -427, 403, -423, 409, -423, 408, -1306, 347, -1244, 410, -423, 407, -411, 420, -422, 408, -1243, 411, -1244, 407, -425, 406, -380, 448, -425, 411, -377, 453, -378, 452, -424, 406, -382, 449, -379, 449, -381, 475, -355, 455, -377, 452, -381, 451, -483, 348, -1246, 406, -427, 406, -381, 449, -378, 452, -381, 449, -379, 454, -376, 453, -421, 411, -1232, 421, -408, 446, -353, 481, -398, 406, -380, 452, -1197, 481, -1247, 379, -1246, 404, -1205, 451, -1199, 451, -1247, 409, -1199, 452, -1199, 453, -379, 452, -422, 410, -1305, 349, -379, 451, -483, 347, -450, 382, -415, 415, -421, 409, -380, 449, -381, 475, -399, 410, -1243, 410, -1341, 311, -426, 406, -379, 449, -1243, 411, -484, 345, -449, 384, -379, 453, -419, 411, -380, 452, -1243, 407, -1247, 406, -1245, 411, -482, 347, -419, 413, -425, 404, -422, 406, -412, 421, -421, 412, -423, 404, -450, 383, -1244, 408, -380, 454, -377, 477, -353, 455, -376, 454, -421, 407, -381, 452, -422, 409, -413, 418, -379, 450, -381, 451, -379, 453, -377, 454, -421, 409, -425, 407, -482, 347, -383, 446, -381, 449, -381, 454, -426, 405, -382, 448, -379, 451, -408, 425, -376, 452, -381, 451, -466, 364, -424, 407, -425, 408, -424, 405, -422, 408, -411, 420, -379, 476, -461, 346, -408, 424, -406, 425, -378, 477, -1175, 477, -399, 407, -382, 449, -427, 405, -381, 451, -1241, 411, -1245, 407, -447, 384, -1201, 476, -1175, 479, -1175, 453]

  - platform: template
    id: AC_power_off_btn
    name: AC-Off
    turn_on_action:
      - remote_transmitter.transmit_raw:
          carrier_frequency: 38kHz
          code: [3307, -1622, 415, -427, 403, -428, 403, -1238, 417, -414, 418, -1285, 363, -429, 402, -428, 404, -428, 406, -1248, 402, -1307, 346, -418, 414, -448, 384, -419, 412, -1243, 410, -1243, 410, -415, 413, -426, 403, -426, 406, -486, 344, -428, 407, -422, 405, -425, 407, -408, 422, -447, 385, -429, 402, -425, 406, -447, 382, -426, 410, -1244, 406, -416, 417, -426, 401, -429, 404, -422, 406, -433, 398, -425, 408, -448, 387, -1243, 408, -425, 405, -421, 411, -422, 404, -431, 400, -1247, 408, -427, 402, -431, 401, -425, 408, -447, 380, -422, 409, -426, 408, -1251, 399, -431, 399, -1208, 447, -1240, 411, -1308, 345, -1242, 415, -1236, 413, -1307, 345]

  - platform: template
    id: TV_power_btn
    name: TV-Switch
    turn_on_action:
      - remote_transmitter.transmit_raw:
          carrier_frequency: 38kHz
          code: [8998, -4448, 560, -553, 574, -548, 574, -553, 570, -545, 577, -548, 574, -1661, 576, -1671, 564, -563, 560, -1665, 570, -1630, 612, -1672, 563, -1665, 571, -1661, 578, -549, 572, -548, 572, -1674, 562, -551, 574, -548, 577, -541, 605, -519, 603, -519, 607, -519, 577, -555, 567, -616, 505, -1664, 574, -1668, 569, -1661, 577, -1661, 574, -1663, 574, -1659, 604, -1704, 506, -1732, 502]

  - platform: gpio
    name: LV-Light1-Switch
    pin: D1
    id: relay1
    on_turn_on:
    - delay: 500ms
    - switch.turn_off: relay1
  
  - platform: gpio
    name: LV-Light2-Switch
    pin: D6
    id: relay2
    on_turn_on:
    - delay: 500ms
    - switch.turn_off: relay2
     
remote_transmitter:
  pin: 
    number: D2
  carrier_duty_percent: 50%

remote_receiver:
  pin: 
    number: D5
    inverted: True
    mode: INPUT_PULLUP
  dump: raw
  idle: 25ms

sensor:
  - platform: dht
    pin: D7
    model: dht22
    temperature:
      id: temp
      name: "DHT22 Temperature"
    humidity:
      id: hum
      name: "DHT22 Humidite"
    update_interval: 30s 
  - platform: uptime
    name: "NodeMcu Uptime"
  - platform: wifi_signal
    name: "NodeMcu WiFi Signal"

# Enable logging
logger:

# Enable Home Assistant API
api:

ota:
  password: "******"

wifi:
  ssid: "******"
  password: "******"

  # Enable fallback hotspot (captive portal) in case wifi connection fails
  ap:
    ssid: "light-433-8266 Fallback Hotspot"
    password: "******"

captive_portal:

```



# PCB
* PCB正面
![PCB正面](https://raw.githubusercontent.com/Chihhao/Public_Drive/main/IMG_3499.jpg)
* PCB特寫
![PCB特寫](https://raw.githubusercontent.com/Chihhao/Public_Drive/main/IMG_3500.jpg)
* PCB背面
![PCB背面](https://raw.githubusercontent.com/Chihhao/Public_Drive/main/IMG_3473.jpg)
* PCB上牆
![PCB上牆](https://raw.githubusercontent.com/Chihhao/Public_Drive/main/IMG_3504.jpg)

# 待改善項目
* 無法得知冷氣與電視是否開啟
* 紅外線LED角度不大，要對準冷氣與電視才會有效


