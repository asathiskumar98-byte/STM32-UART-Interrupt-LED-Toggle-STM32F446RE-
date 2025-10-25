# STM32 UART Interrupt LED Toggle (STM32F446RE)

This project demonstrates **UART interrupt-based communication** on an **STM32F446RE (Nucleo board)**.  
When the MCU receives the string `"Hi"` over UART2, it **toggles the LED on pin PA5**.

---

## 🧠 Project Overview
- **UART Mode:** Interrupt (IT)
- **Baud Rate:** 9600 bps
- **MCU Pin:** 
  - TX → PA2  
  - RX → PA3  
  - LED → PA5
- **Communication Interface:** USART2
- **Condition:** If received data = `"Hi"`, LED toggles.

---

## ⚙️ Working Principle
1. The UART is configured in **interrupt mode** using `HAL_UART_Receive_IT()`.
2. Each time 2 bytes are received, the interrupt triggers.
3. If the characters match `"H"` and `"i"`, the LED (PA5) toggles.
4. The receive buffer resets and waits for the next input.

---

## 🧩 Key Code Snippet (main.c)
```c
uint8_t rx_buffer[3];

while (1)
{
    HAL_UART_Receive_IT(&huart2, rx_buffer, 2);

    if ((rx_buffer[0] == 'H') && (rx_buffer[1] == 'i'))
    {
        HAL_GPIO_TogglePin(GPIOA, GPIO_PIN_5);
        rx_buffer[0] = 0;
        rx_buffer[1] = 0;
    }
}
