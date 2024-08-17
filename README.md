# Project2
Automated Vending Machine 

Code:
#include<xc.h>              
int flag = 0;


#define R1 RB0
#define R2 RB1
#define R3 RB2
#define R4 RB3
#define C1 RB4
#define C2 RB5
#define C3 RB6
#define C4 RB7
 
int cnt=0;
void change_code();
void delay_ms(int ms);
void delay_us(int us);
void Data(int Value);           
void Cmd(int Value);            
void Send2Lcd(const char Adr1, const char *Lcd1, const char *Lcd2);

void key(int arr[]);
void keyinit();
//void store(unsigned char inpu);

unsigned char keypad1[4][4]={{'1','2','3','A'},{'4','5','6','B'},{'7','8','9','C'},{'*','0','#','D'}};
unsigned int keypad2[4][4]={{1,2,3,11},{4,5,6,22},{7,8,9,33},{12,0,13,44}};
//unsigned char keypad1[4][4]={{1,2,3,11},{4,5,6,22},{7,8,9,33},{12,0,13,44}};

unsigned int rowloc, colloc;
//unsigned char input,temp;

char code1[10];
char user1[4]={'1','1','1','1'};

 
void __interrupt() adcint()
{
	 GIE   = 0;
	if (ADIF == 1)
    {
        flag=1;
        ADIF=0;        
    }
	GIE   = 1;
}
 
void main()                     /*  Main Program Definition                              */
{
    unsigned int digital_val,d2,d1,tmp;
    keyinit();
    unsigned int ana_val;
    TRISC=0X00;                /*  (RC1,RC0 ->O/P Setting by Zero)                      */
    TRISD=0X00;
    TRISE = 0x00;
    TRISC4 = 1;
    TRISC5 = 0;
    //ADCON1 = 0x07;
    int c_count = 1;
    int p_count = 1;
    int s_count = 1;
    int f_count = 1;
    char a[9];
    a[1]=0x00;
    a[2]=0x01;
    a[3]=0x02;
    a[4]=0x03;
    a[5]=0x04;
    a[6]=0x05;
    a[7]=0x06;
    a[8]=0x07;
    int i;
    int arr[2];/*  PORTD (0 - 7)pins Config as Output                   */
    TRISA=0b00110000;
    //PORTB=0x00;///need
    //PORTC=0x00;
    ADCON0=0xA0;
    ADCON1=0x00;
    GIE=1;
    PEIE=1;
    ADIE=1;
    ADIF=0;
    ADON=1;
    
    keyinit();
    PORTE = 0x07;
    
    unsigned char b2;
    int p;
 
    delay_ms(15);              /*  Minimum Delay To Power On LCD Module To Recieve Mode */
    Cmd(0X30);	delay_ms(5);    /*  LCD Specification Commands                           */
    Cmd(0X30);	delay_ms(1);    /*  LCD Specification Commands                           */
    Cmd(0X30);	delay_ms(2);    /*  LCD Specification Commands                           */
    Cmd(0X38);                 /*  LCD Double Line Display Command                      */
    Cmd(0X06);                 /*  LCD Auto Increment Location Address Command          */
    Cmd(0X01);                 /*  LCD Display Clear Command                            */
    Cmd(0X0C);                 /*  LCD Display ON Command                               */
    Send2Lcd(0x80,"1)Cola  2)Pepsi" ,"3)Sting 4)Fizz");	/*  LCD Data Outing Function                     */
    delay_ms(3);

    delay_ms(3);
    while(1){
        
        //Cmd(0xc7);
        key(arr);
	PORTCbits.RC4 == 0;

	    b2 = keypad1[arr[0]][arr[1]];
	    p = keypad2[arr[0]][arr[1]];

        if (b2 == '#') { // check condition
	    
	        for(int k=0;k<8;k++){
                PORTE = a[k];
		delay_ms(1000);
            }
            

        }
        else if (b2 == '*') { // check condition
            Send2Lcd(0x80,"item not available"," ");
        }
	else if (b2 == '0') {
            Send2Lcd(0x80,"item not available"," ");
        }
	else if (b2 == '9') {
            Send2Lcd(0x80,"item not available"," ");
        }
	
        else {
	        Send2Lcd(0x80,"Item selected_"," ");
		    Cmd(0xc0);
		    Data(b2);
		    if(b2 == '1' && c_count >=0){
		        if(c_count > 0){
		            Send2Lcd(0x80,"Item selected_"," ");
		            Cmd(0xc0);
		            Data(b2);
		            c_count--;
		        }
		        else{
		            Send2Lcd(0x80,"out of stock"," ");
		            PORTCbits.RC5 = 1;
		            delay_ms(500);
		            PORTCbits.RC5 = 0;
		        }
		    }
		    
		    else if(b2 == '2' && p_count >=0){
		        if(p_count > 0){
		            Send2Lcd(0x80,"Item selected_"," ");
		            Cmd(0xc0);
		            Data(b2);
		            p_count--;
		        }
		        else{
		            Send2Lcd(0x80,"out of stock "," ");
		            PORTCbits.RC5 = 1;
		            delay_ms(500);
		            PORTCbits.RC5 = 0;
		        }
		    }
		    
		     else if(b2 == '3' && s_count >=0){
		        if(s_count > 0){
		            Send2Lcd(0x80,"Item selected_"," ");
		            Cmd(0xc0);
		            Data(b2);
		            s_count--;
		        }
		        else{
		            Send2Lcd(0x80,"out of stock"," ");
		            PORTCbits.RC5 = 1;
		            delay_ms(500);
		            PORTCbits.RC5 = 0;
		        }
		    }
		    
		     else if(b2 == '4' && f_count >=0){
		        if(f_count > 0){
		            Send2Lcd(0x80,"Item selected_"," ");
		            Cmd(0xc0);
		            Data(b2);
		            f_count--;
		        }
		        else{
		            Send2Lcd(0x80,"out of stock"," ");
		            PORTCbits.RC5 = 1;
		            delay_ms(500);
		            PORTCbits.RC5 = 0;
		        }
		    }
	
		
		//delay_ms(15);
	    
	        while(1){
                if(PORTCbits.RC4 == 1){
                    PORTE= a[p];
		    delay_ms(1500);
		    PORTE= 0x07;
		    Send2Lcd(0xc7,"                "," ");
                    Cmd(0X01);                 /*  LCD Display ON Command                               */
                    Send2Lcd(0x80,"1)Cola  2)Pepsi" ,"3)Sting 4)Fizz");
                    break;
		}
                else{
                    PORTE = 0x07;
                }
            }  
		
        }

    }
     
}
/*
*******************************************************************************************
* 	Function    : Cmd
* 	Description : Function to send a command to LCD
* 	Parameters  : Value, contains the command to be send
*******************************************************************************************
*/
void Cmd(int Value)
{
     PORTD=Value;
     RC1=0;                     /*  RC1=0(RS=0)		[Command Registr Selection])    */
     RC0=0;                     /*  RC0=0(R/W=0)	[Write Process])                */
     RC2=1;                     /*  RC2=1(Enable=1)	[Enable Line ON]                */
     delay_ms(4);               /*  Minimun Delay For Hold On Data                  */
     RC2=0;                     /*  RC2=0(Enable=0)	[Enable Line OFF]               */
}	
/*
*************************************************************************************
* 	Function    : Data
* 	Description : Function to send a data to LCD
* 	Parameters  : Value, contains the data to be send
*************************************************************************************
*/ 
void Data(int Value)
{
     PORTD=Value;
     RC1=1;                     /*  RC1=1(RS=1)		[Data Registr Selection])       */
     RC0=0;                     /*  RC0=0(R/W=0)	[Write Process])                */
     RC2=1;                     /*  RC2=1(Enable=1)	[Enable Line ON]                */
     delay_ms(4);               /*  Minimun Delay For Hold On Data                  */
     RC2=0;                     /*  RC2=0(Enable=0)	[Enable Line OFF]               */
}
/*
*************************************************************************************
* 	Function    : Send2Lcd
* 	Description : Function to send a string of data in to LCD
* 	Parameters  : adr, contains the adddress of a string 
*************************************************************************************
*/
void Send2Lcd(const char Adr1, const char *Lcd1, const char *Lcd2)
{
     Cmd(Adr1);
     while(*Lcd1!='\0')	{	
         Data(*Lcd1);		
         Lcd1++;	}
     
     Cmd(0xc0);
     while(*Lcd2!='\0')	{	
         Data(*Lcd2);		
         Lcd2++;	}
}
/*
*************************************************************************************
* 	Function    : delay_ms
* 	Description : Function for delay
* 	Parameters  : ms, contains millesec
*************************************************************************************
*/
void delay_ms(int ms)
{
	int i,count;
	for(i=1;i<=ms;i++)
	{
		count=1000;
		while(count!=1)
		{
			count--;
		}
	}
}
/*
************************************************************************************
* 	Function    : delay_us
* 	Description : Function for delay
* 	Parameters  : ms, contains microesec
************************************************************************************
*/
void delay_us(int us)
{
	us=us>>1;
	while(us!=1)
	us--;
}

void keyinit()
{
    TRISB = 0xF0;
    OPTION_REG &= 0x7F; // ENABLE PULL UP
}

void key(int arr[]) {
    PORTB = 0x00;
    while (C1 && C2 && C3 && C4);
    while (!C1 || !C2 || !C3 || !C4)
    {
        R1 = 0;
        R2 = R3 = R4 = 1;
        if (!C1 || !C2 || !C3 || !C4)
        {
            rowloc = 0;
            break;
        }
        R2 = 0;
        R1 = 1;
        if (!C1 || !C2 || !C3 || !C4)
        {
            rowloc = 1;
            break;
        }
        R3 = 0;
        R2 = 1;
        if (!C1 || !C2 || !C3 || !C4)
        {
            rowloc = 2;
            break;
        }
        R4 = 0;
        R3 = 1;
        if (!C1 || !C2 || !C3 || !C4)
        {
            rowloc = 3;
            break;
        }
    }
    if (C1 == 0 && C2 != 0 && C3 != 0 && C4 != 0)
        colloc = 0;
    else if (C1 != 0 && C2 == 0 && C3 != 0 && C4 != 0)
        colloc = 1;
    else if (C1 != 0 && C2 != 0 && C3 == 0 && C4 != 0)
        colloc = 2;
    else if (C1 != 0 && C2 != 0 && C3 != 0 && C4 == 0)
        colloc = 3;
    while (C1 == 0 || C2 == 0 || C3 == 0 || C4 == 0);
    //led (keypad[rowloc][colloc]);
    arr[0]=rowloc;
    arr[1]=colloc;
    Cmd(0x01);//clearing screen
    //return (keypad[rowloc][colloc]);
} 
//done

