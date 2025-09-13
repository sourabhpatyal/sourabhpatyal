<!-- Profile README -->

<div align="center">

# 🔌 Sourabh Patiyal  
**Embedded Engineer • Mentor • Microcontroller Whisperer**

*“I don’t just blink LEDs — I light up understanding.”*

[![Email](https://img.shields.io/badge/Email-sourabhpatyal%40gmail.com-informational?style=flat)](mailto:sourabhpatyal@gmail.com)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-sourabhpatyal-blue?style=flat)](https://www.linkedin.com/in/sourabh-patiyal-0b956912b/)
[![GitHub](https://img.shields.io/badge/GitHub-sourabhpatyal-black?style=flat)](https://github.com/sourabhpatyal)

</div>

> 

---

## 👋 About Me
I’m an **Embedded Systems Engineer** and **Technical Trainer** based in India.  
I work close to the silicon—**bare-metal C, registers, timing, and protocols**—and mentor engineers to think like firmware architects, not just coders.

---

## ⚙️ Core Strengths
- 🛠️ **Bare-metal C** (no HAL when not needed) • **CMSIS** • **MISRA-C mindset**  
- 🧾 **Datasheet-first design** — registers drive architecture & ISR boundaries  
- 🧵 **Precision ISRs** — latency budgets, lockless queues, finite state machines  
- 🔁 **Bootloaders & OTA** — DFU patterns, versioning, rollback strategy  
- 📡 **Protocol stacks** — UART/SPI/I²C/CAN, RS-485/Modbus RTU (master & slave)  
- 🔬 **Debugging discipline** — binary search, trace, instrumentation hooks

---

## 🧰 Tech Stack (quick view)

<!-- icon strip -->
<p align="left">
  <img src="https://skillicons.dev/icons?i=c,cpp,cmake,git,linux,raspberrypi,arduino,stm32,python" alt="skills" />
</p>

**Languages**: C, Embedded C, basic C++  
**Build**: Make/CMake • **MCU**: AVR, PIC, STM32, ESP32  
**Tools**: Keil uVision, STM32CubeIDE, AVR-GCC, MPLAB X, OpenOCD  
**PCB**: Altium, KiCad  
**Infra**: Node-RED, Mosquitto MQTT, Grafana  
**Standards**: CMSIS, MISRA-C mindset

---

## 🧵 Hardware I Work With

| MCU / SoC                | What I Build On It                                                      |
|--------------------------|-------------------------------------------------------------------------|
| **AVR ATmega8 / 328P**   | UART/ADC, Modbus RTU (MAX485), LED logic trainers, command parsers     |
| **PIC 16F / 18F**        | EEPROM cfg, LCD/7-segment, timer frameworks                            |
| **STM32F1 / F4**         | FATFS + SD, BLE/GPS, lightweight web server, ring buffers, DMA basics  |
| **8051 / AT89C52**       | Timing labs, ISR hygiene, classic peripheral bring-up                   |
| **ESP32 / ESP8266**      | Wi-Fi MQTT, Node-RED integrations, BLE Mesh experiments                |
| **Raspberry Pi 4**       | Python GPIO automation, service daemons, ROS/Makefile workflows        |

---

## 🔄 Protocols & Buses

| Stack / Bus               | Notes                                                                  |
|---------------------------|------------------------------------------------------------------------|
| **UART**                  | CLI + framed packets + CRC; RX ring buffers + ISR-driven TX            |
| **SPI**                   | Sensors, SD; full-duplex DMA patterns                                  |
| **I²C**                   | RTC/EEPROM; bit-bang fallbacks                                         |
| **RS-485 / Modbus RTU**   | Master/Slave, register map design, timeout strategy                    |
| **MQTT**                  | IIoT pipelines (Node-RED → Grafana)                                    |
| **CAN (basic)**           | ID planning, filter masks, mailbox hygiene                             |

---

## 🚀 Selected Projects (Problem ➜ System)
- 🚗 **Vehicle Tracker** — GNSS + LCD + DGUS  
  *Event-driven firmware, buffered IO, NMEA parsing, fault-safe state machine.*
- 🔋 **Energy Logger** — STM32 + BLE + mini Web UI  
  *Persistent ring-buffer logging, config channel, versioned settings.*
- 📡 **Modbus RTU Stack** — ATmega8 + MAX485  
  *Register model, CLI tools, deterministic timing under bus contention.*
- 📶 **BLE Mesh + Android Sync**  
  *Resilient config transport, failure semantics, OTA flow draft.*
- 📈 **IIoT Dashboard** — MQTT → Node-RED → Grafana  
  *Topic taxonomy, alert rules, retention windows.*
- 🤖 **Obstacle-Avoid Bot** — IR + Ultrasonic  
  *Sensor fusion → PWM drive; watchdog + brown-out handling.*
- 💡 **Pattern LED Trainer** — 500+ logic drills  
  *Teaches timing, debouncing, state transitions.*

> _Tip:_ Pin your best repos so they appear on your profile (Settings → Customize your pins).

---

## 🎓 Teaching & Mentoring
- **AVR & PIC in C** (bare-metal mindset, not Arduino)  
- **Peripherals**: GPIO, ADC, Timers, PWM, Interrupts, DMA basics  
- **Protocols**: UART, SPI, I²C, Modbus RTU (Master/Slave)  
- **Linux GPIO on Raspberry Pi** with Python  
- **Altium PCB** for practical projects  
- **Debugging Mindset**: instrumentation hooks, traces, and fault isolation

> *Looking to run a cohort or 1:1 deep-dive?* → Email me.

---

## 🧪 Signature Snippet

<details>
<summary><b>Minimal ISR-safe ring buffer (RX)</b></summary>

```c
typedef struct { volatile uint8_t q[128]; volatile uint8_t h, t; } rb_t;

static inline void rb_put(rb_t* r, uint8_t b){
    uint8_t n = (r->h + 1) & 127;
    if (n != r->t) { r->q[r->h] = b; r->h = n; }
}
static inline int rb_get(rb_t* r){
    if (r->t == r->h) return -1;
    uint8_t b = r->q[r->t];
    r->t = (r->t + 1) & 127;
    return b;
}

// ISR (UART RX):
void USARTx_IRQHandler(void){
    if (UART_RXNE()) { rb_put(&rxbuf, UART_READ()); }
}
