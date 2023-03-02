# `kARCA` User Manual

This document is intended to be used as a manual for working with the `kARCA` system.
It is not a document for explaining the exact inner workings of the system (for that, see
[karca.md](karca.md)).

## Running the controller

These are bare-minimum instructions for running the engine controller.
To do more complex things, you will need to review some of the later procedures in addition to this
one.

### Controller: Materials required

- Assembled `kARCA` box
- 1 12V battery
- 1 2-pin Anderson to Fucking Power Connector cable
- 1 male-to-male Ethernet cable
- Computer with SSH client and `slonkboard` installed, as well as Ethernet capability or adapter

### Controller: Assembly

![The final assembled setup for running the controller.](img/running_controller_assembly.png)

- Connect the 12V battery to the MAIN POWER connector on the `kARCA` box with the 2-pin Anderson to
  Fucking Power Connector cable.
- Connect your computer to the `kARCA` box using the male-to-male Ethernet cable.

### Controller: Procedure

1. **Turn on the box.**
   Inside of the `kARCA` box, toggle the main power switch to the ON state.

   ![The location of the main power switch of the controller.](img/main_power_switch.png)

   After the main power switch has been turned on, the following things should happen:

   - 3 red "power status" LEDs on the `kARCA` PCB inside the box should turn on.
   - 6 blue "fuse status" LEDs on the `kARCA` PCB inside the box should turn on.
   - 0 white "driver status" LEDs on the `kARCA` PCB inside the box should turn on.
   - 1 red "power" LED on the Raspberry Pi inside the box should turn on.
   - 1 green "activity" LED on the Raspberry Pi inside the box should start flashing intermittently.

1. **Wait for the Raspberry Pi to boot.**
   Wait approximately 1 minute for the Raspberry Pi to boot.

   After the Pi has finished booting, the green "activity" LED on the Raspberry Pi should stop
   flashing so often.

1. **Connect to the Raspberry Pi via SSH.**
   MacOS and Linux users prefer to use `ssh` to connect to the Raspberry Pi; Windows users typically
   prefer to use PuTTY or MobaXTerm to connect.

   On MacOS or Linux, execute the following command in a shell session to begin an SSH session:

   ```sh
   ssh pi@karca.local
   ```

   If all goes well, you will then be prompted to enter a password to log in.
   Enter the password `riceeclipse` to log in.

1. **Begin running the `slonk` engine controller software.**
   Inside the SSH session, navigate to the directory where the `slonk` repo is stored.
   You can do this with the following command:

   ```sh
   cd ~/slonk
   ```

   Next, begin running the controller software.
   The `slonk` binary takes two arguments - a configuration JSON file and a log file directory.
   Configuration files are stored in the `config` subdirectory of the `slonk` repository, and log
   files may be stored anywhere (but we recommend storing them _outside_ of the `slonk` directory).
   Traditionally, log files are stored in the directory `~/slogs`.
   Pi.

   An example command to run the controller looks like this:

   ```sh
   sudo ./target/release/slonk config/titan-karca.json ../slogs/your-log-folder-here
   ```

   If all succeeds, some debug information will be printed to the console.
   Here's some sample output using the Titan configuration:

   ```text
   [1677731707833558761] [DEBUG] Parsing configuration file...
   [1677731707834807791] [DEBUG] Successfully parsed configuration file
   [1677731707834859375] [DEBUG] Creating log files
   [1677731707834990504] [INFO] Created directory ../slogs/your-log-folder-here/Thermocouples
   [1677731707835107088] [INFO] Created log file ../slogs/your-log-folder-here/Thermocouples/TC1: Oxidizer tank.csv
   [1677731707835229193] [INFO] Created log file ../slogs/your-log-folder-here/Thermocouples/TC2: Combustion chamber.csv
   [1677731707835395207] [INFO] Created directory ../slogs/your-log-folder-here/Pressure transducers
   [1677731707835505865] [INFO] Created log file ../slogs/your-log-folder-here/Pressure transducers/PT1: Combustion chamber.csv
   [1677731707835622905] [INFO] Created log file ../slogs/your-log-folder-here/Pressure transducers/PT2: Oxidizer feedline.csv
   [1677731707835726179] [INFO] Created log file ../slogs/your-log-folder-here/Pressure transducers/PT3: Injector.csv
   [1677731707835825508] [INFO] Created log file ../slogs/your-log-folder-here/Pressure transducers/PT4: Oxidizer tank.csv
   [1677731707835990269] [INFO] Created directory ../slogs/your-log-folder-here/Load cells
   [1677731707836105837] [INFO] Created log file ../slogs/your-log-folder-here/Load cells/Main axial cell.csv
   [1677731707836294026] [DEBUG] Successfully created log files
   [1677731707836352894] [DEBUG] Now acquiring GPIO
   [1677731707836411931] [DEBUG] Successfully acquired GPIO handles
   [1677731707836467380] [DEBUG] Now spawning sensor listener threads...
   [1677731707837198161] [DEBUG] Successfully spawned sensor listener threads.
   [1677731707837258496] [DEBUG] Opening network...
   [1677731707837401398] [INFO] Opened TCP listener on address 0.0.0.0:2707
   [1677731707837433165] [DEBUG] Handling clients...
   ```

1. **Begin running the dashboard software.**
   In a new shell session, navigate to the directory containing the `slonkboard` repository.
   Start the dashboard using the following command:

   ```sh
   ELECTRON_ENABLE_LOGGING=1 npm start
   ```

   It is not required to set `ELECTRON_ENABLE_LOGGING` to 1; however, this enables some extra debug
   printouts which are handy to read.

   If your dashboard computer is set up correctly, a a window should appear displaying the
   `slonkboard` application.

1. **Connect the dashboard to the engine controller.**

   ![The location of the address and port input for `slonkboard`](img/slonkboard/address_input.png)

   In the top right corner of the `slonkboard` window, populate the address and port information.
   By default, address `karca.local` and port 2707 are correct; however, if you want to use a
   differing host address or port you may.

   On some routers, the Raspberry Pi will not be set up with a string as its host, so you can use
   `nmap` to find the IP address for the Raspberry Pi and directly connect using that instead.

   After connection, new graphical elements should appear on `slonkboard`, and the controller
   software should also print out a confirmation that a new client has connected.

## Using the antennae

### Antennae: Materials required

- 1 assembled `kARCA` box
- 1 12V battery
- 1 2-pin Anderson to Fucking Power Connector cable
- 1 male-to-male Ethernet cable
- 1 XLR to router barrel-jack cable
- 1 Linksys 1200ac router
- 2 Fucking Router Connector to Fucking Antenna Connector cables
- 2 long-range 2.4 GHz directional antennae
- 1 Fucking Router Connector to USB or similar adapter
- Computer with SSH client and `slonkboard` installed

### Antennae: Assembly

![The final assembly setup for using the long-range antennae.](img/using-antennae-assembly.png)

- **Basic power setup.**
  Set up the battery connector as described in
  [the controller setup procedure](#controller-assembly) and turn the main power switch on.

- **Connect the router to the box.**
  Connect the XLR to router barrel-jack cable between the "ROUTER POWER" output on the box and the
  female barrel jack connector on the box.
  Connect the male-to-male Ethernet cable to the RJ45 output on the box and to the RJ45 connector
  for port 1 on the router.

- **Connect the router to an antenna.**
  Connect one of the antenna out connectors on the router to the antenna using a Fucking Router
  Connector to Fucking Antenna Connector cable.

- **Connect the dashboard computer to another antenna.**
  Connect the dashboard computer to the USB adapter and then the adapter to the second antenna with
  a Fucking Router Connector to Fucking Antenna Connector cable.

- **Point the antennae toward each other.**
  The antennae are directional, so they must be pointed toward each other to work correctly.
  They seem to be relatively picky about their angle, so this may be difficult.

### Antennae: Procedure

- **Turn on the box.**
  For more details, refer to [the controller setup procedure](#controller-procedure).

- **Turn on the router.**
  To do so, flip the power switch on the back face of the router.
  The router power light should immediately turn on.

- **Wait for the router to boot.**
  The router will take roughly 1 minute to boot.
  When it is finished, the port 1 light on the router should begin flashing and the "internet"
  light should flash orange.

- **Connect the dashboard computer to the router's WiFi network.**
  If all was done correctly, the router's network should appear as a visible Wi-Fi network to the
  dashboard computer.
  Connect to this network.
  Historically, the name of this network has been `rice-eclipse-box-2g`.

- **Run the controller and dashboard software.**
  For more details, refer to [the controller setup procedure](#controller-procedure).
