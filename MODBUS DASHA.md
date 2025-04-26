# Modbus Function Code Reference

## Bit Access (1-bit — ON/OFF)

| FC (Dec) | FC (Hex) | Data Type      | Action                  | Display Address | Wire Address       | Access     |
|----------|----------|----------------|-------------------------|-----------------|--------------------|------------|
| 01       | 0x01     | Coil (1 bit)   | Read Coils              | 00001 – 09999   | 0x0000 – 0xFFFF    | Read       |
| 05       | 0x05     | Coil (1 bit)   | Write Single Coil       | 00001 – 09999   | 0x0000 – 0xFFFF    | Write      |
| 15       | 0x0F     | Coil (1 bit)   | Write Multiple Coils    | 00001 – 09999   | 0x0000 – 0xFFFF    | Write      |
| 02       | 0x02     | Discrete Input (1 bit) | Read Discrete Inputs | 10001 – 19999 | 0x0000 – 0xFFFF  | Read only  |

---

## 16-Bit Register Access (0 – 65535)

| FC (Dec) | FC (Hex) | Data Type               | Action                        | Display Address | Wire Address       | Access          |
|----------|----------|-------------------------|-------------------------------|-----------------|--------------------|-----------------|
| 03       | 0x03     | Holding Register (16 bit) | Read Holding Registers      | 40001 – 49999   | 0x0000 – 0xFFFF    | Read            |
| 06       | 0x06     | Holding Register (16 bit) | Write Single Register       | 40001 – 49999   | 0x0000 – 0xFFFF    | Write           |
| 16       | 0x10     | Holding Register (16 bit) | Write Multiple Registers    | 40001 – 49999   | 0x0000 – 0xFFFF    | Write           |
| 23       | 0x17     | Holding Register (16 bit) | Read/Write Multiple Registers | 40001 – 49999 | 0x0000 – 0xFFFF    | Read + Write    |
| 04       | 0x04     | Input Register (16 bit)  | Read Input Registers         | 30001 – 39999   | 0x0000 – 0xFFFF    | Read only       |

---

## Diagnostics & Special

| FC (Dec) | FC (Hex) | Data Type        | Action                          | Display Address | Wire Address    | Access          |
|----------|----------|------------------|---------------------------------|-----------------|-----------------|-----------------|
| 07       | 0x07     | —                | Read Exception Status           | —               | —               | Read only       |
| 08       | 0x08     | —                | Diagnostics (sub-codes 00–18)   | —               | —               | Read + Write    |
| 11       | 0x0B     | —                | Get Com Event Counter           | —               | —               | Read only       |
| 12       | 0x0C     | —                | Get Com Event Log               | —               | —               | Read only       |
| 17       | 0x11     | —                | Report Server ID                | —               | —               | Read only       |
| 20       | 0x14     | File record      | Read File Record                | —               | —               | Read only       |
| 21       | 0x15     | File record      | Write File Record               | —               | —               | Write           |
| 22       | 0x16     | Holding Register | Mask Write Register             | 40001 – 49999   | 0x0000 – 0xFFFF | Write           |
| 24       | 0x18     | —                | Read FIFO Queue                 | —               | —               | Read only       |
| 43       | 0x2B     | —                | Read Device Identification (MEI)| —               | —               | Read only       |

---




















# Notes

- **Wire address = Display address − 1**  
  Example: Display `40001` → Wire address `0x0000`

- **The display prefix (0 / 1 / 3 / 4) never goes on the wire.**  
  The Function Code tells the slave which table to look in.

- **FC 08** has 19 sub-codes for loopback tests, counter resets, and diagnostics.

- **FC 23** reads and writes in a single packet — useful when you want to send a setpoint and receive a reading in one round trip.

- **Most commonly used in the field:** FC 01, 02, 03, 04, 05, 06, 16.




# Yes! Exactly Right! Data Types Consume Multiple Registers

## Why? Because Each Register is Only 16-bit

```
16-bit register = 2 bytes = max value 65535

But Float needs 32 bits = 4 bytes = 2 registers!
Long needs  32 bits = 4 bytes = 2 registers!
64-bit needs 64 bits = 8 bytes = 4 registers!
```

---

## Each Data Type — How Many Registers It Uses

| Display Option | Bits | Registers Used | Address Space |
|---|---|---|---|
| **Binary** | 16 | 1 register | 40001 only |
| **Decimal** | 16 | 1 register | 40001 only |
| **Hex** | 16 | 1 register | 40001 only |
| **Long Integer** | 32 | 2 registers | 40001 + 40002 |
| **Long (Swapped)** | 32 | 2 registers | 40001 + 40002 |
| **Floating Point** | 32 | 2 registers | 40001 + 40002 |
| **Float (Swapped)** | 32 | 2 registers | 40001 + 40002 |
| **64-Bit Float** | 64 | 4 registers | 40001–40004 |
| **64-Bit Swapped** | 64 | 4 registers | 40001–40004 |

---

## Your ModSim Screenshot — I Can See This!

```
40014: <00546>
40015: <00798>   ← these two together = one Float value!
40016: <00000>

40026: <00546>
40027: <06789>   ← these two together = another Float value!
40028: <00000>
```

---

## Example — Storing Float Values

```
If you store Float at 40001:
  40001 → High word (first 16 bits)
  40002 → Low word  (last 16 bits)
  40003 → Next value starts HERE

NOT like this ❌:
  40001 → Float value
  40002 → another Float   ← WRONG! 40002 is still occupied by first float!
```

---

## Practical Rule

```
Decimal/Hex/Binary → every address is independent
                      40001, 40002, 40003... each has own value

Float / Long       → skip every 2 addresses
                      40001 (value1), 40003 (value2), 40005 (value3)

64-bit             → skip every 4 addresses
                      40001 (value1), 40005 (value2), 40009 (value3)
```

---

## What is Swapped?

```
Normal Float:   [ High Word ][ Low Word ]
                   40001        40002

Swapped Float:  [ Low Word  ][ High Word ]
                   40001        40002
```

Some devices store bytes in different order (**Big Endian vs Little Endian**). If your value looks garbage — try the Swapped version!

---

## So When You Enter Values in ModSim

```
Select Display → Floating Point
Double click 40001 → enter your float value
ModSim automatically splits it across 40001 and 40002

Your next float value → put at 40003 (not 40002!)
```

Want me to explain Endianness (swapped vs normal) in more detail?











# It's the Off-By-One Address Difference!

## What You Did vs What Vinci Shows

```
ModSim display  →  40042  (you put 4444 here)
Vinci address   →  41     (shows 4444 here)
```

## Why? Display Address vs Wire Address

```
ModSim shows:   40042
                  ↓
Wire address =  40042 - 40001 = 41
                  ↓
Vinci reads wire address → shows 41
```

## Same Thing for Your 333 Value Too!

```
ModSim  40019  →  Vinci address 18   (19-1 = 18) ✓
ModSim  40042  →  Vinci address 41   (42-1 = 41) ✓
```

## Simple Rule

```
ModSim display address    Wire address (Vinci)
      40001          →         0
      40002          →         1
      40018          →        17
      40019          →        18   ← your 333
      40042          →        41   ← your 4444
```

---

## It Is Not a Bug — It Is Normal!

```
ModSim → shows 4xxxx format  (human readable display)
Vinci  → shows raw wire address (what actually goes on the RS485 wire)

Both are pointing to the EXACT SAME register!
Just different ways of displaying it.
```

> Always remember: **Wire address = Display address − 40001** for Holding Registers









# Modbus Exception Codes — Simply Explained

## What is an Exception Code?

```
Normal response:   Master asks → Slave replies with DATA
Exception response: Master asks → Slave replies with ERROR code

How to know it's an error?
→ Slave sends back FC + 0x80

Example:
  You sent  FC 03 (0x03)
  Error back FC 83 (0x03 + 0x80 = 0x83)  ← error flag!
```

---

## Exception Code 01 — ILLEGAL FUNCTION

**Simple meaning: "I don't know this command!"**

```
Real world example:
  You ask a basic calculator → "what is sin(x)?"
  Calculator says → "I don't have that function!"

Modbus example:
  Master sends FC 08 (Diagnostics)
  Slave is a simple cheap sensor
  Slave replies → Exception 01
  "I don't support FC 08, I only know FC 03 and 04!"
```

---

## Exception Code 02 — ILLEGAL DATA ADDRESS

**Simple meaning: "That address doesn't exist in me!"**

```
Real world example:
  Building has floors 1 to 10
  You press button 15 in elevator
  Elevator says → "Floor 15 doesn't exist!"

Modbus example:
  Slave has registers from 0 to 99 (100 registers)
  Master asks: "Give me 5 registers starting at address 96"
  That means → 96, 97, 98, 99, 100
  But address 100 doesn't exist! → Exception 02 ❌

  If master asked 4 registers from 96 → 96,97,98,99 ✅ works fine!
```

---

## Exception Code 03 — ILLEGAL DATA VALUE

**Simple meaning: "The number you sent makes no sense!"**

```
Real world example:
  Form asks for your age
  You type "abc" or "-500"
  Form says → "Invalid value!"

Modbus example:
  FC 01 Read Coils → max quantity allowed = 2000
  Master sends quantity = 5000
  Slave replies → Exception 03
  "I can't read 5000 coils at once, max is 2000!"

⚠️ Important: This does NOT mean the value is wrong
   for your application. Modbus doesn't care if 
   you write 9999 to a temperature register.
   It only checks protocol-level limits!
```

---

## Exception Code 04 — SERVER DEVICE FAILURE

**Simple meaning: "I understood you, but something broke inside me!"**

```
Real world example:
  ATM understands your request
  But the cash mechanism is jammed
  ATM says → "Unable to process, internal error"

Modbus example:
  Master asks to read temperature sensor register
  Slave's internal ADC hardware failed
  Slave replies → Exception 04
  "I got your request but my hardware failed to read!"
```

---

## Exception Code 05 — ACKNOWLEDGE

**Simple meaning: "I got your request, but give me time — I'll get back to you!"**

```
Real world example:
  You order food at restaurant
  Waiter says → "Order received! It will take 30 minutes"
  Waiter didn't bring food yet — just confirmed!

Modbus example:
  Master sends a firmware update command
  Slave says → Exception 05
  "I accepted it! But it takes long time to process.
   Poll me later to check if I'm done!"

Master should then send "Poll Program Complete" 
message to check if slave finished.
```

---

## Exception Code 06 — SERVER DEVICE BUSY

**Simple meaning: "I'm busy right now, try again later!"**

```
Real world example:
  You call a customer care number
  "All our agents are busy, please call again later"

Modbus example:
  Slave is running a long program/calibration
  Master sends a new request during that time
  Slave replies → Exception 06
  "I'm busy! Retransmit your message when I'm free!"
```

---

## Exception Code 08 — MEMORY PARITY ERROR

**Simple meaning: "I tried to read my own memory but it's corrupted!"**

```
Real world example:
  You try to open a file on your PC
  Windows says → "File is corrupted, cannot read!"

Modbus example:
  Master asks to Read File Record (FC 20)
  Slave tries to read from its internal file storage
  Finds memory corruption
  Slave replies → Exception 08
  "My memory has a parity error, data is corrupted!"
```

---

## Exception Code 0A — GATEWAY PATH UNAVAILABLE

**Simple meaning: "The gateway can't find a route to the slave!"**

```
Real world example:
  You send a package via courier
  Courier says → "We have no route to that location,
                  our internal routing is broken!"

Modbus example:
  You use a Modbus TCP→RTU gateway
  Gateway can't connect its input to output port
  Gateway misconfigured or overloaded
  Returns → Exception 0A
```

---

## Exception Code 0B — GATEWAY TARGET DEVICE FAILED TO RESPOND

**Simple meaning: "Gateway found the route but slave is not responding!"**

```
Real world example:
  Courier found the address
  But nobody is home to receive the package!

Modbus example:
  Gateway correctly routed the request to slave
  Slave device is powered off / disconnected / dead
  No response came back
  Gateway returns → Exception 0B
  "I reached that address but the device isn't there!"
```

---

## Quick Cheat Sheet

| Code | Name | One Line |
|---|---|---|
| 01 | Illegal Function | FC not supported |
| 02 | Illegal Address | Address out of range |
| 03 | Illegal Value | Quantity/value invalid |
| 04 | Device Failure | Hardware error inside slave |
| 05 | Acknowledge | Accepted, still processing |
| 06 | Device Busy | Try again later |
| 08 | Memory Parity Error | Slave memory corrupted |
| 0A | Gateway Path Unavailable | Gateway misconfigured |
| 0B | Gateway No Response | Slave device is missing/dead |