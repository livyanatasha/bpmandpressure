# bpmandpressure
import random
import time

# Connects to a device-specific MQTT endpoint on IoT Hub
from azure.iot.device import IoTHubDeviceClient, Message
# Device connection string to authenticate the device with IoT hub
CONNECTION_STRING = "HostName=iot-hub-livya.azure-devices.net;DeviceId=PythonDevice;SharedAccessKey=MB3QvK2fbFk1fdKMMwE290AOIObpzDfh13pnjipy7PI="

# Define the JSON message to send to IoT Hub
BPM = 60.0
PRESSURE = 40.0
MSG_TXT = '{{"bpm": {bpm}, "pressure": {pressure}}}'

def iothub_client_init():
    # Create an IoT Hub client
    client = IoTHubDeviceClient.create_from_connection_string(CONNECTION_STRING)
    return client

def iothub_client_telemetry_bpm_run():

    try:
        client = iothub_client_init()
        print ( "IoT Hub device sending periodic messages, press Ctrl-C to exit" )

        while True:
            # Build the message with simulated telemetry values
            bpm = BPM + (random.random() * 40)
            pressure = PRESSURE + (random.random() * 20)
            msg_txt_formatted = MSG_TXT.format(bpm=bpm, pressure=pressure)
            message = Message(msg_txt_formatted)

            # Send the message
            print( "Sending message: {}".format(message) )
            client.send_message(message)
            print ( "Message successfully sent" )
            time.sleep(60)

    except KeyboardInterrupt:
        print ( "IoTHubClient stopped" )

if __name__ == '__main__':
    print ( "IoT Hub Quickstart #1 - Simulated device" )
    print ( "Press Ctrl-C to exit" )
    iothub_client_telemetry_bpm_run()
