## EV3 Project GitHub


## Code

```
#pragma config(Sensor, S1, c1, sensorEV3_Color)
#pragma config(Sensor, S2, c2, sensorEV3_Color)
#pragma config(Motor,motorB,lm,tmotorEV3_Large, PIDControl, encoder)
#pragma config(Motor,motorC,rm,tmotorEV3_Large, PIDControl, encoder)

int nMotorSpeedSetting = 55;
int error = 0,sum=0,diff=0,last_error=0;
float Kp=0.9,Kd=260,Ki=Kp*Kp/(4*Kd);

int Bound1(){
   int bound;
   int black=0;
   int white=0;
   while(getButtonPress(buttonEnter)==0){ }
      for(int i=0;i<5;i++){
         white+=getColorReflected(c1);
         sleep(10);
         playTone(240,20);sleep(500);
      }
   while(getButtonPress(buttonEnter)==0){ }
      for(int i=0;i<5;i++){
         black+=getColorReflected(c1);
         sleep(10);
         playTone(240,20);sleep(500);
      }
   bound = (white/5 + black/5)/2;
   return bound;
}

void go(int bound)
{


   error = getColorReflected(c1) - bound;
   sum = sum + error;
   diff = error - last_error;
   setMotorSpeed(lm, nMotorSpeedSetting - (error*Kp+sum*Ki+diff*Kd));
   setMotorSpeed(rm, nMotorSpeedSetting + (error*Kp+sum*Ki+diff*Kd));
   last_error = error;
}

void stopMotor(){
   setMotorSpeed(lm,0);
   setMotorSpeed(rm,0);
}


task main()
{
    int bound = Bound1();
   while(true)
   {

      go(bound);
      if((getColorReflected(c1)<bound) && (getColorReflected(c2)<bound)){
         setMotorSpeed(lm,10);
         setMotorSpeed(rm,10);
         sleep(850);
         if((getColorReflected(c1)<bound) && (getColorReflected(c2)<bound)){
            setMotorSpeed(lm,10);
            setMotorSpeed(rm,10);
            sleep(1500);
            setMotorSpeed(lm,10);
            setMotorSpeed(rm,-10);
            sleep(2600);
            setMotorSpeed(lm,-10);
            setMotorSpeed(rm,10);
            sleep(4800);
               stopMotor();

          }
          break;
       }
   }
   playTone(240,20);sleep(200);
   stopMotor();


}
```

[EV3 작동영상](https://youtu.be/xTUUVcHlovE)

[중간발표](https://youtu.be/DopUDFiI_Tg)

[최종발표_1](https://youtu.be/aAQp6w9G0Zw)

[최종발표_2](https://youtu.be/kH51Rj3uVHQ)
