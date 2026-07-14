---
title: "Surge"
author: "DynamicWhiteHat"
description: "A Low-Cost Dysphagia Patch for Post-Stroke Patients"
created_at: "2026-07-13"
---

# July 13th: Initial Research and First Schematic

### Research

Today, I began my project by doing some research into the issue. I found that post-stroke patients can find themselves facing difficulties swallowing, a condition known as dysphagia. This can lead to *silent aspiration*, which is when food enters the airway without choking or other visible signs, and is very dangerous. As a result, patients struggling with dysphagia typically have recurring tests conducted to track their muscle condition and ability to swallow, with individual tests costing upwards of $2000. New research into surface electromyography (sEMG) shows that ICs like the ADS1292 are capable of performing high-quality sEMG analysis. Coupled with the AD5941 bioelectric measurement IC, a PCB could effectively measure a patient's capability to swallow. This measurement would be non-invasive, 24/7, and low-cost, which covers many problems associated with current testing methods. As such, I decided to create a PCB with these ICs.

### Schematic Design

I began by creating a base schematic for an ESP-32 S3 SoC. I have created many boards with ESP-32 modules, but never just the bare SoC before. I found [this video]([url](https://www.youtube.com/watch?v=yxU_Kw2de08)) from Phil's Lab that covers exactly what I needed. I just pulled up the datasheets for the ESP-32 S3 since he used a C3, made some minor changes, and ended up with the following:

| | |
|---|---|
| <img width="827" height="692" alt="image" src="https://github.com/user-attachments/assets/f0708e9e-0f63-4cb5-a411-13ae308d0f71" /> | <img width="597" height="640" alt="image" src="https://github.com/user-attachments/assets/53ec4fea-67b7-4773-910c-c3562510e08d" /> |
| <img width="1162" height="641" alt="image" src="https://github.com/user-attachments/assets/aa9660d2-8c76-427d-87b3-89552752eb9a" /> | <img width="757" height="317" alt="image" src="https://github.com/user-attachments/assets/507a52a2-0504-4690-a9b1-2f7e42b6482c" />|

At the core is an ESP-32 S3 R8 coupled with a 16 MB flash chip. The battery circuit, mostly taken from my [custom ZMK keyboard]([url](https://github.com/DynamicWhiteHat/Arc/)), features battery charging and USB switchover, as well as a physical power switch and a battery gauge. The antenna is a simple CLC circuit attached to an AN043 antenna, which has been tested and proven to work by Ti. I then added the AD5941 IC. This is a very sensitive analog device that is going to require special routing to ensure the signals are kept safe. I looked at reference designs online and came up with this:

<img width="676" height="642" alt="image" src="https://github.com/user-attachments/assets/91c87434-dd2e-4cd6-801e-1e83d1acc5b1" />

Next, I set up my board for a 4-layer design, following JLCPCB's specifications for their JLC04161H-7628 stackup, which is cheap and good for impedance control. I used KiCad's built in calculator to get a track width of 0.3812 mm for a 50 ohm impedance trace for the antenna.

**Total time spent: 4 hours**
