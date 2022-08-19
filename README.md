![0001](https://user-images.githubusercontent.com/111597413/185678184-1234193f-66ed-43e0-a9d7-737720cc4042.jpg)
![0002](https://user-images.githubusercontent.com/111597413/185678190-b9f474df-17f0-4297-b76b-b542edc78f91.jpg)
![0003](https://user-images.githubusercontent.com/111597413/185678197-5787db6a-815a-49b4-aeb7-ae1b23278ee9.jpg)
![0004](https://user-images.githubusercontent.com/111597413/185678201-0c463b1f-f7dd-49d2-aa60-385ded521f28.jpg)
![0005](https://user-images.githubusercontent.com/111597413/185678206-80488d19-ee18-4206-82c2-f8efcbf79087.jpg)
![0006](https://user-images.githubusercontent.com/111597413/185678213-c5e66845-8e23-416d-9cbb-54fa65030562.jpg)
![0007](https://user-images.githubusercontent.com/111597413/185678222-3c320855-9c76-4e1f-b5aa-296c31a0b725.jpg)
![0008](https://user-images.githubusercontent.com/111597413/185678227-79a6ea30-61f4-49e3-b804-a448685bcdc2.jpg)
![0009](https://user-images.githubusercontent.com/111597413/185678233-03d6610a-05ed-4063-84c6-f5a92811fde8.jpg)
![0010](https://user-images.githubusercontent.com/111597413/185678176-9ebf514d-afa5-40f6-bc15-eaf0c1d6dda7.jpg)

// PIC16F877A Configuration Bit Settings

// 'C' source line config statements

// CONFIG
#pragma config FOSC = HS        // Oscillator Selection bits (HS oscillator)

#pragma config WDTE = OFF       // Watchdog Timer Enable bit (WDT disabled)

#pragma config PWRTE = OFF      // Power-up Timer Enable bit (PWRT disabled)

#pragma config BOREN = OFF      // Brown-out Reset Enable bit (BOR disabled)

#pragma config LVP = OFF        // Low-Voltage (Single-Supply) In-Circuit Serial Programming Enable bit (RB3 is digital I/O, HV on MCLR must be used for programming)

#pragma config CPD = OFF        // Data EEPROM Memory Code Protection bit (Data EEPROM code protection off)

#pragma config WRT = OFF        // Flash Program Memory Write Enable bits (Write protection off; all program memory may be written to by EECON control)

#pragma config CP = OFF         // Flash Program Memory Code Protection bit (Code protection off)

// #pragma config statements should precede project file includes.

// Use project enums instead of #define for ON and OFF.//

#include<xc.h> 

#define _XTAL_FREQ 20000000//Crystal Frequency is set 20MHz

void __interrupt() isr(void) //interrupt service routine

{

    if( RB2 == 0 && RB1 == 0 )//Checking sensor 03 & sensor 02
    
    {   
    
        RC0 = 0;//Motor 02 is switched off
        
        RC1 = 1;//Motor 01 will be switched on
        
        __delay_ms(500);//
        
        RC1 = 0;//again Motor 01 is switched off
    }
    
    INTF = 0;
    
}

void main(void)

{

    GIE = 1;   
    
    INTE = 1;
    
    PEIE = 1 ;
    
    INTF = 0;
    
    INTEDG = 0;//interrupt edge select bit it set to detect a falling edge
    
    TRISB0 = 1;//Sensor 03
    
    TRISB1 = 1;//Sensor 02
    
    TRISB2 = 1;//Sensor 01
    
    TRISC0 = 0;//Motor 02
    
    TRISC1 = 0;//Motor 01
    
    while(1)//
    
    {   //HERE in this lab we have used IR Sensors, IR sensors will give A output of logic low when the sensors our triggered.  
    
        if(RB2 == 0 && RB1 == 1 && RB0 == 1){//Checking Sensor 01 
        
            RC0 = 1;//Motor 02 will start
            
            RC1 = 0;//Motor 01 is still in off condition 
            
        }
        
        if(RB2 == 0 && RB1 == 0 && RB0 == 1){//Checking Sensor 02
        
            RC0 = 1;//Motor 02 will remain in start
            
            RC1 = 0;//Motor 01 is still in off condition
            
        }else if(RB2 == 1){//Defines that without any trigger in Sensor 01 there cant be any function in both Motors
        
            RC0 = 0;//Motor 02 is in off condition
            
            RC1 = 0;//Motor 01 is in off condition
            
        }
        
    }
    
    return;
    
}
