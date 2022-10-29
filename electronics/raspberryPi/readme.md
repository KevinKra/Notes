1. what is an actuator?
1. what operating system is on the RaspberryPi?
1. what is the latest single board computer (`SBC`) from RaspberryPi?
1. what is a `microcontroller`, what is the name of RPi's micro controller?

1.
1. RaspberryPi runs on the Linux operating system.
1. RaspberryPi 4 is the latest single board computer.

---

## General Questions

### general questions

1. what is a compiled language, what is the (general) flow, and what are examples?
1. what is an interpreted language, what is the (general) flow, and what are examples?
1. whats the key difference in error handling between compiled languages and interpreted languages?
1. what is a benefit of compiled languages?
1. what is a disadvantage of compiled languages?
1. what is a benefit of interpreted languages?
1. what is a disadvantage of interpreted languages?
1. how does the python interpreter work?
1. explain the initial python compilation of your python source code.
1. once your python code is compiled and translated into python byte code, what happens?
1. what is a virtual machine?
1. is javascript compiled?
1. regarding javascript, what converts (compiles) your source code into machine readable language?
1. what is the simplest description for compiling?
1. what is the simplest description for interpreting?
1. is typescript compiled, how is it different from javascript
1. what is transpiling?
1. what is function overloading in Typescript?
1. what are OOP's 4 pillars?
1. describe OOP polymorphism.
1. what is the difference between polymorphism and inheritance?
1. describe OOP abstraction.
1. describe OOP encapsulation.
1. what is the difference between an expression and a statement?
1. what is the difference between dynamically and statically type languages?
1. do dynamic languages have to have an interpreter?
1. what is duck typing, examples?
1. explain strong-typing vs static-typing.
1. what is weak/loose typing?
1. is Typescript strongly typed or statically typed?
1. what is a shim, what is a polyfill?
1. what is a pure function?
1. what is function composition?
1. what are side effects?
1. what is an idempotent function?
1. what is an operand?
1. describe scalar and non-scalar types.
1. what is a generator function?
1. what is an example of a generator behavior?
1. describe the contrast between a normal function and a generator function.
1. what is the difference between `yielding` and `returning` from a generator function?
1. what is a system language?
1. what is an application language?
1. what are class attributes?
1. what are instance attributes?
1. what is postponed evaluation (python)?
1. describe a class method.
1. describe a static method.
1. what does the constructor do?
1. what does `super()` do?
1. describe a getter/setter.

### general answers

1. A compiled language is a programming language where the program, once compiled, is expressed in the **instructions of the target machine**; this machine code is undecipherable by humans. A compiler is computer software that transforms computer code written in one programming language (the source language, like JavaScript, Python, etc.) into another programming language (the target language, like machine code, python byte code, etc). Types of compiled language – `C`, `C++`, `C#`, `CLEO`, `COBOL`, etc. `language > compiled > machine code > read to run`.
1. An interpreted language is one where the **instructions are not directly executed by the target machine**, but instead, read and executed by some other program. An interpreter is a computer program that directly executes instructions written in a programming or scripting language without requiring them previously to have been compiled into a machine language program. It translates one statement at a time. Interpreted language ranges – `JavaScript`, `Perl`, `Python`, `BASIC`, etc. `language > ready to run > interpreted by virtual machine > machine code`
1. with compiled languages, compilation errors prevent the code from compiling. Interpreted languages all the debugging occurs at run-time.
1. An advantage for compiled programs, that are compiled into native machine code, is that they tend to be faster than interpreted code. This is because the process of translating code at run time (via the interpreter) adds to the overhead, and can cause the program to be slower overall.
1. Disadvantages of compiled languages include additional time needed to complete the entire compilation step before testing and platform dependence of the generated binary code.
1. Interpreted languages tend to be more flexible, and often offer features like dynamic typing and smaller program size. Also, because interpreters execute the source program code themselves, the code itself is platform independent.
1. The most notable disadvantage for interpreted languages is typical execution speed compared to compiled languages.
1. **An interpreter is a kind of program that executes other programs.** When you write Python programs, it converts source code written by the developer into **intermediate language** (bytecode potentially) which is again translated into the native language/machine language that is executed.
1. The python code you write is compiled into python bytecode, which creates file with extension `.pyc`. **`.pyc` files are bytecode.** Python source code was parsed, optimized and compiled to create them. The bytecode compilation happens internally, and almost completely hidden from developer. **Compilation is simply a translation step**, and bytecode is a lower-level, platform-independent, representation of your source code. Generally, each of your source code statements are translated into a group of bytecode instructions. This bytecode translation is performed to speed up execution since bytecode can be run much quicker than the original source code statements.
1. The `.pyc` file , **created in the initial compilation step**, is then executed by an appropriate virtual machine. **The Virtual Machine is simply a loop that iterates through your bytecode instructions, one by one, to carry out their operations.** The Virtual Machine is the **runtime engine** of Python (like Spidermonkey or V8 in browsers --javascript) and it is always present as part of the Python system, and is the component that truly runs the Python scripts. Technically, it’s just the last step of what is called the Python interpreter.
1. The virtual machine is the runtime engine of Python that runs the scripts after they've been compiled from python source code into `.pyc` byte code. The Virtual Machine just a big loop that iterates through your byte code instructions, one by one, to carry out their operations.
1. **Javascript is compiled**. The browser (runtime) engine serves as the interpreter that takes the source code you provided, **tokenizes it**, **parses it (into an Abstract Syntax Tree)**, and then performs **code generation** with the AST used as the input. Following these operations, an executable byte-code is generated that is understood by the environment (or platform) where the executable code will be running.
1. The browser engine, V8 (Chrome) or Spidermonkey (Firefox), takes your javascript source code and compiles it into machine readable byte code. Then the JS virtual machine/engine serves it to the machine. To conclude, JavaScript code gets compiled. **It is compiled every time.** Next time, if someone asks the question, Does JavaScript really compile? The answer is a loud YES. **After the compilation process produces a binary byte code, the JS virtual machine executes it.**
1. A compiler is a _"program that translates computer code written in one programming language (the source language) into another language (the target language)"_. Note that there is no constraint on the output being any specific language, e.g. assembly or machine code.
1. An interpreter is a _"program that directly executes instructions written in a programming or scripting language, without requiring them previously to have been compiled"_
1. TypeScript is a "compiled language". Right now in 2022, at least, that's how almost everyone in the world is using it. **The compiler takes TypeScript code, removes the type information, and generates JavaScript code.** The fact that the target of this compilation is itself a "source language" (javascript) makes this a special case of compilation: **source-to-source compilation.** This is often called 'transpilation' because it is translating from one source language to another. Make no mistake, transpilation is a form of compilation, as per our earlier definition.
1. Transpiling is translating (or compiling) one **source language** to another. Typescript is compiled/transpiled into javascript.
1. TypeScript provides the concept of function overloading. **You can have multiple functions with the same name but different parameter types and return type.** However, (specifically regarding TS) the number of parameters must be the same.
1. OOP 4 pillars are: **Inheritance, Encapsulation, Abstraction, Polymorphism,** or _APIE_.
1. Polymorphism, similar to inheritance, is the overriding of methods derived from a parent (or super) class. Parent `Vehicle.getWheels()` returns 4, child `Truck.getWheels()` returns 18.
1. In inheritance, we create new classes that inherit features of the superclass while polymorphism decides what form of method to execute. Inheritance applies to classes, whereas polymorphism applies to methods. Polymorphism deals with how the program decides which methods it should use, depending on what type of thing it has. If you have a `Person`, which has a `read` method, and you have a `Student` which extends `Person`, which has its own implementation of `read`, which method gets called is determined for you by the runtime, depending if you have a `Person` or a `Student`. Thats the polymorphism in action.
1. Through the process of abstraction, a programmer hides all but the relevant data about an object in order to reduce complexity and increase efficiency.
1. encapsulation refers to the bundling of data with the methods that operate on that data, or the restricting of direct access to some of an object's components. Encapsulation is used to hide the values or state of a structured data object inside a class, preventing direct access to them by clients in a way that could expose hidden implementation details or violate state invariance maintained by the methods.
1. at its simplest: an expression evaluates to a value. A statement does something.
1. Dynamically-typed languages perform type checking at runtime, while statically typed languages perform type checking at compile time. **This means that scripts written in dynamically-typed languages can compile even if they contain errors that will prevent the script from running properly (if at all).** If a script written in a statically-typed language (such as Java, Golang) contains errors, **it will fail to compile until the errors have been fixed.**
1. No, dynamic languages _do not_ have to have an interpreter. The relationship between having an interpreter, or just outright compiling source code, is independent from from the typing characteristics of the source code itself. Traditionally, and most commonly through convention, strongly typed languages (Java, Golang, C++) don't have an interpreter because they decided to provide strong/static typing instructions to the compiler for optimization. Dynamically, or weakly, typed languages like Python or Javascript, simply hand off their source code to an interpreter which than _interprets_ the code and converts it into machine readable code. "_If you write Python on a whiteboard, it's still called Python! It's the implementation that can be an interpreter or a compiler. Being statically-typed or dynamically-typed (of kind of a hybrid of both) is a property of the language, while executing a program by interpreting or compilation is a property of the implementation."_
1. Duck typing is a concept related to **dynamic typing**, where the type or the class of an object is less important than the methods it defines. **When you use duck typing, you do not check types at all. Instead, you check for the presence of a given method or attribute.** `duck` and `cat` are both objects. Both of them respond to the method `type()` (which you defined). Therefore as far as JavaScript is concerned both objects are of the same type. Dynamic languages, like `Ruby`, `Python`, `Javascript`, are all duck-typed languages, in which we don't check the types.
1. **Strongly typed** means that there are restrictions between conversions between types. **Statically typed** means that the types are not dynamic - you can not change the type of a variable once it has been created.
1. A programming language is loosely typed, or weakly typed, when it does not require the explicit specification of different types of objects and variables.
1. Typescript is **strongly aka statically typed**.
1. A shim is a piece of code used to correct the behavior of code that already exists, usually by adding new API that works around the problem. This differs from a polyfill, which implements a new API that is not supported by the stock browser as shipped.
1. a **pure function** has two requirements: given the same input, always returns the same output _and_ it produces no side effects. `double(5)` is a pure function, however `Math.random()` is not.
1. **Function composition** is the process of combining two or more functions to produce a new function. Composing functions together is like snapping together a series of pipes for our data to flow through.
1. A pure function produces no side effects, which means that it can’t alter any external state.
1. Idempotence is the property of certain operations in mathematics and computer science whereby they can be applied multiple times without changing the result beyond the initial application. `.toString()` is an example of an idempotent function, calling it multiple times does not change the output.
1. operand is the quantity on which an operation is to be done. `5(operand) +(operator) 7(operand)`
1. Scalars are typically contrasted with _compounds_, such as arrays, maps, sets, structs, etc. A scalar is a "single" value - integer, boolean, perhaps a string - while a compound is made up of multiple scalars (and possibly references to other compounds). "Scalar" is used in contexts where the relevant distinction is between single/simple/atomic values and compound values.
1. A generator is a function that can stop midway and then continue from where it stopped. In short, a generator appears to be a function but it behaves like an iterator.
1. _"Imagine you are reading a nail-biting techno-thriller. All engrossed in the pages of the book, you barely hear your doorbell ring. It’s the pizza delivery guy. You get up to open the door. However, before doing that, you set a bookmark at the last page you read. You mentally save the events of the plot. Then, you go and get your pizza. Once you return back to your room, you begin the book from the page that you set the bookmark on. You don’t begin it from the first page again. In a sense, you acted as a generator function."_
1. A normal function such as this one cannot be stopped before it finishes its task i.e its last line is executed. It follows something called **run-to-completion model**. In contrast, a generator is a function that can stop midway and then continue from where it stopped. In JavaScript, a generator is a function which returns an object on which you can call `next()`. Every invocation of `next()` will return an object of shape — `{ value: Any, done: true|false }`
1. yielding _does not_ necessarily end the iteration process through the generator. Future `.next()` calls will continue to progress through the function. To `return` from a generator ends the iteration process and all subsequent code (yields for instance) are unreachable.

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
1. `Bus:` Describes the set of conductors that transmit data or that address or control information to the microprocessor’s different elements. (MPU usually has 3 types: data bus, the address bus, and control bus.)
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

---

#### SBC Questions

1. what is an `SBC`?
1. what is a CPU?
1. how does SBC work?
1. explain `CPU/SoC`
1. explain `Clock/Timer`
1. explain `I/O port`
1. explain `Serial Ports`
1. explain `ADC/DAC`
1. explain `Wireless Connectivity`

#### SBC Answers

1. an `SBC` is a Single Board Computer. Single Board Computers are small computing devices that have all of the elements of a complete computer contained within one single circuit board. It is a low cost, self-contained, and simple computer, that could be easily connected to other hardware.
1. CPU is the core component in a computing unit, which is responsible for processing and executing instructions. It runs the operating system and applications, constantly receiving input from the user or active software programs.
1. SBCs uses a **System-on-Chip (SoC)**, which integrates all or most components of a computer or other electronic system. It works exactly like how an MPU does, but provides even more features, such as signal processing, wireless communication, artificial intelligence etc., on top of transmitting data.
1. `CPU/SoC:` Brain of the SBC, responsible for fetching the instruction, decodes it, then execute it.
1. `Memory:` Similar to how it functions in `MPU` and `MCU`, used to store data and program. However, unlike the previous 2, there are RAM and ROM or flash memories for storing program source codes.
1. `Clock/Timer:` Provides all timing and counting functions inside the microcontroller. It is used to perform clock functions, modulations etc.
1. `I/O port:` It allows you to drive/interface various devices such as LCD’S, LED’S, printers, etc to a microcontroller.
1. `Serial Ports:` Provides various serial interfaces between a microcontroller and other peripherals like parallel ports.
1. `ADC/DAC:` Analog to Digital Converter (ADC), which converts the analog signal into the digital signal. Digital to Analog Converter (DAC) and it converts the Digital signal into an analog signal.
1. `Wireless Connectivity:` Consists of Bluetooth and WiFi.

## Resources:

[https://www.seeedstudio.com/blog/2020/10/27/all-about-cpus-microprocessor-microcontroller-and-single-board-computer/](https://www.seeedstudio.com/blog/2020/10/27/all-about-cpus-microprocessor-microcontroller-and-single-board-computer/)
[https://www.raspberrypi.com/news/introducing-raspberry-pi-hats/](https://www.raspberrypi.com/news/introducing-raspberry-pi-hats/)

## General Resources:

[https://blog.greenroots.info/javascript-interpreted-or-compiled-the-debate-is-over](https://blog.greenroots.info/javascript-interpreted-or-compiled-the-debate-is-over)
[https://codeburst.io/understanding-generators-in-es6-javascript-with-examples-6728834016d5](https://codeburst.io/understanding-generators-in-es6-javascript-with-examples-6728834016d5)
