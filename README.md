# mimic
Smart card reader, writer, and emulator for a specific, undisclosed line of value cards

## About This Repository
This repository is the result of analyzing and reverse engineering a proprietary contact card communication protocol in active use in the wild for value transfer at vending kiosks. The interface is only compliant with parts 1 and 2 of ISO7816, and relies heavily on security through obscurity. Documentation in this README includes light protocol information, with more detail (flow charts and software) contained within.

It is unlikely this system will be revised because of these findings. The manufacturer's own documentation about their affected systems appears to be 10-20 years old. To avoid harm to owners of affected deployments, the specific brand and application will not be published here. However, to maintain the educational value of this repository, all other details of the protocol and testing hardware will be. Example data may also be redacted or randomized to prevent identifying card (or site) serial numbers.

## Liability Disclaimer
The information shared here has been cleansed of identifying information to the furthest extent possible. To the reader, this information must be used only to legally interact with hardware you own. The owner(s) of, and all contributor(s) to, this repository are not responsible for any injury or harm, including but not limited to physical and monetary damages, which arise from unauthorized use of any of the contents of this repository. There are always personal safety concerns when interacting with energized equipment. This repository and its contents are provided AS IS, with no guarantee of user personal safety or the integrity of their hardware. By using the software, hardware, and information within, you agree to these terms and the repository's license.

## Background Info (unaffiliated)
[Laundry Card hack - Hak5 Forums](https://forums.hak5.org/topic/10817-laundry-card-hack/)
[ISO7816 Smart Card Standard (Parts 1 and 2)](https://cardwerk.com/iso-7816-smart-card-standard/)

## Tools Used In This Process
Saleae Logic Pro 8 (Bus Pirate also usable)

## Recommendations for Operators
To the extent possible, actively monitor your value transfer equipment for suspected 
