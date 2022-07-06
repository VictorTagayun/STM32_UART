STM32 UART DMA Testing

1. NUCLEO-G474RE_UART_TwoBoards_ComDMA-repository = Started from repository
2. NUCLEO-G474RE-UART-NormalTX-DMA = Transmit Normal DMA
3. NUCLEO-G474RE-UART-NormalTXRX-DMA = Transmit and Receive Normal DMA
4. NUCLEO-G474RE-UART-NormalTX-CircRX-DMA = Transmit Normal and Receive Circular DMA
5. NUCLEO-G474RE-UART-CircTX-CircRX-DMA = Transmit and Receive Circular DMA


Notes:

1. Cannot remove LPUART1_IRQHandler
2. NUCLEO-G474RE-UART-CircTX-CircRX-DMA = cannot issue DMA command consecutively, need to abort/stop the current DMA before changing parameters

NOK:

HAL_UART_Transmit_DMA(&hlpuart1, (uint8_t *)aTxBuffer, TXBUFFERSIZE);
HAL_UART_Transmit_DMA(&hlpuart1, (uint8_t *)UartTxBuffer, 1)

OK : (RX is also affected so enable it again)

HAL_UART_Transmit_DMA(&hlpuart1, (uint8_t *)aTxBuffer, TXBUFFERSIZE);
HAL_UART_Abort(&hlpuart1); or HAL_UART_DMAStop(&hlpuart1);
HAL_UART_Transmit_DMA(&hlpuart1, (uint8_t *)UartTxBuffer, 1)
HAL_UART_Receive_DMA(&hlpuart1, (uint8_t *)UartRxBuffer, 1)

HAL_UART_AbortCpltCallback & HAL_UART_AbortTransmitCpltCallback seems not to be called