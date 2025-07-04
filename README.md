# Mimic
Smart card reader, writer, and emulator for a specific, undisclosed line of laundry value cards

## About This Repository
I spend money to do my laundry. So, like any lawfully paying customer, I like to watch the elves work. But the tiny elves in electronic payment systems are secretive. This repository is the product of analyzing and reverse engineering the proprietary contact card communication protocol used for value transfer at local laundry machine kiosks. Documentation in this README includes protocol discovery details, with exact timings specified in the PIO configuration files (and software documentation) within. I hope these findings inspire IT/security professionals to investigate their smart card systems, and help operators force industry adoption of modern data security principles.

It is unlikely that a responsible disclosure process would lead to the manufacturer revising this card communication protocol. The manufacturer's public documentation for these systems is 20 years old, and there is significant investment $$$ required to replace impacted cards and readers. To avoid harming owners of these deployments, the specific brand name and example appliances will not be published here. However, to maintain the educational value of this repository, all other details of the protocol and testing hardware will be shared. Captured data has been anonymized to prevent identification of card serial number(s) or site code(s).

## Licenses and Disclaimers
This project is licensed under the [MIT License](LICENSE). The hardware is licensed under the [CERN-OHL-P version 2](https://ohwr.org/cern_ohl_p_v2.txt).

Please see both license agreements for information on warranty/liability disclaimers.

## Background Info and Inspiration
[Laundry Card hack - Hak5 Forums](https://forums.hak5.org/topic/10817-laundry-card-hack/)

[ISO7816 Smart Card Standard (Parts 1 and 2)](https://cardwerk.com/iso-7816-smart-card-standard/)

[I, Cat: Laundry Part 2](https://i-cat.blogspot.com/2008/01/laundry-part-2.html)

[Bypassing smart-card authentication and blocking debiting](https://archived.neilpahl.com/site/assets/files/1035/defcon_presentation.pdf)

Laundry services typically use one of three payment methods: classic coin payment (coins go in tray, tray goes in machine, machine go brrr), electronic terminal payment via credit card, or reloadable charge card payment. The first method is straightforward and pretty uninteresting. When payment is put into the machine, it toggles an electrical signal that enables the big **START** button. The second method uses standard payment processors, is always networked, and uses ISO7816 smart card standards with applicable cryptography sections. I have only seen this deployed in laundromats.

The third method is what I decided to peer into, and is what I have access to locally. Although they look like the estranged cousins of credit cards, laundry payment cards obey a limited subset of ISO7816-1, -2 and, rarely, -3. The _exact_ electrical goings-on are not obvious without analysis. These systems are ubiquitous for properties where installations may be 10-20 years old and built at a time when smart cards [weren't so smart](https://www.youtube.com/watch?v=zU3nS-l__PA).

## Tools Used In This Process
Personal laundry card

Packing tape and craft knife

Soldering iron and 2.54mm pitch proto boards, along with the parts listed in the Mouser BOM

Logic analyzer with > 10 MHz sampling frequency and analog inputs (e.g. Saleae Logic Pro 8 and Saleae Logic v2.4.29)

[Adafruit Feather RP2350](https://www.adafruit.com/product/6130) and custom open-drain interface board

## Recommendations for Laundry System Operators
When decommissioning legacy coin systems and migrating to smart card systems, ensure vendors utilize (asymmetric) cryptography. Get written guarantees of the cryptography in use before agreeing to a site installation. Adjectives like "secure", "advanced", and "proprietary" are meaningless without more information, and are certainly not interchangeable with "cryptographic" or "encrypted". Be direct. Ask vendors what information is "transmitted unencrypted" between the interface and the card. If they don't give you a straight answer, look for someone better.

If a vendor swap is impossible, actively monitor your value transfer equipment for suspected tampering. This may include periodic CCTV reviews. Don't wait for audit time. Speaking of which...

Perform audits of cash flows with your vending equipment, with enough granularity to summarize transactions by equipment and by card number. Replace obsolete equipment with models capable of performing these audits.

# Reverse Engineering the Physical Interface and Protocol

Before analyzing any signals, let's consider the user interface. An example debit transaction involves inserting a card into a reader at an available machine, after which the balance is listed on the display and settings are revealed for the user to select. The display alternates between the card balance and the amount due for the chosen cycle. When the user presses **START**, some data is presumably exchanged, the machine starts, and the new, freshly-deducted balance is revealed on the glorious VFD or LED display. Physically, there are no obvious connections to the machines other than power and water.

When crediting the card, the process is similar to a debit. Insert the card, read the balance, select an amount to load, present the payment, wait for some data to be exchanged, then observe the new balance on the display. One thing is immediately obvious about this system: the card's value must be stored in its own memory. Otherwise, there's no reason to have a dedicated terminal for loading value. The value would be stored in a database.

Next were the card contacts themselves. The cards have all 8 pads for standard ISO7816-2 compliance. Smart cards featured in the aforementioned references only use 5 pads: VCC (C1), GND (C5), RST (C2), CLK (C3), and I/O (C7). Is that true here, too? To find out, I used a known working card, a known working value loading terminal, and a known working washer and dryer, along with packing tape and a craft knife. The physical thickness of the card is 0.8 mm, and the card slots have some extra clearance. I used the thin packing tape to cover up individual contacts until something didn't work.

The first target pad was C6, called SPU. Based on ISO7816-3, this programming voltage contact can be used for proprietary signaling by vendors. But it rarely is. Covering C6 with the tape (making sure the result was flush and could not separate in the reader), I completed a standard washer transaction without issues. How about knocking out C4 and C8, too, seeing as that's so common? Taping them both presents the user with an **Err** message on the display before the balance can appear. Interesting. Just C8? Still **Err**. Just C4? Works like a charm, and now my clothes are dry. So C1, C2, C3, C5, C7, and C8 are required for debiting.

Lastly, to make sure the card crediting process uses the same pads, I kept C6 and C4 covered with tape when loading $10 from the value transfer terminal. No problems at all. Some inexpensive tape saved a lot of time and money that would have been spent trying and failing to reproduce behavior with a standard ISO7816 reader.

Using a multimeter on the resistance setting, no obvious resistive connections or shorts were observed between any of the card pads. The next step was to record an exchange to understand the voltage levels and signal patterns involved. This testing is critical because an unknown interface might deliver VCC below 5V. Plugging the card into a standard reader might just kill the card. Listening to a transaction also tells us more specifically:
* Whether we can pick out a known protocol from the recording
* Whether each lane is open-drain or push-pull
* Whether one or both sides communicate on a lane

Other posts linked above have used bodge wires or copper tape soldered to the card contacts. I wanted to avoid that because of the risk of permanent card damage. What about an interposer? Smart cards are about 0.8 mm thick (standard half-thickness PCB), and they are a reasonable size for low-cost fabrication. The breakout PCB in this repo [link TBD](tbd) uses a socket to connect either directly to a control board or via a ribbon cable. THe first test involved plugging the dummy board into a machine with only the logic analyzer connected (in 5V mode).

This interface is only compliant with parts 1 and 2 of ISO7816: the card shape/size and contact placement. Specifically, I observed the following differences from ISO7816-3:
