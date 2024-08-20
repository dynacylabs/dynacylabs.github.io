---
layout: post
title:  "OpenPilot on MK6 Golf"
date:   2023-10-24 00:00:00 +0000
categories: jekyll update
---

# Repository
* The full repository with images and information is available on [Github](https://github.com/dynacylabs/openpilot_on_mk6_golf)

# Acronyms

|------|--------------------------------------------------------------------|
| ACC  | Adaptive Cruise Control                                            |
| LA   | Lane Assist                                                        |
| LKAS | Lane Keep Assist (System)                                          |
| EPS  | Electronic Power Steering                                          |
| CC   | Cruise Control                                                     |
| CEL  | Check Engine Light/Lamp (Light on the cluster indicating an error) |

# Interesting Links
* [Retrofitting ACC to Passat CC](https://www.drive2.com/l/525060508923987417)
* [Reference PDF for Lane Assist](https://www.motor-talk.de/forum/aktion/Attachment.html?attachmentId=751655)
* [Reference PDF](https://www.dropbox.com/s/1v9gxusxpqdxpxt/umruestung-acc-final.pdf?dl=0)

## Translated Reference PDF
### ACC / FRONT ASSIST RETROFITTING GOLF VI GTI MY 2011 [B]

### Hints for conversion:
1. The GRA lever part number 3C9 953 501 BM must be disassembled and installed / replaced in the GRA lever part number 5K0 953 501 CS. It should be noted that all levers are the ATW company, otherwise compatibility is not guaranteed.
2. The cladding of the steering column, of course, no longer fits with the Golf's because of the third lever. Therefore, the lower fairing with part number 5N0 858 566D is required. The upper panel is identical and can still be used. However, this disguise is probably only in the sentence.
3. The steering column control unit from a Passat B7 would fit, also has the required ignition connector, but this is no longer wired to board according to several statements (SMD fuse). Although this can be retrofitted and thus a control unit from a Passat would be suitable. However, this can be circumvented by using the SMLS controller part number 5K0 953 569 AL from a Tiguan / Sharan. Background to this is the following. The Passat has no more ignition connector as in the Golf VI but it runs a lot on CAN or external/extra wires. Since the Golf VI does not have this of course the ZAS plug must work.
4. Conversion ZAS plug. The ZAS connector on the SMLS of the Golf VI has 8 pins. However, the new 5K0 953 569 AL can only accept 6 pins. The plug must be rewired using the overview on the next pages.
5. For the radar there is an adapter cable with the number 1J0 973 715. The line is the 10-pin connector for the radar available and about 1m line. Of course, that's not enough for the interior of the vehicle. Therefore, this line must be extended. Wiring diagram see below.
6. After everything has been connected and installed it goes to the coding ...
7. Of course, an extended gateway is required.
8. Regarding VW emblem and attachment. The VW emblem from the Passat B7 fits freely. So only needs to be replaced. A bracket has been custom made and can be bolted to the front.

Thanks again to the two users [VCP-Forum] "bluemevo" and "jmb123". Without the guys it
would not have gone that way :) THANK YOU !!!

### Components Used:
* Panel 5N0 858 566D below / 5N0 858 565 B above
* Steering column switch (3 lever) from Tiguan, Sharan ... 5K0 953 501 CS (ATW)
* Steering column switch (3 lever) from Passat, 3C9 953 501 BM (steering lever 3C9 953 502 E) (ATW)
* Control unit 5K0 953 569 AL
* Ignition Lock Plug 5K0 905 865 (6-pin)
* Adapter cable Radar with connector 1J0 973 715 (10pin)
* Mounting plate 5K0 953 223 (ATW)
* B7 Radar 3AA 907 567 A HW036/SW0170
* VW Emblem 3D0 853 601 F JZA Passat B7 [3C0 853 601 A Passat B6]

### Pin assignment of radar camera with vehicle: [3AA 907 567 A]

|--------|---------------------------------|
| Radar  | Aim                             |
| 10     | Plus / Ignition SC4             |
| 9/2    | Mass                            |
| 5 LOW  | 7 Gateway                       |
| 4 HIGH | 17 Gateway                      |
| 8      | T16 / 5 steering column (16pol) |

**checked 23.10.2016 i.O**

### Pin assignment SMLS control unit: [5K0 953 569 AL]

|------------------------|------------------------|-----------------------|
| Ignition lock [Black]  | SMLS 8 pin [ALT white] | SMLS 6pin [NEW white] |
| 1                      | 1                      | 5                     |
| 2                      | 3                      | 6                     |
| 3                      | 4                      | 4                     |
| 4 (Free)               |                        |                       |
| 5 (Free)               |                        |                       |
| 6                      | 5                      | 3                     |
| * DSG Key Trigger Lock | SMLS 8 pin [ALT white] | SMLS 6pin [NEW white] |
| 2 [rt+]                | 7                      | 2 [rot]               |
| 1 [bl-]                | 8                      | 1 [blue]              |

**not applicable to vehicles with manual transmission**

## Coding
### Address 01: Engine electronics (CCZ) Label file: 06J-907-115-CCZ.clb
* Part number SW: 5K0 907 115 HW: 1K0 907 115 AA
* Component: 2.0l R4 / 4V TFSI 0050
* Revision: AAH18 --- Serial Number:
* Coding: 0403000C1C070260
  * Coding byte 5; Bit 1 to 0; Bit 2 to 1 (ACC)

### Address 03: Brake electronics (J104) Label file: DRV \ 1K0-907-379-60EC1F.clb
* Part Number SW: 1K0 907 379 BJ HW: 1K0 907 379 BJ
* Component: ESP MK60EC1 H31 0121
* Revision: 00H31001
* Coding: 143B600D092B00FF281006E7901C0041150C00
  * Coding byte 16; Bit 5 to 0

### Address 16: Steering wheel electronics (J527) Label file: DRV \ 5K0-953-569.clb
* Part number SW: 5K0 953 501 CS HW: 5K0 953 569 AL
* Component: STEERING MODULE 016 0140
* Revision: FF010042 Serial number: 20131220300868
* Coding: 389AA40000
  * Coding byte 2; Bit 7 to 1; Bit 4 to 1

### Address 13: Distance control Label file: DRV \ 3AA-907-567.clb
* Part Number SW: 3AA 907 567 A HW: 3AA 907 567
* Component: AC201 RDW A 036 0170
* Revision: 00036000 Serial Number: 00000000902524
* Coding: 0010000
  * Coding 0010000 (DSG)
* Adaptation channels:
  * Brakes deactivated to standstill (only works with newer ABS control units) 
  * Electronic parking brake disabled
  * Procedure Coding: [Login: 201] Activation of the adaptation channels; Unlock in APK2 with login code, coding and locking again.

### Address 19: Diagnostic Interface (J533) Label file: DRV \ 7N0-907-530-V2.clb
* Part Number SW: 7N0 907 530 AA HW: 7N0 907 530 G
* Component: J533 Gateway H41 1632
* Revision: H41 serial number: 080412F6000270
* Coding: 350002
  * Add list to address 13.

!FTODO (Add Images)

**!!Caution!! It is a complex retrofit that a layman does not do should. In particular, it must be mentioned that the airbag must be dismantled. That's why I give no guarantee of completeness and correctness. I have everything documented and best knowledge. For me it worked! There is only a small handful of vehicles for the conversion. Therefore, that said, it's not a universal guide for anyone. I also had a lot of support.**

## Radar Mount
!FTODO (Add Image of Radar Mount)

## Testing Support
### Testing if ECU Supports ACC
1. Open 01-Engine Module
2. Open Security Access – 16, Enter 12233 for Security Access
3. In Coding II – 11, Enter the following codes
    * 16167 – Deactivate Cruise Control
    * 13377 – Activate ACC
    * 30903 – Activate ACC follow to stop (Requires EPB)
4. Open 03-ABS Module
5. Open Coding – 07
6. Open Long Coding Helper
    * Change byte 16, set bit 5 from 1 to 0
      * Need to change binary from 10110111 to 10010111
7. To disable ACC (and turn off CEL that flashes), revert the binary change in step 6, and re-enter the codes from step 3. Under Coding II – 11, Enter 11463 to re-enable factory cruise control.

### Testing for LA AND PLA Support
1. Steering Assist – 44
2. Adaptations
3. Set Lane Assist Installed to 1
    * If Accepted, then EPS Supports LA and PLA

### Testing if New Steering Wheel Controller Is Needed
1. Steering Wheel Controller
2. Change CC Stalk Type to ACC 6 Position Separate Stalk

# Parts Needed
* J533 Sniffer Cable
  * Can order from @jyoung8607 on Discord ($68USD in USA, $96.70USD for Other Countries)
  * Could also build one per [J533 Sniffing Cable](https://community.comma.ai/wiki/index.php/J533_Sniffing_Cable)
* Panda
  * [Panda](https://community.comma.ai/wiki/index.php/Panda)
  * $99 Free Shipping to US (Maybe other countries?)
  * No information on which version is needed. I went with White Panda as it was cheaper.
* EON Device Kit
  * [EON Device](https://comma.ai/shop/products/eon-gold-dashcam-devkit)
    * $599.00, No Comma Harness Needed
* See Parts List at end for part and price breakdown

# Parts Suggested by @Edgy
## CC Stalk
* !FTODO (Add Image)
* 1K0 953 513 H

## Bottom Steering Column Fairing/Panel
* !FTODO (Add Image)
* 5N0 858 566 D

## Radar with Custom Mounting Plate
* !FTODO (Add Images)
* 3AA 907 567 A (SW0170 / HW036)
  * See Page 8 (Drawing from Reference PDF) for Custom Mounting Plate Dimensions
* Radar will need custom mount
  * See PDF for dimensions and possible part number
  * [3d Printable Model](https://www.thingiverse.com/thing:3954928)
    * 3d Printable Model was determined to be too flexible/not sturdy enough. Not recommended

## LKAS Camera
* !FTODO (Add Images)
* 3AA 980 654 D

## LKAS Camera Mirror Mount
* !FTODO (Add Image)
* 5K0 857 511 B

## Project Parts/Costs

| Item                          | Part Number    | Quantity | Unit Cost | Shipping | Line Total |
|-------------------------------|----------------|----------|-----------|----------|------------|
| Steering Column Under Fairing | 5N0 858 566 D  |     1    |  $21.79   |  $15.56  |  $37.35    |
| Stalk                         | 1K0 953 513 H  |     1    |  $85.08   |  $8.51   |  $93.59    |
| LKAS Camera                   | 3AA 980 654 D  |     1    |  $87.04   |  $29.01  |  $116.05   |
| Steering Module Controller    | 1K0 953 549 CD |     1    |  $56.64   |  $21.45  |  $78.09    |
| Radar                         | 3AA 907 567 A  |     1    |  $453.94  |  $13.62  |  $467.56   |
| LKAS Camera Mount             | 5K0 857 511 B  |     1    |  $141.88  |  $11.34  |  $153.21   |
| ACC Harness                   |                |     1    |  $35.06   |  $2.77   |  $37.83    |
| LKAS Harness                  |                |     1    |  $25.69   |  $2.77   |  $28.45    |
| Panda                         |                |     1    |  $99.00   |  $0.00   |  $99.00    |
| Extended CAN Gateway          | 7N0 907 530 AJ |     1    |  $24.58   |  $5.51   |  $30.09    |
| Aluminum Plate                |                |     1    |  $21.24   |  $0.00   |  $21.24    |
| Sniffer Cable                 |                |     1    |  $68.00   |  $0.00   |  $68.00    |
| EON                           |                |     1    |  $599.00  |  $0.00   |  $599.00   |
| Radar Mounting Post           | 3C0 907 179 B  |     1    |  $9.76    |  $0.00   |  $9.76     |
| Shipping to USA               |                |     1    |  $0.00    |  $155.50 |  $155.50   |
| Grand Total                   |                |          |           |          |  $2,267.29 |

**Disclaimer: Prices are USD and are accurate as of around November 2019. Costs not reflected may include import/export taxes and fees.**