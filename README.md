# ATLAS-NODE v1.6 "IRONCLAD" — Plataforma Didática (ESP32-S3)

Resumo
-------
ATLAS-NODE v1.6 "IRONCLAD" é uma plataforma didática para engenharia de sistemas embarcados baseada no ESP32-S3 (8MB PSRAM). O objetivo é ensinar orquestração de subsistemas, políticas térmicas/energéticas, redes e armazenamento versionado em dispositivos com recursos limitados.

Conteúdo entregue
-----------------
- Firmware ESP-IDF em C (modular).
- Scripts Python para testes e análise.
- Estrutura de diretórios para SD card.
- Métricas de performance e instruções de montagem.
- Template de paper acadêmico.

Requisitos de hardware
----------------------
- ESP32-S3 (8MB PSRAM)
- OLED 1.3" (I2C, 128x64)
- DS18B20 (1-Wire)
- INA219 (I2C)
- Ventoinha controlada por PWM (5V)
- SD Card breakout (SDMMC preferencial)
- Fonte de alimentação adequada e cabos

Estrutura do firmware
---------------------
- components/
  - sys_control/
  - sys_net/
  - sys_data/
  - thermal/
  - power/
  - cluster/
  - decision/
  - logging/
- main/
  - main.c
- CMakeLists.txt

Como compilar
-------------
1. Instale toolchain e ESP-IDF (recomendado ESP-IDF v4.4+ ou v5.x).
2. Clone o repositório para seu workspace.
3. Execute:
   ```
   idf.py set-target esp32s3
   idf.py menuconfig   # configurar pins e Wi-Fi
   idf.py build
   idf.py flash monitor
   ```

Exemplos de uso
---------------
- Acesse o AP/STA configurado e abra: http://<node_ip>/status
- Endpoints principais:
  - /status — status do sistema (JSON)
  - /telemetry?page=n&start=...&end=... — paginado
  - /queue — informação da fila se servidor está lotado
  - /commit — POST para adicionar ponto com commit

Limitações
----------
- Máximo de 3 conexões HTTP simultâneas.
- Dataset local limitado a 100MB (enforce).
- Sistema didático — não substituir por soluções industriais sem revisão.

Melhorias sugeridas (v2.0)
--------------------------
- PCB profissional com medidores integrados.
- Protocolo de cluster com TLS e multiplexação.
- API REST completa (OpenAPI), autenticação por JWT.
- Sincronização de dataset entre nós (replicação).

Licença
-------
MIT

Contato
-------
Autor: Equipe ATLAS (Exemplo)