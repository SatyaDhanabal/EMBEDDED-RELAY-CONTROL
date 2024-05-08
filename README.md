# EMBEDDED-RELAY-CONTROL
import time
import paho.mqtt.client as mqtt
import RPi.GPIO as GPIO
GPIO.setwarnings(False)
GPIO.setmode(GPIO.BOARD)
GPIO.setup(36, GPIO.OUT)
GPIO.setup(38, GPIO.OUT)
GPIO.setup(40, GPIO.OUT)
# The callback for when the client receives a CONNACK response from the server.
def on_connect(client, userdata, flags, rc):
    #print("Connected with result code "+str(rc))
    client.subscribe("ETS/IOTKIT/RELAY") # Subscribe Message
    print("SUBSCRIBED...")
    print("Enter Command")
  
# The callback for when a PUBLISH message is received from the server.
def on_message(client, userdata, msg):
    print(msg.topic+" "+str(msg.payload))
    x=msg.payload.decode('utf-8')
    print(x)
    if x == 'ON1':  # Payload for Relay 1 ON
        GPIO.output(36, 1)
        print("Relay1 ON")
    if x == 'OFF1': 
        GPIO.output(36, 0)
        print("Relay1 OFF")
        
    if x == 'ON2':
        GPIO.output(38, 1)
        print("Relay2 OFF")
        
    if x == 'OFF2':
        GPIO.output(38, 0)
        print("Relay2 OFF")
        
    if x == 'ON3':
        GPIO.output(40, 1)
        print("Relay3 OFF")
    
    if x == 'OFF3':
        GPIO.output(40, 0)
        print("Relay3 OFF")
       
# Create an MQTT client and attach our routines to it.
client = mqtt.Client()
client.on_connect = on_connect
client.on_message = on_message

client.connect("test.mosquitto.org", 1883, 60)
client.loop_forever()

SERVER PROGRAM:
# MQTT Publish Demo
# Publish two messages, to two different topics
import time
import paho.mqtt.publish as publish
publish.single("ETS/IOTKIT/RELAY", "ON1", hostname="test.mosquitto.org")
time.sleep(5)
publish.single("ETS/IOTKIT/RELAY", "OFF1", hostname="test.mosquitto.org")
time.sleep(5)
publish.single("ETS/IOTKIT/RELAY", "ON2", hostname="test.mosquitto.org")
time.sleep(5)
publish.single("ETS/IOTKIT/RELAY", "OFF2", hostname="test.mosquitto.org")
time.sleep(5)
publish.single("ETS/IOTKIT/RELAY", "ON3", hostname="test.mosquitto.org")
time.sleep(5)
publish.single("ETS/IOTKIT/RELAY", "OFF3", hostname="test.mosquitto.org")
time.sleep(5)

#publish.single("CoreElectronics/test", "hello", hostname="test.mosquitto.org")
#print publish.single("CoreElectronics/topic", "world", hostname="test.mosquitto.org")
print("Done")

OUTPUT AT CLIENT:

Python 3.7.3 (default, Jan 22 2021, 20:04:44) 
[GCC 8.3.0] on linux
Type "help", "copyright", "credits" or "license()" for more information.
>>> 
 RESTART: /home/pi/Adafruit_python_DHT/DemoCode/11.MQTT/MQTT_Relay_Control1.py 
SUBSCRIBED...
Enter Command
ETS/IOTKIT/RELAY b'ON2'
ON2
Relay2 OFF
ETS/IOTKIT/RELAY b'OFF2'
OFF2
Relay2 OFF
ETS/IOTKIT/RELAY b'ON3'
ON3
Relay3 OFF
ETS/IOTKIT/RELAY b'OFF3'
OFF3
Relay3 OFF
ETS/IOTKIT/RELAY b'ON1'
ON1
Relay1 ON
ETS/IOTKIT/RELAY b'OFF1'
OFF1
Relay1 OFF
ETS/IOTKIT/RELAY b'ON2'
ON2
Relay2 OFF
ETS/IOTKIT/RELAY b'OFF2'
OFF2
Relay2 OFF
ETS/IOTKIT/RELAY b'ON3'
ON3
Relay3 OFF
ETS/IOTKIT/RELAY b'OFF3'
OFF3
Relay3 OFF
ETS/IOTKIT/RELAY b'ON1'
ON1
Relay1 ON
ETS/IOTKIT/RELAY b'OFF1'
OFF1
Relay1 OFF
ETS/IOTKIT/RELAY b'ON2'
ON2
Relay2 OFF
ETS/IOTKIT/RELAY b'OFF2'
OFF2
Relay2 OFF
ETS/IOTKIT/RELAY b'ON3'
ON3
Relay3 OFF
ETS/IOTKIT/RELAY b'OFF3'
OFF3
Relay3 OFF
ETS/IOTKIT/RELAY b'ON1'
ON1
Relay1 ON
ETS/IOTKIT/RELAY b'OFF1'
OFF1
Relay1 OFF
ETS/IOTKIT/RELAY b'ON2'
ON2
Relay2 OFF
ETS/IOTKIT/RELAY b'OFF2'
OFF2
Relay2 OFF
ETS/IOTKIT/RELAY b'ON3'
ON3
Relay3 OFF
ETS/IOTKIT/RELAY b'OFF3'
OFF3
Relay3 OFF

OUTPUT AT SERVER:
Python 3.7.3 (default, Jan 22 2021, 20:04:44) 
[GCC 8.3.0] on linux
Type "help", "copyright", "credits" or "license()" for more information.
>>> 
===== RESTART: /home/pi/Adafruit_python_DHT/DemoCode/11.MQTT/mqtttest.py =====
Done
>>>  
