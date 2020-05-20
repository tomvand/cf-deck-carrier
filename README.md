# Crazyflie 2 deck carrier
Rev 0.1A

<img src="doc/cf-deck-carrier-front.png" alt="Front" height="400"/>

Schematic: [here](cf-deck-carrier.pdf)

Gerbers: [here](gerber/cf-deck-carrier/)

This carrier board lets the Crazyflie 2 decks by [Bitcraze AB ](https://www.bitcraze.io/) be carried by other drones. The board functions similarly to a breakout board but with additional functions. The board is originally designed for the Eachine Trashcan, but may also be useful for other drones.

Nearly all of the features on this carrier board are optional, unused components can be left out to save weight and assembly time. Refer to the *optional features* section below to check which components should be installed for your application.


## Interface
The carrier board should be connected to an autopilot using the connectors `J1` to `J5`. The connectors are grouped by function. In nearly all cases the board can be connected directly to the corresponding autopilot pins.

<img src="doc/cf-deck-carrier-back.png" alt="Pinouts" height="400"/>

PWR (`J1`):
- VCOM provides power to this board and the decks. VCOM does not need to be regulated.
- VUSB is a power supply *to* the drone, for instance using the Qi charging deck.

UART (`J2`):

SPI (`J3`):
- The IO pins on `J5` are typically used as Chip Select.

I2C (`J4`):

IO (`J5`):
- IO functionality can be replaced by the on-board I2C-to-GPIO and I2C-to-1-Wire bridges for controllers with few free pins.

AUX (`J6`):
- The auxiliary UART is connected directly to either UART1 or UART2.

## Bill of materials
| Ref          | Qnty | Value        | Cmp name                                               | Footprint |
|--------------|------|--------------|--------------------------------------------------------|-----------|
| C1,          | 1    | 1uF          | C                                                      | 0805      |
| C2,          | 1    | 10nF         | C                                                      | 0805      |
| C3,          | 1    | 4.7uF        | C                                                      | 0805      |
| C4, C5,      | 2    | 0.1uF        | C                                                      | 0805      |
| J1,          | 1    | PWR          | Molex PicoBlade 1x3 Horizontal Through-hole 53048-0310 |           |
| J2,          | 1    | UART         | Molex PicoBlade 1x4 Horizontal Through-hole 53048-0410 |           |
| J3,          | 1    | SPI          | Molex PicoBlade 1x3 Horizontal Through-hole 53048-0310 |           |
| J4,          | 1    | I2C          | Molex PicoBlade 1x2 Horizontal Through-hole 53048-0210 |           |
| J5,          | 1    | IO           | Molex PicoBlade 1x5 Horizontal Through-hole 53048-0510 |           |
| J6,          | 1    | UART_AUX     | Molex PicoBlade 1x4 Horizontal Through-hole 53048-0410 |           |
| P1,          | 1    | CF_EXP_LEFT  | BF090-10-X-B2                                          |           |
| P2,          | 1    | CF_EXP_RIGHT | BF090-10-X-B2                                          |           |
| R1, R2, R3,  | 3    | 2.2k         | R                                                      | 0805      |
| SW1,         | 1    | CAS-220      | CAS-220B1                                              |           |
| U1,          | 1    | LP2985-3.0   | LP2985-3.0                                             | SOT-23-5  |
| U2,          | 1    | PCA9536D     | PCA9536D                                               | SOIC-8    |
| U3,          | 1    | DS2484       | DS2484                                                 | SOIC-8    |

## Ordering information
### PCB
- PCB size: 33x28mm
- PCB layers: 4

Approx. PCB manufactoring cost: ~€4.57 / 5 (excl. shipping cost, at JLCPCB.com, estimated by pcbshopper.com on Wednesday May 20th 2020)

Find the gerber files [here](gerber/cf-deck-carrier/).

## Assembly
Recommended soldering order:
1. Passives (C, R)
1. ICs (U)
1. Switch (SW)
1. Headers (P)
1. Connectors (J)

## Optional features
### Molex Picoblade connectors
Components: `J1` to `J6`

<img src="doc/cf-deck-carrier-connectors.png" alt="Connectors" height="200"/>

The PCB is designed for through-hole Molex Picoblade connectors. Unused connectors can be left out to save weight. Wires can also be soldered directly to the connector pads for permanent connections.

### Voltage regulator
Components: `U1`, `C1`, `C2`, `C3`

<img src="doc/cf-deck-carrier-regulator-sch.png" alt="Schematic" height="200"/><img src="doc/cf-deck-carrier-regulator.png" alt="Regulator" height="200"/>

The carrier board includes a voltage regulator that supplies 3.0V at max 150mA. Input voltage VCOM can be ≤16V, **although compatibility with the crazyflie decks should be checked!**

For autopilots that already supply regulated 3.0V on VCOM, the voltage regulator can be replaced by a solder bridge between VCOM (pad 1) and VCC (pad 5) of `U1` (marked by white line).

<img src="doc/cf-deck-carrier-regulator-marking.png" alt="Solder bridge marking" height="200"/>

### I2C Pull-ups
Components: `R1`, `R2`

<img src="doc/cf-deck-carrier-pullups-sch.png" alt="Schematic" height="200"/><img src="doc/cf-deck-carrier-pullups.png" alt="Pull-up resistors" height="200"/>

The board has space for I2C pull-up resistors (0805) (`R1`, `R2`) if these are not present on the autopilot.

### I2C-to-GPIO
Components: `U2`, `C4`

<img src="doc/cf-deck-carrier-gpio-sch.png" alt="Schematic" height="200"/><img src="doc/cf-deck-carrier-gpio.png" alt="I2C-to-GPIO bridge" height="200"/>

For drones with a limited number of free pins, the I2C-to-GPIO bridge `U2` can be installed to drive the decks' IO pins. Since most of the IO pins serve as Chip Select for SPI communication, software drivers should take care that the I2C/GPIO operation is completed before SPI communication begins.

The IO inputs on the `IO` connector should not be used if the I2C-to-GPIO bridge is in use.

### I2C-to-1-Wire
Components: `U3`, `R3`, `C5`

<img src="doc/cf-deck-carrier-ow-sch.png" alt="Schematic" height="200"/><img src="doc/cf-deck-carrier-ow.png" alt="I2C-to-1-Wire bridge" height="200"/>

For drones with a limited number of free pins, the I2C-to-1-Wire bridge `U3` can save another pin by routing 1-Wire data over the existing I2C lines. Note that most decks only use 1-Wire for identification and that not all autopilots support this feature. (Paparazzi, for instance, does not.)

The OW input on the `IO` connector should not be used if the I2C-to-1-Wire bridge is installed.

### Auxiliary connector
Components: `J6`, `SW1`

<img src="doc/cf-deck-carrier-sw-sch.png" alt="Schematic" height="200"/><img src="doc/cf-deck-carrier-j6-sch.png" alt="Schematic" height="200"/><img src="doc/cf-deck-carrier-aux-sw.png" alt="I2C-to-1-Wire bridge" height="200"/><img src="doc/cf-deck-carrier-aux-j6.png" alt="I2C-to-1-Wire bridge" height="200"/>

The AUX connector `J6` can be used to connect another board to the carrier deck. The AUX connector provides unregulated power (VCOM) and a UART bus. Using switch `SW1`, the auxiliary UART can be connected to UART1 or UART2, depending on the decks that are in use.

For permanent installations, switch `SW1` can be replaced by solder bridges between the relevant pads.

<img src="doc/cf-deck-carrier-aux-sw-marking.png" alt="I2C-to-1-Wire bridge" height="200"/>
