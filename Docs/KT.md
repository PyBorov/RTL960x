# Данная инструкция предназначена для клиентов АО Казахтелеком
⚠️ Проверенно только на терминалах Nokia/Alcatel и ТОЛЬКО ИНТЕРНЕТ

⚠️ Внимание: за все совершенные вами действия автор ответсвенности не несет

⚠️ Мной проверено:
1. На терминале ODI DFP-34X-2C2 (UPC) с переходником на APC
2. На терминале ODI DFP-34X-2IY3 (APC)

Перед продолжением рекомендую ознакомится с данными ссылками:
1. [Общие инструкции](https://github.com/PyBorov/RTL960x/tree/main?tab=readme-ov-file#guide-links-info)
2. [Описание DFP-34X-2C2 на Hack GPON](https://hack-gpon.org/ont-odi-realtek-dfp-34x-2c2/)


## Настройка
Установил на свой стик эту прошивку: [M110_sfp_ODI_220923FS.tar](https://github.com/PyBorov/RTL960x/blob/main/Firmware/DFP-34X-2C2/M110_sfp_ODI_220923FS.tar), так как есть в ней функция подкидывания корректной версии прошивки для OLT а так же она использует MODE который нам необходим: SFU

На моем терминале я указывал данные переменные(Все эти данные необходимо взять из терминала выданного КТ):
| Переменная            | Описание |
|-------|----------|
| ELAN_MAC_ADDR         | ваш мак |
| MAC_KEY               | ключ на основе вашего мака, получается с помощбю комманды: `echo -n "hsgq1.9aВАШ_МАК_БОЛЬШИМИ_БУКВАМИ" \| md5sum` |
| HW_SERIAL_NO          | ваш Серийный номер формата: `ALCL123456AA` |
| GPON_SN               | ваш Серийный номер формата: `ALCL123456AA` |
| HW_CWMP_MANUFACTURER  | Производитель терминала, напрмер `ALCL` для Nokia/Alcatel |
| PON_VENDOR_ID         | Производитель терминала, напрмер `ALCL` для Nokia/Alcatel |
| HW_CWMP_PRODUCTCLASS  | Модель вашего терминала, например `G-010-S` |
| GPON_ONU_MODEL        | Модель вашего терминала, например `G-010-S` |
| EPON_EXTONU_MODEL     | Модель вашего терминала, например `G-010-S` |
| HW_HWVER              | Апаратная версия вашего терминала, например `3AB12345CDEF` |
| GPON_PLOAM_FORMAT     | Формат PLOAM |
| GPON_PLOAM_PASSWD     | Пароль PLOAM в формате HEX, у КТ отличного от `44454641554C54` не видел, так что смело используйте этот |
| OMCI_FAKE_OK          | Может ли быть получен поддельный статус O5, где `1` это да а `0` нет |
| OMCI_SW_VER1          | Версия прошивки вашего терминала, например `3AB12345CDEF67` |
| OMCI_SW_VER2          | Версия прошивки вашего терминала, например `3AB12345CDEF67` |
| LAN_SDS_MODE          | Про этот параметр лучше почитать [тут](https://github.com/PyBorov/RTL960x/blob/main/Docs/FLASH_GETSET_INFO.md#lan_sds_mode). У меня установлено значение `1`, другие не проверял так как не было нужды |
| OMCI_OLT_MODE         | Я так понял это настройки взаимодействия ONT c OLT (вроде как влияет на какие конфиги и как использовать), необходимо установить значение: `3` |
| DEVICE_TYPE           | хотя [тут](https://github.com/PyBorov/RTL960x/blob/main/Docs/FLASH_GETSET_INFO.md#device_type) говорится что для нашего стика значение `1`, нам нужно `0` |



Вот так: 
```
flash set ELAN_MAC_ADDR 000000111111
flash set MAC_KEY ee37acc2fd5b2440f08ef8ac1340bd37
flash set HW_SERIAL_NO ALCL123456AA
flash set GPON_SN ALCL123456AA
flash set HW_CWMP_MANUFACTURER ALCL
flash set PON_VENDOR_ID ALCL
flash set HW_CWMP_PRODUCTCLASS G-010-S
flash set GPON_ONU_MODEL G-010-S
flash set EPON_EXTONU_MODEL G-010-S
flash set HW_HWVER 3AB12345CDEF
flash set GPON_PLOAM_FORMAT 0
flash set GPON_PLOAM_PASSWD 44454641554C54
flash set OMCI_FAKE_OK 1
flash set OMCI_SW_VER1 3AB12345CDEF67
flash set OMCI_SW_VER2 3AB12345CDEF67
flash set LAN_SDS_MODE 1
flash set OMCI_OLT_MODE 3
flash set DEVICE_TYPE 0
```

И так же необходимо указать версию прошивки вашего терминала этими командами (для того что бы верно подкидывала версию прошивки для OLT):
```
nv setenv sw_custom_version0 3AB12345CDEF67
nv setenv sw_custom_version1 3AB12345CDEF67
```

На своем роутере так же необходимо указать VLAN подключения, по стандарту это `40` но может и меняться
