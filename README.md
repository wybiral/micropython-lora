# micropython-lora
MicroPython library for controlling a Semtech SX127x LoRa module over SPI.

The logic for the code came from [this module](https://github.com/Wei1234c/SX127x_driver_for_MicroPython_on_ESP8266) but was streamlined and rewritten to be more MicroPython-friendly.

## Examples

### Initialize

The module requires an SPI bus connected to the SX127x, one pin to be `cs` (chip select), and one to be the `rx` (receive IRQ).

```
lora = LoRa(
    spi,
    cs=Pin(CS, Pin.OUT),
    rx=Pin(RX, Pin.IN),
)
```

### Sending

You can send bytes or a string (which will be encoded to bytes). A ValueError exception will be raised if the message exceeds the allowed payload length. Currently this method blocks until the message is sent.

```
lora.send('Hello world!')
```

### Receiving

Receiving is done by attaching a handler using `on_recv` and then calling `recv` to put the device in receive mode. Receive mode is non-blocking so other code can run after calling `recv` but if you call `send` afterward you will need to put the device back into receive mode again.

```
def handler(x):
    print(x)

lora.on_recv(handler)
lora.recv()
```
