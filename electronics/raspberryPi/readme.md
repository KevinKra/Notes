1. what is an actuator?
1. what operating system is on the RaspberryPi?
1. what is the latest single board computer (`SBC`) from RaspberryPi?
1. what is a `microcontroller`, what is the name of RPi's micro controller?

1.
1. RaspberryPi runs on the Linux operating system.
1. RaspberryPi 4 is the latest single board computer.

---

#### Component Questions

1. what is a `HAT`?
1. can `SBCs` like the Raspberry Pi automatically identify connected HATs?
1. what is `GPIO`?
1. what is an `ADC`?
1. what is a `DAC`?
1. Arduino vs Raspberry Pi
1. what is a `Photo Transistor`?
1. what is an `LDR`?
1. what is the Raspberry Pi `Pico`?
1. what are some differences between the Raspberry Pi and the Pico?
1. what languages can you use to program on the Pico?
1. what is a `PWM channel`, what is another name for it?
1. what is an `RTC`?
1. what does `NOOBS` mean?
1. what is a `optoisolater` | `photocoupler`? What other names can be used for this?
1. describe `analogue` vs `digital` signals.

#### Component Answers

1. `HAT` (Hardware Attached on Top). A `HAT` is an add-on board for B+ that conforms to a specific set of rules that will make life easier for users.
1. yes, A significant feature of HATs is the inclusion of a system that allows the B+ to identify a connected HAT **and automatically configure the GPIOs and drivers for the board.**
1. `GPIO` (General Purpose Input/Output). At the most basic level, GPIO refers to a set of pins on your computer’s mainboard or add-on card. These pins can send or receive electrical signals, but **they aren’t designed for any specific purpose.** This is why they’re called “general-purpose” IO.
1. `ADC` (analog-to-digital converter). In electronics, an analog-to-digital converter (ADC, A/D, or A-to-D) is a system that converts an analog signal, such as a sound picked up by a microphone or light entering a digital camera, into a digital signal.
1. `DAC` (digital-to-analog converter) converts a digital signal from the computer into an electrical voltage which can be used to drive electrical equipment, for example, a stirrer motor.
1. a `Photo Transistor` is a sensor that detects ambient light, a bit like an LDR (and works in a similar way). When light enters the chip on top, it creates a flow of current from the long to the short pin. When there's no light there's almost no current going across the pins at all. It manages to detect light in a similar way to your own eyes, so it's great for projects that require human-level light monitoring.
1. `LDR` (Light Dependent Resistors) are little light sensors, as the squiggly face on its head is exposed to more light, the resistance goes down. When it's light, the resistance is about 5-10K ohms, when dark it goes up to 200K ohms.
1. the `Pico` is an entirely new type of **microcontroller** from Raspberry Pi. Small, cheap and flexible.
1. the `Pico` isn’t designed to replace the Raspberry Pi, which is a different class of device known as a **single-board computer**. Whereas you might use your Raspberry Pi to play games, write stories and browse the web, your Raspberry Pi Pico is designed for physical computing projects where it can control familiar components such as LEDs, buttons, sensors, motors and even other microcontrollers.
1. the `Pico` can be quickly and easily programmed using **MicroPython and C/C++** using popular editors such as Thonny.
1. a `PWM channel` (Pulse-Width modulation), or **pulse-duration modulation** (PDM), is a method of reducing the average power delivered by an electrical signal, by effectively chopping it up into discrete parts.
1. `RTC` (Real Time Clock) accessory plugs on top of your Pi's GPIO pins and contains a clock chip and a super-cap that allows your Raspberry Pi to remember the time.
1. `NOOBS` means "New Out of Box Software".
1. An `optoisolator` (also known as an `optical coupler`, `photocoupler`, `optocoupler`) is a semiconductor device that transfers an electrical signal between isolated circuits using light. These electronic components are used in a wide variety of communications and monitoring systems that use electrical isolation to prevent high voltage emitters from affecting lower power circuitry receiving a signal.
1. `analog` signals have continuous electrical signals, while `digital` signals have non-continuous electrical signals.

---

#### SBC Questions

1. what is an SBC?
1. what is a CPU?
1. how does SBC work?
1. explain `CPU/SoC`
1. explain `Clock/Timer`
1. explain `I/O port`
1. explain `Serial Ports`
1. explain `ADC/DAC`
1. explain `Wireless Connectivity`

#### SBC Answers

1. an `SBC` is a Single Board Computer. Single Board Computers are small computing devices that have all of the elements of a complete computer contained within one single circuit board. It is a low cost, self-contained, simple that could be easily connected to other hardware.
1. CPU is the core component in a computing unit, which is responsible for processing and executing instructions. It runs the operating system and applications, constantly receiving input from the user or active software programs.
1. SBCs uses a **System-on-Chip (SoC)**, which integrates all or most components of a computer or other electronic system. It works exactly like how an MPU does but even more, such as signal processing, wireless communication, artificial intelligence etc., on top of transmitting data.
1. `CPU/SoC:` Brain of the SBC, responsible for fetching the instruction, decodes it, then execute it.
   Memory: Similar to how it functions in MPU and MCU, used to store data and program. However, unlike the previous 2, there are RAM and ROM or flash memories for storing program source codes.
1. `Clock/Timer:` Provides all timing and counting functions inside the microcontroller. It is used to perform clock functions, modulations etc.
1. `I/O port:` It allows you to drive/interface various devices such as LCD’S, LED’S, printers, memories, etc to a microcontroller.
1. `Serial Ports:` Provides various serial interfaces between a microcontroller and other peripherals like parallel ports.
1. `ADC/DAC:` Analog to Digital Converter (ADC), which converts the analog signal into the digital signal. Digital to Analog Converter (DAC) and it converts the Digital signal into an analog signal.
1. `Wireless Connectivity:` Consists of Bluetooth and WiFi

---

#### MPU Questions

1. what is an MPU?
1. how does an MPU work?
1. what is a `Bus`? What are the tree types of buses usually present on an MPU?
1. what is `clock speed` used to measure, what type of measurements are there?
1. what is an `instruction set`?
1. what is `word length`?
1. what is `cache memory`?

#### MPU Answers

1. an `MPU` is a Microprocessor (or Micro Processing Unit). It is the central unit of a computer system that performs arithmetic and logic operations. It also incorporates the function of a CPU on an integrated circuit.
1. MPU basically accepts binary data as the input, processes the data according to instructions stored in its memory, and provides results (also in binary form) as output.
1. `Bus:` Describe the set of conductors that transmit data or that address or control information to the microprocessor’s different elements. (MPU usually has 3 types: data bus, the address bus, and control bus.)
1. `Clock Speed:` Normally measured in Hertz and expressed in measurements like MHz (megahertz) and GHz (gigahertz), refers to the speed at which a microprocessor could execute instructions.
1. `Instruction Set:` A series of commands that a microprocessor can understand, basically the interface between hardware and software.
1. `Word Length:` The number of bits in the processor’s internal data bus, an 8-bit MPU can process 8-bit data at one time.
1. `Cache Memory:` It stores data or instructions that the software or program frequently references during operation, increases overall speed as it allows the processor to access data more quickly.

---

#### MCU Questions

1. what is an MCU?
1. how does an MCU work?
1. explain `CPU`.
1. explain `Memory`.
1. explain `Timer`.
1. explain `I/O Port`.
1. explain `Serial Ports`.
1. explain `Interrupt control`.
1. what bit types of MCUs exist?

##### MCU Answers

1. an `MCU` is a Microcontroller. The Microcontroller is also known as Microcontroller unit, it is an `integrated circuit (IC) device` used for controlling other portions of an electronic system, usually via a **microprocessor unit (MPU), memory, and some peripherals.**
1. They basically take inputs from the device they controlling and retain control by sending the device signals to different parts of the device, usually run on one specific program and are dedicated to a single task. Thus, they’re comparably weaker than MPU.
1. `CPU:` Brain of the MCU, responsible for fetching the instruction, decodes it, then execute it.
1. `Memory:` Similar to how it functions in MPU, used to store data and program. But in MCU, there are RAM and ROM or flash memories for storing program source codes.
1. `Timer:` Provides all timing and counting functions inside the microcontroller. It is used to perform clock functions, modulations etc.
1. `I/O port:` It allows you to drive/interface various devices such as LCD’S, LED’S, printers, memories, etc to a microcontroller.
1. `Serial Ports:` Provides various serial interfaces between a microcontroller and other peripherals like parallel ports.
1. `Interrupt control:` Provides interrupt (delay) for a working program, it could occur internally or externally.
1. 8-bit (cheapest and most common), 16-bit, 32-bit.

## Resources:

[https://www.seeedstudio.com/blog/2020/10/27/all-about-cpus-microprocessor-microcontroller-and-single-board-computer/](https://www.seeedstudio.com/blog/2020/10/27/all-about-cpus-microprocessor-microcontroller-and-single-board-computer/)
[https://www.raspberrypi.com/news/introducing-raspberry-pi-hats/](https://www.raspberrypi.com/news/introducing-raspberry-pi-hats/)
