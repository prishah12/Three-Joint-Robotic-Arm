\# Three Joint Robotic Arm With Camera

The project I chose to create in BlueStamp was the three joint robotic arm with a camera modification. The robotic arm functions from four servos which are all connected to an Arduino board. The robotic arm is also connected to a raspberry pi and pi camera for object recognition. I decided to train the algorithm used for object recognition, called YOLO, so that the camera could specifically recognize what I trained it to. The main function of my project is that when the camera recognizes an object, the raspberry pi sends signals to the arduino, which would then run a code for a specific set of movements based on which object was detected.

| **Engineer** | **School** | **Area of Interest** | **Grade** |
|:--:|:--:|:--:|:--:|
| Priyanka S | Staten Island Technical High School | Mechanical Engineering | Incoming Junior

![IMG_2357 (1)](https://github.com/user-attachments/assets/20707ab5-b202-4427-89c3-f8856b0b49c4)

# Final Milestone

<iframe width="560" height="315" src="https://www.youtube.com/embed/GbHSHsga8gE?si=z9KUXgOqZiy1fpZb" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

I finally managed to finish training the YOLO model for the candy object recogniton. Originally, I ran all the scripts in the raspberry pi terminal, but when I tried to run the training, it never fully finished. This was because the pi didn’t have the resources to finish training and to more specifically create the weights folder. Therefore I had to repeat the whole process on my laptop by installing and using Anaconda and rerunning the commands in there. To speed up the process, I emailed the images and label files I already created to myself in the pi browser, so I could then just download them to my laptop. I managed to finish this process relatively quickly since I was now already familiar with it, and when I ran the training it was fully completed after about an hour. I now had all the necessary files to continue. I then emailed the weights folder to myself which I then could open in the pi browser and download there. I then ran the last command to open the camera which successfully identified the two candy bars. This was definitely my biggest challenge so when the camera detected the twix and snickers bar I was really proud of how I persevered until it finally worked.

![IMG_1879](https://github.com/user-attachments/assets/6dd54e22-5919-4ab3-b394-004474e25916)

The final step that I wanted to finish before demo night was creating the code that would allow the raspberry pi to control the robotic arm, which would then perform a specific set of movements depending on which object the camera detected. I had to create two seperate codes: one in arduino for the arm movements and then another in python in VS Code for the raspberry pi. Although both codes were correct, I originally set the threshold value too high, so although the raspberry pi would send signals to the arduino, nothing would actually happen. I resolved this by running a few test codes to determine what would be the appropriate threshold value. Once I had the correct value, my project was fully functioning.

![IMG_1918](https://github.com/user-attachments/assets/176bc745-ecb0-4456-95b8-1fb423eef9cf)

The main key topics I learned through BlueStamp was how to use and operate a rapsberry pi for object recognition and how I could use the raspberry pi to control the robotic arm by using python and arduino code. Learning how to work with the raspberry pi was very interesting and a litle frustrating at times, since it was something I have never worked with before, and training the YOLO model took the longest amount of time. However, when everything came together, I felt proud of what I created. Next, I plan on trying to create a code that I could use to have the arm actually pick up the candy bars. In the future, I plan to use everything I have learned at BlueStamp and apply it to projects that I may work on in the future.

Final Arm Code (Controlled by Raspberry Pi)
```c++
#include <Servo.h>
const int sensorPin = A0;
const int sensor2Pin = A1;
const int threshold = 600;

Servo servo1;
Servo servo2;
Servo servo3;  // create servo object to control a servo
// twelve servo objects can be created on most boards

int pos = 0;  // variable to store the servo position

void setup() {
  pinMode (3, OUTPUT);
  pinMode (5, OUTPUT);
  pinMode (9, OUTPUT);
  servo1.attach(3);  // attaches the servo on pin 9 to the servo object
  servo2.attach(5);
  servo3.attach(9);
  Serial.begin(9600);
}

void snickers() {
  // Movement for servo 1 in set 1
  servo1.write(0);
  delay(500);
  // Movement for servo 2 in set 1
  servo2.write(60);
  delay(500);
  // Add movements for other servos in set 1
  servo3.write(0);
  delay(500);
}

void twix() {
  // Movement for servo 1 in set 2
  servo1.write(180);
  delay(500);
  // Movement for servo 2 in set 2
  servo2.write(1356);
  delay(500);
  // Add movements for other servos in set 2
  servo3.write(180);
  delay(500);
}

void loop() {
  int sensorValue = analogRead(sensorPin);
  Serial.println(sensorValue);
  if (sensorValue >= threshold) {
  snickers();
  delay(2000);  // Wait before switching to the next set
  }

  sensorValue = analogRead(sensor2Pin);
  Serial.println(sensorValue);
  if (sensorValue >=threshold) {
  twix();
  delay(2000);  // Wait before repeating
  }
}
```

# Second Milestone

<iframe width="560" height="315" src="https://www.youtube.com/embed/dShGSw_TvkY?si=SJ3BTxmqkmSZqKv2" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

I managed to figure out why the turn table was not rotating. After unscrewing the arm from the base, I took out the servo and after testing it, it appeared to be working perfectly fine. The reason why it was not turning the turn table though was because the shorter side of the servo arm was not cut short enough, and therefore hung slightly over the edge, preventing it from being fully aligned and attached to the servo. To fix this, all I had to do was cut the short side a little more so that it would fit in.
![IMG_1665](https://github.com/user-attachments/assets/4e00e02c-33a8-445d-a5dc-1c78ef2eaa10)

I also began working on my modification of adding a camera by using a raspberry pi. After setting up the pi and camera, including installing VS Code and VNC Viewer, I began researching information regarding the object recognition progress. With assistance from a guide, I managed to install and use YOLO in VNC viewer. I then used a demo code which opened up a window for the pi camera that then detected the objects it saw. It included a label of what it believed each object was and a confidence rate.
![IMG_1701](https://github.com/user-attachments/assets/8efbcae0-c264-466d-b14f-e176fc90c883)
![IMG_1693](https://github.com/user-attachments/assets/229d88b7-0384-49ad-949a-89ba01a543dc)

Although YOLO was already pre-trained to recognize some basic objects, I wanted to train it to recognize specific objects of my choosing. To do this, I needed to create a data set of images which I would then upload to Label Studio where I would label the objects. I decided to use two types of candy--twix and snickers--as the objects I wanted the camera to recognize. This process was very time consuming as this was all done in the VNC browser, which was very slow. However, once I finished I was able to download a zip file that contained the images and labels. I then started the process of organizing the files using several commands in the terminal that prevented me from having to do it manually. I ran into a few problems during this process mainly regarding files that were either not properly downloaded or named, but with assistance from my instructor I managed to overcome them.

So far, the most suprising part of this project has been using the raspberry pi. I usually prefer doing more hands-on based projects rather than computer based ones, but learning how to use and operate the raspberry pi in VNC and VS Code has been really interesting and different from what I'm used to. At first, training the model seemed a bit intimidating and complicated because of all the steps, but now that I'm near the end, I definitely feel like I've been able to learn more about something I was completely unaware of before. Before my final milestone, I need to finish the training and ensure that the trained YOLO model works, and then work on the code so that the raspberry pi can control the arm based on which object the camera detects.

Object Recognition Code in VNC
```python
import cv2
from picamera2 import Picamera2
from ultralytics import YOLO

# Set up the camera with Picam
picam2 = Picamera2()
picam2.preview_configuration.main.size = (1280, 1280)
picam2.preview_configuration.main.format = "RGB888"
picam2.preview_configuration.align()
picam2.configure("preview")
picam2.start()

# Load YOLOv8
model = YOLO("yolov8n.pt")

while True:
    # Capture a frame from the camera
    frame = picam2.capture_array()
    
    # Run YOLO model on the captured frame and store the results
    results = model(frame)
    
    # Output the visual detection data, we will draw this on our camera preview window
    annotated_frame = results[0].plot()
    
    # Get inference time
    inference_time = results[0].speed['inference']
    fps = 1000 / inference_time  # Convert to milliseconds
    text = f'FPS: {fps:.1f}'

    # Define font and position
    font = cv2.FONT_HERSHEY_SIMPLEX
    text_size = cv2.getTextSize(text, font, 1, 2)[0]
    text_x = annotated_frame.shape[1] - text_size[0] - 10  # 10 pixels from the right
    text_y = text_size[1] + 10  # 10 pixels from the top

    # Draw the text on the annotated frame
    cv2.putText(annotated_frame, text, (text_x, text_y), font, 1, (255, 255, 255), 2, cv2.LINE_AA)

    # Display the resulting frame
    cv2.imshow("Camera", annotated_frame)

    # Exit the program if q is pressed
    if cv2.waitKey(1) == ord("q"):
        break

# Close all windows
cv2.destroyAllWindows()
```

# First Milestone

<iframe width="560" height="315" src="https://www.youtube.com/embed/MxmCbUDl0ow?si=gnqRHwxgELLbn0v-" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

For my first milestone, I started by testing the main components of the robotic arm including the four servos and joysticks. The arm also consisted of the Arduino Nano, Nano Shield, and acrylic parts.

Once I tested the servos and joysticks to confirm they worked, I set all the servos to 90°, and then began assembling the arm. I finished building the arm in a couple days with not too many problems, but I did run into a few challenges while assembling it. First, since the Nano shield I was using was different from the shield used in the kit, I was only able to attach one end of the shield to the base, meaning it wasn't fully secure. Some parts were also a bit tedious to assemble due things like tiny screws and tight places to screw things in.

Once I finished assembling and wiring the arm, I tested it out with the Arm code, which made me realize I needed to readjust the second servo back to 90°. Otherwise, the controller did work, but the movement of servo 2 was very jittery so that is something I need to fix in the future. Additionally, the first servo used to rotate the turntable was not fully turning, so I need to fix that as well. For my second milestone, I plan on resolving these problems and then work on adding a camera onto the arm.

![IMG_1547](https://github.com/user-attachments/assets/d62eb814-781d-4129-8fe6-739da0b867bf)

# Schematics 
![72256](https://github.com/user-attachments/assets/c1e09c01-729e-40da-959b-fbcda8ad6c62)

# Code

Joystick Test Code
```c++
void setup() { 
pinMode(3, INPUT); 
Serial.begin(9600); 
} 
void loop(){
int value = 0; 
value = analogRead(A0); 
Serial.print("X:"); 
Serial.print(value, DEC); 
value = analogRead(A1); 
Serial.print(" | Y:"); 
Serial.print(value, DEC); 
value = digitalRead(3); 
Serial.print(" | Z: "); 
Serial.println(value, DEC); 
delay(100); 
}
```

Servo Test Code
```c++
int servopin=10;   //Define digital interface 10 to connect servo servo signal line
int myangle;       //Define the Angle variable 0-180
int pulsewidth;    //Define the pulse width variable
int val;           //0-9
void servopulse(int servopin,int myangle)  //Define an impulse function
{
pulsewidth=(myangle*11)+500;               //Convert Angle to 500-2480 pulse width
digitalWrite(servopin,HIGH);               //Set the interface level of steering gear to high
delayMicroseconds(pulsewidth);             //Delay millisecond
digitalWrite(servopin,LOW);                //Lower the interface level of the steering gear
delay(20-pulsewidth/1000);
}
void setup()
{
pinMode(servopin,OUTPUT);        //Set servo interface as output interface set servo interface as output mode
Serial.begin(9600);             //The baud rate is 9,600
Serial.println("servo=o_seral_simple ready" ) ;
}
void loop()                      //Main loop function
{
val=Serial.read();              //Read the value of the serial port
if(val>'0'&&val<='9')
{
val=val-'0';                        
val=val*(180/9);                    //Convert Numbers into angles
Serial.print("moving servo to ");
                                   //DEC:Converts the number to an angular decimal representation that outputs the ASCII encoded value of b,                                   //followed by a carriage return and a newline character
Serial.print(val,DEC);
Serial.println();
for(int i=0;i<=50;i++)             
{
servopulse(servopin,val);          //Call the impulse function
}
}
}
```

Servo 90° Code
```c++
#include<Servo.h>
Servo myservo1;  // Create a servo class
Servo myservo2;  // Create a servo class
Servo myservo3;  // Create a servo class
Servo myservo4;  // Create a servo class

void setup() {  
myservo1.attach(4);  //Set the servo control pin as D4
myservo2.attach(5);  //Set the servo control pin as D5
myservo3.attach(6);  //Set the servo control pin as D6
myservo4.attach(7);  //Set the servo control pin as D7
delay(100);          //delay 100ms 
}
/////////////////////////////////////////////////////////
void loop() {
 myservo1.write(90);  //The servo is 90 degrees
 myservo2.write(90);  //The servo is 90 degrees
 myservo3.write(90);  //The servo is 90 degrees
 myservo4.write(90);  //The servo is 90 degrees
 delay(1000);
 }
```

Arm Control Code
```c++

#include "src/CokoinoArm.h"
#define buzzerPin 9

CokoinoArm arm;
int xL,yL,xR,yR;

const int act_max=170;    //Default 10 action,4 the Angle of servo
int act[act_max][4];    //Only can change the number of action
int num=0,num_do=0;
///////////////////////////////////////////////////////////////
void turnUD(void){
  if(xL!=512){
    if(0<=xL && xL<=100){arm.up(10);return;}
    if(900<xL && xL<=1024){arm.down(10);return;} 
    if(100<xL && xL<=200){arm.up(20);return;}
    if(800<xL && xL<=900){arm.down(20);return;}
    if(200<xL && xL<=300){arm.up(25);return;}
    if(700<xL && xL<=800){arm.down(25);return;}
    if(300<xL && xL<=400){arm.up(30);return;}
    if(600<xL && xL<=700){arm.down(30);return;}
    if(400<xL && xL<=480){arm.up(35);return;}
    if(540<xL && xL<=600){arm.down(35);return;} 
    }
}
///////////////////////////////////////////////////////////////
void turnLR(void){
  if(yL!=512){
    if(0<=yL && yL<=100){arm.right(0);return;}
    if(900<yL && yL<=1024){arm.left(0);return;}  
    if(100<yL && yL<=200){arm.right(5);return;}
    if(800<yL && yL<=900){arm.left(5);return;}
    if(200<yL && yL<=300){arm.right(10);return;}
    if(700<yL && yL<=800){arm.left(10);return;}
    if(300<yL && yL<=400){arm.right(15);return;}
    if(600<yL && yL<=700){arm.left(15);return;}
    if(400<yL && yL<=480){arm.right(20);return;}
    if(540<yL && yL<=600){arm.left(20);return;}
  }
}
///////////////////////////////////////////////////////////////
void turnCO(void){
  if(xR!=512){
    if(0<=xR && xR<=100){arm.close(0);return;}
    if(900<xR && xR<=1024){arm.open(0);return;} 
    if(100<xR && xR<=200){arm.close(5);return;}
    if(800<xR && xR<=900){arm.open(5);return;}
    if(200<xR && xR<=300){arm.close(10);return;}
    if(700<xR && xR<=800){arm.open(10);return;}
    if(300<xR && xR<=400){arm.close(15);return;}
    if(600<xR && xR<=700){arm.open(15);return;}
    if(400<xR && xR<=480){arm.close(20);return;}
    if(540<xR && xR<=600){arm.open(20);return;} 
    }
}
///////////////////////////////////////////////////////////////
void date_processing(int *x,int *y){
  if(abs(512-*x)>abs(512-*y))
    {*y = 512;}
  else
    {*x = 512;}
}
///////////////////////////////////////////////////////////////
void buzzer(int H,int L){
  while(yR<420){
    digitalWrite(buzzerPin,HIGH);
    delayMicroseconds(H);
    digitalWrite(buzzerPin,LOW);
    delayMicroseconds(L);
    yR = arm.JoyStickR.read_y();
    }
  while(yR>600){
    digitalWrite(buzzerPin,HIGH);
    delayMicroseconds(H);
    digitalWrite(buzzerPin,LOW);
    delayMicroseconds(L);
    yR = arm.JoyStickR.read_y();
    }
}
///////////////////////////////////////////////////////////////
void C_action(void){
  if(yR>800){
    int *p;
    p=arm.captureAction();
    for(char i=0;i<4;i++){
    act[num][i]=*p;
    p=p+1;     
    }
    num++;
    num_do=num;
    if(num>=act_max){
      num=0;
      buzzer(600,400);
      }
    while(yR>600){yR = arm.JoyStickR.read_y();}
    //Serial.println(act[0][0]);
  }
}
///////////////////////////////////////////////////////////////
void Do_action(void){
  if(yR<220){
    buzzer(200,300);
    for(int i=0;i<num_do;i++){
      arm.do_action(act[i],15);
      }
    num=0;
    while(yR<420){yR = arm.JoyStickR.read_y();}
    for(int i=0;i<2000;i++){
      digitalWrite(buzzerPin,HIGH);
      delayMicroseconds(200);
      digitalWrite(buzzerPin,LOW);
      delayMicroseconds(300);        
    }
  }
}
///////////////////////////////////////////////////////////////
void setup() {
  //Serial.begin(9600);
  //arm of servo motor connection pins
  arm.ServoAttach(4,5,6,7);
  //arm of joy stick connection pins : xL,yL,xR,yR
  arm.JoyStickAttach(A0,A1,A2,A3);
  pinMode(buzzerPin,OUTPUT);
}
///////////////////////////////////////////////////////////////
void loop() {
  xL = arm.JoyStickL.read_x();
  yL = arm.JoyStickL.read_y();
  xR = arm.JoyStickR.read_x();
  yR = arm.JoyStickR.read_y();
  date_processing(&xL,&yL);
  date_processing(&xR,&yR);
  turnUD();
  turnLR();
  turnCO();
  C_action();
  Do_action();
}
```

# Bill of Materials

| **Part** | **Note** | **Price** | **Link** |
|:--:|:--:|:--:|:--:|
| Robot Arm Kit | Contains the acrlyic parts to assemble the arm and the necessary electronic componenets inlcuding the arduino board, servos, wires, and joysticks | $45 | <a href="(https://www.amazon.com/LK-COKOINO-Compliment-Engineering-Technology/dp/B081FG1JQ1)"> Link </a> |
| Servo Shield | Used to control the servos | $10.98 | <a href="https://www.amazon.com/HiLetgo-Expansion-Sensor-Arduino-Duemilanove/dp/B07VQRCC8F/ref=sr_1_1_sspa?crid=IY8280UJPZ8D&dib=eyJ2IjoiMSJ9.gOnvWbSP2fpJyjlzThZoFsFPHoeaF2QpSk_jNdngKIr1twGn_LzcDoaoxYvFyCU-mVjs0xm0675XcM9jJCRLlzDOmjbGgP1sIqUhTjt4NviT5cbtoA-UvEYAIHWDWIfkb2aFMmhgHU544Wc7YJiipzzt3fuSGamCrVeh0ONFUE7GqEzOyVIpGdjm_kZqEYrk4l6Ol054nebh1I2eZg7hcYRPAX8iNqbzSBQnTX3EaUY.ewdYdtnT9O7qRCuhV_2P0vAhp7a5Ue2sdk1REW8_gKI&dib_tag=se&keywords=arduino+nano+servo+shield&qid=1716857827&s=toys-and-games&sprefix=arduino+nano+servo+shield%2Ctoys-and-games%2C85&sr=1-1-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9hdGY&psc=1"> Link </a> |
| Screwdriver Kit | Provides different sized screwdriver bits | $5.94 | <a href="https://www.amazon.com/Small-Screwdriver-Set-Mini-Magnetic/dp/B08RYXKJW9/"> Link </a> |
| Electronics Kit | Contains multiple wires, USB cables LEDs, resistors, breadboard | $14 | <a href="https://www.amazon.com/Smraza-Electronics-Potentiometer-tie-Points-Breadboard/dp/B0B62RL725/ref=sxts_b2b_sx_reorder_acb_business?content-id=amzn1.sym.f63a3b0b-3a29-4a8e-8430-073528fe007f%3Aamzn1.sym.f63a3b0b-3a29-4a8e-8430-073528fe007f&crid=2IC3T44H3U3WG&cv_ct_cx=breadboard+kit&dib=eyJ2IjoiMSJ9.TUd5tu2T8rmms7ZuJ0UzmbtpLL1zsu93bQM0PzwnP4E.sT0V0vL_QtbYv8ymVTCcRkhFNgBtRvRiT7G4FT1oGTE&dib_tag=se&keywords=breadboard+kit&pd_rd_i=B0B62RL725&pd_rd_r=67e1f4ff-e3b9-44e4-b441-b4ae282f036b&pd_rd_w=UjFaP&pd_rd_wg=0xRoC&pf_rd_p=f63a3b0b-3a29-4a8e-8430-073528fe007f&pf_rd_r=BFGP77H27ZN31W4PZAW6&qid=1715911733&sbo=RZvfv%2F%2FHxDF%2BO5021pAnSA%3D%3D&sprefix=breadboard+kit%2Caps%2C109&sr=1-2-9f062ed5-8905-4cb9-ad7c-6ce62808241a"> Link </a> |
| 9V Barrel Jack | Used to put on the battery to plug into the Arduino board | $6 | <a href="https://www.amazon.com/DZS-Elec-Connector-Experimental-5-5x2-1mm/dp/B07FDS11ZY/ref=sr_1_5?crid=2KDQRHR9QTG87&dib=eyJ2IjoiMSJ9.QXzrFs_APhSZ1IJhcXZvMQHwewvRuQ3vr1brQtDco3W0bnAprDG7jH7ie8dBlokDPWbOLcDtgbrHrNUzcyb61YgxbGO0UFeN6K8ktLZDkV3jlxoO940ZYOk8jrd3G8yxrkH-cUJgXaiOka1FWDDJJssGcdvyH2WlPRHUtZKQgBpoGa4M3j8wwx3yssPZrOJK32Pfs9ZLtCibGXHxhNbXOBuXOisFlpDByQ2NJcndu5iOa0dZ8jknYgybT1KOyzP9_lSVyQNCkcxcjanEjyf4Z6jMdRX-G08K6SY7IM-agSA.UzM8eWF_dtBmatnqwrbt1mCm8-reUmM7Mqm3SWpbviM&dib_tag=se&keywords=9v+to+barrel+jack&qid=1716857906&s=electronics&sprefix=9v+to+barrel+jack%2Celectronics%2C98&sr=1-5"> Link </a> |
| USB-USBC | USB to USBC convertor | $2.99 | <a href="https://www.amazon.com/ENVEL-Transfer-Converter-Thunderbolt3-Compatible/dp/B0D3T2QDVJ/ref=sxin_17_pa_sp_search_thematic_sspa?content-id=amzn1.sym.70fcaece-2dd2-4653-bf00-fb6af1af1b93%3Aamzn1.sym.70fcaece-2dd2-4653-bf00-fb6af1af1b93&crid=2XKXL9JJ62FRH&cv_ct_cx=usba%2Bto%2Busbc&keywords=usba%2Bto%2Busbc&pd_rd_i=B0D3T2QDVJ&pd_rd_r=4d705a77-7d1c-4543-b61d-c95f071f99c3&pd_rd_w=zydwI&pd_rd_wg=Ra4PI&pf_rd_p=70fcaece-2dd2-4653-bf00-fb6af1af1b93&pf_rd_r=GA7XX674ZRQ1VKTH3HWX&qid=1750358432&sbo=RZvfv%2F%2FHxDF%2BO5021pAnSA%3D%3D&sprefix=usba%2Bto%2Busb%2Caps%2C106&sr=1-1-e169343e-09af-4d41-85b1-8335fe8f32d0-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9zZWFyY2hfdGhlbWF0aWM&th=1"> Link </a> |
| DMM | Digital multimeter to measure voltage, current, and resistance | $11 | <a href="https://www.amazon.com/AstroAI-Digital-Multimeter-Voltage-Tester/dp/B01ISAMUA6/ref=sxin_17_pa_sp_search_thematic_sspa?content-id=amzn1.sym.e8da13fc-7baf-46c3-926a-e7e8f63a520b%3Aamzn1.sym.e8da13fc-7baf-46c3-926a-e7e8f63a520b&cv_ct_cx=digital+multimeter&dib=eyJ2IjoiMSJ9.5LQumrfBR8l0mKnJCJlRg73dxpou0gqYD_ffU3srgs0Utegwth8GcQCSVXVzeZeLSJx5J3itz5TLdmJHsrVITQ.-00jRPoT-bBy26YC4LzQ-S4cYdztgmSMGb83_WEm6HY&dib_tag=se&keywords=digital+multimeter&pd_rd_i=B01ISAMUA6&pd_rd_r=e1ff2570-7e4a-4906-bc55-6f819d48d1bc&pd_rd_w=h7HgL&pd_rd_wg=0ZcFH&pf_rd_p=e8da13fc-7baf-46c3-926a-e7e8f63a520b&pf_rd_r=R6YKX3NXTDQ1PQP4H8RM&qid=1715911879&sbo=RZvfv%2F%2FHxDF%2BO5021pAnSA%3D%3D&sr=1-1-7efdef4d-9875-47e1-927f-8c2c1c47ed49-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9zZWFyY2hfdGhlbWF0aWM&psc=1"> Link </a> |
| 9V Batteries | Used to power the arduino | $12.37 | <a href="https://www.amazon.com/dp/B00MH4QM1S/ref=vp_d_pb_TIER4_cml_lp_B0BJ26CHZB_pd?_encoding=UTF8&pf_rd_p=b8d9960f-63a9-4d69-a8de-de9514a27e41&pf_rd_r=1RRARBM9YNNHR89D8B2N&pd_rd_wg=FwKYY&pd_rd_i=B00MH4QM1S&pd_rd_w=XrNnI&content-id=amzn1.sym.b8d9960f-63a9-4d69-a8de-de9514a27e41&pd_rd_r=edb0610d-b8f5-4671-814f-f6cb22938f22&th=1"> Link </a> |
| Raspberry Pi Starter Kit | Contains raspberry pi, pi camera, SD card to operate the Pi, wall plug for a power source, USBC wire to connect to laptop, flashdrive to put SD card in | $100 | <a href="https://www.amazon.com/RasTech-Raspberry-Starter-Heatsink-Screwdriver/dp/B0C8LV6VNZ/ref=sr_1_4?crid=3506HY00MCGVM&dib=eyJ2IjoiMSJ9._zkM62vSQ8p7tNr88715LdMv_qHh72Je-tkF9PXEa3chDE53QT4aZu4AGAb4ihE61QY4ZD55nKF6Fp2Kfs8t7AbafM_JrlJFfHo9OB4eAVGqa0EB-7aoBQHPmhKHZ2MW8ny-Kd44bMVlVxPlTWVk5YHIN5P3uKVqrE5Dcal0rKkHny-O6Xyb5ux2AOU6OwVbkag_bqBX66RQNRrgBuz-0pS43mcx93IZTQA9R8NaJJypYU2HAycp-XicTFmyU60a01Nfm9iuyo6B9yA8ppN3OQQyJ-NQ9xyNPxfTLwkqtng.yAYpU6outhQcZmOZhN9Wb6yTw7A85CNUbXZguGInZNg&dib_tag=se&keywords=raspberry%2Bpi%2Bkit&qid=1718848547&s=electronics&sprefix=rasbperry%2Bpi%2Bkit%2Celectronics%2C83&sr=1-4&th=1"> Link </a> |
| Raspberry Pi Camera Module | Camera stand for pi cam | $10 | <a href="https://www.amazon.com/gp/product/B0BLZ98RW1/ref=sw_img_1?smid=A8GZTQ9P9SC8J&psc=1"> Link </a> |
| Portable Charger | Power source | $10 | <a href="https://www.amazon.com/dp/B072K24KDR?ref_=ppx_hzsearch_conn_dt_b_fed_asin_title_1&th=1"> Link </a> |

# Resources
[Robotic Arm Instructions](https://github.com/Cokoino/CKK0006/tree/master)

[Raspberry Pi + Pi Cam Setup Guide](chrome-extension://efaidnbmnnnibpcajpcglclefindmkaj/https://mail-attachment.googleusercontent.com/attachment/u/0/?ui=2&ik=e62868ecde&attid=0.1&permmsgid=msg-a:r-3392424760405621202&th=198762682390f083&view=att&disp=inline&realattid=08768CE1-5557-4D9B-A3D5-BF8822A79934&zw&saddbat=ANGjdJ_6egqo-5YXatXvNiuNwza0D9c2o34GK7kXYAlOrYhEcqsnLXfTeWYcB0OaIeqUkE76V8gz9GmXx6OvWZtIGoaA6QsrBqbHmg6eqfQ4bkPch7KT4WAAWOgp5S1tdeHnKIvwDTy6vos-Oi49eLlkcJTgGDcT5vfckLcp3hl1Y4RwmNxuQjl5O2U17sDmgY_us_wYrwHIV9RSLoQsaJCI_QQZxEEBnN5o7-WLSzfI0FcAQWPZX6TeKftQIIT0XjS7w84ntIZ6gkHQ9uqn3Xot8u0YDz_3uphluBcsTgvy4W5FTad-Gqc7N5ljjcBcFaCINN7mV5O_3Pj3CaoaI1Mmd5V8SHO5p2cdYBrfa1lj2aaYrEqki8UUNAic5zsYMCD4msX6gDCXUP8HsU5l4dZ8__PhWOs4f8_8_TEpPcGrs90iutt3GDAO0myeUQOjJ6TpWQh9sMlAQxoAhXU5vq7CVswQAXsI5kTq-Efd15SLgEUezERznzeAdzwodo_TX6LY-piYzININYtlwdWJVO1zepK0PiHxr5sSOwtRm5EE8Zv7LI65_LzyCgEY9Z3qgqbQ981mF34CYxrhq0EqoAkBizaJu5R-x9tt69WNQFvA_tifo6eTXlsoctJUpHYDuKY-o_uyY6qSFkZjSlB-QZJdcwzqySFimciF9rPB_Hna9XscqPDMzlMkH-G4GZBD6GvrtRk9RFP6kZqjKe33cMR_9ll-MfEZ-v6xrevHzOo3ENO7lhLh3NEChHp_4qRP3vSNSaCQoPuwnmN91aq5-LV4zyAMFegc1j7DbGfkcM-gWihBYI4rN07z1DFFJu1EEi8GCIKTdFd64i47SOt1bCR5UmWh1pww-rqVoOQ-g6VBFSnToGnmsMv2rVQXGzWeHAZ8eGprHlequWxyL3z1vXD-kWiaclyE1bqFRg4qJbiyxunLW5WMMBUuPIuYedwcsYC9qQd8004Xyfi7gZJJ6Q1IItEmAw-IL1VcJ2DcyeVos8M4wffQTXEqVhOPaIHi5v49IRSZN6lGBgKCARcn)

[Raspberry Pi Object Recognition Introduction](https://core-electronics.com.au/guides/raspberry-pi/getting-started-with-yolo-object-and-animal-recognition-on-the-raspberry-pi/)

[Raspberry Pi Object Recognition Introduction (Video)](https://www.youtube.com/watch?reload=9&app=desktop&v=XKIm_R_rIeQ)

[How to Train YOLO Object Recognition Models](https://www.ejtech.io/learn/train-yolo-models)

