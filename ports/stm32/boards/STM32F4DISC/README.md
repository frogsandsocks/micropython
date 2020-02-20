# Как сконфигурировать UART2 на нужные выводы и собрать прошивку для Trackduino


## Переназначить выходы UART

Переназначить выходы для `MICROPY_HW_UART2_TX` и `MICROPY_HW_UART2_RX` в конфигурационном файле `mpconfigboard.h` 

Для `STM32F407` этот конфигурационный файл хранится в `micropython/ports/stm32/boards/STM32F4DISC/mpconfigboard.h`

В репозитории лежит уже готовый конфигурационный файл.


## Собрать прошивку

Собрать `mpy-cross`:

```
$ cd mpy-cross
$ make
```

Собрать и загрузить прошивку:

```
$ cd ports/stm32
$ make submodules
$ make BOARD=STM32F4DISC
$ make BOARD=STM32F4DISC deploy
```

[Подробная инструкция для `STM32F407`](https://github.com/micropython/micropython/wiki/Board-STM32F407-Discovery)


# Работа с UART

```pyhon
from pyb import UART

uart = UART(3, 9600)

def irq_uart_handler(obj):
  received = uart.read()
  print(received)

uart.write('Hello World')

uart.irq(handler=irq_uart_handler, trigger=UART.IRQ_RXIDLE)
```
