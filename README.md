# SIT210_Task8.1D_RPi_I2C
# NOTE: my light sensor is not soldered for this task 

import smbus
import time
 
# Define some constants from the datasheet
DEVICE     = 0x23 # Default device I2C address
POWER_DOWN = 0x00 # No active state
POWER_ON   = 0x01 # Power on
RESET      = 0x07 # Reset data register value
ONE_TIME_HIGH_RES_MODE = 0x20
 
bus = smbus.SMBus(1)  # get the i2c bus
 
def convertToNumber(data):
    # Simple function to convert 2 bytes of data
    # into a decimal number
    return ((data[1] + (256 * data[0])) / 1.2)
 
def readLight(addr=DEVICE):
    data = bus.read_i2c_block_data(addr,ONE_TIME_HIGH_RES_MODE)
    return convertToNumber(data)
 
def main():

    while True:
        if readLight() >= 20000:
            print('Very Bright')
            print('Light Level : ' + str(readLight()) + ' lux')
            time.sleep(0.5)
        if readLight() >=15000 and readLight() <20000:
            print('Bright')
            print('Light Level : ' + str(readLight()) + ' lux')
            time.sleep(0.5)
        if readLight() >=10000 and readLight() <15000:
            print('Medium')
            print('Light Level : ' + str(readLight()) + ' lux')
            time.sleep(0.5)
        if readLight() >=5000 and readLight() <10000:
            print('Dark')
            print('Light Level : ' + str(readLight()) + ' lux')
            time.sleep(0.5)
        if readLight() < 5000:
            print('Too Dark')
            print('Light Level : ' + str(readLight()) + ' lux')
            time.sleep(0.5)
 
if __name__ =="main":
    main()
