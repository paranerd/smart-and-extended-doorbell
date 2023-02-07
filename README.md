# Extended and Smart Doorbell

This project makes an existing doorbell 'smart' and extends it with a second doorbell to be activated once the primary doorbell rings.

## Parts used

- [Honeywell D117](https://livewell.honeywellhome.com/de/klassisch/d117/) (or any mechanical gong that can run on ~5VDC)
- [Wemos D1 Mini](https://www.az-delivery.de/en/products/d1-mini) (or any ESP board with 5VDC input pin)
- [Shelly 1](https://www.shelly.cloud/de/products/product-overview/1xs1)
- 12VDC power supply
- PN2222 (or any other NPN transistor)
- 5VDC power supply (probably >1.5A)
- 1N4007 Diode
- Jumper Cables

## Instructions original doorbell

![Wiring doorbell](/wiring/smart-doorbell-wiring.png)

The wiring above shows the setup for my situation where the gong needs 8VAC.

Basically, the Shelly reads when the button is closed and then closes the connection between "I" and "O" which will power the gong.

In addition it is hooked up to Home Assistant (or any other smart home system) to report its current state.

**Important:** This only works with the Shelly 1 as it has potential free outputs! If try to do this with another Shelly, stuff may blow up. You have been warned, don't blame me if things go bust.

**Alternative:**

If your gong can be powered with 12VDC, you can remove the 8VAC power supply and would simply bridge "L" to "I" and gong "+" to "N" (DC +).

## Instructions extended doorbell

![Wiring extended doorbell](/wiring/extended-smart-doorbell-wiring.png)

The extended doorbell is controlled by ESPHome on a Wemos D1 Mini. When Home Assistant receives a "gong has been activated" signal from the Shelly 1, it sends a signal to the D1 to ring the extended gong.

**Important things:**
- You cannot power the gong from the D1 directly as its output is not high enough, we need a transistor to do the switching for us
- Order is important: The 5VDC must go directly to the gong and THEN to the transistor
  - I believe this is due to the fact that the gong uses a coil which doesn't get powered up enough if connected the other way around - but I may be completely wrong here
- Add a diode between transistor collector and 5VDC+ to avoid induction from coil discharge to go back in the wrong direction
