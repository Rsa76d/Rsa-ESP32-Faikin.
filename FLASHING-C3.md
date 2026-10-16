# Прошивка Faikout на ESP32-C3

Джерело файлів: https://codeberg.org/RevK/ESP32-Faikout/src/branch/main/ESP/release
Офсети взяті з README автора (для S1 bootloader на 0x1000, для S3 на 0x0; для C3 на 0x0).

## Що означають файли

| Файл | Призначення |
|---|---|
| `Faikout-C3-bootloader.bin` | Перший завантажувач, запускає програму після увімкнення |
| `Faikout-C3-partition-table.bin` | Карта розмітки флешу |
| `Faikout-C3-ota_data_initial.bin` | Службові дані про активний OTA-слот |
| `Faikout-C3.bin` | Сама прошивка |

## Офсети для чистого чипа

| Офсет | Файл |
|---|---|
| 0x0 | Faikout-C3-bootloader.bin |
| 0x8000 | Faikout-C3-partition-table.bin |
| 0xD000 | Faikout-C3-ota_data_initial.bin |
| 0x10000 | Faikout-C3.bin |

## Команда esptool

Порт підстав свій (`COM3` у Windows, `/dev/ttyUSB0` або `/dev/ttyACM0` у Linux):

```
esptool.py -p COM3 write_flash 0x0 Faikout-C3-bootloader.bin 0x8000 Faikout-C3-partition-table.bin 0xd000 Faikout-C3-ota_data_initial.bin 0x10000 Faikout-C3.bin
```

## Нюанси

- Усі чотири файли бери з однієї папки (`release`), не змішуй з `beta`.
- Якщо плата не входить у режим прошивки сама: затисни BOOT, коротко натисни RESET, відпусти BOOT, потім запускай команду.
- Якщо пристрій уже працює з Faikout і онлайн, простіше оновити по OTA. Повну прошивку роби на чистому чипі або для скидання.
- Файли `.manifest` потрібні веб-інсталятору автора, там можна звірити офсети.
- Прошивати зручніше з ПК, ніж з Android.
