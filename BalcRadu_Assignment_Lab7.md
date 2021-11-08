# Lab 7: RADU BALC
Link to this file in your GitHub repository:

[https://github.com/balc-radu](https://github.com/balc-radu)

### Analog-to-Digital Conversion

1. Complete table with voltage divider, calculated, and measured ADC values for all five push buttons.

   | **Push button** | **PC0[A0] voltage** | **ADC value (calculated)** | **ADC value (measured)** |
   | :-: | :-: | :-: | :-: |
   | Right  | 0&nbsp;V | 0   | 0 |
   | Up     | 0.495&nbsp;V | 101 | 100 |
   | Down   |   1.202 V    |   246  | 256 |
   | Left   |   1.97 V    |   403  | 410 |
   | Select |    3.18 V   |   651  | 640 |
   | none   |    5 V   |  1023   | 1023 |

2. Code listing of ACD interrupt service routine for sending data to the LCD/UART and identification of the pressed button. Always use syntax highlighting and meaningful comments:

```c
/**********************************************************************
 * Function: ADC complete interrupt
 * Purpose:  Display value on LCD and send it to UART.
 **********************************************************************/
ISR(ADC_vect)
{
    // WRITE YOUR CODE HERE
    uint16_t value = 0;
    char lcd_string[4] = "0000";
    value=ADC;
    
    //Clear previous value
    lcd_gotoxy(8,0);
    lcd_puts("    ");
    //Put new value TO LCD
    itoa(value, lcd_string, 10);  // Convert decimal value to string
    lcd_gotoxy(8,0);
    lcd_puts(lcd_string);
    // send the same value to UART
     uart_puts(lcd_string);
     uart_puts(" ");
     
     
    //Clear previous value
    lcd_gotoxy(13,0);
    lcd_puts("    ");
    //Put new value to LCD
    
    //display value in hexa
    itoa(value, lcd_string, 16);  // Convert decimal value to string
    lcd_gotoxy(13,0);
    lcd_puts(lcd_string);
    
    //display what button was pressed
     lcd_gotoxy(8,1);
     lcd_puts("    ");
     lcd_gotoxy(12,1);
     lcd_puts("    ");
    
    // here is the sequence of code that displays witch button was pressed, based on the value of the ADC we have calculated and measured previously
     lcd_gotoxy(8, 1);
     itoa(value, lcd_string, 10);
     if (value>1000) 
        { lcd_puts("none");
          uart_puts("none");}
         if ((value>600)&&(value<1000)) 
            { lcd_puts("select");
              uart_puts("select");}
              if ((value>350)&&(value<450)) 
                 { lcd_puts("left");
                   uart_puts("left");}
                   if ((value>200)&&(value<270)) 
                      { lcd_puts("down");
                        uart_puts("down");}
                        if ((value>5)&&(value<120)) 
                           { lcd_puts("up");
                             uart_puts("up");}
                             if (value==0) 
                                { lcd_puts("right");
                                   uart_puts("right");}
             
     // lcd_puts(lcd_string);
;
}
```

### UART communication

1. (Hand-drawn) picture of UART signal when transmitting three character data `De2` in 4800 7O2 mode (7 data bits, odd parity, 2 stop bits, 4800&nbsp;Bd).

D = 0x44 = 100 0100

e = 0x65 =  110 0101

2 = 0x32 =  010 010

D – 1 – Odd parity

e – 1 – Odd parity

2 – 0 – Odd parity

4800 Bd ; 1/T =4800, therefore, T is equal to about 208ms.

The representation in 7O2 mode. I chose to represent „De2” in separate rows( D, e, 2 ) so the drawing would be clearer.

   ![your figure](BalcRadu_Lab7_SignalSchematic.png)

2. Flowchart figure for function `uint8_t get_parity(uint8_t data, uint8_t type)` which calculates a parity bit of input 8-bit `data` according to parameter `type`. The image can be drawn on a computer or by hand. Use clear descriptions of the individual steps of the algorithms.

   ![your figure](BalcRadu_Lab7_Schematic.png)

### Temperature meter

Consider an application for temperature measurement and display. Use temperature sensor [TC1046](http://ww1.microchip.com/downloads/en/DeviceDoc/21496C.pdf), LCD, one LED and a push button. After pressing the button, the temperature is measured, its value is displayed on the LCD and data is sent to the UART. When the temperature is too high, the LED will start blinking.

1. Scheme of temperature meter. The image can be drawn on a computer or by hand. Always name all components and their values.

   ![your figure](BalcRadu_Lab7_SchematicDrawn.jpg)
