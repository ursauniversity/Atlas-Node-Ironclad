# Guia de montagem (resumo)

Componentes:
- ESP32-S3 dev kit (pinos GPIO conforme menuconfig)
- OLED 1.3" (I2C): SDA -> GPIO 21, SCL -> GPIO 22 (configurável)
- DS18B20: DATA -> GPIO 4 (+4.7k pull-up to 3.3V), GND, VCC
- INA219: SDA -> GPIO 21, SCL -> GPIO 22 (I2C bus)
- Fan PWM: FAN PWM -> GPIO 18 (LEDC), FAN GND -> GND, FAN V+ -> 5V
- SD Card: usar SDMMC pins ou SPI - prefer SDMMC para performance (ex.: VSPI)
- Fonte: 5V/2A recommended

Esquemático (texto):
- Conectar OLED e INA219 no mesmo I2C bus (endereços diferentes)
- DS18B20 em pino dedicado com resistor pull-up de 4.7k
- Fan transistor/MOSFET recomendado para isolar 5V PWM (use MOSFET N-channel low-side)
- SD Card via SDMMC pins: CMD, CLK, D0, D1, D2, D3 ou SPI (MOSI/MISO/SCLK/CS)

Segurança:
- Proteja linhas de alimentação.
- Coloque fusível de 2A na linha 5V.