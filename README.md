# mimic
Smart card reader, writer, and emulator for a specific, undisclosed line of value cards

## About This Repository
This repository is the product of analyzing and reverse engineering a proprietary contact card communication protocol actively used in the wild for value transfer at machine kiosks. The interface is only compliant with parts 1 and 2 of ISO7816, and it relies heavily on security through obscurity. Documentation in this README includes light protocol information, with more detail (flow charts and software) contained within. I hope these findings inspire security professionals to investigate the capabilities of their "smart" card systems, and operators to adopt vendors that embrace modern security principles.

It is unlikely that the target system will be revised because of these findings. The manufacturer's own documentation about these systems appears to be about 20 years old. To avoid harm to owners of affected deployments, the specific brand and application will not be published here. However, to maintain the educational value of this repository, all other details of the protocol and testing hardware will be. Captured data may be randomized to prevent identification of the card (or site) serial number.

## Licenses and Disclaimers
This project is licensed under the [MIT License](LICENSE).

The hardware is licensed under the [CERN-OHL-P version 2](https://ohwr.org/cern_ohl_p_v2.txt).

Please see both license agreements for information on warranty, and liability, disclaimers. As with all tools for hardware reverse engineering, act on this information only with the written consent of the owner(s) of the target equipment.

## Background Info (unaffiliated)
[Laundry Card hack - Hak5 Forums](https://forums.hak5.org/topic/10817-laundry-card-hack/)

[ISO7816 Smart Card Standard (Parts 1 and 2)](https://cardwerk.com/iso-7816-smart-card-standard/)

## Tools Used In This Process
Soldering iron and proto boards, along with the parts listed in the Mouser BOM

Saleae Logic Pro 8 (Bus Pirate also usable, but may lack analog oscilloscope features)

Saleae Logic software (Linux AppImage)

[Adafruit Feather RP2350](https://www.adafruit.com/product/6130)



## Recommendations for Operators
Migrate from legacy coin systems to smart card systems that utilize asymmetric cryptography. Get written guarantees of the cryptography in use before agreeing to a site installation. Words like "security", "advanced", and "proprietary" are meaningless without more information, and are certainly not interchangeable with "cryptographic" or "encrypted". Be direct. Ask vendors what information is "transmitted unencrypted" between the interface and the card. If they don't give you a straight answer, move onto someone better.

If migration is impossible, actively monitor your value transfer equipment for suspected tampering. This may include periodic surveillance reviews. Don't wait for audit time. Speaking of which...

Perform audits of cash flows with your vending equipment, with enough granularity to summarize transactions by equipment and by card number. Replace end-of-life equipment with models capable of performing these audits.
