# Custom-Macropad</br>
This is my documentation of designing and creating my very own macropad. </br>

# Tools used</br>
PCB Design software: Kicad</br>
Schematic Design software: Kicad</br>
CAD Modeling software: Solidworks</br>

# Planning </br>
I stared out with designing what I would want my layout to look like. I laid out my components I needed and put them on the schematic editor. This is my first time designing a pcb.</br> 
I love macro pads and own an led one from Pimoroni. I also enjoy customizing my hot swappable mechanical keyboard with new keycaps, switches and different themes. </br>

## Planning My Components and Layout</br>

Before I opened KiCad at all, I sat down and figured out what I actually wanted this macropad to work. I knew I wanted it small. A macropad that sits next to my keyboard.</br>

Here's what I landed on:</br>

- **Controller:** Adafruit KB2040 (the pink one!). It runs on the RP2040, and to keep the board small I'm putting it on the **back** of the PCB instead of making the board bigger to fit it on the front.</br>
- **Switches:** 12 KeyGeek Y2X pink linear switches. They're MX-style, and I'm using **Kailh hotswap sockets**, so I can swap switches later without desoldering anything. If you know me and my keyboard, you know I love customizing it and adding new switches and keycaps.</br>
- **Diodes:** a 1N4148W diode for every key.</br>
- **Lighting:** per-key RGB with Adafruit's reverse-mount NeoPixels (SK6812MINI-E). They go on the back of the board and shine up through cutouts under each switch. I love lights and LEDs, so this part was non-negotiable.</br>
- **Display:** a 240x240 ST7789 SPI screen (7-pin), mounted flat on a socket so I can pop it off if I need to.</br>
- **Encoder:** I really wanted a **roller wheel** and not a regular knob, so I went with the Panasonic EVQWGD001. It's the kind of scroll wheel you'd find on a mouse or an old phone.</br>

| Components used |  |
|---|---|
| <img width="657" height="650" alt="Screenshot 2026-11-16 002729" src="https://github.com/user-attachments/assets/c07e0f87-6c53-4e9d-b387-0e247c759384" /> | <img width="645" height="458" alt="Screenshot 2026-11-16 002748" src="https://github.com/user-attachments/assets/cb39706b-905f-40ef-a84c-e88e8767e5f6" /> |
| <img width="644" height="591" alt="Screenshot 2026-11-16 002803" src="https://github.com/user-attachments/assets/d232edf2-b1c6-4014-bcf2-8610965fa45e" /> | <img width="1344" height="1083" alt="Screenshot 2026-11-16 002852" src="https://github.com/user-attachments/assets/a6b42d96-3946-49c9-93ea-8512d8738060" /> |
| <img width="1138" height="462" alt="Screenshot 2026-11-16 003514" src="https://github.com/user-attachments/assets/63a9a7dd-8310-4bc1-95e0-3ed7ddf95bc3" /> | x |


For the layout, I got all my real keycaps out with the display and mocked it up by hand to see what felt right. I ended up with a **4 x 3 grid of keys on the right**, the **display on the left**, and the **encoder above the display** near the top edge. The board comes out to about **123.5 x 65.7 mm**. However, I am still deciding on the final size.</br>

Mocking it up with real parts helped so much. Things look very different on paper than when they're sitting in front of you.</br>

<img width="4032" height="1537" alt="IMG_6124" src="https://github.com/user-attachments/assets/ba34ebba-48f5-49f2-9e66-95952c049c3e" /></br>
</br>
</br>
---</br>
</br>
## Creating My First Schematic</br>

This was my first schematic ever. At my job as a electronics/production technician, I read a lot of schematics and assemble products from them. This is the first time I am creating one from scratch from a personal project. Some of it confused me at first, especially the key matrix, the diodes, and net labels, but once it clicked it made a lot of sense. I watched youtube to learn the new program Kicad.</br>
</br>
**The key matrix:** You don't give every key its own pin on the controller (you'd run out of pins fast). The keys get wired into **rows and columns**, and the controller checks them one at a time to find out which key is pressed. My 12 keys use 4 columns and 3 rows, so only 7 pins instead of 12.

**Why the diodes:** If you press a few keys at once, the signal can sneak backwards through the matrix, and the controller thinks you pressed a key you didn't ( I learned this is called "ghosting"). A diode on each key only lets current go one way, so every key press reads correctly.

**Net labels:** Instead of drawing a wire across the whole page, you can give two points the same label name and KiCad knows they're connected. It keeps the schematic so much cleaner.

Here's how I wired everything to the KB2040:</br>

| What | KB2040 Pins |
|---|---|
| Key columns | D4 – D7 |
| Key rows | D8 – D10 |
| Encoder (A / B / switch) | A0 / A1 / A2 |
| Display SPI | SCK + MOSI, RES on D0, DC on A3 |
| LED data (all 12 chained) | D1 |
<img width="1396" height="958" alt="Screenshot 2026-11-15 193719" src="https://github.com/user-attachments/assets/4d6137d6-4f42-4218-97b9-c79f26c18445" /></br>



I also had to bring in some custom libraries because not every part was built into KiCad:</br>

- **Keyswitch KiCad Library** (from the Content Manager) for the hotswap socket footprints</br>
- **ProMicroKiCad** for the KB2040 footprint</br>
- A community footprint for the **EVQWGD001** roller encoder</br>

The best moment was running the **Electrical Rules Check (ERC)** and having it pass. Every part is wired up and has a footprint, so it's ready to move over to the PCB editor. </br>

<img width="3508" height="2481" alt="micropad 1" src="https://github.com/user-attachments/assets/126a7852-ea08-4c32-a87e-8ced6a853c28" /></br>

</br>
---
</br>
## Designing the PCB</br>
</br>
### Day 1</br>
</br>
Day 1 was all about getting everything off the schematic and onto the board. I brought all my parts into the PCB editor and lined up the switches and LEDs in the same order as my matrix and LED chain, so the routing will make more sense later.</br>
</br>
Seeing all the footprints show up for the first time was so exciting. But it was very very messy. I ended up having to change things around until it looked neat. It finally started to look like a real thing and not just a drawing.</br>

<img width="2448" height="1296" alt="Screenshot 2026-11-16 002354" src="https://github.com/user-attachments/assets/0ce7c48a-7971-4222-831b-416fec9fd1b9" /></br>

</br>
**Next up:**</br>
- Draw the board outline (123.5 x 65.7 mm)</br>
- Place the display socket and the encoder</br>
- Route all the traces</br>
</br>
I'll keep updating this as I go!</br>
