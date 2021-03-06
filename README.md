# ANT+ plugin for Apache Cordova

This plugin enables communication between Android phone and ANT+ devices

Originally created for MoBio app:
* [MoBio](https://github.com/NEUROINFORMATICS-GROUP-FAV-KIV-ZCU/MoBio) 

## Supported Platforms

* Android

# ANT support

In order to use this plugin install the following apps to your phone.
The apps enable the communication with ANT+ OTG modul if you are using it.
* [ANT Radio Service] (https://play.google.com/store/apps/details?id=com.dsi.ant.service.socket)
* [ANT+ Plugins Service] (https://play.google.com/store/apps/details?id=com.dsi.ant.plugins.antplus)   
* [ANT USB Service] (https://play.google.com/store/apps/details?id=com.dsi.ant.usbservice)

# Installing

## Cordova

    $ cordova plugin add cordova-plugin-antplus

# API

## Methods

- [antplus.searchDevices](#searchdevices)
- [antplus.stopSearchDevices](#stopsearchdevices)
- [antplus.subscribeHR](#subscribehr)
- [antplus.unsubscribeHR](#unsubscribehr)
- [antplus.subscribeWGT](#subscribewgt)
- [antplus.unsubscribeWGT](#unsubscribewgt)
- [antplus.requestBasicWGT](#requestbasicwgt)
- [antplus.requestAdvancedWGT](#requestadvancedwgt)
- [antplus.subscribeSDM](#subscribesdm)
- [antplus.unsubscribeSDM](#unsubscribesdm)
- [antplus.subscribeBike](#subscribebike)
- [antplus.unsubscribeBike](#unsubscribebike)


## searchDevices

Search and discover ANT+ devices.

    antplus.searchDevices(comma_separated_device_definition, success, failure)

### Description

Function `searchDevices` scans for the defined ANT+ devices. Success callback is called every time the a device is discovered.
First parameter is string - comma separated values of device types that the function scans for. 
Possible values are:
- HEARTRATE
- BIKE_SPD
- BIKE_CADENCE
- BIKE_SPDCAD
- STRIDE_SDM
- WEIGHT_SCALE

### success response parameters

- resultID
- describeContents
- antDeviceNumber
- antDeviceType
- deviceDisplayName
- isAlreadyConnected
- isPreferredDevice
- isUserRecognizedDevice

### error response parameters

- event
- message
- code

### Example

    antplus.searchDevices('BIKE_SPD,BIKE_CADENCE,BIKE_SPDCAD', function(device) {
        console.log(JSON.stringify(device));
    }, failure);

## stopSearchDevices

Terminate scanning for ANT devices.

    antplus.stopSearchDevices(success, failure)

## subscribeHR

Function `subscribeHR` starts listening for Heart rate monitor events.
The first parameter is `antDeviceNumber` returned from [antplus.searchDevices](#searchdevices). Success callback is called every time the an event is generated. 
Each event has different type and data. Events are distinguished by the parameter `event`.

### Example
    antplus.subscribeHR(antDeviceNumber, function(response) {
        console.log(JSON.stringify(response));
    }, failure);

### success callback parameters
- antDeviceNumber
- timestamp
- event: __'heartRateData'__
    - eventFlags
    - heartRate
    - heartBeatCount
    - heartBeatEventTime
    - dataState
- event: __'page4AddtData'__
    - eventFlags
    - estTimestamp
    - manufacturerSpecificByte
    - previousHeartBeatEventTime
- event: __'cumulativeOperatingTime'__
    - eventFlags
    - estTimestamp
    - cumulativeOperatingTime
- event: __'manufacturerAndSerial'__
    - eventFlags
    - estTimestamp
    - manufacturerID
    - serialNumber
- event: __'versionAndModelEvent'__
    - eventFlags
    - estTimestamp
    - hardwareVersion
    - softwareVersion
    - modelNumber
- event: __'calculatedRrIntervalEvent'__
    - eventFlags
    - estTimestamp
    - rrInterval
    - flag

### error callback parameters
- event: __'error'__
- message
- code
- antDeviceNumber

## unsubscribeHR

Terminate listening for Heart rate monitor events.

    antplus.unsubscribeHR(success, failure)

## subscribeWGT

Function `subscribeWGT` starts listening for Weight scale events.
The first parameter is `antDeviceNumber` returned from [antplus.searchDevices](#searchdevices). Success callback is called every time the an event is generated. 
Each event has different type and data. Events are distinguished by the parameter `event`.

### Example
    antplus.subscribeWGT(antDeviceNumber, function(response) {
        console.log(JSON.stringify(response));
    }, failure);

### success callback parameters
- antDeviceNumber
- timestamp
- event: __'bodyWeightBroadcastData'__
    - eventFlags
    - estTimestamp
    - bodyWeightStatus
    - bodyWeight (note: present only if bodyWeightStatus == 'VALID')
- event: __'manufacturerIdentificationData'__
    - eventFlags
    - estTimestamp
    - hardwareRevision
    - manufacturerID
    - modelNumber
- event: __'productInformationData'__
    - eventFlags
    - estTimestamp
    - mainSoftwareRevision
    - supplementalSoftwareRevision
    - serialNumber


### error callback parameters (common for above)
- event: __'error'__
- message
- code
- antDeviceNumber


## unsubscribeWGT

Terminate listening for Weight Scale events.

    antplus.unsubscribeWGT(success, failure)

## requestBasicWGT

The methdod requests for the basic weight scale measurement - body weight.

    antplus.requestBasicWGT(function(response) {
        console.log(JSON.stringify(response));
    }, failure);

### success callback parameters
- antDeviceNumber
- timestamp
- event: __'basicMeasurementData'__
    - eventFlags
    - estTimestamp
    - status
    - bodyWeight (kg)

## requestAdvancedWGT

The methdod requests for the advanced weight scale data. The user profile is required input.

    var userProfile = {
        age: 27, // (int)
        height: 85, // (int)
        gender: 'MALE', // or 'FEMALE' (string)
        lifetimeAthlete: false, (bool)
        activityLevel: 3 // possible values are 1-6 (int)
    };

    antplus.requestAdvancedWGT(userProfile, function(response) {
        console.log(JSON.stringify(response));
    }, failure);

### success callback parameters
- antDeviceNumber
- timestamp
- event: __'advancedMeasurementData'__
    - eventFlags
    - estTimestamp
    - status
    - bodyWeight (kg)
    - hydrationPercentage (%)
    - bodyFatPercentage (%)
    - muscleMass (kg)
    - boneMass (kg)
    - activeMetabolicRate (kcal)
    - basalMetabolicRate (kcal)

## subscribeSDM

Function `subscribeSDM` starts listening for Stride and Distance Monitor events.
The first parameter is `antDeviceNumber` returned from [antplus.searchDevices](#searchdevices). Success callback is called every time the an event is generated. 
Each event has different type and data. Events are distinguished by the parameter `event`.

### Example
    antplus.subscribeSDM(antDeviceNumber, function(response) {
        console.log(JSON.stringify(response));
    }, failure);

### success callback parameters
- antDeviceNumber
- timestamp
- event: __'instantaneousSpeedData'__
    - eventFlags
    - estTimestamp
    - instantaneousSpeed
- event: __'instantaneousCadenceData'__
    - eventFlags
    - estTimestamp
    - instantaneousCadence
- event: __'cumulativeDistanceData'__
    - eventFlags
    - estTimestamp
    - cumulativeDistance
- event: __'strideCountData'__
    - eventFlags
    - estTimestamp
    - cumulativeStrides
- event: __'computationTimestampData'__
    - eventFlags
    - estTimestamp
    - timestampOfLastComputation
- event: __'latencyData'__
    - eventFlags
    - estTimestamp
    - updateLatency
- event: __'sensorStatusData'__
    - eventFlags
    - estTimestamp
    - sensorLocation
    - batteryStatus
    - sensorHealth
    - useState
- event: __'calorieData'__
    - eventFlags
    - estTimestamp
    - cumulativeCalories
- event: __'manufacturerIdentificationData'__
    - eventFlags
    - estTimestamp
    - hardwareRevision
    - manufacturerID
    - modelNumber
- event: __'productInformationData'__
    - eventFlags
    - estTimestamp
    - mainSoftwareRevision
    - supplementalSoftwareRevision
    - serialNumber
- event: __'manufacturerSpecificData'__
    - eventFlags
    - estTimestamp
    - hexString

### error callback parameters (common for above)
- event: __'error'__
- message
- code
- antDeviceNumber


## unsubscribeSDM

Terminate listening for Stride and Distance Monitor events.

    antplus.unsubscribeSDM(success, failure)

## subscribeBike

Function `subscribeBike` starts listening for Speed and Cadence Monitor events.
The first parameter is `antDeviceNumber` returned from [antplus.searchDevices](#searchdevices). Second parsmeter is `circumference` (millimeters). Third parameter is `deviceType` which defines the required bike sensor. Use one of the following values: '__BIKE_SPD__' for the speed sensor only, '__BIKE_CADENCE__' for the cadence sensor only, '__BIKE_SPDCAD__' for the combined speed and cadence sensor. Success callback is called every time the an event is generated. 
Each event has different type and data. Events are distinguished by the parameter `event`.

### Example
    antplus.subscribeBike(antDeviceNumber, function(response) {
        console.log(JSON.stringify(response));
    }, failure);

### success callback parameters - common for all types of speed & distance sensors
- antDeviceNumber
- timestamp
- event: __'calculatedSpeedData'__
    - eventFlags
    - estTimestamp
    - calculatedSpeed
- event: __'calculatedAccumulatedDistanceData'__
    - eventFlags
    - estTimestamp
    - calculatedAccumulatedDistance
- event: __'cumulativeRevolutionsData'__
    - eventFlags
    - estTimestamp
    - timestampOfLastEvent
    - cumulativeRevolutions
- event: __'cumulativeOperatingTimeData'__
    - eventFlags
    - estTimestamp
    - cumulativeOperatingTime
- event: __'manufacturerAndSerialData'__
    - eventFlags
    - estTimestamp
    - manufacturerID
    - serialNumber
- event: __'versionAndModelData'__
    - eventFlags
    - estTimestamp
    - hardwareVersion
    - softwareVersion
    - modelNumber
- event: __'batteryStatusData'__
    - eventFlags
    - estTimestamp
    - batteryVoltage
    - batteryStatus
- event: __'motionAndSpeedData'__
    - eventFlags
    - estTimestamp
    - isStopped
- event: __'deviceStateChange'__
    - name
    - state

### success callback parameters - common for the combined sensor only
- antDeviceNumber
- timestamp
- event: __'calculatedCadenceData'__
    - eventFlags
    - estTimestamp
    - isCombinedSensor: true
    - calculatedCadence

## unsubscribeBike

Terminate listening for Speed and Cadence Monitor events.

    antplus.unsubscribeBike(success, failure)

