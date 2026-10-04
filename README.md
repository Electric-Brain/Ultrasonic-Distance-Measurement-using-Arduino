# Ultrasonic Distance Measurement using Arduino

**MCA Final Project**

An Arduino UNO based distance meter. An HC-SR04 ultrasonic sensor measures distance, a 2.4"/2.8" TFT shield shows the live reading on a bar gauge, and a buzzer plus red LED alert when an obstacle comes within 8 cm.

---

## Features

- Live distance in cm, updated about every 140 ms
- Colour-coded bar gauge (0 to 100 cm) with an 8 cm danger marker
- Status badge: `CLEAR PATH`, `!! OBSTACLE !!`, `NO SIGNAL`
- Fast beeping buzzer and red LED when distance is 8 cm or less
- Green LED as power indicator
- 4 startup screens: splash, project title, team table, live distance
- Non-blocking code (no `delay()` in the main loop) and minimal screen redraws, so there is little flicker

---

## Hardware

| Component | Quantity |
|---|---|
| Arduino UNO | 1 |
| MCUFRIEND-style 2.4"/2.8" TFT shield (320x240) | 1 |
| HC-SR04 ultrasonic sensor | 1 |
| Active buzzer | 1 |
| Red LED | 1 |
| Green LED | 1 |
| 330 Ω resistor | 2 |
| Jumper wires / breadboard | as needed |

---

## Pin Connections

| Component | Pin | Arduino pin |
|---|---|---|
| HC-SR04 | VCC | 5V |
| HC-SR04 | TRIG | A5 |
| HC-SR04 | ECHO | D10 |
| HC-SR04 | GND | GND |
| Buzzer | + | D12 |
| Buzzer | - | GND |
| Red LED | Anode (+), through 330 Ω | D12 |
| Red LED | Cathode (-) | GND |
| Green LED | Anode (+), through 330 Ω | 5V |
| Green LED | Cathode (-) | GND |

**Resistor paths**

| Branch | Path |
|---|---|
| Red LED | D12 → 330 Ω → LED anode → LED cathode → GND |
| Green LED | 5V → 330 Ω → LED anode → LED cathode → GND |

The buzzer and red LED are in **parallel** on D12. All grounds are common.

### Pins used by the shield (do not reuse)

| Function | Pins |
|---|---|
| LCD data | D2 to D9 |
| LCD control | A0 to A4 |
| SD card | D10 to D13 |

The shield leaves A5 free. D10 and D12 are SD card pins, so **do not insert an SD card** while using this project.

---

## Circuit Diagram

Add the diagram image to the repo and link it here:

```
![Circuit diagram](circuit_diagram.png)
```

---

## Software

**Board:** Arduino UNO

**Libraries** (install from Arduino IDE Library Manager):
- `Adafruit GFX Library`
- `MCUFRIEND_kbv`

**Upload steps**
1. Install the libraries above.
2. Open the `.ino` sketch in Arduino IDE.
3. Select **Tools → Board → Arduino UNO** and the correct port.
4. Upload.

---

## How It Works

1. The Arduino sends a 10 µs pulse on **TRIG**.
2. The sensor sends an ultrasonic burst and raises **ECHO** until the echo returns.
3. `pulseIn()` measures the ECHO pulse width (30 ms timeout).
4. Distance = `time × 0.0343 / 2` cm (speed of sound is 0.0343 cm/µs, divided by 2 for the round trip).
5. If distance is 8 cm or less, the buzzer and red LED toggle every 90 ms. Otherwise they stay off.

### Display logic

| Distance | Colour | Status |
|---|---|---|
| Above 30 cm | Green | CLEAR PATH |
| 8 to 30 cm | Amber | CLEAR PATH |
| 8 cm or less | Red | !! OBSTACLE !! |
| No echo | Grey | NO SIGNAL |

### Screen flow

| Screen | Duration |
|---|---|
| Splash (MCA FINAL PROJECT + loading bar) | about 1.5 s |
| Project title | 3.5 s |
| Team members table | 4.5 s |
| Live distance | continuous |

---

## Configuration

Edit these in the sketch:

| Define | Default | Meaning |
|---|---|---|
| `BEEP_DIST` | 8 | Alert threshold in cm |
| `MAX_DIST` | 100 | Full-scale value of the bar in cm |
| `TRIG`, `ECHO`, `BUZZ` | A5, 10, 12 | Pin assignments |

---

## Notes and Limitations

- HC-SR04 is reliable from about 2 cm to 400 cm. Readings below 2 cm can be wrong.
- Soft or angled surfaces may reflect poorly and show `NO SIGNAL`.
- D12 drives both the buzzer and the red LED. If the buzzer draws more than about 20 mA, drive it through a transistor (BC547 or 2N2222) instead of directly from the pin.
- Active buzzer assumed. For a passive buzzer, use `tone(BUZZ, 2000)` and `noTone(BUZZ)`.
- Display tested rotation: `setRotation(1)` (landscape, 320x240).

---

## Team

| Roll No | Name |
|---|---|
| 68 | Gautam Thakur |
| 69 | Shivraj Khandare |
| 70 | Pratidnya Wakshe |
| 71 | Aditya Yadav |
| 72 | Abhijeet Yadav |
| 73 | Ganesh Lagad |

---

## Suggested Repo Structure

```
ultrasonic-distance-arduino/
├── ultrasonic_distance.ino
├── circuit_diagram.png
├── README.md
└── images/
    └── demo.jpg
```
