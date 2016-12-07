.. title: Disabling/Enabling the Asus UX303 Touchscreen in Ubuntu 16.04
.. slug: disablingenabling-the-asus-ux303-touchscreen
.. date: 2016-12-07 14:20:23 UTC+08:00
.. tags:
.. category:
.. link:
.. description:
.. type: text

Find the Atmel touchscreen device::

  $ xinput --list
  ⎡ Virtual core pointer                    	id=2	[master pointer  (3)]
  ⎜   ↳ Virtual core XTEST pointer              	id=4	[slave  pointer  (2)]
  ⎜   ↳ FocalTechPS/2 FocalTech FocalTech Touchpad	id=17	[slave  pointer  (2)]
  ⎜   ↳ Logitech USB Optical Mouse              	id=20	[slave  pointer  (2)]
  ⎜   ↳ Atmel                                   	id=10	[slave  pointer  (2)]
  ⎣ Virtual core keyboard                   	id=3	[master keyboard (2)]
      ↳ Virtual core XTEST keyboard             	id=5	[slave  keyboard (3)]
      ↳ Power Button                            	id=6	[slave  keyboard (3)]
      ↳ Sleep Button                            	id=9	[slave  keyboard (3)]
      ↳ USB2.0 UVC HD Webcam                    	id=13	[slave  keyboard (3)]
      ↳ Video Bus                               	id=7	[slave  keyboard (3)]
      ↳ AT Translated Set 2 keyboard            	id=16	[slave  keyboard (3)]
      ↳ Video Bus                               	id=8	[slave  keyboard (3)]
      ↳ Asus WMI hotkeys                        	id=15	[slave  keyboard (3)]

The ``Atmel`` device is our touchscreen.

Use the ``xinput disable`` and ``enable`` commands to turn the touchscreen off or on again.::

  $ xinput disable Atmel
  $ xinput enable Atmel

Both commands are silent, unless you specify a device that doesn't exist.
