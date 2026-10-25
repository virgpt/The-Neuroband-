<div align="center">

# 🧠 Neuroband

### An adaptive anxiety-relief wearable that learns each wearer's stress patterns and responds with discreet, real-time haptic feedback.

**Embedded C++ · XIAO ESP32-C3 · MAX30105 · Haptic Feedback · Wearable Electronics**

*Built at Maker's Asylum Innovation School, Goa*

</div>

---

## 🔹 The Problem

Anxiety can strike without warning and leave people overwhelmed, especially in public. It affects over **300 million people worldwide**. Most existing tools do not detect rising anxiety in real time, and they do not adapt to the individual. As a result, they are unreliable at recognising when someone is genuinely at risk of a panic attack.

A fixed heart-rate threshold doesn't solve this. Resting heart rates vary widely from person to person, so one number either misses real anxiety or triggers constantly for someone who is calm.

## 🔹 The Solution

**Neuroband** is a discreet wristband that **calibrates itself to its wearer**. It analyses the BPM at which the user has previously triggered the device, and from those readings it calculates a personalised threshold. When live heart rate crosses that threshold, Neuroband instantly applies haptic pressure at specific acupoints, helping the user regain control anywhere, at any time, without drawing attention to themselves.

---

## ⚙️ How It Works

```
 ┌──────────────┐   I²C   ┌──────────────────┐  GPIO  ┌────────────────┐    ┌─────────────────────┐
 │   MAX30105   │ ──────▶ │  XIAO ESP32-C3   │ ─────▶ │ NPN transistor │ ─▶ │ 3× vibration motors │
 │ pulse sensor │         │  (C++ firmware)  │        │  (low-side)    │    │    at acupoints     │
 └──────────────┘         └──────────────────┘        └────────────────┘    └─────────────────────┘
                                   ▲
                          ┌────────┴────────┐
                          │   Push button   │  manual trigger → logged BPM → threshold update
                          └─────────────────┘
```

1. **Sense:** The MAX30105 optical sensor continuously measures the wearer's pulse.
2. **Process:** C++ firmware on the ESP32-C3 converts the raw signal into live BPM.
3. **Learn:** Each time the user triggers the device, the firmware logs that BPM and recalculates a personal threshold.
4. **Respond:** When BPM rises past the threshold, the ESP32-C3 switches an NPN transistor that drives three vibration motors positioned over acupoints.

## ✨ Key Features

- **Adaptive, per-user threshold:** learns from the wearer's own history instead of relying on a one-size-fits-all number
- **Real-time detection:** continuous on-device heart-rate monitoring with no phone or app required
- **Instant haptic response:** automatic, discreet vibration at targeted acupoints
- **Manual trigger:** a push button lets the wearer activate relief and teach the device at the same time
- **Wearable and battery-powered:** hand-soldered onto a compact perfboard for a small footprint

---

## 🧰 Bill of Materials

| Component | Qty | Purpose |
|---|:---:|---|
| Seeed Studio XIAO ESP32-C3 | 1 | Microcontroller running the C++ firmware |
| MAX30105 pulse sensor module | 1 | Optical heart-rate sensing over I²C |
| Coin vibration motors | 3 | Haptic feedback at acupoints |
| NPN transistor | 1 | Low-side switch driving the motors |
| Diode | 1 | Flyback protection across the motor load |
| Push button | 1 | Manual trigger and calibration input |
| 10 kΩ resistor | 1 | Button pull-down |
| Battery | 1 | Portable power supply |
| Perfboard, wire, solder | — | Compact hand-soldered assembly |

---

## 🔌 Circuit

### Sensor connection (I²C)

```
  MAX30105                XIAO ESP32-C3
  ────────                ─────────────
  VIN  ──────────────────  3V3
  GND  ──────────────────  GND
  SDA  ──────────────────  SDA
  SCL  ──────────────────  SCL
```

### Push button input

```
  3V3 ──── [ Button ] ────┬──── ESP32-C3 GPIO (input)
                          │
                       [10 kΩ]   pull-down
                          │
                         GND
```

### Motor driver

```
  Battery (+) ─────────┬────────────────────┐
                       │                    │
              ┌────────┴────────┐           │
              │  M1   M2   M3   │         ──┴──
              │   (parallel)    │          ▲     flyback diode
              └────────┬────────┘           │     (cathode to +)
                       ├────────────────────┘
                       │
                       C
  ESP32-C3 GPIO ────── B    NPN transistor
                       E
                       │
  Battery (−) ─────────┴───────────── GND
```

**Design notes**
- The MAX30105 communicates with the ESP32-C3 over **I²C**, using only two data lines.
- The three motors are wired **in parallel** and switched low-side by an **NPN transistor**, so the motor current never flows through the microcontroller's GPIO pin.
- A **flyback diode** across the motors absorbs the voltage spike when the motors switch off, protecting the transistor and the board.
- The **10 kΩ pull-down resistor** holds the button input at a clean LOW until it is pressed, preventing false triggers.

---

## 💻 Engineering Skills Demonstrated

| Area | What I did |
|---|---|
| **Embedded C++** | Co-wrote firmware for sensor reading, BPM calculation, threshold logic and motor control |
| **Algorithm design** | Replaced a fixed threshold with an adaptive, user-calibrated detection method |
| **Sensor integration** | Interfaced an optical pulse sensor over I²C and handled noisy physiological signals |
| **Circuit design** | Designed a transistor motor driver with flyback protection and a stable button input |
| **Hardware prototyping** | Assembled the design from breadboard to a compact wearable across 5 prototypes |
| **Human-centred design** | Designed for discretion and comfort in real-world, public settings |

## 🔁 Development Journey

Neuroband went through **five prototype iterations**. The most important change came when we tested on ourselves: our resting heart rates were very different, which showed that a fixed BPM threshold would fail for most users. We redesigned the detection logic so it calibrates to each wearer, then refined the hardware into a compact, wearable form.

> **Key lesson:** When data is limited, build logic that stays sound with what you know and gets stronger with every use.

## 🌍 SDG Goals

**SDG 3 — Good Health and Well-being:** supports mental health by giving people an accessible, discreet tool for managing anxiety.

---

## 📄 License

- **Hardware:** CERN Open Hardware License Version 2 – Strongly Reciprocal (CERN-OHL-S-2.0)
- **Software:** MIT License
- **Documentation:** Creative Commons Attribution 4.0 International (CC BY 4.0)

---

## 👥 Team

| Name | Role |
|---|---|
| Vir Chanda | Circuit design, C++ coding, BOM, presentation |
| Kian M. | Coding, Assembly |
| Twisha G. | Soldering |

<div align="center">

*Built at Maker's Asylum Innovation School, Goa*

</div>
