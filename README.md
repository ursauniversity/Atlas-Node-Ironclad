# ATLAS-NODE v1.6 "IRONCLAD"

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Platform: ESP32](https://img.shields.io/badge/Platform-ESP32-blue.svg)](https://www.espressif.com/)
[![Version: 1.6](https://img.shields.io/badge/Version-1.6-green.svg)]()

> **O núcleo de aço para o aprendizado em sistemas embarcados.** Uma plataforma educacional de hardware e software **completamente open-source**, construída em torno do ESP32-S3, que coloca o **realismo técnico acima da promessa vazia**. Projetada para ensinar, testar e validar sistemas distribuídos em condições reais de recursos limitados.

**Repositório Oficial:** `github.com/Kryvion-Tech/Atlas-Node-Ironclad`
**Custo Total do Hardware:** ~R$ 115,00
**Filosofia de Design:** Hardware limitado, aprendizado ilimitado. Documentamos não só o que funciona, mas os limites reais e os modos de falha.

---

## 🎯 Visão Geral

O **ATLAS-NODE v1.6** não é apenas mais uma placa de desenvolvimento ou um firmware de exemplo. É um **laboratório de sistemas embarcados** inteiramente contido em um dispositivo de baixo custo. Ele foi concebido para forçar o confronto com os desafios reais da engenharia: **concorrência com memória limitada, gestão térmica ativa, filas justas de acesso e a orquestração de subsistemas cooperativos.**

### Por Que "Ironclad"?
Porque sua arquitetura é construída para ser **resiliente, observável e didática**. Cada limite (3 usuários, 8 conexões TCP) é uma característica de projeto, não uma limitação não documentada. Aqui, você aprende o "porquê" por trás de cada decisão de sistema.

### Para Quem É Este Projeto?
- **Estudantes de Engenharia:** Que vão além do "Hello World" e querem entender sistemas operacionais de tempo real, escalonamento e concorrência em hardware real.
- **Pesquisadores e Makers:** Que precisam de uma plataforma robusta e instrumentada para prototipagem de soluções de **edge computing** e coleta de dados distribuída.
- **Professores:** Que buscam um equipamento de laboratório acessível e de código aberto para demonstrações práticas de conceitos avançados de sistemas embarcados e redes.

---

## ⚙️ Especificações Técnicas

### Hardware (Bill of Materials)
| Componente | Especificação | Função no Sistema | Custo Aprox. (R$) |
| :--- | :--- | :--- | :--- |
| **MCU** | ESP32-S3 (Xtensa LX7 Dual-Core 240MHz, **8MB PSRAM**, 16MB Flash) | Cérebro da plataforma, executa todos os subsistemas cooperativos. | 42,00 |
| **Display** | OLED 1.3" SH1106 (128x64 pixels, I2C) | Interface local para telemetria em tempo real (temperatura, RAM, fila). | 18,00 |
| **Monitor de Energia** | Sensor INA219 | Medição precisa de corrente, tensão e potência consumida pela placa. | 5,00 |
| **Sensor Térmico** | DS18B20 (Endereçável, 1-Wire) | Monitoramento da temperatura do chip para controle ativo do cooler. | 5,00 |
| **Sistema de Resfriamento** | Cooler 30mm 5V + Dissipador personalizado | Controle térmico ativo para operação contínua e estável. | 15,00 |
| **Gerenciamento de Energia** | Baterias 2x 18650 + BMS Inteligente + Conversor Buck | Fornece autonomia e permite estudos de consumo energético. | 15,00 |
| **Armazenamento** | Leitor de MicroSD (até 32GB) | Dataset local versionado e logs de telemetria de longa duração. | 8,00 |
| **Estrutura & Conectores** | PCB 2 camadas, USB-C, bornes para I/O | Robustez mecânica e facilidade de prototipagem/criação de clusters. | 15,00 |
| | **TOTAL ESTIMADO** | | **~123,00** |

### Software e Capacidades do Sistema
- **Sistema Operacional:** FreeRTOS com ESP-IDF.
- **Arquitetura de Software:** Baseada em **Máquina de Estados Finitos (FSM)** e **subsistemas cooperativos**, não em multitarefa preemptiva pesada.
- **Subsistemas Principais:**
    1.  **`SYS-CONTROL`:** Orquestrador central e monitor de integridade.
    2.  **`SYS-NETWORK`:** Servidor HTTP/WebSocket com **fila de acesso justa** e limite explícito de conexões.
    3.  **`SYS-DATA`:** Gerenciador de dataset local com versionamento estilo Git.
- **Limites Didáticos (Configuráveis):**
    - **Usuários Ativos Simultâneos:** Máximo **3**.
    - **Conexões TCP Concorrentes:** Máximo **8**.
    - **Subsistemas Ativos:** Máximo **2** em execução plena (sempre com `SYS-CONTROL`).
- **Telemetria em Tempo Real:** Monitoramento de RAM livre, temperatura da CPU, tensão da bateria, estado da fila e carga da rede.

---

## 🚀 Primeiros Passos

### 1. Hardware: Montagem e Soldagem
> **Guia completo disponível em:** `/hardware/ASSEMBLY_GUIDE.md`

```bash
# Resumo dos passos críticos:
1.  Soldar os componentes SMD (ESP32, reguladores) na PCB.
2.  Soldar os componentes através-furo (conectores, bornes).
3.  Fixar o cooler e o dissipador térmico no ESP32-S3.
4.  Instalar as baterias 18650 no holder e conectar ao BMS.
5.  Realizar teste de continuidade e inspeção visual.
```

### 2. Software: Configuração do Ambiente de Desenvolvimento

```bash
# 1. Clone o repositório
git clone https://github.com/Kryvion-Tech/Atlas-Node-Ironclad.git
cd Atlas-Node-Ironclad/firmware

# 2. Configure o ambiente (recomendado: VS Code + PlatformIO)
# Instale a extensão "PlatformIO" no VS Code.
# O projeto abrirá automaticamente com todas as dependências configuradas no arquivo `platformio.ini`.

# 3. Conecte o ATLAS-NODE via USB-C e compile/faça upload
pio run --target upload  # Ou use o botão de "Upload" no PlatformIO

# 4. Monitore a saída serial (baud rate: 115200)
pio device monitor
```
Após o upload, o Node iniciará. Conecte-se ao Wi-Fi `ATLAS-NODE-XXXX` e acesse `http://192.168.4.1` para ver o dashboard web.

### 3. Seu Primeiro Experimento: Observando a Fila de Acesso
1.  Conecte **3 dispositivos** (e.g., celulares, laptops) ao Wi-Fi do Node e abra o dashboard.
2.  Tente conectar um **4º dispositivo**. Você será colocado em uma **fila** e verá uma página com sua posição e tempo estimado.
3.  Observe no display OLED como o campo `QUEUE` muda.
4.  Esse é o **mecanismo de justiça (`fairness`)** em ação, um conceito fundamental de sistemas operacionais, demonstrado em hardware.

---

## 🏗️ Arquitetura do Sistema

### Diagrama de Subsistemas Cooperativos
```
┌─────────────────────────────────────────────────────────────────┐
│                       ATLAS-NODE v1.6 CORE                      │
├───────────────────┬───────────────────┬─────────────────────────┤
│    SYS-CONTROL    │   SYS-NETWORK     │       SYS-DATA          │
│    (SEMPRE ON)    │   (ON/OFF/THRT)   │     (ON/IDLE/FULL)      │
├───────────────────┼───────────────────┼─────────────────────────┤
│ • FSM Master      │ • HTTP Server     │ • Dataset Versioning    │
│ • Health Monitor  │ • Access Queue    │ • JSON/CSV Logger       │
│ • Thermal Mgr.    │ • WS Comm         │ • SD Card Interface     │
│ • Power Mgr.      │ • Config API      │ • Query Engine (Basic)  │
│ • OLED Driver     │ (Max 3 users)     │                         │
└─────────┬─────────┴─────────┬─────────┴────────────┬────────────┘
          │                   │                       │
          └───────────────────┴───────────────────────┘
                            │
                 ┌──────────┴──────────┐
                 │   HARDWARE LAYER     │
                 │  ESP32-S3 + Perifs   │
                 └──────────────────────┘
```

### O Protocolo de Comunicação Inter-Nós "Ironclad"
Para criar clusters, os nós se comunicam via um protocolo leve e confiável (especificação completa em `/docs/protocols/IRONCLAD_PROTOCOL.md`).

**Formato do Pacote (32 bytes):**
`[HEADER(4)][SEQ(2)][TYPE(1)][NODE_ID(1)][DATA(22)][RSSI(1)][CRC(1)]`

Isso permite estudos sobre **consenso em redes instáveis, roteamento e tolerância a falhas**.

---

## 📁 Estrutura do Repositório (Tree View)
```
Atlas-Node-Ironclad/
├── LICENSE
├── README.md                           # Este arquivo
├── .gitignore
├── hardware/
│   ├── schematics/                     # Arquivos KiCad (.kicad_sch, .kicad_pcb)
│   ├── gerber/                         # Arquivos para fabricação da PCB
│   ├── bill_of_materials.csv           # Lista completa de componentes com links
│   ├── ASSEMBLY_GUIDE.md               # Guia passo a passo de montagem
│   └── CASE_DESIGN/                    # Modelos 3D (STL) para a carcaça
├── firmware/
│   ├── src/
│   │   ├── main.cpp
│   │   ├── sys_control/               # Núcleo do sistema
│   │   ├── sys_network/               # Servidor web e filas
│   │   ├── sys_data/                  # Gerenciamento de datasets
│   │   ├── drivers/                   (OLED, INA219, DS18B20, SD Card)
│   │   └── protocols/                 (Implementação do Ironclad Protocol)
│   ├── include/
│   ├── lib/
│   └── platformio.ini                  # Configuração do ambiente de build
├── software/
│   └── ground_station/                 # Aplicação Python para estação base/CLI
├── docs/
│   ├── ARCHITECTURE_DECISIONS.md       "Por que escolhemos X e não Y?"
│   ├── FAILURE_LOGS.md                "Registro de crashes e lições aprendidas"
│   └── protocols/                      # Especificações detalhadas
├── research/
│   └── experimental_data/              # Datasets brutos de testes (térmico, consumo)
└── images/                             # Fotos, diagramas e assets para o README
```

---

## 🔬 Casos de Uso e Experimentos

### 1. Laboratório de Sistemas Operacionais
- **Experimento:** Implemente um novo algoritmo de escalonamento para a fila de acesso (e.g., `Priority Queue` ao invés de `FIFO`).
- **Onde começar:** Modifique `sys_network/access_queue.cpp`.
- **Métricas:** Compare o tempo médio de espera e a satisfação do usuário (via pesquisas simuladas).

### 2. Pesquisa em Edge Computing
- **Experimento:** Coleta distribuída de dados de sensores em uma rede de 3 nós.
- **Onde começar:** Use o `Ironclad Protocol` em `firmware/src/protocols/`.
- **Resultado:** Dataset de telemetria sincronizada e tolerante a falhas, pronto para análise.

### 3. Validação de Controle Térmico
- **Experimento:** Estresse o sistema com cálculo contínuo e monitore a curva de temperatura.
- **Onde começar:** O script `research/thermal_stress_test.py` automatiza isso.
- **Aprendizado:** Você verá o `thermal_manager` em ação, degradando funções antes do *crash*.

---

## 🤝 Como Contribuir
Adoramos contribuições! Queremos que este seja um projeto da comunidade.
1.  **Reporte Bugs:** Use a [aba "Issues"](https://github.com/Kryvion-Tech/Atlas-Node-Ironclad/issues) do GitHub. Inclua logs, fotos e passos para reproduzir.
2.  **Sugira Melhorias:** Ideias para novos experimentos, otimizações de hardware ou features são bem-vindas.
3.  **Envie Pull Requests (PRs):**
    - Fork o repositório.
    - Crie uma branch para sua feature (`git checkout -b feature/IncivelFeature`).
    - Commit suas mudanças (`git commit -am 'Add: Minha Incrível Feature'`).
    - Push para a branch (`git push origin feature/IncivelFeature`).
    - Abra um Pull Request no GitHub.

**Diretrizes:** Código limpo, documentação atualizada e testes são altamente valorizados. Consulte `CONTRIBUTING.md` (a ser criado) para detalhes.

---

## 📜 Licença e Atribuição
- **Licença do Software (Firmware, Estação Base):** `MIT License`. Veja o arquivo [LICENSE](LICENSE).
- **Licença do Hardware (Esquemáticos, PCB):** `CERN Open Hardware Licence Version 2 - Permissive`.
- **Documentação e Conteúdo:** `Creative Commons Attribution 4.0 International (CC BY 4.0)`.

Você é livre para usar, modificar e distribuir este projeto, mesmo comercialmente. A única exigência é a **atribuição apropriada**. Se este projeto ajudou você em uma pesquisa ou produto, citar o repositório original é a forma correta de retribuir.

---

## ❓ FAQ (Perguntas Frequentes)
- **P:** Posso usar um ESP32 comum (sem PSRAM)?
  **R:** Não recomendado para a v1.6. A PSRAM é essencial para os subsistemas e a fila em memória. A versão 1.6 foi otimizada para o S3.
- **P:** O cooler é realmente necessário?
  **R:** **Sim.** Para operação contínua acima de 1 minuto sob carga, o ESP32-S3 aquece significativamente. O controle térmico ativo é parte do ensino.
- **P:** Como faço para conectar vários nós em um cluster?
  **R:** Consulte o tutorial `docs/cluster_setup.md`. Você configurará um nó como coordenador usando o `Ironclad Protocol`.

---
**Desenvolvido com 🔩 e ⚡ por [Kryvion Technologies](https://github.com/Kryvion-Tech).**  
*"Nós não simulamos limites. Nós os construímos, os instrumentamos e os estudamos."*
