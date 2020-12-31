# Sensor to CAN Project Proposal

## Proposed Block Diagram:
![System Block Diagram](SystemOverview.png?raw=true "System Overview")

## Block Diagram Explanation:
1. Analog/Digital sensors connect to MCU directly
    - Digital is mostly to be safe, we don't currently have any
2. Optional and Configurable Analog Filtering
    - Maybe controllable programmatically? Would be really cool. I think it's possible?
    - High-pass for sure, maybe low-pass?
    - What about smoothing? Probably not necessary.
3. Can be used as a bridge for other CAN sensors (i.e. tire temps)
    - Will this cause latency problems? I don't think so but would be good to test
    
4. MCU - probably either STM32 or ESP32
    - If we can do some testing on ESP32 and confirm that it works well I think it will be the way to go in the future since it's dual-core
        - One thread for getting inputs from sensors at very low latency
        - One thread for CAN comms
5. Use standard MCP2515 CAN setup that we've used elsewhere.
    - Proven to work and no real downsides
6. Onboard 12V-5V regulator with some basic smoothing/filtering
    - Means we have a local 5V reference for our sensors
        - Theoretically means readings will be better, but realistically it probably won't matter
        - Don't need to run 5V from Motec all the way to sensor, just +12V (can ground to chassis close to module)
    - Don't need to worry about introducing noise onto existing 5V busses with our hardware
7. Would be REALLY cool - but not super necessary - if we could configure the whole thing over USB for each application
    - Determine CAN message structure
    - Determine the resolution and scaling with which to send each signal
    - Determine which inputs are actually being used
    - Determine refresh rates
    - etc.
    - I think it should be relatively possible. Might have to add a flash chip to store configs, but my initial guess is that EEPROM should be sufficient
