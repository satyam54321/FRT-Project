import board
import adafruit_dht
import datetime
import time
import os
import psutil
import azure.iot.device

def getDHT22info():
    

##### Adding Loop to counter the buffer not read issue in DHT22
    i = 0
    while i < 1:
        dhtDevice = adafruit_dht.DHT22(board.D16, use_pulseio = False)
        humDHT = dhtDevice.humidity
        tempDHT = dhtDevice.temperature
        if (humDHT is not None) and (tempDHT is not None):
            humLab = round (humDHT,1)
            tempLab = round (tempDHT, 1)
            i += 1
        else:
            print ("Value of i:", i)
            time.sleep(10)
        return (humLab, tempLab)      


##########################
## Capturing Local PI Date like Cpu Temperature, CPU Usage, DISK information, RAM Information
#####


# Return CPU temperature as a character string
def getCPUtemperature():
    res = os.popen('vcgencmd measure_temp').readline()
    return(res.replace("temp=","").replace("'C\n",""))

# Return RAM information (unit=kb) in a list
# Index 0: total RAM
# Index 1: used RAM
# Index 2: free RAM
def getRAMinfo():
    p = os.popen('free')
    i = 0
    while 1:
        i = i + 1
        line = p.readline()
        if i==2:
            return(line.split()[1:4])

# Return % of CPU used by user as a character string
#def getCPUuse():
#    return(str(os.popen("top -n1 | awk '/Cpu\(s\):/ {print 100-$8}'").readline().strip()))
#def getCPUuse():
#    return str(os.popen("top -n1 | awk '/Cpu\(s\):/ {print $2}'").readline().strip())
def getCPUuse():
    return str(psutil.cpu_percent(4))
# Return information about disk space as a list (unit included)
# Index 0: total disk space
# Index 1: used disk space
# Index 2: remaining disk space

# Index 3: percentage of disk used
def getDiskSpace():
    p = os.popen("df -h /")
    i = 0
    while 1:
        i = i +1
        line = p.readline()
        if i==2:
            return(line.split()[1:5])
######
# CPU informatiom
#CPU_temp = getCPUtemperature()

# RAM information
# Output is in kb, here I convert it in Mb for readability
#RAM_stats = getRAMinfo()
#RAM_total = round(int(RAM_stats[0]) / 1000,1)
#RAM_used = round(int(RAM_stats[1]) / 1000,1)
#RAM_free = round(int(RAM_stats[2]) / 1000,1)

# Disk information
#DISK_stats = getDiskSpace()
#DISK_total = DISK_stats[0]
#DISK_used = DISK_stats[1]
#DISK_free = DISK_stats[2]
#DISK_perc = DISK_stats[3]


############## End Local PI Information Collection

# Get data (from All of local sensors)
def getSensorData():
    global timeString
    global humLab
    global tempLab
    global CPU_temp
    global CPU_usage
    global DISK_total
    global DISK_used
    global DISK_free
    global DISK_perc 
    global RAM_total
    global RAM_used
    global RAM_free

humLab, tempLab = getDHT22info()
# CPU informatiom
CPU_temp = getCPUtemperature()
CPU_usage = getCPUuse()

# RAM information
# Output is in kb, here I convert it in Mb for readability
RAM_stats = getRAMinfo()
RAM_total = round(int(RAM_stats[0]) / 1000,1)
RAM_used = round(int(RAM_stats[1]) / 1000,1)
RAM_free = round(int(RAM_stats[2]) / 1000,1)

# Disk information
DISK_stats = getDiskSpace()
DISK_total = DISK_stats[0]
DISK_used = DISK_stats[1]
DISK_free = DISK_stats[2]
DISK_perc = DISK_stats[3]


now = datetime.datetime.now()
timeString = now.strftime("%Y-%m-%dT%H:%M:%S+5:30")


#####

# Display on screen important data
def printData():
    print ("Local Station Time:            ", timeString)
    print ("Station Air Temperature:       ", tempLab, "oC")
    print ("Station Air Humidity:          ", humLab, "%")
    print("CPU Temperature--> ", CPU_temp)
    print("CPU Usage--> ", CPU_usage)
    print("Disk Total-->", DISK_total)
    print("Disk Used-->", DISK_used)
    print("Disk Free-->", DISK_free)
    print("Disk Percentage-->", DISK_perc)
    print("Ram Total-->", RAM_total)
    print("Ram Used-->", RAM_used)
    print("Ram Free-->", RAM_free)


#getSensorData()
#printData()

from azure.iot.device import IoTHubDeviceClient, Message

#iot-hub
CONNECTION_STRING = ""

#iot-central
#CONNECTION_STRING = ""

MSG_SND = '{{"Humidity": {humLab}, \
"Ambient_Temperature": {tempLab}, \
"CPU_Temperature": {CPU_temp}, \
"CPU_Usage": {CPU_usage}, \
"Ram_Used": {RAM_used}, \
"Disk_Used": {DISK_used}}}'


def iothub_client_init():  
    client = IoTHubDeviceClient.create_from_connection_string(CONNECTION_STRING)  
    return client  
def iothub_client_telemetry_sample_run():  
    try:  
        client = iothub_client_init()  
#        print ( "Sending data to Azure IoT Hub, press Ctrl-C to exit" )  
        msg_txt_formatted = MSG_SND.format(tempLab=tempLab, humLab=humLab, CPU_temp=CPU_temp, CPU_usage=CPU_usage, RAM_used=RAM_used, DISK_used=DISK_used)  
        message = Message(msg_txt_formatted)  
#        print ("[INFO] msg_txt_formatted :", msg_txt_formatted)
        print ("[INFO] message :", message)
        client.send_message(message)
#print( "[INFO] Data Sent to Azure IOT: {}".message )  
#        print ( "Message successfully sent" )  
#        time.sleep(3)  
    except KeyboardInterrupt:  
        print ( "IoTHubClient stopped" )  
if __name__ == '__main__':  
#    print ( "Press Ctrl-C to exit" )  
    iothub_client_telemetry_sample_run()


#
