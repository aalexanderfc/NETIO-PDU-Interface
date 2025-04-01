# 🔌 NETIO PDU 4C Dashboard Control with ThingsBoard via Modbus

This repository demonstrates how to integrate and control a **NETIO PowerPDU 4C** over **Modbus TCP** using **ThingsBoard**, an open-source IoT platform. This project highlights how to read telemetry data and control power outputs directly through a custom dashboard.

---

## 🎯 Purpose

The goal of this integration is to **remotely control and monitor a NETIO PowerPDU 4C device from outside the default NETIO web interface**, using a **custom-built dashboard inside the `delta.acandia` ThingsBoard instance**.

This solution provides:
- Centralized **visibility of electrical parameters** like voltage, current, power, frequency, and power factor.
- Remote **power output management** (ON/OFF/TOGGLE) per socket.
- Integration into a larger IoT ecosystem for future expansion (alerts, reports, automation, etc.).
- A practical alternative interface for environments where the default NETIO web UI is not suitable or accessible.

By using **Modbus TCP**, this setup ensures robust industrial-grade communication, enabling seamless real-time data synchronization between the physical PDU and the ThingsBoard interface hosted at `delta.acandia`.

---

## 📸 Screenshots

<table>
  <tr>
    <td><img src="assets/NETIO-dashboard.jpg" width="400"/></td>
    <td><img src="assets/Latest-telemetry.jpg" width="400"/></td>
  </tr>
  <tr>
    <td align="center">📊 Real-time Dashboard</td>
    <td align="center">📅 Telemetry View</td>
  </tr>
  <tr>
    <td><img src="assets/Output-buttons.jpg" width="400"/></td>
    <td><img src="assets/Outputs-socket.jpg" width="400"/></td>
  </tr>
  <tr>
    <td align="center">👡️ Output Control Switches</td>
    <td align="center">🔌 NETIO PowerPDU 4C Device</td>
  </tr>
</table>

---

## 🧰 About NETIO PDU 4C

The **NETIO PowerPDU 4C** is a smart power distribution unit with four individually controllable IEC-320 C13 power outputs. It supports multiple M2M protocols including **Modbus TCP**, MQTT, SNMP, and more. Designed for IT, AV, and industrial use, it allows remote switching, power measurement, and automation integration.

Key Features:
- 4x switchable power outputs (socket control)
- Power metering per output (depending on model)
- Industrial-grade M2M communication protocols
- API and local web interface

Useful Register Map (from [NETIO Modbus TCP API Manual](./NETIO-Modbus-TCP_M2M-API-Protocol.pdf)):

| Function                  | Register Address | Type   | Description                                    |
|---------------------------|------------------|--------|------------------------------------------------|
| Power grid frequency     | 0                | `uInt16` | x100 Hz                                       |
| Voltage RMS              | 1                | `uInt16` | x10 Volts                                     |
| True Power Factor        | 2                | `uInt16` | /1000                                          |
| Current (all outputs)    | 100              | `uInt16` | mA                                             |
| Power (all outputs)      | 200              | `int16`  | Watts                                          |
| Output N State (R/W)     | 101-104          | `uInt16` | Read or set socket ON/OFF/TOGGLE state         |

---

## 🛠️ Technologies Used

- ⚙️ **Modbus TCP** (protocol for communication with the NETIO device)
- 📡 **ThingsBoard Community Edition v3.9.0**
- 🧠 **Custom Modbus configuration** for telemetry and RPC control
- 💻 **Custom Dashboard** with:
  - Power and current charts
  - Switch controls per output
  - Frequency and voltage visualizations

---

## 📦 Features

- ✅ Real-time visualization of:
  - Voltage (V)
  - Current (mA)
  - Power (W)
  - Frequency (Hz)
  - True Power Factor
- ✅ Switch control of 4 output ports (ON/OFF/TOGGLE)
- ✅ Telemetry collected via Modbus input and holding registers
- ✅ Remote management through ThingsBoard RPC

---

## 🗺️ Modbus Configuration

The configuration was defined using:
- **Holding Registers (Function Codes 03/06)** for Output control and state
- **Input Registers (Function Code 04)** for measurement telemetry

Example register mapping:

| Metric            | Register | Type   | Description                         |
|-------------------|----------|--------|-------------------------------------|
| Voltage           | 1        | `uInt16` | Voltage (×10)                       |
| Frequency         | 0        | `uInt16` | Frequency (×100)                    |
| All Outputs Power | 200        | `int16`  | Power in Watts                      |
| Output 1 Control  | 101      | `uInt16` | Write: Action for Output 1 (6)function   |
| Output 1 Status   | 101      | `uInt16` | Read: State of Output 1 (0/1) (3)function |

---

## ⚡ Output Control Actions

Control outputs using RPC calls:
- `0` = OFF
- `1` = ON
- `2` = Short OFF
- `3` = Short ON
- `4` = TOGGLE
- `5` = No action

---

## 🧹 Dashboard Interaction

- Switches are configured via ThingsBoard widgets.
- Telemetry is visualized using real-time charts and historical data panels.
- The Modbus connector parses registers and sends updates to ThingsBoard.
- Control is implemented using **RPC methods** tied to register writes.

---

## 🧐 Author

**Alexander Flores**  
IoT & Embedded Systems Developer  
GitHub: [aalexanderfc](https://github.com/aalexanderfc)
