# Interfacing-of-ADC-with-LPC1768-ARM-Processor

**AIM**

To write an embedded C program to interface a Stepper Motor with ARM processor LPC1768.

**COMPONENTS REQUIRED**

**Hardware**
- ARM LPC1768
- Stepper Motor

PROCEDURE

1. Open Keil µVision and select Project → New µVision Project.

2. Browse to your project folder, enter a project name, and click Save.

3. When the Select Device for Target window appears, choose NXP → LPC1768 and click OK.

4. When prompted for startup code, click Yes to include the LPC17xx Startup File.

5. Create a new C file using File → New and type the program.

6. Save the file as main.c (example: abc.c).

7. Right-click Target 1 and add the required files to Source Group 1 (main.c, delay.c, system_LPC17xx.c, gpio.c).

8. Build the project and correct compilation errors or warnings if any.

9. To generate a .bin file, open Target Options.

10. Set IROM1 start address to 0x2000 (bootloader uses 0x0000 – 0x1FFF).

11. Enter the command to convert the .axf file to a .bin file:

          fromelf --bin projectname.axf --output projectname.bin


12. Add required include paths under C/C++ → Include Paths (e.g., Desktop → 00-libfiles).

13. Rebuild the project; the .bin file will be generated.

14. Check the project output folder to confirm that the .bin file exists.

**FILES TO ADD**

**Source Group 1**

- Startuplpc17xx.s
- main.c
- delay.c
- system_LPC17xx.
- cgpio.c

**Header Files**

- delay.h
- stdutils.h
- gpio.h

**PIN DIAGRAM**

<img width="859" height="494" alt="image" src="https://github.com/user-attachments/assets/b509b5cd-9cf6-496c-8446-908b4544b13f" />

**CIRCUIT DIAGRAM**

<img width="831" height="421" alt="image" src="https://github.com/user-attachments/assets/368aeb64-7ef2-4180-a139-2517db82efbb" />


**PROGRAM**
```
#include <lpc17xx.h>
#include "gpio.h"

#define pin1 20
#define pin2 21
#define pin3 22
#define pin4 23

#define control LPC_GPIO1->FIOPIN

void cmotor1()
{
    control |=  (1 << pin1);
    control |=  (1 << pin2);
    control &= ~(1 << pin3);
    control &= ~(1 << pin4);
}

void cmotor2()
{
    control &= ~(1 << pin1);
    control |=  (1 << pin2);
    control |=  (1 << pin3);
    control &= ~(1 << pin4);
}

void cmotor3()
{
    control &= ~(1 << pin1);
    control &= ~(1 << pin2);
    control |=  (1 << pin3);
    control |=  (1 << pin4);
}

void cmotor4()
{
    control |=  (1 << pin1);
    control &= ~(1 << pin2);
    control &= ~(1 << pin3);
    control |=  (1 << pin4);
}

void delay_ms(unsigned int ms)
{
    unsigned int i, j;
    for (i = 0; i < ms; i++)
        for (j = 0; j < 20000; j++);
}

int main()
{
    SystemInit();   // Clock and PLL configuration

    LPC_PINCON->PINSEL3 = 0x00000000;  // Configure P1.20 - P1.23 as GPIO
    LPC_GPIO1->FIODIR = (1 << pin1) | (1 << pin2) | (1 << pin3) | (1 << pin4); // Output pins

    while (1)
    {
        cmotor1();
        delay_ms(50);
        cmotor2();
        delay_ms(50);
        cmotor3();
        delay_ms(50);
        cmotor4();
        delay_ms(50);
    }
}
```

**OUTPUT**

<img width="687" height="521" alt="image" src="https://github.com/user-attachments/assets/4cccc026-2c97-4e8e-813d-f0d313620c95" />

**RESULT**

Thus, interfacing the Stepper Motor with ARM processor LPC1768 is completed and verified successfully.
