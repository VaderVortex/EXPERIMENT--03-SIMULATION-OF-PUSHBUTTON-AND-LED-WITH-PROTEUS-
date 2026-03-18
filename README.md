# EXPERIMENT – 03

## SIMULATION OF PUSHBUTTON AND LED INTERFACE WITH ARM CONTROLLER USING PROTEUS

---

## Aim

To interface a **digital output (LED)** and **digital input (Pushbutton)** to an ARM development board and simulate it in Proteus.

---

## Components Required

* STM32CubeIDE
* Proteus 8 Simulator

---

## Theory

The full form of ARM is **Advanced Reduced Instruction Set Computer (RISC)**. It is a 32-bit processor architecture developed by ARM Holdings. ARM processors are widely used in microcontrollers and embedded systems.

The ARM architecture is licensed by multiple semiconductor companies to design System-on-Chip (SoC) products and CPUs. Companies such as Samsung, Atmel, and Texas Instruments manufacture ARM-based SoCs.

### What is an ARM7 Processor?

The ARM7 processor is commonly used in embedded systems. It provides a balance between traditional architectures and newer Cortex series processors. It is well-documented (notably by NXP Semiconductors), making it suitable for beginners to understand both hardware and software design implementation.

---

### STM32F401xB / STM32F401xC Features

* ARM® Cortex®-M4 32-bit CPU with FPU
* Adaptive Real-Time Accelerator (ART Accelerator™)
* 0-wait state execution from Flash memory
* Frequency up to 84 MHz
* Memory Protection Unit (MPU)
* 105 DMIPS / 1.25 DMIPS/MHz (Dhrystone 2.1)
* DSP instructions

**Memory:**

* Up to 256 KB Flash
* Up to 64 KB SRAM

---

## Procedure

### 1. Open STM32CubeIDE

The following screen will appear:

![image](https://user-images.githubusercontent.com/36288975/226189166-ac10578c-c059-40e7-8b80-9f84f64bf088.png)

---

### 2. Create a New Project

* Click **File → New → STM32 Project**

![image](https://user-images.githubusercontent.com/36288975/226189215-2d13ebfb-507f-44fc-b772-02232e97c0e3.png)
![image](https://user-images.githubusercontent.com/36288975/226189230-bf2d90dd-9695-4aaf-b2a6-6d66454e81fc.png)

---

### 3. Select Target Device

Choose the appropriate MCU and click **Next**

![image](https://user-images.githubusercontent.com/36288975/226189280-ed5dcf1d-dd8d-43ae-815d-491085f4863b.png)

---

### 4. Enter Project Name

![image](https://user-images.githubusercontent.com/36288975/226189316-09832a30-4d1a-4d4f-b8ad-2dc28f137711.png)

---

### 5. IOC File Generation

The `.ioc` configuration file will be generated automatically.

![image](https://user-images.githubusercontent.com/36288975/226189378-3abbdee2-0df6-470f-a3cd-79c74e3d3ad8.png)

---

### 6. Configure GPIO Pins

Select required pins and configure them as:

* GPIO Input / Output
* USART (if required)

![image](https://user-images.githubusercontent.com/36288975/226189403-f7179f1a-3eae-4637-826b-ab4ec35ba1e1.png)
![image](https://user-images.githubusercontent.com/36288975/226189425-2b2414ce-49b3-4b61-a260-c658cb2e4152.png)

---

### 7. Generate Code

Press **Ctrl + S** to generate the C code automatically.

![image](https://user-images.githubusercontent.com/36288975/226189443-8b43451d-0b14-47e4-a20b-cc09c6ad8458.png)
![image](https://user-images.githubusercontent.com/36288975/226189450-85ffa969-2ffb-4788-81e5-72d60fdda0f1.png)

---

### 8. Modify Program

Edit the generated code as required.

![image](https://user-images.githubusercontent.com/36288975/226189461-a573e62f-a109-4631-a250-a20925758fe0.png)

---

### 9. Build Project

![image](https://user-images.githubusercontent.com/36288975/226189554-3f7101ac-3f41-48fc-abc7-480bd6218dec.png)

---

### 10. Build Completion

![image](https://user-images.githubusercontent.com/36288975/226189577-c61cc1eb-3990-4968-8aa6-aefffc766b70.png)

---

### 11. Debug Project

![image](https://user-images.githubusercontent.com/36288975/226189625-37daa9a3-62e9-42b5-a5ce-2ac63345905b.png)

---

### 12. Proteus Simulation Setup

---

### 13. Create Proteus Project

* Create a new project
* Select **STM32F40xx MCU** (same as CubeIDE project)

---

### 14. Design Circuit

![image](https://user-images.githubusercontent.com/36288975/233856847-32bea88a-565f-4e01-9c7e-4f7ed546ddf6.png)

---

### 15. Load HEX File

* Double-click MCU
* Set **Program File path** to generated `.hex`
* Set **External crystal = 8 MHz**
* Click OK

![image](https://user-images.githubusercontent.com/36288975/234186668-f21e74f6-8958-4eb2-899f-8e53770a5c06.png)

---

### 16. Run Simulation

![image](https://user-images.githubusercontent.com/36288975/233856904-99eb708a-c907-4595-9025-c9dbd89b8879.png)

---

## STM32Cube Program

```c
#include "main.h"
#include "stdbool.h"

void push_button();
bool button_status;

void SystemClock_Config(void);
static void MX_GPIO_Init(void);

int main(void)
{
  HAL_Init();
  SystemClock_Config();
  MX_GPIO_Init();

  while (1)
  {
    push_button();
  }
}

void push_button()
{
    button_status = HAL_GPIO_ReadPin(GPIOC, GPIO_PIN_13);

    if (button_status == 0)
    {
        HAL_GPIO_WritePin(GPIOA, GPIO_PIN_5, GPIO_PIN_SET);
    }
    else
    {
        HAL_GPIO_WritePin(GPIOA, GPIO_PIN_5, GPIO_PIN_RESET);
    }
}

void SystemClock_Config(void)
{
  RCC_OscInitTypeDef RCC_OscInitStruct = {0};
  RCC_ClkInitTypeDef RCC_ClkInitStruct = {0};

  __HAL_RCC_PWR_CLK_ENABLE();
  __HAL_PWR_VOLTAGESCALING_CONFIG(PWR_REGULATOR_VOLTAGE_SCALE2);

  RCC_OscInitStruct.OscillatorType = RCC_OSCILLATORTYPE_HSI;
  RCC_OscInitStruct.HSIState = RCC_HSI_ON;
  RCC_OscInitStruct.HSICalibrationValue = RCC_HSICALIBRATION_DEFAULT;
  RCC_OscInitStruct.PLL.PLLState = RCC_PLL_NONE;

  if (HAL_RCC_OscConfig(&RCC_OscInitStruct) != HAL_OK)
  {
    Error_Handler();
  }

  RCC_ClkInitStruct.ClockType = RCC_CLOCKTYPE_HCLK | RCC_CLOCKTYPE_SYSCLK
                             | RCC_CLOCKTYPE_PCLK1 | RCC_CLOCKTYPE_PCLK2;

  RCC_ClkInitStruct.SYSCLKSource = RCC_SYSCLKSOURCE_HSI;
  RCC_ClkInitStruct.AHBCLKDivider = RCC_SYSCLK_DIV1;
  RCC_ClkInitStruct.APB1CLKDivider = RCC_HCLK_DIV1;
  RCC_ClkInitStruct.APB2CLKDivider = RCC_HCLK_DIV1;

  if (HAL_RCC_ClockConfig(&RCC_ClkInitStruct, FLASH_LATENCY_0) != HAL_OK)
  {
    Error_Handler();
  }
}

static void MX_GPIO_Init(void)
{
  GPIO_InitTypeDef GPIO_InitStruct = {0};

  __HAL_RCC_GPIOC_CLK_ENABLE();
  __HAL_RCC_GPIOA_CLK_ENABLE();

  HAL_GPIO_WritePin(GPIOA, GPIO_PIN_5, GPIO_PIN_RESET);

  GPIO_InitStruct.Pin = GPIO_PIN_13;
  GPIO_InitStruct.Mode = GPIO_MODE_INPUT;
  GPIO_InitStruct.Pull = GPIO_PULLUP;
  HAL_GPIO_Init(GPIOA, &GPIO_InitStruct);

  GPIO_InitStruct.Pin = GPIO_PIN_5;
  GPIO_InitStruct.Mode = GPIO_MODE_OUTPUT_PP;
  GPIO_InitStruct.Pull = GPIO_NOPULL;
  GPIO_InitStruct.Speed = GPIO_SPEED_FREQ_LOW;
  HAL_GPIO_Init(GPIOA, &GPIO_InitStruct);
}

void Error_Handler(void)
{
  __disable_irq();
  while (1)
  {
  }
}

#ifdef USE_FULL_ASSERT
void assert_failed(uint8_t *file, uint32_t line)
{
}
#endif
```

---

## Output Screenshots (Proteus)

### LED OFF

<img width="599" height="622" alt="image" src="https://github.com/user-attachments/assets/3e48677c-47ad-4b78-933d-7652e2f27fbf" />

---

### LED ON

<img width="605" height="620" alt="image" src="https://github.com/user-attachments/assets/81ab6e9f-04ee-4303-8e23-7215e8118601" />

---

## Proteus Layout

<img width="558" height="599" alt="image" src="https://github.com/user-attachments/assets/af026834-c61c-4235-8eb3-6a2815ddd547" />

---

## Result

Interfacing of a **digital output (LED)** and **digital input (pushbutton)** with an ARM microcontroller was successfully simulated in Proteus, and the results were verified.

---
