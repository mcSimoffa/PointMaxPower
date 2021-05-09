# PointMaxPower
There are perfoming PMP (point maximal Power) optimization continiuosly.  
# How connect:  
I use STM32F103C8T6 mini board (blue pill), STM32CubeMX 6.2.1, EWARM IAR v9.10.1  
PA6 - analog input for measuring a voltage   
PA7 - analog input for measuring a current  
PA9 - PWM output to invertor control  
PC13 - LED to 3V3 for PWM duty cycle limit indication  

# Customizing  
```define PMP_TICK_PERIOD         500```  time in ms between PMP optimizations  
```define PWM_DUTY_CYCLE_DEFAULT  120```  120/1024 (htim1.Init.Period = 1024) - duty cycle at microcontroller start  
```define DUTY_STEP               10```   duty cycle step  
```define DUTY_CYCLR_MAX          768```  768/1024 up limit  
```define DUTY_CYCLR_MIN          150```  150/1024 down limit  

# How it work  
An ADC conversion is launched every ```PMP_TICK_PERIOD``` from sysTick interrupt . It perform 6 sample in that sequence: U, I, U, I, U, I.
There are runs callback ```sample_handling()```  as soon as DMA getting that samples. This routine calculate average I and U, consumtion power, 
then it compare with previous values and make correct a duty cycle by ```__HAL_TIM_SET_COMPARE```.
If duty cycle achieve a limits a change is blocked.

