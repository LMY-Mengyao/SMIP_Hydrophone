## Description

This demo runs on an Ambiq Apollo3 EVB and synchronizes to a SmartMesh IP mote using TIMEn.

After the mote joins the network (OPERATIONAL), the Apollo3 schedules a shared UTC start time
(aligned to a UTC minute boundary) and then outputs a continuous **2 Hz pulse** (500 ms period)
on a GPIO pin. For each pulse, the Apollo3 also sends a small timestamp packet over UART.

- **Pulse output:** GPIO 26
- **Pulse period:** 500 ms (2 Hz)
- **Timestamp UART:** UART1 TX (GPIO 35)
- **Timestamp packet:** `[0x73][NodeID][UTC_ms (uint32 little-endian)]`

---

## Notes (Apollo3)

- The three Apollo3 boards have been pre-flashed with the firmware in this repository.
- If needed, you can re-flash the firmware using SEGGER J-Link (J-Link OB via USB, or an external J-Link via SWD).
- Pre-built binaries:
  - [firmware/SMIP1PPS_3nodes_node1.bin](firmware/SMIP1PPS_3nodes_node1.bin)
  - [firmware/SMIP1PPS_3nodes_node2.bin](firmware/SMIP1PPS_3nodes_node2.bin)
  - [firmware/SMIP1PPS_3nodes_node3.bin](firmware/SMIP1PPS_3nodes_node3.bin)

Install SEGGER J-Link:  
https://www.segger.com/downloads/jlink/

## Notes (RPi)

- Demo Python code:
  - https://github.com/liyinze1/pi-hydrophone

---

## Hardware Connections

### Apollo3 EVB ↔ SmartMesh Mote (UART0 + TIMEn)

| Apollo3 EVB        | SmartMesh Mote |
| ------------------ | -------------- |
| UART0 TX (GPIO 22) | RX             |
| UART0 RX (GPIO 23) | TX             |
| GPIO 12            | TIMEn          |
| GND                | GND            |
| VDD (3V3)          | VBAT           |
| GND                | TX_CTSn        |
| VDD (3V3)          | RX_RTSn        |

### Apollo3 EVB ↔ HiFiBerry RPi Shield (UART1 + Pulse)

| Apollo3 EVB        | HiFiBerry RPi Shield |
| ------------------ | -------------------- |
| UART1 TX (GPIO 35) | RX                   |
| GPIO 26            | Pulse input          |
| GND                | GND                  |

**Default UART settings:** 115200 8N1

### UART timestamp output

Each pulse triggers a **6-byte** binary packet on UART1:

- Byte0: `0x73`
- Byte1: `NODE_ID` (configured in the source)
- Byte2..5: `UTC_ms` (little-endian `uint32`)

---

## Hardware Photos

<p><b>SMIP mote</b><br>
<img src="https://github.com/LMY-Mengyao/SMIP_Hydrophone/raw/main/images/IMG_5647.jpg" width="600"></p>

<p><b>Apollo3 EVB</b><br>
<img src="https://github.com/LMY-Mengyao/SMIP_Hydrophone/raw/main/images/IMG_5648.jpg" width="600"></p>

<p><b>HiFiBerry RPi Shield</b><br>
<img src="https://github.com/LMY-Mengyao/SMIP_Hydrophone/raw/main/images/IMG_5649.jpg" width="600"></p>

---

## How to Run

### Power on

1. Power the **SmartMesh Manager**.
2. Power each **Apollo3 EVB + SmartMesh Mote** node.
3. Wait until all motes join the network (this may take up to **~10+ minutes**).

### Check join status (Manager CLI)
See: [SMIPmanager_README.md](ALL_README/SMIPmanager_README.md)
Open the SmartMesh Manager **CLI serial port** (often the **3rd COM port**), then run:

```text
login user
sm
```
### Pulse / timestamp start condition

- The Apollo3 outputs pulses and timestamps only after the mote is OPERATIONAL.
- After join, the node waits until the next UTC minute boundary, then starts:
  - Pulse on GPIO 26 (2 Hz, 500 ms period)
  - Timestamp packets on UART1 TX (GPIO 35)

### Recommended reset (stability)

After confirming (via Manager CLI) that all nodes have joined, reset all Apollo3 boards. This usually results in faster re-join and more stable pulse synchronization.

