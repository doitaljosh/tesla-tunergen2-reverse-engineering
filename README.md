Tesla HARMAN Tuner

## Overview:
The HARMAN tuner is an ARM Linux device based on the TI Jacinto 5 platform. It is used in the Model 3/Y and connects to the ICE via 100base-T1 ethernet.

## Hardware:
- SoC: TI DRA604 Jacinto 5 (automotive grade AM335x)
- RAM: Micron 128MB DDR2
- ROM: Micron 256MB NAND
- MCU: Renesas RL78 (used for temperature monitoring and power management)
- Ethernet PHY: Marvell 88Q1010 connected to CPSW0
- Tuner baseband: NXP SAF3600EL
- Tuner DSP: NXP SAF7750HN
- Tuner RF front-end: TEF6686HN

## Software architecture:
### Audio stream:
                         Tuner side                           |                         ICE side                           
snd_soc_tunergen2-->ntg6csb_audiorouting-->avbcontroller-->ethernet-->avb_streamhandler-->alsabridge-->audioweaver-->audiod
### Command stream:
             ICE side            |                   Tuner side                       
QtCarRadioServer-->vsomeipd-->ethernet-->vsomeipd-->TunerApp-->spi_dab_plugin-->spidev
