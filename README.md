# Microsoft IoT Hackathon - "Smart Dorm Room" project
In this project we will make a "Smart Dorm Room". Project consists of arduino code for the room sensing and control unit, and an UWP application (not implemented yet). Communication is done via the Azure IoT Hub (not implemented yet).

Devices currently connected to the project's Room sensing and control unit:
- temperature sensor
- ambient light sensor
- door magnetic open/close sensor
- window magnetic open/close sensor
- light switch button

Devices planned in the original idea:
- temperature sensor - *for sensing the room temperature, the data can be used for pure statistics, but also for other functions*
- ambient light sensor - *for controlling the automatic sun blinds*
- automatic sun blinds motor - *for automatically raising/lowering the sun blinds*
- window motor - *for both automatic and one-touch manual control of the window*
- NFC door lock - *for easily controlling access to the room*
- smoke sensor
- RGB light strip - *for lighting the room*
- humidity sensor - *for automatically starting and stopping the bathroom ventillation*

*More sensors and/or devices possible in the future.*

# Room sensing and control unit's Arduino code description
The program is reading data from sensors. Data is sent to Azure IoT hub (not implemented yet).

Methods:

    - double getTemperature(int pin){} 
        + Reads ambient room temperature from LM35 analog temperature sensor. Method returns value as double 
        data type.
    
    - int calibrateLight(int pin){}
        + Method calibrates operation of the analog grayscale sensor, which we use as ambient light sensor. 
        Calibration acours wheh you press the button during the boot of the device and by putting the sensor in a first light, then dark space.
        Values are shown in the serial monitor.
        
        - int calibrateLowLight(int pin){}
            +This method is just a supporting function for the previous calibration method.
    
    - int operateBlinds(int low, int high, int state, int level){}
        + Readings from the ambient light sensor are checked. When there is too much light
        in the room blinds are lowered and when there is too little light, blinds are raised. 

    - void devicesSensor (){}
        + Method reads the values from various sensors.

