
# Touch key

# Purpose:
For this goal we used two **touch sensor** to design a 4 switch moods.
## Requirements:

 - STM32 F4
 -  2 x Touch sensor **ttp223**
 - jumper wires
 - LEDs
 
![enter image description here](https://lh3.googleusercontent.com/IdGA6WgG2ji3tq5bpiLbbiWmiBz8EL9_51GsPZeJiMkB41F1GXpaa9sTDbfxvm5jxlPvz8wGJhk=s600 "jj")![enter image description here](https://lh3.googleusercontent.com/BnhyKwAxPjFOTV4lkmO_WnWpqSqCodRrRzx9yVBL28Lx-hA-5VaixDcIhxiXYOPPFoEXBaGVNE4 "i") ![enter image description here](https://lh3.googleusercontent.com/gUM_zsduSyuxertKzyi_t-JkSRNSxXUdV9ClCcPPClPzuI6gbSkoQHyhxgO_K3K_qXVUYenYR6I "g")
![enter image description here](https://lh3.googleusercontent.com/KI5VXrclKS99Q2yuVAWYlTJvhAWIqJrr2aZFWod1Gtw418oIVeo9y14BrkK4bYnocMgH54xbD68 "h")

    
   




## First


   Connect sensors to the board.
   
   ![](https://lh3.googleusercontent.com/ETWeU44y4d5kdeNuskyMIdrp1wTf6LX9yx1B2H2NdOVMX6yoMTQ9oONuvK1eGJbmZsZddIL04Os "p")

actually we used **D0 , D1** as inputs and **E10 to E15** as outputs.

   # Actions:
   we are trying to design a touch key in these 4 **moods**:
   

 - Tap
 - Double tap
 - Sweep right
 - Sweep left

 ## Second
Define all action that can happen:

    const  short touch1=1,touch2=2,left=3,right=4,double1=5,double2=6;

 1.  **Touch1**: 
 if sensor 1 touched.
 
 2.  **Touch2**: 
 if sensor 2 touched.
 
 3.  **Left**: 
 sweep left.(means first sensor1 touched,then sensor2 touched)
 
 4.  **Right**: 
 sweep right.(means first sensor2 touched,then sensor1 touched).
 
5.  **Double1**: 
 sensor1 touched twice.
 
 6.  **Double2**: 
 sensor2 touched twice.

## Third:
as you see:

 

 

    while (1)
	{
		up:
	if(touch1_Pin==1)
					{i=0;
					touch_value=touch1;
					wait_for_release_touch(1);
					do{ 	i++;
							HAL_Delay(1);
							if(touch2_Pin==1){touch_value=left; 	wait_for_release_touch(2);goto up;}
					else  if(touch1_Pin==1){touch_value=double1; wait_for_release_touch(1);goto up;}
						}while(i<500);
					}
	if(touch2_Pin==1)
					{i=0;
					touch_value=touch2;
					wait_for_release_touch(2);
					do{ i++;
						HAL_Delay(1);
						if(touch1_Pin==1){touch_value=right; 	wait_for_release_touch(1);goto up;}
				else  if(touch2_Pin==1){touch_value=double2; wait_for_release_touch(2);goto up;}
						}while(i<500);
					}

 

  - if sensor1 touched then,
	  1. if sensor2 touched,it means **Double1**
	 2. Otherwise,if sensor2 touched,it means **Sweep left**

	 
  - if sensor2 touched then,
	  1. if sensor2 touched,it means **Double2**
	 2. Otherwise,if sensor2 touched,it means **Sweep right**

## Forth
Now,it is time to **set** ports for each actions:

> note:
> it valuated before "while(1)"
 

 

    #define  touch1_Pin (HAL_GPIO_ReadPin(GPIOD ,GPIO_PIN_0))
	#define  touch2_Pin (HAL_GPIO_ReadPin(GPIOD ,GPIO_PIN_1))
	
	#define  touch1_Port_set (HAL_GPIO_WritePin(GPIOE,GPIO_PIN_10,GPIO_PIN_SET))
	#define  touch2_Port_set (HAL_GPIO_WritePin(GPIOE,GPIO_PIN_11,GPIO_PIN_SET))
	#define  left_Port_set (HAL_GPIO_WritePin(GPIOE,GPIO_PIN_12,GPIO_PIN_SET))
	#define  right_Port_set (HAL_GPIO_WritePin(GPIOE,GPIO_PIN_13,GPIO_PIN_SET))
	#define  double1_Port_set (HAL_GPIO_WritePin(GPIOE,GPIO_PIN_14,GPIO_PIN_SET))
	#define  double2_Port_set (HAL_GPIO_WritePin(GPIOE,GPIO_PIN_15,GPIO_PIN_SET))
	#define  touch1_Port_reset (HAL_GPIO_WritePin(GPIOE,GPIO_PIN_10,GPIO_PIN_RESET))
	#define  touch2_Port_reset (HAL_GPIO_WritePin(GPIOE,GPIO_PIN_11,GPIO_PIN_RESET))
	#define  left_Port_reset (HAL_GPIO_WritePin(GPIOE,GPIO_PIN_12,GPIO_PIN_RESET))
	#define  right_Port_reset (HAL_GPIO_WritePin(GPIOE,GPIO_PIN_13,GPIO_PIN_RESET))
	#define  double1_Port_reset (HAL_GPIO_WritePin(GPIOE,GPIO_PIN_14,GPIO_PIN_RESET))
	#define  double2_Port_reset (HAL_GPIO_WritePin(GPIOE,GPIO_PIN_15,GPIO_PIN_RESET))

 # Result
 you can see whole of final video on the link below:
 

