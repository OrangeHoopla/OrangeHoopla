---
title:  "Circuit Board Business Card Design"
date:   2021-06-10
categories: [Business Card]
tags: [pcb]
---


<h2>Constraints</h2>

As of when this was written [JLCPCB][jlc] allows for a black silk screen with their SMT assembly, which when I had first created this was not that case. Alongside their library having a limited amount of ICs for this project.

<h2>NFC chip</h2>

the [NT3H1101W0FHKH][nfc] is a NFC(Near Field Communication) tag, to my knowledge the only available one that is a SMD.
The specs are
Operating frequency: `13.56 MHZ`
Data Transfer: `106 kbits/s`
Endurance : `500,000 cycles`
Memory: `64 bytes SRAM`
		`1904 bytes read/write`
I2C(future interface with micro controller?)

<h2>Antenna Design</h2>

[frequency formula](/images/business_card/form.gif)
we can assume the capacitance level of 50 pF as given by the data sheet under 13. Characteristics
[final](/images/business_card/fin.png)
giving us a value of 2.75 μH needed for the antenna
at this point antenna design is a bit beyond myself so i used [ST][ant] to finish the design

<h2>Eagle Design</h2>

[chipper](/images/business_card/chip.png)
Thankfully the footprint for the [NT3H1101W0FHKH][nfc] was already completed online.

[anten](/images/business_card/antenna.png)
completed through the linked tool earlier.

and for the unique traces on the card [GF Williams][gf] made a amazingly useful tool available online
[eagle](/images/business_card/eagle.png)
[render](/images/business_card/render.png)

<h2>Manufacturing</h2>

[me](/images/business_card/jlc.png)

<h2>Final Product</h2>

[front](/images/business_card/front.jpeg)
[back](/images/business_card/back.jpeg)

[gf]: https://gfwilliams.github.io/svgtoeagle/
[ant]: https://eds.st.com/antenna/#/
[jlc]: https://jlcpcb.com/
[nfc]: https://www.mouser.fr/datasheet/2/302/phgl_s_a0001024329_1-2279506.pdf