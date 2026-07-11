# STM32 Basic PWM

Basic example of setting up a PWM output.

## PWM Frequency

The PWM frequency is determined by:
- Timer clock (f_TIM) (APB1 or APB2)
- Prescaler (PSC), divides down the timer's input clock before it reaches the counter
- Auto-reload register (ARR)

Formulae:

$f_{counter} = \frac{f_{TIM}}{PSC+1} \quad \quad \text{(Counter Clock)} $

$f_{PWM} = \frac{f_{TIM}}{(PSC + 1)(ARR+1)} = \frac{f_{counter}}{ARR+1}$

---
## Setup Example: 500 Hz PWM (TIM3)

Assume:
- Timer: TIM3  
- Timer clock: 90 MHz (APB1 timer clock)

### Configuration

- Prescaler:
PSC = 90 - 1

- Auto-reload (Period):
ARR = 2000 - 1

## Resulting Frequency

$f_{counter} = \frac{90MHz}{89+1} = 1MHz \quad \quad \text{(1e6 ticks per second)} $

$f_{PWM} = \frac{1MHz}{1999+1} = 500MHz$

## Duty Cycle

The duty cycle is controlled by the compare value.

Range:
0 → ARR (0 → 1999)

### Examples

| Duty Cycle | Compare Value |
|------------|--------------|
| 0%         | 0            |
| 25%        | 500          |
| 50%        | 1000         |
| 75%        | 1500         |
| ~100%      | 2000         |

## Example Code
``` C
HAL_TIM_PWM_Start(&htim3, TIM_CHANNEL_3);

// Set 25% duty cycle
__HAL_TIM_SET_COMPARE(&htim3, TIM_CHANNEL_3, 500);
```
## Key Idea

ARR = total period  
Compare value = time signal is HIGH  

Duty Cycle = compare / ARR

## Notes

- Maximum compare value is ARR (1999), not 2000  
- compare = 0 → always LOW  
- compare ≈ ARR → almost always HIGH  
