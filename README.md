# Initial setup

## KYW (Know Your Hardware)

> Started from [this](https://github.com/David-OConnor/stm32-hal/tree/main/examples/blinky) guide without KYW.

0. my chip - `SMT32 F103C8T6` - [source](https://www.st.com/en/microcontrollers-microprocessors/stm32f103c8.html)
1. it's Arm 32-bit Cortex-M3 CPU core 
2. Lookup [here](https://docs.rust-embedded.org/book/intro/install.html) 
3. => `rustup target add thumbv7m-none-eabi`
5. `touch memory.x`
6. the board has `64 or 128 Kbytes of Flash memory ; 20 Kbytes of SRAM`
7. mine has 64KB due to `8` in the name
8. `Memory map` in datasheet contains values for ORIGIN
9. fill the memory.x


# Make a project

> Somewhere in-between I switched to [this guide](https://github.com/stm32-rs/stm32f1xx-hal#setting-up-your-project) - it support my board.

10. `cargo init --name blanky .`
11. add deps to Cargo.toml
12. put https://github.com/stm32-rs/stm32f1xx-hal/blob/master/.cargo/config.toml
13. copy https://github.com/stm32-rs/stm32f1xx-hal/blob/master/examples/blinky.rs to main.rs

# Debugging

14. `cargo install probe-rs-tools`
15. blow the dust off the Flipper Zero
16. connect DAP (flipper -> STM32F103): 9 -> 3V3, 10 -> SWCLK, 11 -> GND, 12 -> SWO
17. install DAP Link to flipper, lauch, change DAP pins to 10,12
18. `cargo run` - should run your code on flipper

## Fancy
19. install probe-rs debugging extension to zed
20. make the .zed/debug.json
21. `probe-rs dap-server --port 50000`
22. run the debug task in Zed

# Scratch that

8. go to https://github.com/David-OConnor/stm32-hal/blob/main/Cargo.toml
9. find `F103` - not supported
10. claude told to use another crate: added it to deps
11. switch to https://github.com/stm32-rs/stm32f1xx-hal#quick-start-guide
12. update Cargo.toml from the guide

No way to flush the binary - need a hardware

Thankfully, I have arduino nano. Here's connection guide:
- 5V and GND - connect the STM32 and arduino nano to each other
- connect GND and RST on arduino nano - this makes arduino a USB-UART adapter
- connect arduino RX to STM32 A10 and TX to A9 - that's UART
- connect arduino to type C
- press RESET on STM32
- run `sudo stm32flash /dev/ttyUSB0` to check connection

> this doesn't work for me due to pure connectivity
