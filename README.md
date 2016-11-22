# curiosity

> Modelling robotos on other planets beep boop

## Research

From [Wikipedia](https://en.wikipedia.org/wiki/Curiosity_(rover)):

> Communications: Curiosity is equipped with significant telecommunication redundancy by several means – an X band transmitter and receiver that can communicate directly with Earth, and a UHF Electra-Lite software-defined radio for communicating with Mars orbiters.[31] Communication with orbiters is expected to be the main path for data return to Earth, since the orbiters have both more power and larger antennas than the lander allowing for faster transmission speeds.[31] Telecommunication includes a small deep space transponder on the descent stage and a solid-state power amplifier on the rover for X band. The rover also has two UHF radios,[31] the signals of which the 2001 Mars Odyssey satellite is capable of relaying back to Earth. An average of 14 minutes, 6 seconds will be required for signals to travel between Earth and Mars.[41] Curiosity can communicate with Earth directly at speeds up to 32 kbit/s, but the bulk of the data transfer should be relayed through the Mars Reconnaissance Orbiter and Odyssey orbiter. Data transfer speeds between Curiosity and each orbiter may reach 2000 kbit/s and 256 kbit/s, respectively, but each orbiter is able to communicate with Curiosity for only about eight minutes per day (0.56% of the time).[42] Communication from and to Curiosity relies on internationally agreed space data communications protocols as defined by the Consultative Committee for Space Data Systems.[43]

## Initial Plans

- Build a few modules:
  - `curiosity-rover`: a transponder
  - `MRO`: a node to pass data through, which limits data flow on either side
  - `2001-mars-orbiter`: a slower node to pass data through
  - `earth`: where to spend messages from
- Enable space cats to be sent from earth, through the other two, to the Curiosity rover.
- Enable them to be sent back.
- Put in delays based on average times.
- Put in delays based on current positioning of the planets.
  - Side Note: https://code.nasa.gov is a beautiful website.

Send the details over the SCPS-FP protocol. See [CCSDS](https://public.ccsds.org/Publications/default.aspx).

## Questions

### What is an X band transmitter?

From [Wikipedia](https://en.wikipedia.org/wiki/X_band):

> The X band is a segment of the microwave radio region of the electromagnetic spectrum. In some cases, such as in communication engineering, the frequency range of the X band is rather indefinitely set at approximately 7.0 to 11.2 GHz. In radar engineering, the frequency range is specified by the IEEE at 8.0 to 12.0 GHz.

Looks like I don't want to simulate this, or can't without hardware. I want to see if I can do a simulation just using software, so I don't know if this will be necessary.

Can I buy one? Very no. [$325,000](http://www.sst-us.com/shop/satellite-subsystems/ttc/x-band-downlink-transmitter)

### What is a UHF Electra-Lite software-defined radio?

I know what **UHF** is, besides the movie.

> Ultra high frequency (UHF) is the ITU designation for radio frequencies in the range between 300 MHz and 3 GHz, also known as the decimetre band as the wavelengths range from one meter to one decimetre.

**Electra-Lite** seems to be related to [Electra](https://en.wikipedia.org/wiki/Electra_(radio)):

> Electra, formally called the Electra Proximity Link Payload, is a telecommunications package that acts as a communications relay and navigation aid for Mars spacecraft. The use of such a relay increases the amount of data that can be returned by two to three orders of magnitude.
>
> The ultimate goal of Electra is to achieve a higher level of system integration, thus allowing significant mass, power, and size reductions, at lower cost, for a broad class of spacecraft.

That makes sense. The hard part will be modelling the transceiver:

> Transceiver that runs the free open source RTEMS operating system.

A quick google found that [**RTEMS**](https://en.wikipedia.org/wiki/RTEMS) is "Real-Time Executive for Multiprocessor Systems", a free and open source OS. I could try and model this in javascript. Might be easier to start from less complex and go from there, though. [More on RTEMS](https://www.rtems.org/).

[**Software-defined Radio**](https://en.wikipedia.org/wiki/Software-defined_radio) seems to be another book; this could all be modelled, as it isn't dependent on any hardware.

> Software-defined radio (SDR) is a radio communication system where components that have been typically implemented in hardware (e.g. mixers, filters, amplifiers, modulators/demodulators, detectors, etc.) are instead implemented by means of software on a personal computer or embedded system.

### Can I model the orbiters, too?

I don't see why not.

#### [2001 Mars Odyssey](https://en.wikipedia.org/wiki/2001_Mars_Odyssey)

Much worse telecommunications.

#### [Mars Reconnaissance Orbiter](https://en.wikipedia.org/wiki/Mars_Reconnaissance_Orbiter)

Telecommunications:

> The Telecom Subsystem on MRO is the best digital communication system sent into deep space so far and for the first time using capacity approaching turbo-codes. The Electra communications package is a UHF software-defined radio (SDR) that provides a flexible platform for evolving relay capabilities.[52] It is designed to communicate with other spacecraft as they approach, land, and operate on Mars. The system consists of a very large (3 metres (9.8 ft)) antenna, which is used to transmit data through the Deep Space Network via X-band frequencies at 8 GHz, and it demonstrates the use of the Ka band at 32 GHz for higher data rates. Maximum transmission speed from Mars is projected to be as high as 6 Mbit/s, a rate ten times higher than previous Mars orbiters. The spacecraft carries two 100-watt X-band amplifiers (one of which is a backup), one 35-watt Ka-band amplifier, and two Small Deep Space Transponders (SDSTs)

On this, [Turbo Codes](https://en.wikipedia.org/wiki/Turbo_code) are interesting, because they're a bit future proof. Would be fun to implement these. Never realised I'd stray anywhere near the [Viterbi algorithm](https://en.wikipedia.org/wiki/Viterbi_algorithm) again, but here I am.

### What is a transponder?

[Wikipedia](https://en.wikipedia.org/wiki/Transponder):

> In telecommunication, a transponder is one of two types of devices. In air navigation or radio frequency identification, a flight transponder is an automated transceiver in an aircraft that emits a coded identifying signal in response to an interrogating received signal. In a communications satellite, a satellite transponder receives signals over a range of uplink frequencies usually from a satellite ground station, amplifies them, and re-transmits them on a different set of downlink frequencies to receivers on Earth, often without changing the content of the received signal or signals.

> The term is a portmanteau for transmitter-responder.

### Why 14 minutes, 6 seconds?

Speed of radio, which is an electromagnetic wave, which travels near to the speed of light. From a [blog](http://blogs.esa.int/mex/2012/08/05/time-delay-between-mars-and-earth/):

> During Curiosity EDL, this delay will be 13 minutes, 48 seconds, about mid-way between the minimum delay of around 4 minutes and the maximum of around 24 minutes.
> ...
> At ESOC, we talk about two different times – Spacecraft Event Time (SCET) and Earth Received Time (ERT). The former is what's actually happening at Mars right now, although we won't hear about it until over 13 minutes later, a time we call ERT.
> The delay between the two is usually called the One-Way Light Time (OWLT) and the time for a message to go to Mars and come back is the Two-Way Light Time (TWLT), or round-trip time.

The variability in distance, of course, is due to positioning of Mars comparative to Earth in the solar system (which is not stable). Would be good to get this detail from a simulator to include.

### How can I cap speeds at 32 kbit/s? Why is this the limit?

Yes. Try using [`throttle`](https://www.npmjs.com/package/throttle).

Probably different due to the different hardware. But I don't know!

### Why are the speeds different between the Mars Reconnaissance Orbiter and the Odyssey orbiter?

Different transceivers. See above.

### Can I model the limited time window, too?

Probably. Depends on finding a good space simulation HTTP endpoint (or building your own), and on having a constant feed running.

### Which protocols do they use to communicate, exactly?

Send the details over the SCPS-FP protocol. See [CCSDS](https://public.ccsds.org/Publications/default.aspx).

### Has anyone else done this?

No! But, however, there is someone else working on this _right now_. See:

- https://www.npmjs.com/search?q=mars+rover
- https://github.com/WoodyWoodsta/mars-rover
- https://github.com/WoodyWoodsta/mars-rover-rce

I've pinged him here: https://github.com/WoodyWoodsta/mars-rover/issues/1.

## Contribute

Are you as curious as I am? Feel free to join in. Message me, or [open an issue](//github.com/RichardLitt/curiousity/issues/new).

## License

Code: [MIT](https://opensource.org/licenses/MIT) © 2016 Richard Littauer.
Documentation: [CC-NC-BY-SA 4.0 Unported](https://creativecommons.org/licenses/by-nc-sa/4.0/) © 2016 Richard Littauer.
