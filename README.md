STM32 Bare-Metal Timer Polling (No Interrupts, No Delay)

This project demonstrates pure hardware timer usage on the STM32F401RE microcontroller using bare-metal CMSIS programming — without interrupts or software delay loops.
Instead of traditional delay functions, the application uses TIM2 in polling mode to track time intervals with high accuracy and toggle a GPIO pin accordingly.

Hardware Used
- STM32 Nucleo-F401RE Board  
- Onboard LED (connected to PA5)  
- ST-Link debugger (built-in on Nucleo)

What This Project Does
- Configures TIM2 to overflow every 500 milliseconds  
- In the main loop, polls the timer’s UIF (Update Interrupt Flag)  
- Toggles PA5 each time the overflow occurs  
- No use of delay_ms(), SysTick, or interrupts  

This creates a precise, timer-driven non-blocking LED toggle mechanism entirely based on polling logic.

How It Works
Timer Configuration
TIM2->PSC = 16000 - 1;    // Prescaler: 16 MHz / 16000 = 1 kHz (1ms tick)
TIM2->ARR = 500 - 1;      // Auto-reload for 500 ticks = 500 ms
TIM2->CR1 |= (1 << 0);    // Enable TIM2
Polling Logic
while (!(TIM2->SR & TIM_SR_UIF));   // Wait for overflow
TIM2->SR &= ~TIM_SR_UIF;            // Clear the flag
GPIO Control
GPIOA->ODR ^= (1 << 5);             // Toggle output on PA5

How to Build and Flash
1. Open the project in Keil uVision or STM32CubeIDE
2. Select the correct target device: STM32F401RE
3. Build the project
4. Connect your Nucleo board via USB
5. Flash the code to the board using ST-Link

Key Concepts Demonstrated
- Timer configuration using TIM2
- Polling the UIF flag to detect time overflow
- Bare-metal register-level control (no HAL/LL)
- Loop-based time tracking without delay functions
- GPIO configuration and output toggling
