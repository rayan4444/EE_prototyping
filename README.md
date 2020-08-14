# EE prototyping

Here are some of my tips and tricks on setting up a sustainable and effective prototyping cycle on EE projects. The goal is to safe as much time and effort as possible - and money as a side effect :)

## Step 0: Set up and use a documentation framework early

Start documenting your project early and thoroughly. Keep track of EVERYTHING related to your EE design/project: design changes, calculations, decisions, debugging notes, etc.

**Why?**
* Avoid repeating the same mistakes
* Help interface with other aspects of the project (MechE, firmware)
* Facilitate onboarding, handover, communication etc.

**How?**
* Use git! it keeps track of changes for you and your collaborators.
* The README should give a thorough overview of the project and its status
* Leave meaningful commit notes
* Make a release file archive and tag every time you order a PCB/release a firmware version
* Check out a [template](https://github.com/rayan4444/pcb-repository-template)

> ECAD visual diff is coming soon: good potential for interdisciplinary communication and PM. See [cadlab](https://cadlab.io/), [inventhub](https://inventhub.io/), [Allspice](https://www.allspice.io/)


## Step 1: Invest in block diagrams

A visual overview of your system architecture or board design is both a great communication tool and a great way to review and streamline designs as you iterate.
* Avoid over-complicating your project
* Split your project into easy to work on modules
* You can have diagrams with different levels of detail or keep a clean but thorough master diagram.

**Example**

<img src ="/assets/architecture.jpg" width="400">

> Suggestions: I've been using draw.io for my block diagrams and dropping a link or picture on my Project READMEs

## Step 2: Component selection

As you start polishing the details of design you will have to choose specific components. Key considerations:

* Choose a platform you are familiar with/you find easy to learn (especially for MCU), even if it is more expensive. It saves you a lot of time in the early stages, especially when you are in a hurry to push a POC out the door.

* How important is a component to your product's function? Liability if it fails? (sometimes the cheapest option isn't the smartest)

* Is it easy to source? will it still be available in x years? (supplier, stock, lead times, life cycle, etc. )

* Finally, price. Especially at the quantities you are looking to manufacture at.

**Advice**
* Start by using a module when you can. You are less likely to make mistakes on key circuitry, some are even pre-certified. Example: ESP32 dev board, module and chip.

<img src ="/assets/esp32.PNG" width="300">

* Don't make a custom board unless you have a real reason to. Development tools/boards can bring you far.

## Step 2.5: Software choices
Different ECAD software are relatively similar in features. Some of the most popular are:

* [Altium Designer](https://www.altium.com/): industry standard  
    * Pros: complete professional features & tools, integrates with Solidworks, easy to hire talent that knows it, what most Chinese factories/partners will use
    * Cons: expensive license fees, Windows only


* [Autodesk Eagle](https://www.autodesk.com/products/eagle/free-download): maker friendly
    * Pros: easy to learn, lots of open source designs and libraries  (Arduino, sparkfun, adafruit), supports custom scripts, free for students and hobbyists, affordable license, works with fusion360
    * Cons: less well known/hobby reputation


* [KiCAD](https://kicad-pcb.org/): open source
    * Pros: completely free and open source  
    * Cons: slightly steeper learning curve, less features in the stable versions

> You should choose the one that makes most sense for you, your team, your project long term.

## Step 3: Schematic

The schematic is the single most important document for others (and yourself) to actually
understand your circuit.

**Tips:**
* Organize it in labeled sections

* Follow some sort of convention: signals "flow" from top left to bottom right (Inputs to Outputs, Power in to GND)

* Write down notes where necessary: calculation notes, selection jumpers, different assembly configurations. It saves you time going back and digging through old design notes.


<img src ="/assets/sch.PNG" width="500">


> **Make your early prototypes modular to try out things**: you can try two ways of achieving the same function o put a footprint for two components you want to test, etc. A design like this can be a lasting asset in case you want to come back to it later in your development process.

## Step 3: Placement test for sizing
When you need to find out if your chosen components will roughly fit into your space requirements (if any):
* Place components with constraints first: USB port, buttons, antenna, etc.

* Place the rest of the components in a sensible fashion (ie: group sections of your schematic together)

* Don't route yet: this is just an exercise to check the components you select fit or whether you need new components of more/less space.

## Step 4: Layout

Making a PCB is quick and easy. Hunting down hardware bugs/manufacturing issues is painful and slow.

**For your first iterations**
* Use footprints that can be reworked by hand

* Design expecting you'll need to cut traces, add jumper wires, remove components.

* Isolate functional sections of your circuit (grouped according to your schematic) and leave jumper connections in between. It helps:
    * testing section by section
    * avoiding to frying everything due to one malfunction
    * injecting signals
    * reusing a section in another design
    * example jumpers: 2.54mm headers, solder bridges, 0 ohm resistors

## Step 5: Make your board easy to test
Some features make testing a lot let painful.
* Write down a version number (and your name, so people know who to ask if they have questions)
* Leave a space for jotting down notes directly on the board (silkscreen rectangle)

<img src ="/assets/dft.PNG" width="400">


* Leave a ground connection available for oscilloscope/multimeter probing

* Leave open test points and label them or consider not tenting your vias to have them act as test points

* Annotate when possible, especially writing points and important signals

## Step 6: Assembling and testing your boards
Depending on your prototyping stage, you have a said quantity of boards that are more or less difficult to assemble yourself. My recommendations:

* Assemble your own board first whenever possible:
    * you will make mistakes specifying components others might not realize
    * you can see where your assembly documentation is lacking
    * getting familiar with the assembled board will help you while debugging.


* Always assemble at least 2 boards and keep at least one blank board: simplifies debugging and helps trace issues back to firmware, design, assembly, or manufacturing faults.

* Invest in a stencil: it saves you A LOT of time soldering and good quality is a lot easier to achieve (ergo: less reworking effort). Use fresh and/or well stored solder paste, you can use a reflow oven, hot plate or even heat gun. There are plenty of stenciling tutorials online, like [this one](https://www.youtube.com/watch?v=WDIqtGMROjM)

* Keep a log  of assembly notes on your project documentation, to improve next iterations (ex: capacitors are tombstoning, these components are too close to each other, LED polarity indicator is missing, etc.).

## Step 7: Test, debug, redesign, remake

 * Test your design section by section, noting down any errors for all designs in your errata and soldering errors for individual boards on the board itself

 * Don’t throw out your old boards, you might make an error in the new
revision and want to splice in a section from the previous design or re-use some of its components

* Make a release and tag in your git everytime you order a PCB, that way you can easily come back to check the pinouts, errata etc. You can also compare differences when you’re debugging errors.
