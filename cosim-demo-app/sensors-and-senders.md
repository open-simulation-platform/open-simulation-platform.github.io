---
layout: default
title: "Sensors and Senders"
parent: "Maritime Reference Models"
nav_order: 1
has_toc: false
permalink: /cosim-demo-app/sensors-and-senders
---

# Sensors and Senders

This example configuration demonstrates how sensor FMUs can be used to communicate with an external control system. The aim is to demonstrate how this might be configured using the OSP-IS configuration files and how sensor modelling can be separated from communication.

A general overview of the configuration is shown in Figure 1. The layers are not part of the configuration, but is meant to demonstrate the general philosophy behind the case.

The sensor layers covers FMUs responsible for generating sensor data. By sensor data is meant noise, unit conversions and logic. Noise might be disturbances introduced by the sensor; unit conversions, such as north-east-down to latitiude and lognitude; and logic, such as the number of satellites for the gps.

The communication layer covers FMUs whose sole responsibility is to convert their input into some communication protocol and transfer it to a destination. In this example the data protocol is NMEA and communication protocol UDP. However, we could have used another communication protocol such as serial, as long as the data protocol and interface stays the same. Meaning that the GPS sender implementing NMEA over UDP can be seamlessly switched with another sender implementing NMEA over serial. This is one of the advantages of separating data sources from communication.

{% include figure.html
    img="/assets/img/sensors-and-senders/sensors_and_senders.png"
    num="1"
    caption="Connections between sensors and senders"
%}

## System Description

Each FMU in the example configuration will be described in the following sections. The names of the FMUs are given in the Table 1. 

**Table 1:** *List of FMUs in the sensors and senders example configuration.*

| FMU name                | Description                                                                                                                             |
| ----------------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| *[gps_sender](#gps-sender)*                  | Constructs NMEA [GST](https://www.trimble.com/OEM_ReceiverHelp/V4.44/en/NMEA-0183messages_GST.html) and [GGA](https://www.trimble.com/OEM_ReceiverHelp/V4.44/en/NMEA-0183messages_GGA.html) messages and sends them the ip and port specified in the FMU's `NmeaConfig.json` file |
| *[gyro_sender](#gyro-sender)*           | Constructs NMEA [HDT](https://www.trimble.com/OEM_ReceiverHelp/V4.44/en/NMEA-0183messages_HDT.html)  message and sends them the ip and port specified in the FMU's `NmeaConfig.json` file |
| *[vru_sender](#vru-sender)*    | Constructs Seatex proprietary NMEA SXN message and sends them the ip and port specified in the FMU's `NmeaConfig.json` file   |
| *[wind_sensor_sender](#wind-sensor-sender)*          | Constructs NMEA [MWV](https://gpsd.gitlab.io/gpsd/NMEA.html#_mwv_wind_speed_and_angle) message and sends them the ip and port specified in the FMU's `NmeaConfig.json` file |
| *[gps_sensor](#gps-sensor)*                 | Simple GPS sensor model converting north-east-down coordinates to latitiude and longitude, adding noise, performing lever arm corrections and outputing all data needed for [GST](https://www.trimble.com/OEM_ReceiverHelp/V4.44/en/NMEA-0183messages_GST.html) and [GGA](https://www.trimble.com/OEM_ReceiverHelp/V4.44/en/NMEA-0183messages_GGA.html) messages  |
| *[gyro_sensor](#gyro-sensor)*                 | Simple gyro sensor adding noise to vessel heading and outputing data needed for [HDT](https://www.trimble.com/OEM_ReceiverHelp/V4.44/en/NMEA-0183messages_HDT.html) message |
| *[vru_sensor](#vru-sensor)*                 | Simple vru sensor adding noise to roll and pitch measurements and outputing data needed for SXN message. |
| *[wind_sensor](#wind-sensor)*                 | Simple wind sensor adding noise to wind measurements and outputing data needed for  [MWV](https://gpsd.gitlab.io/gpsd/NMEA.html#_mwv_wind_speed_and_angle) message. |

## GPS Sender

The GPS Sender FMU is meant to represent a GNSS sensor communicating using the NMEA protocol. The FMU is not responsible for generating data, only for packaging its inputs into the [GST](https://www.trimble.com/OEM_ReceiverHelp/V4.44/en/NMEA-0183messages_GST.html) and [GGA](https://www.trimble.com/OEM_ReceiverHelp/V4.44/en/NMEA-0183messages_GGA.html) data structures described by the NMEA standard. Packets are sent to an ip and port specified in the FMUs `NmeaConfig.json` file located in its `resources` folder.

**Table 2:** *GPS sender inputs and outputs (I/O).*

| Name          | I/O   | Description                                                       |
| ------------- | ----- | ----------------------------------------------------------------- |
| `UTCTime` | I | Timestamp of position (hh = hour, mm = minute, ss.ss = second) |
| `Latitude` | I | Latitude in degrees and minutes (dd = degrees, mm.mmmm = minutes) |
| `LatitudeDirection` | I | Direction of latitude (N = North, S = South). |
| `Longitude` | I | Longitudes in degrees and minutes (ddd = degrees, mm.mmmm = minutes. |
| `LongitudeDirection` | I | Direction of longitude (E = East, W = West). |
| `Quality` | I | Accuracy of GPS signal (Range 0→8, normally 2 for differential corrected position). |
| `NumberOfSatellites` | I | Number of satellites in use |
| `HorizontalDilution` | I | Horizontal dilution  |
| `AntennaAltitude` | I | Height above sea level |
| `AntennaUnits` | I | Unit of altitude (M = meters) |
| `GeoidalSeperation` | I | Height of GEO ID (WGS84) |
| `GeoidalSeperationUnits` | I | Unit of height (M = meters) |
| `GpsAge` | I | Time since last GPS update (time in seconds) |
| `StationId` | I | GPS station ID  |
| `SigmaRms` | I | RMS value of the pseudo range residuals; includes carrier phase residuals during periods of RTK (float) and RTK (fixed).  |
| `SigmaMajorAxis` | I | Error ellipse semi-major axis 1 sigma error |
| `SigmaMinorAxis` | I | Error ellipse semi-minor axis 1 sigma error |
| `EllipseOrientation` | I | Error ellipse orientation, degrees from true |
| `SigmaLongitude` | I | Latitude 1 sigma error, in meters |
| `SigmaLatitiude` | I | Longitude 1 sigma error, in meters |
| `SigmaAltitude` | I | Height 1 sigma error, in meters |

## Gyro Sender

The Gyro Sender FMU is meant to represent a gyro sensor communicating using the NMEA protocol. The FMU is not responsible for generating data, only for packaging its inputs into the [HDT](https://www.trimble.com/OEM_ReceiverHelp/V4.44/en/NMEA-0183messages_HDT.html) data structure described in the NMEA standard. Packets are sent to an ip and port specified in the FMUs `NmeaConfig.json` file located in its `resources` folder.

**Table 3:** *Gyro sender inputs and outputs (I/O).*

| Name          | I/O   | Description                                                       |
| ------------- | ----- | ----------------------------------------------------------------- |
| `Heading` | I | | Vessel heading (0-360 degrees) |
| `HeadingOk` | I | Heading value is OK (A = OK) |

## VRU Sender

The Vru Sender FMU is meant to represent a vru sensor communicating using the NMEA protocol. The FMU is not responsible for generating data, only for packaging its inputs into the SXN proprietary data structure. Packets are sent to an ip and port specified in the FMUs `NmeaConfig.json` file located in its `resources` folder.

**Table 4:** *VRU sender inputs and outputs (I/O).*

| Name          | I/O   | Description                                                       |
| ------------- | ----- | ----------------------------------------------------------------- |
| `Roll` | I | Vessel roll in radians (Positive values = port up, negative values = port down). |
| `Pitch` | I | Vessel pitch in radians (Positive values = bow up, negative values = bow down). |
| `SensorOk` | I | VRS sensor is OK (10 = OK, 11 = NOT OK). |

## Wind Sensor Sender

The Wind Sensor Sender FMU is meant to represent a wind sensor communicating using the NMEA protocol. The FMU is not responsible for generating data, only for packaging its inputs into the [MWV](https://gpsd.gitlab.io/gpsd/NMEA.html#_mwv_wind_speed_and_angle) data structure. Packets are sent to an ip and port specified in the FMUs `NmeaConfig.json` file located in its `resources` folder.

**Table 5:** *Wind sensor sender inputs and outputs (I/O).*

| Name          | I/O   | Description                                                       |
| ------------- | ----- | ----------------------------------------------------------------- |
| `Direction` | I |  Wind direction relative vessel bow (+/- 180 degrees). This is direction coming from.  |
| `Reference` | I | Direction reference (R = relative direction) |
| `Speed` | I | Wind speed |
| `SpeedUnit` | I | Wind speed unit (M = meter/second) |
| `Status` | I | Wind sensor is OK (A = OK) |

## GPS Sensor

The GPS sensor is responsible for generating data needed by the GPS sender FMU,  converting north-east-down to latitude and logintude, generating noise and doing lever arm corrections.

**Table 6:** *GPS sensor inputs and outputs (I/O).*

| Name          | I/O   | Description                                                       |
| ------------- | ----- | ----------------------------------------------------------------- |
| `north` | I | Vessel's north position |
| `east` | I | Vessel's east position |
| `down` | I | Vessel's down position |
| `roll` | I | Vessel's roll orientation |
| `pitch` | I | Vessel's pitch orientation |
| `yaw` | I | Vessel's yaw orientation |
| `utc_time` | O | Timestamp of position (hh = hour, mm = minute, ss.ss = second) |
| `latitude` | O | Latitude in degrees and minutes (dd = degrees, mm.mmmm = minutes) |
| `latitude_direction` | O | Direction of latitude (N = North, S = South). Always N. |
| `longitude` | O | Longitudes in degrees and minutes (ddd = degrees, mm.mmmm = minutes. |
| `longitude_direction` | O | Direction of longitude (E = East, W = West). Always E. |
| `quality` | O | Accuracy of GPS signal (Range 0→8, normally 2 for differential corrected position). Always 2. |
| `number_of_satellites` | I | Number of satellites in use |
| `horizontal_dilution` | I | Horizontal dilution  |
| `altitude` | I | Height above sea level |
| `altitude_unit` | I | Unit of altitude (M = meters) |
| `height_geo_id` | I | Height of GEO ID (WGS84) |
| `height_unit` | I | Unit of height (M = meters) |
| `update_time` | I | Time since last GPS update (time in seconds) |
| `station_id` | I | GPS station ID  |
| `rms_residual` | I | RMS value of the pseudo range residuals; includes carrier phase residuals during periods of RTK (float) and RTK (fixed). Always 2.5.  |
| `error_ellipse_major` | I | Error ellipse semi-major axis 1 sigma error. Always 3.5. |
| `error_ellipse_minor` | I | Error ellipse semi-minor axis 1 sigma error. Always 1.5 |
| `ellipse_orientation` | I | Error ellipse orientation, degrees from true. Always 37.0. |
| `lat_sigma_error` | I | Latitude 1 sigma error, in meters. Always 3.1. |
| `lon_sigma_error` | I | Longitude 1 sigma error, in meters. Always 2.2- |
| `height_sigma_error` | I | Height 1 sigma error, in meters. Always 40. |

## Gyro Sensor

The gyro sensor is responsible for generating data needed by the gyro sender FMU and applying noise to heading measurements obtained from a vessel model.

**Table 7:** *Gyro sensor inputs and outputs (I/O).*

| Name          | I/O   | Description                                                       |
| ------------- | ----- | ----------------------------------------------------------------- |
| `heading` | I | Heading from vessel model |
| `heading_measurement` | O | | Vessel heading (0-360 degrees) |
| `heading_ok` | O | Heading value is OK (A = OK) |

## VRU Sensor

The vru sensor is responsible for generating data needed by the vru sender FMU and applying noise to roll and pitch measurements obtained from a vessel model.

**Table 8:** *VRU sensor inputs and outputs (I/O).*

| Name          | I/O   | Description                                                       |
| ------------- | ----- | ----------------------------------------------------------------- |
| `roll` | I | Vessel roll in radians |
| `pitch` | I | Vessel pitch in radians |
| `roll_measurement` | O | Measured vessel roll in radians (Positive values = port up, negative values = port down). |
| `pitch_measurement` | O | Measured vessel pitch in radians (Positive values = bow up, negative values = bow down). |
| `sensor_ok` | O | VRS sensor is OK (10 = OK, 11 = NOT OK). |

## Wind Sensor

The wind sensor is responsible for generating data needed by the wind sensor sender FMU and applying noise to wind speed and direction obtained from an environmental model.

**Table 9:** *Wind sensor inputs and outputs (I/O).*

| Name          | I/O   | Description                                                       |
| ------------- | ----- | ----------------------------------------------------------------- |
| `wind_speed` | I | Wind speed in m/s from environmental model |
| `wind_direction` | I | Wind direction in deg from environmental model | 
| `direction` | O |  Wind direction relative vessel bow (+/- 180 degrees). This is direction coming from.  |
| `direction_reference` | O | Direction reference (R = relative direction) |
| `speed` | O | Wind speed |
| `speed_reference` | O | Wind speed unit (M = meter/second) |
| `sensor_ok` | O | Wind sensor is OK (A = OK) |
