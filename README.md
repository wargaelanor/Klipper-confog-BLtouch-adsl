# Шаги установки
https://github.com/blacknet76/FlyingBear-Reborn-2/tree/main/Klipper/Config

	Ставим сборку MainSale
sudo apt-get update						- проверка обновлений
sudo apt-get upgrade					- обновления
sudo apt-get dist-update				- обновления дистрибутива если есть.
sudo reboot								- перезагрузка

sudo apt-get install git				- Установка пакета git
git clone https://github.com/th33xitus/kiauh.git		- скачивания скрипта kiauh
cd kiauh/								- вход в папку kiauh
chmod +x kiauh.sh scripts/*				- установка прав доступа к файлам
./kiauh.sh								- запуск скрипта kiauh
	удаляем всё
	Ставим всё заново пункты 1 2 4 3
	компилируем прошивку 4 2
	
	extra low
	stm
	stm407
	48Kit
	8MHz
	USB или PA10/PA9(esp8266)
выход 

cp ~/klipper/out/klipper.bin ~/printer_data/config/firmware.bin     для скачивания прошивки из web

ls /dev/serial/by-id/*    поиск устройства
serial: /dev/serial/by-id/usb-Klipper_stm32f407xx_2F002B001350465636393320-if00

камера
cd ~/crowsnest
make uninstall
sudo rm -r ~/crowsnest
cd ~
git clone https://github.com/mainsail-crew/crowsnest.git
cd ~/crowsnest
make config (визде энтер и У)
sudo make install
создаём crowsnest.conf
v4l2-ctl --list-devices   поиск камеры

			---создание пользователя klipper---

sudo adduser klipper					- создать пользователя klipper
sudo usermod -a -G tty klipper				- предоставляет доступ на чтение и запись для устройств /dev/vca
sudo usermod -a -G dialout klipper			- полный доступ к серийному порту
sudo adduser klipper sudo				- разрешает пользоваться правами администратора 
su klipper						- вход под пользователем klipper
cd ~						- переход в домашнюю папку klipper

			--- создание прошивки для платы принтера---

cd ~/klipper 2
make clean					- очищаем, в нашем случае сбросит все настройки menuconfig
make menuconfig					- конфигурируем прошивку под наш контроллер
make						- собираем прошивку.

ls /dev/serial/by-id/*					- узнаем адрес нашей платы по ID


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
			---записать в printer.cfg---
[mcu rpi]
serial: /tmp/klipper_host_mcu
			
[adxl345]
cs_pin: rpi:None

[resonance_tester]
accel_chip: adxl345
probe_points:
    100, 100, 20  # координаты теста

			---В консоле klipper---

ACCELEROMETER_QUERY		- проверка датчика
MEASURE_AXES_NOISE		- проверка уровня шума
TEST_RESONANCES AXIS=X	- тест оси X
TEST_RESONANCES AXIS=Y	- тест оси Y

			---в консоли Linux---

~/klipper/scripts/calibrate_shaper.py /tmp/resonances_x_*.csv -o /tmp/shaper_calibrate_x.png - сохранить результат в png
~/klipper/scripts/calibrate_shaper.py /tmp/resonances_y_*.csv -o /tmp/shaper_calibrate_y.png

