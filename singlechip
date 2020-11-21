#include <reg51.h>          
#include<intrins.h>


typedef unsigned int u16;     
typedef unsigned char u8;

void led1(u8 dat);//扫描亮灯时间
void led2(u8 dat);
void led3(u8 dat);
void led4(u8 dat);
void show();//循环亮灯

u8 x;//判断数 用来跳出循环 

u8 y;//判断数 用来选择时间显示 

u8 time=10;//红灯时间控制   time-3为绿灯时间   黄灯时间为3 

unsigned char second/*亮灯时间*/,timer/*计时器时间累计*/;

//原件hc595 
sbit hbig=P1^2;//总上传口  把寄存器中内容上传 上升沿有效 
sbit hdatt=P1^1;// 单个数据 
sbit hone=P1^0;// 单个数据传输口  上升沿有效 传输到寄存器中

sbit lbig=P1^5;
sbit ldatt=P1^4;
sbit lone=P1^3;

sbit big1=P2^2;
sbit datt1=P2^1;
sbit one1=P2^0;

sbit big2=P2^5;
sbit datt2=P2^4;
sbit one2=P2^3;

sbit big3=P0^2;
sbit datt3=P0^1;
sbit one3=P0^0;

sbit big4=P0^5;
sbit datt4=P0^4;
sbit one4=P0^3;

sbit p16=P1^6;//重置
sbit p17=P1^7;//暂停

//*********行********* 
char code h[]={
0x00,0x00,0x00,0x00,
0x00,0x00,0x00,0x01,
0x00,0x00,0x00,0x02,
0x00,0x00,0x00,0x04,
0x00,0x00,0x00,0x08,
0x00,0x00,0x00,0x10,
0x00,0x00,0x00,0x20,
0x00,0x00,0x00,0x40,
0x00,0x00,0x00,0x80,
0x00,0x00,0x01,0x00,
0x00,0x00,0x02,0x00,
0x00,0x00,0x04,0x00,
0x00,0x00,0x08,0x00,
0x00,0x00,0x10,0x00,
0x00,0x00,0x20,0x00, 
0x00,0x00,0x40,0x00,
0x00,0x00,0x80,0x00,
0x00,0x01,0x00,0x00,
0x00,0x02,0x00,0x00,
0x00,0x04,0x00,0x00,
0x00,0x08,0x00,0x00,
0x00,0x10,0x00,0x00,
0x00,0x20,0x00,0x00,
0x00,0x40,0x00,0x00,
0x00,0x80,0x00,0x00,
0x01,0x00,0x00,0x00,
0x02,0x00,0x00,0x00,
0x04,0x00,0x00,0x00,
0x08,0x00,0x00,0x00,
0x10,0x00,0x00,0x00,
0x20,0x00,0x00,0x00,
0x40,0x00,0x00,0x00,
0x80,0x00,0x00,0x00,
};
//*************************
 
//---------------列-------------- 
char code l1[]={
0xFF,0xF7,0xE0,0x77,0xEF,0xFB,0xEF,0xFB,0xE0,0x6D,0xEF,0xE0,0xEF,0xF7,0x80,0x3B,
0xFB,0xFD,0xBB,0xA0,0xD1,0x7D,0xEA,0xFF,0xDB,0x67,0xBB,0xB8,0xFA,0xFD,0xFD,0xFF,

0xFF,0xF7,0xFF,0xF7,0xC0,0x7B,0xFB,0xDB,0xFB,0xDD,0xFB,0xE0,0xFB,0xF7,0xFB,0xFB,
0xFB,0xFD,0xFB,0xC0,0xFB,0xFD,0xFB,0xFF,0xFB,0xC7,0xFB,0xF8,0x80,0x3D,0xFF,0xFF
};/*"绿"*/	/*"红"*/

char code l2[]={
0xFD,0xDF,0xE0,0x03,0xFD,0xDF,0xFD,0xDF,0x80,0x00,0xFF,0x7F,0xF0,0x07,0xF7,0x77,
0xF7,0x77,0xF0,0x07,0xF7,0x77,0xF7,0x77,0xF0,0x07,0xFD,0xDF,0xFB,0xEF,0xF7,0xF7,

0xFF,0xF7,0xFF,0xF7,0xC0,0x7B,0xFB,0xDB,0xFB,0xDD,0xFB,0xE0,0xFB,0xF7,0xFB,0xFB,
0xFB,0xFD,0xFB,0xC0,0xFB,0xFD,0xFB,0xFF,0xFB,0xC7,0xFB,0xF8,0x80,0x3D,0xFF,0xFF
};/*"黄"*/   /*"红"*/

char code l3[]={
0xFF,0xF7,0xFF,0xF7,0xC0,0x7B,0xFB,0xDB,0xFB,0xDD,0xFB,0xE0,0xFB,0xF7,0xFB,0xFB,
0xFB,0xFD,0xFB,0xC0,0xFB,0xFD,0xFB,0xFF,0xFB,0xC7,0xFB,0xF8,0x80,0x3D,0xFF,0xFF,

0xFF,0xF7,0xE0,0x77,0xEF,0xFB,0xEF,0xFB,0xE0,0x6D,0xEF,0xE0,0xEF,0xF7,0x80,0x3B,
0xFB,0xFD,0xBB,0xA0,0xD1,0x7D,0xEA,0xFF,0xDB,0x67,0xBB,0xB8,0xFA,0xFD,0xFD,0xFF
};/*"红"*/	/*"绿"*/

char code l4[]={
0xFF,0xF7,0xFF,0xF7,0xC0,0x7B,0xFB,0xDB,0xFB,0xDD,0xFB,0xE0,0xFB,0xF7,0xFB,0xFB,
0xFB,0xFD,0xFB,0xC0,0xFB,0xFD,0xFB,0xFF,0xFB,0xC7,0xFB,0xF8,0x80,0x3D,0xFF,0xFF,

0xFD,0xDF,0xE0,0x03,0xFD,0xDF,0xFD,0xDF,0x80,0x00,0xFF,0x7F,0xF0,0x07,0xF7,0x77,
0xF7,0x77,0xF0,0x07,0xF7,0x77,0xF7,0x77,0xF0,0x07,0xFD,0xDF,0xFB,0xEF,0xF7,0xF7
}; /*"红"*/  /*"黄"*/ 
//------------------------------------------ 

void delay(u16 i)	//延时
{
    while(i--);    
}

void t1_init()	//初始定化时器 
{
	TMOD=0x10;
	IE=0x88;
	TH1=0x3c;
	TL1=0xb0;
	TR1=1;
}
void reset()	//初始定化时器 
{
	TMOD=0x01;
	IE=0x88;
	TH1=0x3c;
	TL1=0xb0;
	TR1=1;
}

void settime1(){
	if(y==0){		//判断时间显示   绿灯重置
			TH1=0x3c;
			TL1=0xb0;
			if (timer<15)
			{
				timer=timer+1;
			}
			else if(timer==15)
			{
				timer=0;
				if(second==0)
				{	
					if(second==0){
						led2(0);
					}
					second=time;
					x=0;
				}
				else
				{
					second=second-1;
					if(second==3){
						led1(0);
						led2(2);
						x=0;
					}	
					if(second==2){
						led2(1);
					}
					if(second==1){
						led2(0);
						x=0;
					}	
					
				}
			}	 
			led3(second/10);
			led4(second%10);
			if(second>3){
				led1((second-3)/10);
				led2((second-3)%10);
			}
		}else{//判断。。。 
				TH1=0x3c;
				TL1=0xb0;
				if (timer<15)
				{
					timer=timer+1;
				}
				else if(timer==15)
				{
					timer=0;
					if(second==0)
					{	
						if(second==0){
							led4(0);
						}
						second=time;
						x=0;
					}
					else
					{
						second=second-1;
						if(second==3){
							led3(0);
							led4(2);
							x=0;
						}	
						if(second==2){
							led4(1);
						}
						if(second==1){
							led4(0);
							x=0;
						}	
						
					}
				}	 
				led1(second/10);
				led2(second%10);
				if(second>3){
					led3((second-3)/10);
					led4((second-3)%10);
				}
		}
}

void settime2(){
	if(y==0){		//判断时间显示    黄灯重置
			TH1=0x3c;
			TL1=0xb0;
			if (timer<15)
			{
				timer=timer+1;
			}
			else if(timer==15)
			{
				timer=0;
				if(second==0)
				{	
					if(second==0){
						led2(0);
					}
					second=time;
					x=0;
				}
				else
				{
					second=second-1;
				}
			}	 
			led3(second/10);
			led4(second%10);
			led1(second/10);
			led2(second%10);
		}else{//判断。。。 
				TH1=0x3c;
				TL1=0xb0;
				if (timer<15)
				{
					timer=timer+1;
				}
				else if(timer==15)
				{
					timer=0;
					if(second==0)
					{	
						if(second==0){
							led4(0);
						}
						second=time;
						x=0;
					}
					else
					{
						second=second-1;
					}
				}	 
				led1(second/10);
				led2(second%10);
				led3(second/10);
				led4(second%10);
				}
}


void t1_func() interrupt 3	//中断 
{	
	if(p16==0){
		//初始化亮灯时间方式判断数 
		t1_init();
		if(second<=3){
			second=3;
			settime2();
		}else{
			second=time;
			timer=0;
			settime1();
		}
		
	}
	if(p17==0){
		t1_init();
	}else{
		settime1();
	}
}

//---------------------时间显示扫描----------------------- 
void led1(u8 dat)
{
    int i;
	 big1=0;			//初始化0 为上升沿做准备 
    for(i=0;i<8;i++)
    {
		 dat<<=1;		//左移一位 
	    datt1=CY;		//左移值等于进位标志位 
	 	 one1=0;		//单个数据传送进入寄存器 
	    one1=1;	
    }
	 _nop_();
	 big1=1;			//构成上升沿 将数据送出 
}
void led2(u8 dat)
{
    int i;
	 big2=0;
    for(i=0;i<8;i++)
    {
		 dat<<=1;
	    datt2=CY;
	 	 one2=0;
	    one2=1;	
    }
	 _nop_();
	 big2=1;
}
void led3(u8 dat)
{
    int i;
	 big3=0;
    for(i=0;i<8;i++)
    {
		 dat<<=1;
	    datt3=CY;
	 	 one3=0;
	    one3=1;	
    }
	 _nop_();
	 big3=1;
}
void led4(u8 dat)
{
    int i;
	 big4=0;
    for(i=0;i<8;i++)
    {
		 dat<<=1;
	    datt4=CY;
	 	 one4=0;
	    one4=1;	
    }
	 _nop_();
	 big4=1;
}

//---------------------------------------------------------


//**************************行列扫描******************************
void hang(u8 dat)
{
    int i;
	 hbig=0;
    for(i=0;i<8;i++)
    {
		 dat<<=1;
	    hdatt=CY;
	 	 hone=0;
	    hone=1;	
    }
	 //_nop_();
	 hbig=1;
}
void lie(u8 dat)
{
    int i;
	 lbig=0;
    for(i=0;i<8;i++)
    {
		 dat<<=1;
	     ldatt=CY;
	 	 lone=0;
	     lone=1;	
    }
	 //_nop_();
		lbig=1;
}
 
void display1()
{            
    u8 m,j;
    while(x)
    {
        for(m=0;m<32;m++)//m行数 
        {
				for(j=0;j<8;j++){
					hang(h[4*m+j]);//   32行/8行扫描=4 
				}
		      lie(l1[2*m]);    //   16列/8列扫描=2 
				lie(l1[2*m+1]); 
		      delay(110);           //延时
		   }
    } 
    x=1;    	//重置判断位 
}
void display2()
{            
    u8 m,j;
    while(x)
    {
        for(m=0;m<32;m++)
        {
				for(j=0;j<8;j++){
					hang(h[4*m+j]);
				}
            lie(l2[2*m]);    
			   lie(l2[2*m+1]); 
            delay(110); 

        }
    
    } 
    x=1;    
}
void display3()
{            
    u8 m,j;
    while(x)
    {
        for(m=0;m<32;m++)
        {
				for(j=0;j<8;j++){
					hang(h[4*m+j]);
				}
            lie(l3[2*m]);    
			   lie(l3[2*m+1]); 
            delay(110);    
        }    
    }
    x=1;     
}
void display4()
{            
    u8 m,j;
    while(x)
    {
        for(m=0;m<32;m++)
        {
				for(j=0;j<8;j++){
					hang(h[4*m+j]);
				}
            lie(l4[2*m]);    
			   lie(l4[2*m+1]); 
            delay(110);        
        }    
    } 
    x=1;   
}
//************************************************************************

void f1(){//初始化定时器 
	y=0;		//初始化亮灯时间方式判断数 
	t1_init();
	second=time;
	timer=0;
	display1();		//行列扫描 
}
void f2(){
	display2();
}
void f3(){
	y=1;
	t1_init();
	second=time;
	timer=0;
	display3();
}
void f4(){
	display4();
}
void show(){
	while(1){
		x=1;		//跳出循环判断数初始化 
		f1();
		f2();
		f3();
		f4();
	}
}
void main(){
	show();
}




