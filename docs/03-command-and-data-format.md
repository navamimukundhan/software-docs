# 03 - Command & Data Format Specification (ICD)

This Interface Control Document (ICD) defines the binary or text-based communication protocol agreed between the **PC Application** and the **Embedded Firmware**.

---

## 1. Physical Layer & Connection Settings

| Parameter | Specification |
| :--- | :--- |
| **Interface** | USB Virtual COM Port (UART via USB-Serial bridge) |
| **Baud Rate** | `115200` bps (Default) |
| **Data Bits** | `8` |
| **Parity** | `None` |
| **Stop Bits** | `1` |
| **Flow Control** | `None` |
| **Endianness** | Little-Endian (or ASCII delimited format if text-based) |

---

## 2. Packet Framing Format

### Standard Binary Frame (Recommended)
```
+--------------+----------+--------+----------------+---------------+------------+
| Header (2B)  | Cmd (1B) | Len(1B)|  Payload (N B) | Checksum (1B) | Footer(1B) |
| 0xAA 0x55    | OPCODE   | 0..255 | Parameter data | XOR / CRC-8   | 0x0A (\n)  |
+--------------+----------+--------+----------------+---------------+------------+
```

| Field | Size (Bytes) | Description |
| :--- | :--- | :--- |
| **Header** | 2 | Synchronization marker: `0xAA 0x55` |
| **Command ID (Opcode)** | 1 | Identifies the action or telemetry type (e.g., `0x01` = Ping, `0x02` = Start) |
| **Payload Length** | 1 | Number of data bytes following in the payload (`N`) |
| **Payload** | N | Command arguments or telemetry data |
| **Checksum / CRC** | 1 | Checksum (e.g. XOR sum or CRC8 of Cmd + Len + Payload) |
| **Footer** | 1 | End-of-frame delimiter: `0x0A` (`\n`) |

*(Alternative Text/ASCII Format: `$CMD,<ARG1>,<ARG2>*<CHECKSUM>\r\n`)*

---

## 3. Command Opcode Table (PC -> Firmware)

| Opcode (Hex) | Name | Direction | Payload Description | Expected Response |
| :--- | :--- | :--- | :--- | :--- |
| `0x01` | `PING` | PC -> FW | None (Len = 0) | `0x81 PONG` (`STATUS_OK`) |
| `0x02` | `START_TEST` | PC -> FW | `uint16_t duration_sec`, `uint8_t mode` | `0x80 ACK` or `0x8F NACK` |
| `0x03` | `STOP_TEST` | PC -> FW | None (Emergency stop / manual halt) | `0x80 ACK` |
| `0x04` | `SET_CALIB` | PC -> FW | `uint8_t channel`, `float offset`, `float gain` | `0x80 ACK` |
| `0x05` | `GET_STATUS` | PC -> FW | None | `0x82 STATUS_REPORT` |
| `0x06` | `SET_ACTUATOR`| PC -> FW | `uint8_t actuator_id`, `uint16_t value` | `0x80 ACK` |

---

## 4. Response & Telemetry Frames (Firmware -> PC)

| Opcode (Hex) | Name | Direction | Payload Description |
| :--- | :--- | :--- | :--- |
| `0x80` | `ACK` | FW -> PC | `uint8_t cmd_acknowledged`, `uint8_t status_code` |
| `0x81` | `PONG` | FW -> PC | Firmware version string (e.g. `"FW-v1.2.0"`) |
| `0x82` | `STATUS_REPORT`| FW -> PC | `uint8_t system_state`, `uint16_t fault_flags`, `uint32_t uptime_ms` |
| `0x83` | `STREAM_DATA` | FW -> PC | Periodic sensor packet: `uint32_t timestamp_ms`, `float ch1_val`, `float ch2_val`, `uint16_t raw_adc` |
| `0x8F` | `NACK / ERROR` | FW -> PC | `uint8_t failed_cmd`, `uint8_t error_code` |

---

## 5. Status & Error Codes

| Error Code (Hex) | Name | Meaning | Recommended Recovery |
| :--- | :--- | :--- | :--- |
| `0x00` | `ERR_NONE` | Operation completed successfully | Continue normal operation |
| `0x01` | `ERR_BAD_CHECKSUM`| Checksum mismatch in received packet | PC retransmits frame |
| `0x02` | `ERR_INVALID_CMD` | Unknown or unsupported Opcode | Verify firmware version |
| `0x03` | `ERR_PARAM_RANGE` | Parameter value exceeds hardware limit | Check UI input validation |
| `0x04` | `ERR_BUSY` | System is currently running another task | Wait for task completion or send STOP |
| `0x05` | `ERR_HARDWARE_FAULT`| Sensor disconnected or actuator failure | Trigger emergency stop and inspect hardware |

---

## 6. Example Interaction Trace

```
PC -> FW: 0xAA 0x55 0x01 0x00 0x01 0x0A                     (PING)
FW -> PC: 0xAA 0x55 0x81 0x05 "1.0.0" 0x3E 0x0A             (PONG, Version 1.0.0)

PC -> FW: 0xAA 0x55 0x02 0x03 0x00 0x3C 0x01 0x3E 0x0A       (START_TEST, Duration 60s, Mode 1)
FW -> PC: 0xAA 0x55 0x80 0x02 0x02 0x00 0x80 0x0A            (ACK to Command 0x02, Success)

FW -> PC: 0xAA 0x55 0x83 0x0C [Timestamp] [Sensor1] [Sensor2] [Checksum] 0x0A (STREAM_DATA)
```
