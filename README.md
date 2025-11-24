### **Lighthouse (Pico TLC5947 Controller)**

This project drives a TLC5947 (24-channel LED driver) using the Raspberry Pi Pico. It generates sinusoidal fading patterns using compile-time calculated waveforms and uses these to determine the brightness for the LEDs. All of this is in aid of driving LEDs that are to be embedded into a (physical) map where each LED represents one of the [Greate Lighthouses of Ireland](https://www.greatlighthouses.com/]) (this is a cool site, worth checking out).

-----

### **Hardware Requirements**

  * Raspberry Pi Pico
  * TLC5947 PWM LED Driver Breakout
  * LEDs (up to 15 currently configured)

### **Pin Configuration**

If you don't want to change main, connect the TLC5947 to the Pico as follows:

| TLC5947 Pin | Pico GPIO | Description |
| :--- | :--- | :--- |
| **CLK** | GP24 | Clock |
| **DIN / DATA** | GP25 | Serial Data |
| **BLANK** | GP26 | Output Enable |
| **LAT / LATCH** | GP27 | Data Latch |
| **GND** | GND | Common Ground |
| **V+** | VBUS/VSYS | Power |

-----

### **Build Instructions**

#### **1. Init Submodules**

```bash
# If cloning for the first time:
git clone --recurse-submodules <repo-url>

# Or, if you have already cloned the repo:
git submodule update --init --recursive
```

#### **2. Compile the Project**

Run the following commands from the project root:

```bash
mkdir build
cmake -S . -B build
cmake --build build
```

#### **3. Flash the Pico**

1.  Hold the **BOOTSEL** button on your Pico while plugging it into your computer.
2.  Copy the `lighthouses.uf2` file from the `build` directory to the `RPI-RP2` mass storage drive.

If you are using the debug probe, I will assume you know what you are doing.

-----

### **Fun Stuff**
This is just a section on what I found fun when writing the firmware:
  * **`waveforms.hpp`**: Uses `constexpr` and Taylor series expansion to generate sine wave LUTs at compile-time. This avoids expensive runtime floating-point math. Also, the generated LUTs will wind up in ROM rather than RAM. Mind you, this project isn't really big enough to justify such choices, but it's fun to explore.
  * **`tlc5947`**: Driver implementation for the 12-bit PWM shift register. Bit banging for now, but I'll probably rewrite it to use SPI if it ever becomes an issue.

### To you, dear reader
If you use this please let me know. If you run into any problems, please open an issue.