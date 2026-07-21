---
title: "Surge"
author: "DynamicWhiteHat"
description: "A Low-Cost Dysphagia Patch for Post-Stroke Patients"
created_at: "2026-07-13"
---

# July 13th: Initial Research and First Schematic

**Research**

Today, I began my project by doing some research into the issue. I found that post-stroke patients can find themselves facing difficulties swallowing, a condition known as dysphagia. This can lead to *silent aspiration*, which is when food enters the airway without choking or other visible signs, and is very dangerous. As a result, patients struggling with dysphagia typically have recurring tests conducted to track their muscle condition and ability to swallow, with individual tests costing upwards of $2000. New research into surface electromyography (sEMG) shows that ICs like the ADS1292 are capable of performing high-quality sEMG analysis. Coupled with the AD5941 bioelectric measurement IC, a PCB could effectively measure a patient's capability to swallow. This measurement would be non-invasive, 24/7, and low-cost, which covers many problems associated with current testing methods. As such, I decided to create a PCB with these ICs.

**Schematic Design**

I began by creating a base schematic for an ESP-32 S3 SoC. I have created many boards with ESP-32 modules, but never just the bare SoC before. I found [this video]([url](https://www.youtube.com/watch?v=yxU_Kw2de08)) from Phil's Lab that covers exactly what I needed. I just pulled up the datasheets for the ESP-32 S3 since he used a C3, made some minor changes, and ended up with the following:

| | |
|---|---|
| <img width="827" height="692" alt="image" src="https://github.com/user-attachments/assets/f0708e9e-0f63-4cb5-a411-13ae308d0f71" /> | <img width="597" height="640" alt="image" src="https://github.com/user-attachments/assets/53ec4fea-67b7-4773-910c-c3562510e08d" /> |
| <img width="1162" height="641" alt="image" src="https://github.com/user-attachments/assets/aa9660d2-8c76-427d-87b3-89552752eb9a" /> | <img width="757" height="317" alt="image" src="https://github.com/user-attachments/assets/507a52a2-0504-4690-a9b1-2f7e42b6482c" />|

At the core is an ESP-32 S3 R8 coupled with a 16 MB flash chip. The battery circuit, mostly taken from my [custom ZMK keyboard]([url](https://github.com/DynamicWhiteHat/Arc/)), features battery charging and USB switchover, as well as a physical power switch and a battery gauge. The antenna is a simple CLC circuit attached to an AN043 antenna, which has been tested and proven to work by Ti. I then added the AD5941 IC. This is a very sensitive analog device that is going to require special routing to ensure the signals are kept safe. I looked at reference designs online and came up with this:

<img width="676" height="642" alt="image" src="https://github.com/user-attachments/assets/91c87434-dd2e-4cd6-801e-1e83d1acc5b1" />

Next, I set up my board for a 4-layer design, following JLCPCB's specifications for their JLC04161H-7628 stackup, which is cheap and good for impedance control. I used KiCad's built in calculator to get a track width of 0.3812 mm for a 50 ohm impedance trace for the antenna.

**Total time spent: 4 hours**

# July 15th: Final Schematic and Initial PCB Layout

**Schematic**

I had initially planned to make a separate board for the ADS1292 IC, which would be positioned higher on the neck. I got this idea from an AI generated mockup of the device. However, I later decided that placing the IC on the same board and using and FPC cable instead would help me with routing and signal concerns. I imported the IC and placed it into my schematic. Before I started making connections, I decided to add some more safety and protection lines to my AD5941 IC, such as an inductor and an ESD protection IC. This is how it looks now:

<img width="932" height="887" alt="image" src="https://github.com/user-attachments/assets/18ca424a-32ea-4c1b-b195-f8a1db232ba0" />

U9 is a TPD4E001 from TI, which is designed to catch any stray leakage. I also lowered the resistor value from 10K to 1K to allow the signals to be better intercepted, as previously, it would be very difficult for the IC to read signals that have gone through a 10K resistor. Finally, I also added inductors on the end of the connection, right before the line goes out to the electrode, to catch any RF/EMI signals and remove them before they reach the IC and corrupt its readings.

Next, I began working on the ADS1292 IC. Similar to how I did the AD5941, I looked up a reference design online and got to work copying it. I found the reference in the TI documentation for the IC and adjusted it to match my use case. The electrode pins on this IC are IN2N, IN2P, and RLDOUT, which connect to an FPC connector. It also has the same electrode setup, with a resistor, ESD protection, and an inductor. This is how it looks:

<img width="937" height="705" alt="image" src="https://github.com/user-attachments/assets/202837b8-aad9-4506-a280-f08db6f0b9d0" />

**PCB**

I then got to work on the PCB and did some initial layout of the IC "groups," which helps me with later placement. For example, here is the IC group layout for the AD5942:

<img width="342" height="752" alt="image" src="https://github.com/user-attachments/assets/307d8c91-e084-4aef-a0ac-cec111e13340" />

I did that with most of the major parts, then made my Edge.Cuts for the outline. It is a rough estimate of the size and will be edited once I finish placement. I decided to place the antenna on the left side, as that will help keep RF signals away from the analog ICs, which will go on the right. This is my current PCB:

<img width="541" height="797" alt="image" src="https://github.com/user-attachments/assets/934bd13e-df0a-49fc-acc1-e3af74ac380f" />

**Total time spent: 3 hours**

# July 20th: More PCB Work

I did some more PCB work today. I added the buttons in the top right and figured out where to put the power section. I decided to move the ESP32 chip down to accommodate the power stuff towards the top; that way it sort of blends in with the rest of the devices for the USB. Afterwards, I added the analog devices on the board, but then I realized that it didn't look good where it was, so I asked in Slack for help and am still awaiting a response. For now, this is where I have it.

<img width="552" height="791" alt="image" src="https://github.com/user-attachments/assets/80d98088-4927-4bb5-82e8-63776b607e8f" />
