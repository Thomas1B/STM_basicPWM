# STM32 Basic PWM

Basic example of setting up a PWM output.

## PWM Frequency

The PWM frequency is determined by:
- Timer clock (f_TIM) (APB1 or APB2)
- Prescaler (PSC)
- Auto-reload register (ARR)

Formula:

$f_{PWM} = \frac{f_{TIM}}{(PSC \cdot ARR)}$

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

**Note:**
**In STM32CubeMX, enter value - 1 because the counter starts at 0.**

## Resulting Frequency

$f_{PWM} = 90,000,000 / (90 \cdot 2000) = 500 Hz$

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
