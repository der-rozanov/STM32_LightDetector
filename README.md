# STM32_LightDetector
Программа для STM32 Nucleo F401RE для работы с аналоговыми устройствами, в данном случае, с фоторезистором. 

Для работы с ним собирается схема делителя напряжения где вход АЦП контроллера включается между ограничивающим ток резистором и фотодиодом. Программа работает слудующим образом. 

    HAL_ADC_Start(&hadc1); // запускаем преобразование сигнала АЦП
		HAL_ADC_PollForConversion(&hadc1, 100); // ожидаем окончания преобразования
		adc = HAL_ADC_GetValue(&hadc1); // читаем полученное значение в переменную adc
		if(adc < 1000) {
			HAL_GPIO_WritePin(GPIOA,GPIO_PIN_5,1); //если падение напряжения на фоторзисторе высокое, на вход ацп приходит маленькое значение, включаем освещение
		}
		else {
			HAL_GPIO_WritePin(GPIOA,GPIO_PIN_5,0); //иначе, не включаем освещение его и так хватает
		}

		snprintf(trans_str, 63, "ADC %d\n", adc); // конвертируем число в строку для отправки в COM Port 
		HAL_UART_Transmit(&huart2, (uint8_t*)trans_str, sizeof(trans_str), 1000); // отправляем сообщение в COM PORT

Настройка АЦП при работе с STM32 - важный этап, необходимо подобрать нужные значение делителя. 
