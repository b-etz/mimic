# mimic
Smart card reader, writer, and emulator for a specific, undisclosed line of value cards

## About This Repository
This repository is the result of analyzing and reverse engineering a proprietary contact card communication protocol in active use in the wild for value transfer at vending kiosks. The interface is only compliant with parts 1 and 2 of ISO7816, and relies heavily on security through obscurity. Documentation in this README includes light protocol information, with more detail (flow charts and software) contained within.

It is unlikely this system will be revised because of these findings. The manufacturer's own documentation about their affected systems appears to be 10-20 years old. To avoid harm to owners of affected deployments, the specific brand and application will not be published here. However, to maintain the educational value of this repository, all other details of the protocol and testing hardware will be. Example data may also be redacted or randomized to prevent identifying card (or site) serial numbers.

## Disclaimers
Please see the repository's license agreement for information on warranty, and liability, disclaimers.

## Background Info (unaffiliated)
[Laundry Card hack - Hak5 Forums](https://forums.hak5.org/topic/10817-laundry-card-hack/)

[ISO7816 Smart Card Standard (Parts 1 and 2)](https://cardwerk.com/iso-7816-smart-card-standard/)

## Tools Used In This Process
Saleae Logic Pro 8 (Bus Pirate also usable)

## Recommendations for Operators
To the extent possible, actively monitor your value transfer equipment for suspected 
