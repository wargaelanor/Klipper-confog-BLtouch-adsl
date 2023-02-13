# Шаги установки
https://github.com/blacknet76/FlyingBear-Reborn-2/tree/main/Klipper/Config

### Ставим дистрибутив сборку MainSale
проверка обновлений
```
sudo apt-get update
sudo apt-get upgrade
sudo apt-get dist-update
```
перезагрузка
```	
sudo reboot
```

### Установка пакета git
```
sudo apt-get install git
```
### скачивания скрипта kiauh, устанавливаем
```
git clone https://github.com/th33xitus/kiauh.git		
cd kiauh/
chmod +x kiauh.sh scripts/*
./kiauh.sh
```
* удаляем всё если
* Ставим всё заново пункты `1` `2` `4` `3`
* компилируем прошивку `4` `2`
	
	extra low
	stm
	stm407
	48Kit
	8MHz
	USB или PA10/PA9(esp8266)
* выход 

	cp ~/klipper/out/klipper.bin ~/printer_data/config/firmware.bin     для скачивания прошивки из web

	ls /dev/serial/by-id/*    поиск устройства
serial: /dev/serial/by-id/usb-Klipper_stm32f407xx_2F002B001350465636393320-if00

Установка камера
```
cd ~/crowsnest
make uninstall
sudo rm -r ~/crowsnest
cd ~
git clone https://github.com/mainsail-crew/crowsnest.git
cd ~/crowsnest
make config (визде энтер и У)
sudo make install
```
поиск камеры
```
v4l2-ctl --list-devices
```
создаём `crowsnest.conf` и вписываем
```
[crowsnest]
log_path: ~/printer_data/logs/crowsnest.log
log_level: quiet

[cam 1]
mode: mjpg                              # mjpg/rtsp
port: 8080                              # Port
device: /dev/video0                    # See Log for available ...
resolution: 1920x1080                     # widthxheight format
max_fps: 25                             # If Hardware Supports this it will be forced, ohterwise ignored/coerced.
#custom_flags:                          # You can run the Stream Services with custom flags.
v4l2ctl: brightness=64
```



`----------------------------------------------------------------------------------------------`

## поле для заметрок

В консоле klipper
PID_CALIBRATE HEATER=extruder TARGET=200 	- запустит калибровку экструдера на 200 градусов;
PID_CALIBRATE HEATER=heater_bed TARGET=90 	- запустит калибровку стола на 90 градусов.
SAVE_CONFIG

---в консоли Linux---
~/klippy-env/bin/pip install -v numpy
sudo apt install python3-numpy python3-matplotlib
sudo raspi-config	- включить SPI

cd ~/klipper/
sudo cp "./scripts/klipper-mcu-start.sh" /etc/init.d/klipper_mcu
sudo update-rc.d klipper_mcu defaults

cd ~/klipper/
make menuconfig	- настроить с Linux process,

sudo service klipper stop
make flash
sudo service klipper start
sudo usermod -a -G tty pi
sudo reboot

			---В консоле klipper---

ACCELEROMETER_QUERY		- проверка датчика
MEASURE_AXES_NOISE		- проверка уровня шума
TEST_RESONANCES AXIS=X	- тест оси X
TEST_RESONANCES AXIS=Y	- тест оси Y

			---в консоли Linux---

~/klipper/scripts/calibrate_shaper.py /tmp/resonances_x_*.csv -o /tmp/shaper_calibrate_x.png - сохранить результат в png
~/klipper/scripts/calibrate_shaper.py /tmp/resonances_y_*.csv -o /tmp/shaper_calibrate_y.png

