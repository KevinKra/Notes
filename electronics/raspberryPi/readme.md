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
1. explain strong/static-typing.
1. what is weak/loose-typing?
1. is Typescript strongly typed?
1. what is a shim, what is a polyfill?
1. what is a pure function?
1. what is function composition?
1. what are side-effects?
1. what is an idempotent function?
1. what is an non-idempotent function?
1. what is an operand?
1. describe scalar and non-scalar types.
1. what is a generator function?
1. what is an example of a generator behavior?
1. describe the contrast between a normal function and a generator function.
1. what is the difference between `yielding` and `returning` from a generator function?
1. describe the difference between a `class` and `instance attribute`.
1. describe a `static method` vs an `instance method`.
1. what does the `constructor()` do?
1. what does `super()` do?
1. describe a `getter`/`setter`, what is their purpose?

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
1. at its simplest: an expression evaluates to a value. A statement does something (returns a value).
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
1. a non-idempotent function changes it's return value each time its called. `Math.random()` is a non-idempotent function.
1. operand is the quantity on which an operation is to be done. `5(operand) +(operator) 7(operand)`
1. Scalars are typically contrasted with _compounds_, such as arrays, maps, sets, structs, etc. A scalar is a "single" value - integer, boolean, perhaps a string - while a compound is made up of multiple scalars (and possibly references to other compounds). "Scalar" is used in contexts where the relevant distinction is between single/simple/atomic values and compound values.
1. A generator is a function that can stop midway and then continue from where it stopped. In short, a generator appears to be a function but it behaves like an iterator.
1. _"Imagine you are reading a nail-biting techno-thriller. All engrossed in the pages of the book, you barely hear your doorbell ring. It’s the pizza delivery guy. You get up to open the door. However, before doing that, you set a bookmark at the last page you read. You mentally save the events of the plot. Then, you go and get your pizza. Once you return back to your room, you begin the book from the page that you set the bookmark on. You don’t begin it from the first page again. In a sense, you acted as a generator function."_
1. A normal function such as this one cannot be stopped before it finishes its task i.e its last line is executed. It follows something called **run-to-completion model**. In contrast, a generator is a function that can stop midway and then continue from where it stopped. In JavaScript, a generator is a function which returns an object on which you can call `next()`. Every invocation of `next()` will return an object of shape — `{ value: Any, done: true|false }`
1. yielding _does not_ necessarily end the iteration process through the generator. Future `.next()` calls will continue to progress through the function. To `return` from a generator ends the iteration process and all subsequent code (yields for instance) are unreachable.
1. Class attributes are variables of a class that are shared between all of its instances. They differ from instance attributes in that instance attributes are owned by one specific instance of the class only, and ​are not shared between instances.
1. A static method (or static function) is a method defined as a member of an object but is accessible directly from an API object's constructor, rather than from an object instance created via the constructor. `MyClass.hasAStaticMethod()` vs `instanceOfMyClass.instanceMethod()`
1. A `constructor` creates an object of the class that it is in by initializing all the instance variables and creating a place in memory to hold the object.
1. The `super` keyword is used to call the constructor of its parent class to access the parent's properties and methods.
1. `getters` and `setters` are **used to protect your data**, particularly when creating classes. For each instance variable, a getter method returns its value while a setter method sets or updates its value. Given this, getters and setters are also known as **accessors and mutators**, respectively.

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

#### Electronics Questions

1. what is an `anode`?
1. what is a `cathode`?
1. what is a `main circuit` and a `control circuit`?
1. what is a `switch`?
1. what is a `semiconductor`?
1. what is "doping" in relation to `semiconductors`?
1. describe the difference between a `p-type semiconductor` and an `n-type semiconductor`
1. what is a `transistor`?
1. what is the place where the two semiconductor materials (p and n type) come into contact called?
1. describe the atomic characteristics that make an `insulator` different from a `conductor`.
1. how does a `semiconductor` behave in comparison to `insulators` and `conductors`?
1. describe `low resistance` and `high resistance` in electrical circuits.
1. what is an `ohm`?
1. how does resistance occur? what does it generate?
1. what does a `resistor` do?
1. what is "voltage drop"?
1. what calculation can you use to determine `ohms`? -- `voltage / amps = ohms`
1. what calculation can you use to determine `amps`? -- `voltage / ohms = amps`
1. what is a use case for an `infrared emitter` and `receiver`?
1. what is a `capacitor`?
1. what is a `capacitor` like, how is it different?
1. what charges and discharges faster, a `capacitor` or a `battery`?
1. what utility does a `capacitor` provide to an electrical circuit?
1. what is the physical structure of a `capacitor`?
1. what is a `dielectric` material?
1. what is a `electrode` material?
1. on a `capacitor`, what indicates the negative terminal?
1. what is `potential`?
1. how can `voltage` be described as pressure (and measured with a multimeter)?
1. what needs to happen for the `capacitor` to stop supplying charge?
1. what is `capacitance` and how is it measured?
1. the voltage (V) value on the capacitor describes what?
1. what happens to a `capacitor` if you exceed its maximum voltage?
1. why do we use `capacitors`?
1. what is `induction`?
1. should you be careful when working with `capacitors`?
1. what is a `volt`?
1. one `amp` equals how many `coulombs`?
1. what is a `conduction band`?
1. how does a `semiconductor's` electrons interact with conduction bands?
1. what are the two main types of `transistors`?
1. what does the `bipolar transistor` (BJT) acronym break out to?
1. what are the two functions `transistors` perform?
1. what are the protective case differences between small low power `transistors` and their higher power versions?
1. higher power `transistors` are usually connected to what in order to dissipate heat?
1. `transistors` have 3 pins, what are their letters and names?
1. why do we use `transistors`?
1. what is `current gain`?
1. how can you use the `current gain` measurement to calculate the `Base` and `Collector` currents?
1. what are the two (main) types of bi-polar transistors?
1. describe the `NPN` transistor.
1. describe the `PNP` transistor.
1. what is `conventional current`?
1. is `conventional current` correct? What is the correct flow and name?
1. designs and diagrams always use what current flow?
1. the `NPN` and `PNP` transistor types refer back to the semiconductor materials doped to create them. Which pins fit into which semiconductor materials in the two designs.
1. what is `covalent bonding`?
1. explain `p-type` and `n-type` doping.
1. what `p-n junction`, where the `p` and `n` type materials meet is also called what? What happens there automatically?
1. what happens when we connect a voltage source (like a battery) to the semiconductor? What special factors need to be considered regarding its voltage to create `forward bias`?
1. explain `reverse bias` and how it's created.
1. explain how the `NPN` transistor works at the semiconductor level.

#### Electronics Answers

1. an `anode`, positive charge, is the `electrode` where electricity moves into.
1. The `cathode`, negative charge, is the `electrode` where electricity is given out or flows out.
1. A `control circuit` is a special type of circuit used to control the operation of a completely separate power circuit. A `main circuit` may have much higher voltage or current than what is appropriate for a `control circuit`, so they're isolated.
1. In electrical engineering, a `switch` is an electrical component that can connect, or disconnect, the conducting path in an electrical circuit
1. A `semiconductor` is a material that has specific electrical properties that enable it to serve as a foundation for computers and other electronic devices. A `semiconductor` is a material product **usually comprised of silicon**, which conducts electricity _more_ than an insulator, such as glass, but _less_ than a pure conductor, such as copper or aluminum.
1. A `semiconductor's` conductivity and other properties can be altered with the introduction of impurities, **called doping**, to meet the specific needs of the electronic component in which it resides.
1. A `transistor` is a type of a semiconductor device that can be used to both conduct and insulate electric current or voltage. A `transistor` basically acts as a switch and an amplifier. **In simple words, we can say that a `transistor` is a miniature device that is used to control or regulate the flow of electronic signals.**
1. the space where a `p` and an `n` type semiconductor materials touch is the `p-n junction`?
1. in a `p-type semiconductor`, the semiconductor material is "doped" so that there is space for electrons to move. When surrounded by `n-type semiconductor` material (which is "doped" to have too many electrons), photons from emitted light hit the semiconductor material and electrons move across the n-type to p-type to n-type which creates a flow of electrons. The majority charge carries in an `n-type semiconductor` are electrons (which are negative), thus moves from lower potential to higher potential.
1. `insulators` consist of atoms with a stable outer atomic (valence) shell. By contrast, a `conductor's` protons (nucleus) have a loose, or weak bond, with the electrons in their outer valence shell. As consequence of this weak bond, electrons can translate across a circuit's conductive material from one atom to the next.
1. a `semiconductor` has properties of both a `conductor` and `insulator`. It typically serves as an insulator **until some outside force alters the behaviors of it's electrons** and it performs like an `conductor`.
1. Typically, `low resistance` in the electrical testing industry is referring to any resistance values below 1 ohm. **The lower the resistance, the higher the current flow.**
1. an `ohm` is a unit of electric resistance. The width of a wire, it's length, even it's material, helps determine it's resistance measured in `ohms`.
1. resistance occurs as electrons travel down a circuit and collide with atoms in a given material. This frequency of collisions varies across materials and materials that have more collisions (like iron), generate more heat (and light when it reaches a certain temperature) than materials with less collisions (like copper).
1. a `resistor` is a passive two-terminal electrical component that implements electrical resistance as a circuit element. In electronic circuits, resistors are used to reduce current flow, adjust signal levels, to divide voltages, bias active elements, and terminate transmission lines, among other uses.
1. voltage drop is the reduction in voltage as components along a closed circuit use it.
1. to calculate `ohms`: `voltage / amps = ohms`
1. to calculate `amps`: `voltage / ohms = amps`
1. using an `infrared emitter` and `receiver` can help simplify testing and development since you don't need to protect against visible light.
1. a `capacitor` stores electric charge.
1. a `capacitor` is like a `battery`, however it stores electric charge in an **electric field** as opposed to in a **chemical solution**.
1. a `capacitor` charges and discharges faster than a `battery`.
1. `capacitors` help to smooth out disruptions of electrical supply. In an abstract sense, it serves like a water tank, if the outside supply of water is interrupted the `capacitor` can continue to supply electrical current for a duration until the outside source returns.
1. a `capacitor` is wrapped in a insulating container, which contains two conductive metal plates --typically of aluminum. The layers (from outside in) consist of a protective case, a `dielectric` material (often ceramic), and an `electrode`.
1. a `dielectric` material is a poor conductor of electricity (electrons cant pass through) but an efficient supporter of electrostatic fields. It can store electrical charges, have a high specific resistance, and a negative temperature coefficient of resistance. _"It will polarize when it comes in contact with an electric field."_
1. `Electrodes` and `electrode materials` are metals and other substances used as the makeup of electrical components. They are used to make contact with a nonmetallic part of a circuit, and are the materials in a system through which an electrical current is transferred.
1. a stripe on a `capacitor` indicates the negative terminal.
1. electrical `potential` is the difference in charge between the positive and negative side of a component.
1. a `multimeter` can be used to detect voltage. Using pressure as an analogy, one end of a battery has more pressure then the other end, like water in a barrel.
1. when the positive and negative charges on the electrodes are equal, the flow of electrons motivated by the `capacitor` stop moving (no more current).
1. `capacitance` is the measurement of energy stored in a `capacitor` and they're measured in `Farads (F)` though usually in `microfarads (uF)`.
1. the voltage value on a `capacitor` describes **the maximum number of volts** a `capacitor` can handle.
1. it explodes.
1. one of the most common applications in large buildings is to use a `capacitor bank` to provide "power factor correction". They're also used to smooth out peaks between ac-to-dc conversion.
1. when enough current passes through a coil it creates `induction` which is the process of creating a magnetic field.
1. yes, `capacitors` hold large voltages even for a long time after being disconnected from a circuit.
1. a `volt = joule / coulomb`, where a `joule` is a measurement of work and a `coulomb` is a group of flowing electrons.
1. 1 amp = 1 coulomb.
1. the `conduction band` is a band near the outer `valence shell` of an atom. If an electron is able to reach the `conduction band` it has the potential the break away from the atom it is bound to. In practical terms, a conductive material like copper has a `conductive band` that overlaps with it's `valence shell`, as a consequence the other electron can detach. Insulators, by contrast, have a packed outer shell (no room for outside electrons to join), the nucleus has a tight grip on the electrons, and the `conduction band` does not overlap with the `valence shell`.
1. a `semiconductor` has **one too many electrons** in its outer shell, so it acts like an insulator. However, it's `conduction band` is close to the outer shell, so if we provide an outside force, like light (it's photons), we can knock the extra electron free and generate electricity.
1. the two main types of `transistors` are `bipolar` and `field effect` transistors.
1. Bipolar Junction Transistor.
1. `transistors` switch circuits on and off **and** amplify signals.
1. both the lower and higher power transistors have a resin case to help protect the internal parts of the `transistor`. However, the higher power `transistors` will have a partial metal case to help dissipate heat.
1. higher power `transistors` are often attached to `heat sinks` to help dissipate their heat.
1. the three pins on a `transistor` are **E (emitter) B (base) C (collector)**.
1. we use `transistors` to help control (at an interval for example) the switch behavior of a circuit. An outside circuit can connect to the **B (base)** pin of the transistor and when a current is sent the transistor allows its primary current to travel across it.
1. current gain is `beta (symbol) = collector current / base current`
1. to find the `Base` current: `Collector current / gain`. To find the `Collector` current: `gain * Base current`
1. `NPN` and `PNP`
1. current **combines** in `NPN` transistors. `Collector (C)` is connected to the `cathode (-)` of the battery with conventional current flowing through it. The `Collector (C)` and `Base (B)` pins and outputs (a combined current) in the `Emitter (E)` pin.
1. current **divides** in `PNP` transistors. `Emitter (E)` is connected to the `anode (+)` of the battery with conventional current flowing through it. Current flow in through the `E` pin, and divides among the `C` and `B` pins.
1. `conventional current` is the flow of electrical current from + positive to - negative.
1. it was assumed that `conventional current` was correct, or electrons flow from `+` to `-`. However, it was proven that electrons _actually_ flow from `-` to `+`. This is called `electron flow`.
1. designs and diagrams always show `conventional current`, however engineers really know that `electron flow` (the opposite) current is what's truly happening.
1. in `NPN` transistors, (n and p referring to the semiconductor type), the B pin is connected to the `P-type semiconductor`.
1. `covalent bonding` is when two atoms (silicon for instance) want to satisfy their outer shells (valence) electron count. To do this, they pair --or share, a bond with the silicon next to it which also potentially also has missing electrons. Together, through sharing, the reach their desired electron count.
1. doping in electronics typically occurs with silicon, so we will use that example. Silicon wants to satisfy it's outer valence shell's electron count. It can do this through `covalent bonding`. We can dope the **(n-type)** semiconductor with phosphorus atoms, which has enough outer electrons satisfy the four surrounding silicon atoms' valence electron needs, and also have the extra free flowing electron freely move around the material. Conversely, with **(p-type)** doping, we add a material like aluminum that has one too few electrons in its outer shell. This means that for the four surrounding silicon atoms, one won't be able to satisfy its valence shell electron count and now there is a "hole" where an electron should be.
1. the "Depletion region". In this region, a few of the electrons, and holes, swap spaces. Due to their charge differences (electrons being negatively charged and the holes being considered positively charged) they create a respective slightly negatively and positively charged region. This process creates a small electrical field which **prevents** more electrons from passing across. The `potential difference` in this region is typically around 0.7V.
1. when we add a voltage source, like a battery **(+ to p-type and - to n-type)**, to a semiconductor material, we are able to create a `forward bias`. So long as the added voltage sourced from the battery is **greater than** the `potential difference` of the `depletion region` (say 0.7V) the electrons will be able to move through the region and we will have a current. If the voltage source is too low, the electrons will lack the sufficient power to pass through the barrier of the depletion region.
1. `reverse bias` is when the source (a battery for instance) is connected to the semiconductor in a **(- to p-type and + to n-type)** configuration. The `reverse bias` results in the **electrons being pulled back to the positive terminal** and the **holes being pulled back to the negative terminal.**
1. You have **two junctions** (both sides of the N where it meets the P material) in which the `potential difference` between the regions creates a field, or `depletion region`.

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
