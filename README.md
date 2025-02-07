# apalis-dashboard

Display: KOE TX31D200VM0BAA (12.3", around $250)
A Toradex Apalis iMX6Q System on Module cold booting a graphical instrument cluster in about 1.2 Seconds.
The Graphical User Interface is built with Qt running on Linux. The SoC is an NXP i.MX6 Quad Core.
Toradex provides a Linux BSP based on the Yocto Project. Several parts were optimized to reach this boot time.
To learn more see also: https://developer.toradex.com/easy-installer/toradex-easy-installer/flashing-new-image-using-tezi/

1. What is the Apalis iMX6 Module?
The Toradex Apalis iMX6 is a System on Module (SoM). That means it’s a small board containing:
The main processor (NXP i.MX6)
Memory (RAM, flash storage)
Power management for the SoM
Basic connectivity interfaces (e.g., USB, Ethernet, display signals, etc.)
It does not include standard connectors for your display, USB ports, Ethernet jacks, or other I/O you’d expect on a full single-board computer. Instead, it presents these signals through high-density connectors that mate with a carrier board.

“WinEC” vs. Standard (Linux) Version
“WinEC” typically means Toradex has preloaded a Windows Embedded Compact OS image on the SoM.
Standard version usually ships with either a minimal embedded Linux (e.g., a reference Linux or Torizon) or may be provided “blank” for you to load your own OS (Yocto-based Linux, etc.).
If you want to run Linux or Torizon (Toradex’s Docker-based environment), you’d get the Apalis iMX6 Quad 1GB (non-WinEC). If you specifically want Windows Embedded Compact for some reason, you’d get the WinEC version. Otherwise, the hardware is largely the same i.MX6 Quad SoC with 1 GB of RAM.

2. You Absolutely Need a Carrier Board
Because the Apalis module is just the core SoM, you’ll need a carrier board to break out:

LVDS or parallel RGB signals for your display
USB, Ethernet, CAN, serial, and other connectors
Power input (to power both the SoM and possibly the display backlight)
Toradex Carrier Boards
Toradex provides reference carrier boards for Apalis modules, such as:

Apalis Evaluation Board – larger, with many I/O connectors, good for development.
Ixora Carrier Board – more compact, still offers the main I/O (Ethernet, USB, display interfaces, etc.).
Many people start with one of these official carrier boards for development and proof-of-concept, then design a custom carrier board for their final product dimensions and connector layout.

3. Driving the KOE TX31D200VM0BAA Display
Your KOE 12.3" display likely uses an LVDS interface (check the datasheet to confirm it’s not MIPI DSI or parallel). Key points:

LVDS signals: The i.MX6 supports LVDS natively, and the Apalis module routes those signals out through its module connector. The carrier board then provides the appropriate LVDS connectors or pin header.
Backlight driver: Often, industrial displays have a separate backlight input. You may need a small backlight driver board or use the one integrated on your carrier if available. Make sure the carrier board can supply the correct voltage and current for the backlight.
Display timing configuration: You’ll configure the i.MX6’s display timings (resolution, refresh rate, etc.) in the device tree (Linux) or registry (WinEC).
If you’re using the Toradex Apalis Evaluation Board or the Ixora board, they typically have an LVDS connector. You would need an LVDS cable adapter that matches your display’s pinout. Sometimes you also need an additional separate board or harness to match KOE’s exact connector.

4. Summary: What You Need to Buy
An Apalis iMX6 Module

Choose the non-WinEC version (e.g., “Apalis iMX6 Quad 1GB”) if you plan to run Linux or Torizon.
Choose “WinEC” version only if you specifically need Windows Embedded Compact.
A Carrier Board compatible with Apalis iMX6

For development/prototyping, a typical choice is the Apalis Evaluation Board (full-featured) or Ixora Carrier Board (more compact).
If your end goal is a custom track-car dashboard, you might eventually design your own smaller carrier board—but starting with an official dev board speeds up the “getting it working” phase.
Cables/adapters for your KOE TX31D200VM0BAA display

LVDS cable from the carrier board’s LVDS connector to the display’s interface.
A backlight power cable or driver solution if the display’s backlight is separate.
Power supply

You’ll need a clean 5V or 12V (depending on the carrier board input specs). The board will regulate down to what the module needs, but check recommended input voltage ranges in the carrier board documentation.
Any additional accessories (optional)

Touch panel controller or USB touch interface if the display is touch-capable (and you intend to use it).
External CAN transceiver or cables if you plan to utilize CAN bus. Some Toradex carrier boards might have onboard CAN transceivers, others may require an add-on or custom circuit.
5. Next Steps
Check Toradex Documentation

Look for instructions on hooking up an LVDS display to Apalis iMX6 on the Ixora or Apalis Eval Board.
Ensure the resolution (1920 × 720, or whatever the display is) is within the i.MX6’s LVDS capabilities (it likely is, but always good to confirm).
Plan Your OS

If you want a fast-boot Linux environment, consider Torizon or a minimal Yocto build. Torizon has tutorials on the Toradex site that show how to enable LVDS displays, set resolutions, etc.
Prototype, Then Optimize

Boot times on a stock Linux image can exceed 10 seconds, so you might have to trim services or use Torizon’s fast-boot strategies. If sub-10-second boot is critical, plan to spend time optimizing.
In Short:
Yes, you do need a carrier board to use the Apalis iMX6 with your KOE display. You can’t just buy the Apalis SoM alone and plug in the LCD—there’s nowhere to plug it in without a carrier.
The “WinEC” version means a preinstalled Windows Embedded Compact image. If you don’t specifically need that, pick the standard “Apalis iMX6 Quad” version for Linux or Torizon.
You’ll need the correct LVDS cables, plus possibly a backlight driver arrangement for the KOE panel.
That’s it! Once you have the SoM + carrier + correct cables + your display, you can power it all up and develop your custom dashboard UI.
