
//1-array_addition::::::::::::::::::::
#include <xc.h>
#include <pic18f4550.h>
#include <stdio.h>
void main(void) {
    int arr[5]={1,2,3,4,5};
    int i,sum=0;
    for(i=0;i<5;i++){
        sum=sum+arr[i];
    }
    TRISB=0;
    PORTB=sum;
}

//2-menu-driven for mult and div:::::::::
#include <xc.h>
#include <pic18f4550.h>
#include <stdio.h>
 void main(void){
    int n=2;
    switch(n){
        case 1:
             TRISB=0;
            int a,b,mult=0;
            a=5;
            b=2;
            for(int i=0;i<b;i++){
                mult=mult+a;
            }
            PORTB=mult;
            break; 
        case 2:
            TRISB=0;
            TRISC=0;
            int dividend,divisor,Q=0,R=0;
            dividend=20;
            divisor=5;
            for(int i=0;dividend>=divisor;i++){
                dividend=dividend-divisor;
                Q=Q+1;
                
            }
            R=dividend;
            
            PORTB=Q;
            PORTC=R;
            break;
    }
    return;
}
 
//3-ascending_order:::::::::::::::::
#include <xc.h>
#include <pic18f4550.h>
#include <stdio.h>
void main (void){
    int a[]={2,4,1,7,3};
    int temp,i,j;
    for(i=0;i<4;i++){
        for(j=0;j<4-i;j++){
            if(a[j]>a[j+1]){
                temp=a[j];
                a[j]=a[j+1];
                a[j+1]=temp;
            }
        }
    }
}

//4-descending order:::::::::::::::
#include <xc.h>
#include <pic18f4550.h>
#include <stdio.h>
void main (void){
    int a[]={2,4,1,7,3};
    int temp,i,j;
    for(i=0;i<4;i++){
        for(j=0;j<4-i;j++){
            if(a[j]<a[j+1]){
                temp=a[j];
                a[j]=a[j+1];
                a[j+1]=temp;
            }
        }
    }
}

//5-LED_interfacing::::::::::::::
#include <xc.h>
#include <pic18f4550.h>
#include <stdio.h>
void delay();
void main (void){
    TRISDbits.RD0=0;
    while(1){
        TRISDbits.RD0=1;
        delay();
        TRISDbits.RD0=0;
        delay();
    }
}
void delay(){
    unsigned int i;
    for(i=0;i<10000;i++);
}

//6-Timer-led:::::::::::::::::::::
#include <pic18f4550.h>
#include <stdio.h>
#pragma config FOSC=HS
#pragma config WOT=OFF
#pragma config LVP=OFF
#pragma config PBADEN=OFF
void timer_isr(void);
#pragma code_HIGH_INTERRRUPT_VECTOR=0x0008
#pragma interrupt timer_isr
void timer_isr(void){
    TMR0H=0x48;
    TMR0L=0xE5;
    PORTB=~PORTB;
    INTCONbits.TMR0IF=0;
}
void main(){
    ADCON1=0x0F;
    INTCON2bits.RBPU=0;
    TRISB=0;
    PORTB=0xFF;
    T0CON=0x07;
    TMR0H=0x48;
    TMR0L=0xE5;
    INTCONbits.TMR0IF=0;
    INTCONbits.TMR0IE=1;
    T0CONbits.TMR0ON=1;
    INTCONbits.GIE=1;
    while(1); 
}

//LCD_interfacing::::::::::::::::
#include <xc.h>
#include <pic18f4550.h>
#define LCD_EN LATAbits.LA1
#define LCD_RS LATAbits.LA0
#define LCDPORT LATB
void lcd_delay(unsigned int time){
    unsigned int i,j;
    for(i=0;i<time;i++){
        for(j=0;j<100;j++){}
    }
} 
void SendInstruction(unsigned char command){
    LCD_RS=0;
    LCDPORT=command;
    LCD_EN=1;
    lcd_delay(10);
    LCD_EN=0;
    lcd_delay(10);
}
void SendData(unsigned char lcddata){
    LCD_RS=1;
    LCDPORT=lcddata;
    LCD_EN=1;
    lcd_delay(10);
    LCD_EN=0;
    lcd_delay(10);
}
unsigned char *string1="Welcome to";
unsigned char *string2="SCOE IT Dept";
void main (void){
    ADCON1=0x0f;
    TRISE=0x00;
    TRISAbits.RA0=0;
    TRISAbits.RA1=0;
    SendInstruction(0x38);
    SendInstruction(0x06);
    SendInstruction(0x0C);
    SendInstruction(0x01);
    SendInstruction(0x80);
    while(*string1){
        SendData(*string1);
        string1++;
    }
    SendInstruction(0xC0);
    while(*string2){
        SendData(*string2);
        string2++;
    }
    while(1);
    return;
}

//8_DC-motor::::::::::::::::::::::
#include <pic18f4550.h>
void myMsDelay(unsigned int time) {
    unsigned int i,j;
    for(i=0;i<time;i++)
        for(j=0;j<710;j++);       
}
void main (void){
    TRISC=0x00;
    TRISD=0x00;
    PR2=0x9B;
    CCP1CON=0x0C;
    T2CON=0x07;
    PORTD=0x05;
    while(1){
        CCP1CONbits.DC1B1=0;
        CCPR1L=0x1F;
        myMsDelay(2000);
        CCP1CONbits.DC1B0=0;
        CCP1CONbits.DC1B1=0;
        CCPR1L=0x3E;
        myMsDelay(2000);
        CCP1CONbits.DC1B0=0;
        CCP1CONbits.DC1B1=0;
        CCPR1L=0x5D;
        myMsDelay(2000);
        CCP1CONbits.DC1B0=0;
        CCP1CONbits.DC1B1=0;
        CCPR1L=0x7C;
        myMsDelay(2000);  
    }
}

//9_serial_communication::::::::::
#include <xc.h>
#include <PIC18F4550.h>
#pragma idata
unsigned char string1[]={"\n\rSinhgad College of Engineering"};
unsigned char string2[]={"\n\rUSART Test Code"};
unsigned char string3[]={"\n\rSend 10 character to uC\n\r"};
unsigned char string4[]={"\n\rTransmitted character are"};
unsigned char string5[]={"\n\rRx Tx test complete"};
void TXbyte(unsigned char data);
void TXstring(unsigned char *string);
void main(void) {
unsigned char i = 0;
unsigned char rx_data[20]; 
TRISCbits.TRISC7 = 1; 
TRISCbits.TRISC6 = 0; 
SPBRG = 0x08;
SPBRGH = 0x02; 
TXSTA = 0x24; 
RCSTA = 0x90; 
BAUDCON = 0x08; 
TXstring(string1); 
TXstring(string2); 
TXstring(string3); 
for(i=0;i<10;i++){
while(PIR1bits.RCIF==0); 
rx_data[i]=RCREG; 
}
rx_data[10]=0; 
TXstring(string4); 
TXstring(rx_data); 
TXstring(string5); 
while(1);
}
void TXbyte(unsigned char data){
while(TXSTAbits.TRMT==0); 
TXREG = data; 
}
void TXstring(unsigned char *string){
unsigned char i=0;
for(i=0;string[i]!='\0';i++) 
{
TXbyte(string[i]); 
}
}







