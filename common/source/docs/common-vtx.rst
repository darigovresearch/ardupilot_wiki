.. _common-vtx:

Video Transmitter Support
=========================

Video transmitter (VTX) support allows the flight controller to control an attached video transmitter. Depending on the VTX and protocol used, components that can be controlled include video band, video channel, transmitter power and pit mode. Supported protocols are SmartAudio and TBS Crossfire (CRSF).

.. note:: Not all video transmitters claiming SmartAudio support fully support the specification and some functions may not be provided in that transmitter, or implemented in a non-standard way. Consult the manufacturer's documentation if questions arise.

How to setup video transmitter support
======================================

To enable video transmitter support you need to set the following parameters and reboot your autopilot:

- Set :ref:`VTX_ENABLE <VTX_ENABLE>` to 1 to enable video transmitter support. Although this enables support it will not achieve anything without an appropriate video transmitter protocol enabled, as described below.

SmartAudio
----------

SmartAudio is a one-wire protocol so you will need to connect the SmartAudio pin of your VTX to the TX pin of the serial port you want to use. You will also need to make sure your VTX is configured for SmartAudio control. For example if attached to SERIAL port 5:

- Set :ref:`SERIAL5_PROTOCOL <SERIAL5_PROTOCOL>` to 37 to enable SmartAudio.
- Set :ref:`SERIAL5_OPTIONS <SERIAL5_OPTIONS>` to 4 to enable half-duplex which SmartAudio requires
- Set :ref:`SERIAL5_BAUD <SERIAL5_BAUD>` to 4800 to set the SmartAudio baud rate

CRSF (Crossfire)
----------------

CRSF is a bi-directional protocol that requires both TX and RX of a serial port to be connected. Follow the instructions for CRSF telemetry to setup CRSF support (See :ref:`common-tbs-rc`). If you only wish to use CRSF for VTX support, then (again using SERIAL5 as an example) :ref:`SERIAL5_PROTOCOL <SERIAL5_PROTOCOL>` should be set to 29 rather than 23. You will also need to make sure your VTX is configured for CRSF control.

When using CRSF for both RC and VTX control it is important to disable "my VTX" support on Open TX transmitters. Follow your transmitters' instructions.

Reboot your flight controller and power your video transmitter. At startup you should see a video message indicating the current video settings. All the video settings are stored in the ``VTX_x`` parameters and by default these will be changed to reflect the currently configured VTX settings. Once booted you can modify these settings and they will be reflected in the VTX configuration.

Video transmitter settings
--------------------------

- Set :ref:`VTX_BAND <VTX_BAND>` to modify the VTX band.
- Set :ref:`VTX_CHANNEL <VTX_CHANNEL>` to modify the VTX channel.
- Set :ref:`VTX_FREQ <VTX_FREQ>` to modify the VTX frequency. Band/channel and frequency are mutually exclusive, if you set band and/or channel then the frequency will be automatically updated. If you set the frequency then the band/channel will be automatically updated. Not all frequencies, channels and bands are supported unless the transmitter is unlocked. Please consult local regulations before doing this. If you select an unsupported frequency then the current frequency will not be changed.
- Set :ref:`VTX_POWER <VTX_POWER>` to modify the VTX power in mw. Not all transmitters support all power values. In particular in Europe only 25mw is allowed by default. To allow other values the transmitter must be unlocked. If you select a power level that is unsupported by the transmitter then the actual power value will not be changed.
- Set :ref:`VTX_MAX_POWER <VTX_MAX_POWER>` to set the maximum VTX power allowed in mw. This is used by ``RCx_OPTION`` = 94 which allows the VTX power to be changed via a switch or dial.
- Set :ref:`VTX_OPTIONS <VTX_OPTIONS>` to set options on the VTX. The most common option is 1 which puts the VTX into pit mode if supported. Option 2 can be used to unlock the transmitter, but note this is a one-way operation than cannot be undone through software. Other options allow pit mode to be set on disarming.

.. note:: "unlocking" can be done differently, depending on transmitter brand. Also, using unlocked frequencies/power levels may violate local laws and restrictions. 

Setting video transmitter settings
----------------------------------

Video transmitter settings can be changed in multiple ways but always go via the ``VTX_x`` parameters. So any option which advertises VTX control will be setting a ``VTX_x`` parameter which in turn will interface with the protocol backends. Here are the current ways that video transmitter settings can be modified:

- Parameter modification through your ground station
- Transmitter power via RC switch (``RCx_OPTION`` = 94).
- Parameter modification via the OSD (See :ref:`common-paramosd`)
- Parameter modification via CRSF OpenTX lua scripts (or OpenTX AgentX lua scripts) - CRSF only
- Spektrum VTX support. VTX settings on your Spektrum transmitter will be translated by either the DSMX or SRXL2 drivers and the appropriate VTX settings updated