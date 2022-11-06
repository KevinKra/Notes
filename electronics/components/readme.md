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

## Electronics Questions

1. what is a `main circuit` and a `control circuit`?
1. what is a `switch`?
1. describe the atomic characteristics that make an `insulator` different from a `conductor`.
1. how does a `semiconductor` behave in comparison to `insulators` and `conductors`?
1. describe `low resistance` and `high resistance` in electrical circuits.
1. what is an `ohm`?
1. how does resistance occur? what does it generate?
1. what does a `resistor` do?
1. what is "voltage drop"?
1. what calculation can you use to determine `ohms`?
1. what calculation can you use to determine `amps`?
1. what is a use case for an `infrared emitter` and `receiver`?
1. what is a `volt`?
1. one `amp` equals how many `coulombs`?

## Electronics Answers

1. A `control circuit` is a special type of circuit used to control the operation of a completely separate power circuit. A `main circuit` may have much higher voltage or current than what is appropriate for a `control circuit`, so they're isolated.
1. In electrical engineering, a `switch` is an electrical component that can connect, or disconnect, the conducting path in an electrical circuit
1. `insulators` consist of atoms with a stable outer atomic (valence) shell. By contrast, a `conductor's` protons have a loose, or weak bond, with the electrons in their outer valence shell. As consequence of this weak bond, electrons can translate across a conductive path and move from conductive atom to conductive atom.
1. a `semiconductor` has properties of both a `conductor` and `insulator`. It typically serves as an insulator **until some outside force alters the behaviors of it's electrons** and it performs like an `conductor`.
1. Typically, `low resistance` in the electrical testing industry is referring to any resistance values below 1 ohm. **The lower the resistance, the higher the current flow.**
1. an `ohm` is a unit of electric resistance. The width of a wire, it's length, even it's material, helps determine it's resistance measured in `ohms`.
1. resistance occurs as electrons travel down a conductive path and collide with atoms in a given material. This frequency of collisions varies across materials, and materials that have more collisions (like iron), generate more heat (and light when it reaches a certain temperature) than materials with less collisions (like copper).
1. a `resistor` is a passive two-terminal electrical component that implements electrical resistance as a circuit element. In electronic circuits, resistors are used to reduce current flow, adjust signal levels, to divide voltages, bias active elements, and terminate transmission lines, among other uses.
1. voltage drop is the reduction in voltage as components along a closed circuit use it.
1. to calculate `ohms`: `voltage / amps = ohms`
1. to calculate `amps`: `voltage / ohms = amps`
1. using an `infrared emitter` and `receiver` can help simplify testing and development since you don't need to protect against visible light.
1. a `volt = joule / coulomb`, where a `joule` is a measurement of work and a `coulomb` is a group of flowing electrons.
1. 1 amp = 1 coulomb.

---

### Solenoid Valve Questions

1. what do `solenoid valves` do?
1. a `solenoid valve` is comprised of two parts, what are they?
1. how does the `solenoid` work?
1. what dictates the shape variations in a solenoid valve?
1. how does a "direct operated valve" work?
1. what is an `NO valve` and a `NC valve`?
1. how does the electromagnetic field work with a `NC valve`?
1. how does the electromagnetic field work with a `NO valve`?

### Solenoid Valve Answers

1. `solenoid valves` are used to convert electrical energy into mechanical energy.
1. a `solenoid valve` consists of a `solenoid` and a `valve`.
1. the `solenoid` allows an electric current to pass through and move through a `solenoid coil` which generates an **electromagnetic field**.
1. the shape variations of a `solenoid valve` depend on the capacity of the valve, the pressure its working on, and the different internal mechanisms.
1. a "direct operated valve has a `solenoid coil` in the solenoid and either a `NO valve` or a `NC valve`. The coil completely surrounds a pillar called an `armature` which is purposely located at the center of the electromagnetic field. Inside the `armature` is a `plunger` and `spring`.
1. `NO valve` is a normally open valve, whereas a `NC valve` is a normally closed valve.
1. the electromagnetic field, generated via induction by the coil, passes into the `plunger` and pulls it upwards against the `spring`, thus opening the valve. The `plunger` is placed in the center of the `armature` because the EF is strongest there.
1. the electromagnetic field, generated via induction by the coil, passes into the `spring` (which normally holds the plunger up) and pushes the `plunger` down.

---

### Diode Questions

1. what does a `diode` look like?
1. what is another example of `diode` besides a standard `diode`.
1. what does a `diode` do?
1. how can a `diode` be used?
1. why do we use `copper` in wires?
1. how does a `diode` work?
1. do `diodes` have a range for how much current and voltage they can support while acting as a conductor or insulator?

### Diode Answers

1. a `diode` typically has a black cylinder body with a stripe at one end with two leads --anode and cathodes terminals (cathode on the same side as the stripe).
1. an `LED` or Light Emitting Diode, is an example.
1. a `diode` allows current to only flow in one direction within a circuit.
1. a `diode` can be used as both an insulator and a conductor along a circuit depending on it's orientation. With the `cathode` facing the same direction as the electron flow, it creates a forward bias; if reversed with the `anode` facing against the incoming current (- -) it repels it and creates a reverse bias. These biases either pull or push incoming electrons away.
1. `copper` has a lot of free electrons that makes it very easy for electrons to move through it.
1. inside a `diode` it has `n-type` and `p-type` semiconductor materials. The `cathode` terminal connects with the `n-type` semiconductor and as electrons from the circuit flow in, they are able to overcome the potential difference generated within the `p-n junction`'s **"depletion region"** (specifically the electric field generated by the swapped holes and electrons) and push the current through the `diode` and out the `anode`. This pushing force requires sufficient voltage (typically 0.7V) and is an example of forward bias.
1. yes, each `diode` has a varying range with how much current and voltage they can support before the `diode` is overwhelmed, breaks, and potentially destroys your circuit as well. So make sure you stay within acceptable bounds.

---

### Capacitor Questions

1. what is a `capacitor`?
1. what is a `capacitor` like, how is it different?
1. what is an `anode`?
1. what is a `cathode`?
1. what is an `electrode`?
1. what is a `dielectric` material?
1. what charges and discharges faster, a `capacitor` or a `battery`?
1. what utility does a `capacitor` provide to an electrical circuit?
1. what is the physical structure of a `capacitor`?
1. on a `capacitor`, what indicates the negative terminal?
1. when we pass electrons into the negative terminal of a `capacitor` what happens?
1. at what point will electrons stop flowing from the `battery` to the `capacitor`?
1. what is `potential`?
1. describe `electric field attraction` and how its used in capacitors.
1. what happens with the `capacitor` when you reconnect the `battery` to the circuit?
1. what is a `multimeter` and how can `voltage` be described as pressure?
1. what needs to happen for the `capacitor` to stop supplying charge?
1. what is `capacitance` and how is it measured?
1. the **voltage (V)** value on the capacitor describes what?
1. what happens to a `capacitor` if you exceed its maximum voltage?
1. why do we use `capacitors`?
1. should you be careful when working with `capacitors`?

### Capacitor Answers

1. a `capacitor` stores electric charge.
1. a `capacitor` is like a `battery`, however it stores electric charge in an **electric field** as opposed to in a **chemical solution**.
1. an `anode`, negative charge, is the `electrode` where electrons flow out.
1. The `cathode`, positive charge, is the `electrode` where electricity is given in or flows in.
1. an `electrode` is a conductor through which electricity enters or leaves an object, substance, or region. `anode` and `cathode` are used to describe current flow in relation to the `electrode`.
1. a `dielectric` material is a material which serves as an insulator, or electrons cannot pass. There are solid, liquid, and gas `dielectric` materials. Ceramic, plastic, distilled water, nitrogen, vacuum, so on.
1. a `capacitor` charges and discharges faster than a `battery` due to them using an `electric field`, rather than a sequence of chemical reactions to hold and move electrons.
1. `capacitors` help to smooth out disruptions of electrical supply. In an abstract sense, it serves like a water tank, if the outside supply of water is interrupted the `capacitor` can continue to supply electrical current for a duration until the outside source returns.
1. a `capacitor` is wrapped in a insulating container, which contains two conductive metal plates --typically of aluminum, sandwiching a `dielectric` material (often ceramic). The layers (from outside in) consist of a protective case, an `electrode`, a dielectric layer, and another `electrode`.
1. a stripe on a `capacitor` indicates the negative terminal.
1. when we connect a `battery` to a `capacitor`, current moves from the `anode` of the battery to one plate on the capacitor. The electrons will build up on that plate while `electrons` on the other plate discharge from the `capacitor`. Electrons can't pass across the `capacitor` because of the insulating material.
1. once the `capacitor` has the **same voltage** as the `battery`, electrons will stop flowing. Now that one of the `capacitor's` plates is full of electrons (charge), we can release this stored energy later. This gives us a difference in **potential**, or a **voltage difference** between both sides (- electron filled plate and empty positive plate),
1. `electrical potential` is the difference in charge between the positive and negative sides of a component.
1. an `electric field` leverages the attraction of positively charged particles (or holes) and negatively charged particles (electrons). A dielectric material (in the capacitor for instance) keeps the attracted elements in the electrodes physically separated, however they still have the attraction pulling them together and thus a capacitor is able to hold charge until a suitable path is made that can connect the electrodes until the difference of potential equalizes and the voltage becomes zero.
1. when you reconnect the `battery` to the circuit, the `capacitor` will begin to recharge as electrons flow in and the push the electrons out that had accumulated on the other electrode while the battery was disconnected (negatives repel).
1. a `multimeter` can be used to detect voltage and other electrical characteristics like resistance and current. Using pressure as an analogy, one end of a battery has more pressure then the other end, like water in a pipe.
1. when the positive and negative charges on the electrodes are equal, the flow of electrons motivated by the `capacitor` stop moving (no more current).
1. `capacitance` is the measurement of energy stored in a `capacitor` and they're measured in `Farads (F)`, though usually in `microfarads (uF)`.
1. the voltage value on a `capacitor` describes **the maximum number of volts** a `capacitor` can handle.
1. it explodes.
1. one of the most common applications in large buildings is to use a `capacitor bank` to provide "power factor correction". They're also used to smooth out peaks between ac-to-dc conversion.
1. yes, `capacitors` hold large voltages even for a long time after being disconnected from a circuit.

---

### Inductor Questions

1. what is an `inductor`?
1. what is an `inductors` main utility?
1. regarding, water, what is an `inductor` like?
1. explain the relation, in terms of water, of the `inductor` and a resistive path.
1. what causes `inductors` to work?
1. what happens if we change the flow of current in a wire?
1. what happens if we add more current to a wire?
1. coiling an `inductor` does what?
1. what do `inductors` look like?
1. what can `inductors` be used for?
1. how is `inductance` measured?
1. what is `induction`?

### Inductor Answers

1. an `inductor` is a component in an electrical circuit that stores energy in its magnetic field. It can release this energy almost instantly.
1. `inductors` can release energy almost instantly.
1. an `inductor` is like a water wheel, where it takes time to move but gradually collects energy and captures it in it's spin.
1. using the water analogy and the `inductor` serving as a water wheel, as water flows through a split in the pipe it has two choices, the path with resistance, or the path with the `inductor`. At first, the `inductor` requires too much energy to spin (water wheel), so the water flows through the resistive path (think an LED). Eventually, the water is able to push the wheel and flow freely; once that happens the path with the wheel (inductor) provides less resistance than the resistive path and now the resistive path becomes secondary to the `induction` supporting path. If the water source is turned off, the `inductor` while continue to work (and move the current) for a short period of time due to the energy stored.
1. when we pass current (electrons) through a conductive path it generates a magnetic field (all those negatively charged electrons) and increase in size up to its maximum.
1. if we change the flow of current, the magnetic fields reverse.
1. if we add more current, the magnetic field gets larger.
1. by coiling a wire (`inductor`) we create a larger magnetic field around the wire.
1. in circuit boards, inductors look like copper wire wrapped around a ring. Sometimes they have casing to shield the magnetic current and prevent interacting with other components.
1. `inductors` can be used to boost converters to increase DC voltage output. They can be used to choke AC supply and allow only DC to pass. They can be used to filter and separate difference frequencies. Used in transformers, motors, and relays.
1. in units of **Henry (H)**. `1H = 1V of EMF cross the inductor with 1 Ampere of current.`
1. when enough current passes through a coil it creates `induction` which is the process of creating a magnetic field.

---

### Transistor Questions

1. what is a `semiconductor`?
1. what is **"doping"** in relation to `semiconductors`?
1. what is a `transistor`?
1. what is the place where the two semiconductor materials (p and n type) come into contact called?
1. what is a `conduction band`?
1. how does a `semiconductor's` electrons interact with conduction bands?
1. what are the two main types of `transistors`?
1. what does the `bipolar transistor` (BJT) acronym break out to?
1. what are the two functions `transistors` can provide?
1. what are the protective case differences between small low power `transistors` and their higher power versions?
1. higher power `transistors` are usually connected to what in order to dissipate heat?
1. `transistors` have 3 pins, what are their letters and names?
1. do **all** transistors use the EBC pin configuration
1. why do we use `transistors`?
1. what is `current gain`?
1. how can you use the `current gain` measurement to calculate the `Base` and `Collector` currents?
1. what are the two (main) types of **bi-polar transistors**?
1. describe the `NPN` transistor.
1. describe the `PNP` transistor.
1. what is `conventional current`?
1. is `conventional current` correct? What is the correct flow and name?
1. designs and diagrams always use what current flow?
1. the `NPN` and `PNP` transistor types refer back to the semiconductor materials doped to create them. Which pins fit into which semiconductor materials in the two designs.
1. what is `covalent bonding`?
1. explain `p-type` and `n-type` doping.
1. regarding the `p-n junction`, where the `p` and `n` type semiconductor materials meet is also called what? What happens there automatically?
1. what happens when we connect a voltage source (like a battery) to the semiconductor? What special factors need to be considered regarding its voltage to create `forward bias`?
1. explain `reverse bias` and how it's created.
1. explain how the `NPN` transistor works at the semiconductor level.

### Transistor Answers

1. A `semiconductor` is a material that has specific electrical properties that enable it to serve as a foundation for computers and other electronic devices. A `semiconductor` is a material product **usually comprised of silicon**, which conducts electricity _more_ than an insulator, such as glass, but _less_ than a pure conductor, such as copper or aluminum.
1. A `semiconductor's` conductivity and other properties can be altered with the introduction of impurities, **called doping**, to meet the specific needs of the electronic component in which it resides.
1. A `transistor` is a type of a semiconductor device that can be used to both conduct and insulate electric current or voltage. A `transistor` basically acts as a switch and/or an amplifier. **In simple words, we can say that a `transistor` is a miniature device that is used to control or regulate the flow of electronic signals.**
1. the space where a `p` and an `n` type semiconductor materials touch is the `p-n junction`.
1. the `conduction band` is a band near the outer `valence shell` of an atom. If an electron is able to reach the `conduction band` it has the potential the break away from the atom it is bound to. In practical terms, a conductive material like copper has a `conduction band` that overlaps with it's `valence shell`, and as a consequence, the outer electron _can_ detach. Insulators, by contrast, have packed outer shells (no room for outside electrons to join), the nucleus has a tight grip on the electrons, and the `conduction band` does not overlap with the `valence shell`.
1. a `semiconductor` has **one too many electrons** in its outer shell, so it acts like an insulator. However, it's `conduction band` is close to the outer shell, so if we provide an outside force, like photons from light, we can knock the extra electron free and generate electricity. Therefor, this material can act as _both_ an insulator and a conductor.
1. the two main types of `transistors` are `bipolar` and `field effect` transistors.
1. BJT = Bipolar Junction Transistor.
1. `transistors` can switch circuits on and off **and** amplify signals.
1. both the low and high power transistors have a resin case to help protect the internal parts of the `transistor`. However, the higher power `transistors` will have a partial metal case to help dissipate heat.
1. higher power `transistors` are often attached to `heat sinks` to help dissipate their heat.
1. the three pins on a `transistor` are **E (emitter) B (base) C (collector)**.
1. no, some transistors _can_ have different PIN configurations. Check the manufacturers datasheet to confirm.
1. we use `transistors` to help control (at an interval for example) the switch behavior of a circuit. An outside circuit can connect to the **B (base)** pin of the transistor and when a current is sent the transistor, it allows its primary current to travel across it.
1. `current gain` is `beta (symbol) = collector current / base current`. We can use `current gain` to measure the amount of gain (or amplification) between a circuit connected to the base pin and it's gain via the collector circuit.
1. to find the `Base` current: `Collector current / current gain`. To find the `Collector` current: `current gain * Base current`.
1. `NPN` and `PNP`.
1. current **combines** in `NPN` transistors. The `Collector (C)` pin is connected to the `cathode (+)` of the battery **creating a reverse bias**. The higher the voltage of the `Base (B)` pin, the more "open" the transistor becomes meaning more electrons and current moving into the p-type (B pin) layer and also more electrons are pulled across the reverse bias and out the `Collector`; we also see more electrons flowing in through the `emitter` then out the `collector`. The `Collector (C)` and `Base (B)` pins current combines and outputs via the `Emitter (E)` pin.
1. current **divides** in `PNP` transistors. `Emitter (E)` is connected to the `anode (-)` of the battery. Current flow in through the `E` pin, and divides among the `C` and `B` pins.
1. `conventional current` is the flow of electrical current from + positive to - negative.
1. it was assumed that `conventional current` was correct, aka electrons flow from `+` to `-`. However, it was proven that electrons _actually_ flow from `-` to `+`. This is called `electron flow`.
1. designs and diagrams always show `conventional current`, however engineers really know that `electron flow` (the opposite) current is what's truly happening.
1. in `NPN` transistors, (`n` and `p` referring to the semiconductor type), the `B (Base) pin` is connected to the `P-type semiconductor`.
1. `covalent bonding` is when two atoms (silicon for instance) want to satisfy their outer (valence) shell's electron count. To do this, they pair --or share, a bond with the silicon next to it which also has missing electrons. Together, through sharing, the reach their desired electron count.
1. doping in electronics typically occurs with silicon, so we will use that example. Silicon wants to satisfy it's valence shell's electron count. It can do this through `covalent bonding`. We can dope the **(n-type)** semiconductor with phosphorus atoms, which has enough outer electrons satisfy the four surrounding silicon atoms' valence electron needs, and also have the extra free flowing electron freely move around the material. Conversely, with **(p-type)** doping, we add a material like aluminum that has one too few electrons in its outer shell. This means that for the four surrounding silicon atoms, one won't be able to satisfy its valence shell electron count and now there is a "hole" where an electron should be.
1. the **"Depletion region"**. In this region, the extra free-floating electrons from the `n-type` semiconductor material translate over to the `p-type` semiconductor material to satisfy the missing electron requirements of the semiconductor material's silicon valence shells (the holes created by the doping of aluminum and silicon). Due to their charge differences they create slightly negatively and positively charged region, or an **electric field**. This process creates a small **electrical field** which **prevents** more electrons from translating across. The `potential difference` in this region is typically around 0.7V.
1. when we add a voltage source, like a battery **(+ to p-type and - to n-type)**, to a semiconductor material, we are able to create a `forward bias`. So long as the added voltage sourced from the battery is **greater than** the `potential difference` of the `depletion region` (say 0.7V) the electrons will be able to move through the region and we will have a current. If the voltage source is too low, the electrons will lack the sufficient power to overcome the `potential difference` and no longer pass through the barrier of the depletion region.
1. `reverse bias` is when the source (a battery for instance) is connected to the semiconductor in a **(- to p-type and + to n-type)** configuration. The `reverse bias` results in the **electrons being pulled back to the positive terminal** and the **holes being pulled back to the negative terminal.**
1. You have **two junctions** (both sides of the N where it meets the P material) in which the `potential difference` between the regions creates a field, or `depletion region`.

---

### Battery Questions

1. how is energy stored in a battery, how is it output when needed?
1. when connecting a battery's terminals to a circuit, what happens?
1. battery life depends on what two factors?
1. what is load?
1. what is the positive terminal of a battery called?
1. what is the negative terminal of a battery called?
1. are the anode and cathode electrically isolated from each other?
1. what is the general anatomy of a battery?
1. do the positive and negative terminals on a battery touch?
1. how is voltage important regarding electrons?
1. what is an `ion`?
1. whats the difference between a neutral, positive, and negative ion?
1. the chemical reactions in the battery cause what?
1. do the electrons returning back into the `cathode` do anything?
1. what two ways can batteries be combined?
1. what happens when we connect batteries in a `series`?
1. what happens when we connect batteries in a `parallel`?
1. what benefits does `parallel` battery configuration have?
1. what does `2500 mAH` mean?
1. what formula can you use to determine battery life?

### Battery Answers

1. batteries store energy as **chemical energy** and release it as **electrical energy**.
1. provided there is sufficient chemical energy stored, the battery provides pushing energy (volts) to move electrons across the circuit.
1. battery life depends on how much energy is stored in the battery and how much energy is demanded by the load.
1. load is any component along a circuit that requires electricity to work.
1. the positive terminal of a battery is called the `cathode`.
1. the negative terminal of a battery is called the `anode`.
1. yes, the anode and cathode are electrically isolated from each other.
1. typically, from the outside in, you have a steel casing with nickle plating which serves as a barrier from outside elements. Then, you have the **cathode material** along the internal outside of the battery (often comprised of manganese oxide and graphite). The next layer is a thin fibrous barrier that prevents the previously mentioned `cathode` and deeper `anode` materials from coming in contact. This fibrous material has microscopic holes that allows ions to pass through it. During manufacture, alkaline material (hence name) is sprayed inside the fibrous material. Lastly, the `anode` or core of the battery is added comprised of a zinc powder and gelling agent. A nylon cap is added to the bottom of the battery (anode terminal) with a brass pin and a steel cap.
1. no, the positive terminal (`cathode`) accounts for nearly the entire steel casing. However, the nylon casing added to the bottom of the battery provides a separation between the `anode`'s steel casing the steel casing of the battery proper (`cathode`).
1. without voltage, free floating electrons will move around in random directions. Voltage provides a current or force to move the previously free floating electrons down a controlled path.
1. `ions` are simply atoms with an **unequal** amount of protons or neutrons.
1. a neutral ion has equal protons and electrons, positive has more protons, and negative has more elections.
1. the chemical reactions in the battery cause a build up of electrons in the brass pin of the `anode` or negative terminal. This build up of electrons in the `anode` results in a voltage difference between the `anode` and `cathode`. Electrons repel each other (- -), they want to spread out, they need a route to reach the `cathode` --a conductive path, or circuit allows that.
1. yes, the electrons returning back through the `cathode` trigger chemical reactions that eventually result in more electrons being moved into the brass pin and passed into the `anode` and thus back into the conductive wire.
1. batteries can be combined in `series` (vertically) or in `parallel` (side-by-side in same orientation).
1. when we connect batteries in a `series` (vertically) the voltage of each battery is added together.
1. when we connect batteries in a `parallel` (side-by-side in same orientation) the voltage of each battery remains the same. This happens because the **paths merge at the supply (anodes connected together) and then splits at the return (path splits into each cathode)**
1. `parallel` configuration provides longer lifespan (higher capacity) of batteries with a larger current.
1. the battery can provide 2,500 milliamps of power for one hour, or 1,250 milliamps of power for 2 hours, and so on. However, this measurements are perfectly accurate since, as time goes on, the chemical reaction of the battery slows. Age, temperature, etc. also impact the lifespan.
1. `battery life = Capacity (mAh) / Circuit Current (mA)`

---

### Electrical Current Questions

1. what is current?
1. why do we often use copper as the conductor in wires?
1. what does voltage do in regards to the free floating electrons in copper?
1. what happens if **too many** electrons pass through a cable or component?
1. what is the flow of electrons measured in?
1. how does current flow with AC current?
1. how does current flow with DC current?
1. why do power stations use AC, instead of DC, to transport electricity? (2)
1. why do we use DC for electronic devices? (2)
1. can some household appliances use AC and DC? What's an example.
1. what can you use to convert DC to AC?
1. what can we use to measure the amount of current in a circuit?
1. what is the measurement of an amp?
1. whats a better tool than an `ammeter` for measuring amps (among other things)?
1. what does it mean to say a circuit is in "series"?
1. what is the opposite of a `series circuit`?
1. what how are amps measured differently between a series and parallel circuit?
1. what is a simplified interpretation of `resistors` on a circuit?
1. in addition to `resistors`, what else can we use to limit current?
1. how does a `fuse` work?
1. how does a `circuit breaker` work at a simple level?

### Electrical Current Answers

1. current is the flow of electrons in a circuit.
1. copper has an extra free electron in the outer (valence) shell that is very easy to move. In fact, it moves so easily that the extra electrons will move randomly across other copper atoms.
1. voltage moves the free-floating copper electron (in this case) in a specific direction.
1. it will burn out and break.
1. Amperes or Amps (A).
1. current flows back and forth along a circuit with AC.
1. current flows in one direction in a circuit with DC.
1. AC is much more efficient in transporting current over long distances. Additionally, voltage can be easily increased, or decreased, using transformers.
1. DC is used in electronic devices because it's easier to control and allows devices to be more compact.
1. Yes, some household appliances use **both** AC and DC. A washing machine for instance uses AC to power its AC `induction motor` and then DC to power the `control circuit board.`
1. you can use an `inverter` to convert DC to AC.
1. an `ammeter` can be used to measure the amount of current in a current.
1. one amp equals one `coulomb`, with a `coulomb` equaling 6.242x10^18 electrons per second.
1. a `digital multimeter` has more utility than an `ammeter`.
1. if a circuit is in "series", that means that _all_ electrons are flowing along the same path.
1. a `parallel circuit` is the opposite of a `series circuit`.
1. in a parallel circuit, in the main wire (to and from the battery) we get the full amps, but on the branch of each lamp we get a division of the amps based on resistance. In series, the amps are measured the same across the circuit.
1. `resistors` act like "speed bumps", or a kink in a garden hose, and can slow the current down.
1. a `fuse` can also be used to limit current.
1. a `fuse` is a safety device that melts and breaks an electric circuit if the current exceeds a safe level. In a basic sense, fuses have a thin wire inside of them that is rated to handle a certain current, if exceed, it melts and breaks the circuit protecting the more expensive components.
1. a `circuit breaker` behaves like a `fuse`, but instead of burning out, it serves as a `switch` and if too many amps are detected it breaking the closed circuit.

### Inverter Questions

1. what does an `inverter` do?
1. what can you use to convert AC into DC?
1. what sort of power sources created DC electricity and thus need an `inverter` to power AC appliances?
1. what can you use to see the electrical wave form of a circuit?
1. what does DC look like with an `oscilloscope`?
1. what does AC look like with an `oscilloscope`?
1. is the voltage constant in AC, would a `multimeter` convey this?
1. what is a simple way to describe how AC current is generated?
1. how do we measure frequency?
1. in North America, what is the frequency?
1. in the rest of the world, what is the frequency?
1. what does an `inverter` consist of?
1. since we'd have a low voltage output from the battery, and AC components typically need high voltage, what can we use to increase voltage?
1. when we use an `inverter`, technically it outputs an AC current (the electrical wave form is both positive and negative), but it doesn't look like a sine wave. What can be used to correct this?
1. regarding `PWM`, how can we modify the output voltage?
1. regarding `PWM`, how can we modify the output frequency?
1. if using an `oscilloscope`, how would the line appear between a `single phase` and `three phase`?
1. what does a `three phase` have over a `single phase`?

### Inverter Answers

1. an `inverter` is used to convert DC into AC.
1. you cna use a `full bridge rectifier` to convert AC into DC.
1. batteries, solar panels, cars (their batteries), are DC power sources and need to be converted to AC for certain appliances that require AC load.
1. you can use an `oscilloscope` to view the electrical wave form in a circuit.
1. DC, when measured through an `oscilloscope`, looks like a flat line in the positive region.
1. AC, when measured through an `oscilloscope`, looks like a wave the flows between the positive and negative regions.
1. voltage is not constant in AC, a `multimeter` may show it as constant, but that is false, it fluctuates between positive and negative in the sine wave.
1. AC current, and its back and forth current, is caused by a magnet within a generator that rotates and uses induction (specifically varying levels of magnetic intensity as it rotates) to move electrons, in nearby coils, back and forth.
1. in terms of AC sine waves, frequency refers to how many times the sine wave repeats per second and is measured in **Hertz (Hz)**.
1. in North America, the sine wave repeats 60 times per second (60Hz). Polarity (+ and -) changes 120 times/second.
1. in the rest of the world, the sine wave repeats 50 times per second (50Hz). Polarity (+ and -) changes 100 times/second.
1. the `inverter` consists of several electronic **switches** called `IGTBs`, the opening and closing of these `IGTB` switches is controlled by a `controller`. `IGTBs` can open and close in pairs incredibly fast to control the flow of electricity. This path tweaking for electricity allows inverters to produce AC from a DC source.
1. a "step-up" `transformer` can be used, following the inverter, to increase low AC voltage to high AC voltage.
1. we can use `Pulse Width Modulation` to break the **flat** positive and negative waves into smaller segments of pulse width to create an **average** flow, or wave form, that imitates a sine wave. The more segments it has, the closer in mimics a sine wave.
1. we can control the output voltage by **controlling how long the switches are closed for** for each segment group. The less completely filled a segment group is, the lower the voltage.
1. we can control the output frequency by **controlling the timing of the switches**. The more spread out the segment groups are, the lower the frequency.
1. a `single phase` would appear as a single sine wave, whereas a `three phase` would appear as three equally inter-spaced sine waves.
1. a `three phase` has no (significant) gaps between positive and negative (the three sine waves overlap) and thus it has **more power**.

### Full Wave Bridge Rectifier Questions

1. what does a `FWBR` look like?
1. what configuration do `FWBR` diodes often come in?
1. an `FWBR` converts current from what, to what?
1. what would you use to convert DC to AC?
1. what is a `half-wave rectifier circuit`?
1. is a `half-wave rectifier` circuit useful at all?
1. how could a `center tapped transformer` be used within a `full-wave rectifier` using two `diodes`?
1. what is the most common form of a `full-wave rectifier`?
1. what can you use to smooth a DC ripple?
1. how does a `capacitor` even out the DC ripples?
1. will adding a `capacitor` completely smooth out the DC ripples?
1. what should we place following a `capacitor`, to keep us safe?
1. why would the voltage drop from the AC input to the DC output?
1. if we place a `capacitor` after the `rectifier`, would the output DC voltage now be higher than the input AC voltage? How is that possible?
1. what other components could you use to even out the DC voltage?

### Full Wave Bridge Rectifier Answers

1. `FWBR` have many different appearances, but typically consist of 4 `diodes`.
1. `FWBR` diodes typically form in a diamond pattern, but they can be aligned in other ways.
1. `FWBR` converts AC to DC (opposite of an `inverter`).
1. an `inverter`.
1. a `half-wave rectifier` circuit is a circuit where a `diode` blocks one of the AC patterns from the AC generator. The output is _technically_ DC because electrons are only flowing along the circuit in **one direction** (due to the `diode` preventing both directions of AC). Using an `oscilloscope` you would only see the positive, or the negative (`diode` orientation), parts of the sine wave from the AC current. Note: whatever orientation (negative or positive) of DC current we have, it wouldn't be very good DC current due to it _not_ being a flat positive line.
1. while not ideal universally, a `half-wave rectifier` circuit _could_ be useful for simple circuits where you're powering a lightbulb or battery. Though, it is **not** useful for circuit boards that need a constant DC current.
1. a `center tapped transformer` just has another wire on the secondary side of the `transformer` that is connected to the **center** of the coil. The current flows in through the first `diode`, through the `load`, and out trough the `center tapped transformer`. We can use another `diode` to prevent the AC from traveling back through the `center tapped transformer`. With this setup, **only half** of the `transformer` coil is therefor being used at a given time. As the current alternates, the current flows in just one direction to the `load` creating a positive-only sine wave (still not flat) where the negative half of the AC has been converted into a positive half.
1. the most common form of a `full-wave rectifier` is a rectifier using **four** diodes.
1. you can use a `capacitor`, in parallel to the load, to smooth a DC ripple.
1. as the voltage decreases in the DC ripple, the capacitor having a higher voltage, discharges electrons which helps even out the wave. As the wave increases, it stores the extra electrons.
1. no, adding a `capacitor` will only _help_ reduce the crests and troughs of a wave. Adding a larger `capacitor`, and/or multiple `capacitors`, only further reduces it. The approach **does not** result in a fully smooth DC current.
1. following the `capacitor` we should place a `bleeder resistor` across the output; this is a high-value capacitor that will drain the `capacitor` as the circuit is off for safety. Effectively, the `bleeder resistor` rapidly discharges the `capacitor` much faster than otherwise.
1. the `diodes` used in the rectifier have a `load`, or cause "voltage drop".
1. it's not really higher, it's simply the format used to measure AC voltage (RMS) doesn't return the **peak voltage**. A `multimeter` reads the **Vrms not peak voltage** (Vrms is like an average from the sine wave) from an AC input. Capacitors charge up to the **peak voltage** and then release. Note: this **peak voltage** is less than the original **peak voltage** because of the voltage drop caused by the `diodes` earlier in the circuit.
1. `inductors` and voltage `regulators` can also be used to even out the DC voltage.

### Transformer Questions

### Transformer Answers

---

### MPU Questions

1. what is an MPU?
1. how does an MPU work?
1. what is a `Bus`? What are the tree types of buses usually present on an MPU?
1. what is `clock speed` used to measure, what type of measurements are there?
1. what is an `instruction set`?
1. what is `word length`?
1. what is `cache memory`?

### MPU Answers

1. an `MPU` is a Microprocessor (or Micro Processing Unit). It is the central unit of a computer system that performs arithmetic and logic operations. It also incorporates the function of a CPU on an integrated circuit.
1. MPU basically accepts binary data as the input, processes the data according to instructions stored in its memory, and provides results (also in binary form) as output.
1. `Bus:` Describes the set of conductors that transmit data or that address or control information to the microprocessor’s different elements. (MPU usually has 3 types: data bus, the address bus, and control bus.)
1. `Clock Speed:` Normally measured in Hertz and expressed in measurements like MHz (megahertz) and GHz (gigahertz), refers to the speed at which a microprocessor could execute instructions.
1. `Instruction Set:` A series of commands that a microprocessor can understand, basically the interface between hardware and software.
1. `Word Length:` The number of bits in the processor’s internal data bus, an 8-bit MPU can process 8-bit data at one time.
1. `Cache Memory:` It stores data or instructions that the software or program frequently references during operation, increases overall speed as it allows the processor to access data more quickly.

---

### MCU Questions

1. what is an MCU?
1. how does an MCU work?
1. explain `CPU`.
1. explain `Memory`.
1. explain `Timer`.
1. explain `I/O Port`.
1. explain `Serial Ports`.
1. explain `Interrupt control`.
1. what bit types of MCUs exist?

### MCU Answers

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

### SBC Questions

1. what is an `SBC`?
1. what is a CPU?
1. how does SBC work?
1. explain `CPU/SoC`
1. explain `Clock/Timer`
1. explain `I/O port`
1. explain `Serial Ports`
1. explain `ADC/DAC`
1. explain `Wireless Connectivity`

### SBC Answers

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
