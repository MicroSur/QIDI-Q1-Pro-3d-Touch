# QIDI-Q1-Pro-3d-Touch

Переделка QIDI Q1 Pro с индуктивного датчика стола на 3д-тач для прошивки OpenQ1

Что плохо:
- все это справедливо для кастомной прошивки OpenQ1 ( https://github.com/frap129/OpenQ1 ), для стоковой прошивки Не подходит.
- нужно отрезать уши крепления у тача.
- подрезать уголок передней крышки головы (фото), чтобы снималась как раньше.
- Переписать в printer.cfg все координаты, где использовался индуктивный датчик на новые (примеры внизу)
- иметь кримпер для фиксации проводов в разъемы
- Это должно напугать и остановить, если нет то дальше...

Что надо:
- 3д-тач. Я брал у треугольников. https://aliexpress.ru/item/32949450525.html?sku_id=12000025395041340
  Можно любой, лишь бы влез в крепление и вы знаете распиновку.
  Распиновка используемого тача по цветам на фото.
- Распечатать крепление (stl), Тач должен крепко садиться в гнездо. У меня из ABS. Ориентацию на столе поменяйте, как вам удобно.
  Крепление в исходнике не мое, отредактировал для QIDI Q1 Pro добавив 10мм проставку. Исходник для qidi max3: https://www.printables.com/model/590047-qidi-x-max-3-x-plus-3-bl-touch-holder
- Распиновка (фото), используются pазъемы на плате головы J7 ( +5, Gnd, IO21 ) и J11 ( IO11 - верхний, второй нижний не использовать ).
  Вторую землю от 3д-тача можно не тащить и никуда не втыкать.

![Распиновка 3д-тач на голове](https://github.com/MicroSur/QIDI-Q1-Pro-3d-Touch/blob/main/3dt_head_pinout.jpg?raw=true "Распиновка 3д-тач на голове")

![Красивый вариант на голове](https://github.com/MicroSur/QIDI-Q1-Pro-3d-Touch/blob/main/last_pins_view.jpg?raw=true "Красивый вариант на голове")

![Крепление](https://github.com/MicroSur/QIDI-Q1-Pro-3d-Touch/blob/main/3dt_mount.jpg?raw=true "Крепление")

![Распиновка 3д-тач по цветам](https://github.com/MicroSur/QIDI-Q1-Pro-3d-Touch/blob/main/pinout_3dtouch.jpg?raw=true "Распиновка 3д-тач по цветам")


Пример изменений в printer.cfg

    #Секцию пробы и кулера платы головы закомментировать всю.
    
    #[qdprobe]
    #pin:!gpio21
    #z_offset:0.000001

    # [heater_fan hotend_fan2]
    # pin:gpio11
    # max_power: 1.0
    # shutdown_speed:1.0
    # kick_start_time: 0.5
    # heater: extruder
    # heater_temp: 50.0
    # fan_speed: 1.0
    # off_below: 0

    #добавить секцию bltouch
    [bltouch]
    sensor_pin: gpio21 # Probe
    control_pin: !gpio11 # servo
    x_offset: 29 #induct 17.6 #3d-touch 29
    y_offset: 4.4
    #z_offset: 0.0
    speed: 7.0 
    lift_speed: 20
    samples: 3
    samples_result: median #average
    sample_retract_dist: 2.0
    samples_tolerance: 0.05
    samples_tolerance_retries: 4
    
    [z_tilt]
    z_positions:
        -59,125
        307.5,125
    points:
        0,125
        190,125 #inductive probe = 215,125  # 3d-touch 190,125
    speed: 300
    horizontal_move_z: 5
    retries: 3
    retry_tolerance: 0.01
    
    [bed_mesh]
    speed: 300
    horizontal_move_z: 5 #7 #stock
    mesh_min: 32,15 #20,15 ind
    mesh_max: 230,230 #induct 230,230
    probe_count: 8,8
    algorithm: bicubic
    bicubic_tension: 0.2 #stock
    adaptive_margin: 5
    mesh_pps: 2, 2 #stock
    
    [bed_screws]
    screw1: 10,10
    screw1_name: Front left
    screw2: 190,10 #ind 230,10
    screw2_name: Front right
    screw3: 93,240 #ind 125,240
    screw3_name: Back center
    probe_height: 0.2
    horizontal_move_z: 10
    
    [screws_tilt_adjust]
    screw1: 10,10
    screw1_name: Front left
    screw2: 190,10 #ind 220,10
    screw2_name: Front right
    screw3: 93,230 #ind 125,230
    screw3_name: Back center
    screw_thread: CW-M4
    
    
