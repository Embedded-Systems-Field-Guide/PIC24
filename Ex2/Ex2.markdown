# Exercise 2: Debounced Button Input with LED Toggle

## Scope
Implement robust button reading with hardware debouncing to control multiple LEDs, demonstrating real-world input handling.

## Assignment
1. **Configure:**
   - RB0 and RB1 as inputs.
   - RB2 and RB3 as outputs for LED control.
2. **Detect button presses (active-low):**
   - Implement button pushes to function as toggle switches.
   - Reflect presses on inputs to corresponding outputs.

## Debouncing Algorithm Requirements
- Sample input every 10ms for 4 consecutive lows.
- Reject pulses shorter than 50ms.
- Wait for button release before next detection.

## Circuit Considerations
The development board should include hardware debouncing solutions:
- External 10kΩ pull-up resistor (optional with internal pull-up).
- 100nF capacitor across button for hardware debouncing.

## Guide and Hints
The goal is to transform the behavior of RB0 and RB1 from push buttons to toggle switches using software.

### Initial Setup
To begin, implement a basic setup to ensure the system is functioning:

```c
#ifndef FCY
#define FCY 4000000UL
#endif

#include "p24FJ64GA002.h"
#include "libpic30.h"
```

This setup enables the following delay commands:
- `__delay_ms(delay);`
- `__delay_us(delay);`

Start with a simple test in the main loop to verify functionality:

```c
AD1PCFG = 0xFFFF; // Disable analog functions (1=digital, 0=analog)
while(1) {
    LATBbits.LATB3 ^= 1;
    __delay_ms(500);
}
```

This code creates a basic blinking LED with a 1-second period on RB3. This is not part of the assignment but serves as a sanity check to confirm the setup is working.

### Implementation Approach
To achieve the toggle switch functionality:
- Store the state of the input pins (RB0 and RB1) and track changes using a counter.
- For each input state (determined by the pull-up or pull-down resistor setup), check if the state remains consistent for a specified number of cycles.
- If the state is stable (e.g., 4 consecutive lows), toggle the corresponding LED and wait for the button to return to its default state before detecting the next press.
- Use the debouncing requirements (10ms sampling, 50ms pulse rejection, wait for release) to ensure reliable input detection.

Focus on tracking state changes and implementing the debouncing logic without directly polling the pins continuously. Consider using variables to store previous states and counters to verify stability over time.