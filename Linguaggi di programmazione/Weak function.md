#tetrapak #C

---

When a function is prepended with the descriptor __weak it basically means that if you (the coder) do not define it, it is defined here.

Let us take a look at my arch-nemesis "HAL_UART_RxCpltCallback()".

This function exists within the HAL of the STM32F4-HAL code base that you can download from ST-Micro.

Within the file stm32f4xx_hal_uart.c file you will find this function defined as:

```c
__weak void HAL_UART_RxCpltCallback(UART_HandleTypeDef *huart)
{
  /* NOTE: This function Should not be modified, when the callback is needed,
       the HAL_UART_RxCpltCallback could be implemented in the user  file
  */
}
```

So, as the note within the code here says, place this function inside your own user files. However when you do that, do not put in the `__weak` term. This means that the linker will take your definition of the HAL_UART_RxCpltCallback() function and not the one defined within the stm32f4xx_hal_uart.c file.

This gives the generic code base the ability to always compile. You don't have to write a whole bunch of functions that you are not interested in but it will compile. When it comes time to writing your own, you just have to NOT define yours as `__weak` and write it.