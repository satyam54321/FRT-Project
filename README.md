# FRT-Project
Folloeing video link will demonstrate my project-
https://www.youtube.com/watch?v=mW8BaS8AlKo
Submission for Future Ready Talent internship project
I will deploy critical Device monitoring like ambient temperature and Humidity as well as device health data Disk usage, RAM usage, CPU usage , etc to the cloud . If the cloud detects the device health is not good it will trigger a disable since if the telemetry device is not functioning correctly there is a risk of failre in the monitored device.THe monitoring data may also lead to execution of Cloud logic based on threshold parameters.

The following hardware is required , but you may use a different sensor if you wish!

1)Raspberry pi v4 model b (v3 wll also work)

2)dht22 sensor (a fairly old sensor so replacing this may definetly help!)

3)Mxchip AZ3166 (Not required but nice to have)

4)A MicroSD card with minimu 4 GB storage (To install our OS for raspberry pi Itdefinetly helps to have it!)

5)Jumper cables as required

Optional

6)A 5 k OHM resistor (Note: My connection did not require one)

7)More Telemetry sensors (Eg pressure or gas sensor as you may seem fit)

STEP 1)

Install and burn an OS image for rasbian OS through the Rasberry PI OS installer tool

Other OS like the DietPi may work but i have not tested with the same.

you may follow online walk-throughs or the official link to get this OS here https://www.raspberrypi.com/software/

NOTE :- PLEASE ENSURE THE ETCHING PROCESS IS NOT INTERRUPTED

fill in the required details tocreate a root user account

STEP 2)

Once you have your Desktop open connect to a network with internet access through which you intend to send the data to the Azure IOT hub.

We can now begin downloading the required packages

the packages required are :-

1)PIP

2)Python3 open bash shell with root account

You would need to verify that theyare present using the following commands
python3 -V
pip -V
AFTER making sure PIP and Python3 are installed

3)azure.iot.device

4)adafruit_dht

5)psutil

6)Adafrut_blinka (contains the "Board" module for raspberry pi)

NOTE:- there is a chance PIP may just be pre-installed along with python3 package but for different versions of LINUX this may not be the case.In rasbian OS the above installation is not required.

After verifying that PIP and python3 are installed.

open bash shell with root account

sudo apt-get update
sudo apt upgrade
sudo pip install azure-iot-device
sudo pip install Adafruit_DHT 
sudo pip install Adafruit_blinka
Now we use the Code in my repo ensure you paste the right connections string

After saving the python file we make a cron job that repeatedly executes the said file in regular intervals the code or doing the same every two minutes is given below

open bash shell with root account
crontab -e
*/2   *    *    *    *  python3   <your directory>
  After that fill in the code in thonny python IDE or the IDE of your choice generate A CRON JOB that repeatedly executes the CODE and you should have your device sending Telemetry data regularly without a hitch!

I defintely recommend the Az3166 chip due to the high number of high accuracy sensors that it has in a very neat package.In myprocet it acts as the SECOND IOT device which communicates with the Raspberry PI and also acts as validation for temperature and humidity.THe cloud commands are routed to both this and raspberry PI and only after approval form both are executed.

It also purpose built for microsoft azure hence is easy to setup for it.I used it to detect gyroscopic alterations or change in current through the magnetometer, again I WILL NOT RECOMMEND USING A LIVE CURRENT CONTROLLING SYSTEM although certainly possible and the aim of this experiment, there are many things that can go wrong and cause severe damage to property or life.hence I did not make the connections for the same.

To setup the Mxchip AZ3166 please follow these steps for clean setup the chip with minimal coding:-

Step 1)

First of all , most az3166 will ship with outdated firware hence we update it as the first step.

connect the Az3166 to your devlopement machine to a USB 2.0 type A port and a USB A to USB micro cable which is data enabled.

The machine should detect the chip as a storage device.
