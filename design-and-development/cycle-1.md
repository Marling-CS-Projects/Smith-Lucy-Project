# 2.2.1 Cycle 1: Data Collection

## Design

In this first cycle, I have decided to use the Particle Argon (a Wi-Fi enabled development board) to program data collection for my personal weather station. I will use Visual Studio Code as an IDE and the Particle Workbench extension to create my project.&#x20;

I aim to learn how the Particle Argon communicates with and takes readings from sensors to take measurements from the environment. This will include measurements of pressure, humidity, temperature and air quality. Once I have tested how these different sensors work individually, I will design a program that takes readings from these sensors simultaneously.

### Objectives

* [x] Set up basic project with Particle Argon
* [ ] Design a program that communicates with sensors using C++

### Usability Features

In this cycle, I need to ensure that my weather station can easily be used and navigated by a stakeholder. The program should collect accutrate data from sensors at regular intervals. It must then be able to upload this data over Wi-Fi. I also need to try to minimise the storage required to run the program.

### Tests I need to conduct

<table><thead><tr><th width="69">ID</th><th>Test</th><th>Description</th><th>Pass Criteria</th></tr></thead><tbody><tr><td>1.1</td><td>Set up a basic program that takes readings from a sensor</td><td>I will use the Barometer Sensor (BME280) to take inputs of temperature, pressure and humidity. The device can be used to estimate altitude. I will write the program using C++</td><td>The program should run on the Particle device and I should recieve values on Particle dashboard.</td></tr><tr><td>1.2</td><td>Incoperate other sensors into the program</td><td>Similar to before, I will add other sensor readings to my program and compare their results.</td><td>All of the values should be output onto the Particle dashboard</td></tr><tr><td>1.3</td><td>Check that the values measured on sensors are correct</td><td>In this test I will compare the values from the sensors with manual measurements to check their accuracy.</td><td>All the values should be the same to manual measurements</td></tr></tbody></table>

### Pseudocode

```
baroPin = input
int temp = 0
int humid = 0
int press = 0
int alti = 0

getBaroTemp(){
    activate baroPin sensor
    temp = read temperature
    return temp
}
getBaroPress(){
    activate baroPin sensor
    press = read pressure
    return press
}
getBaroHumid(){
    activate baroPin sensor
    humid = read humidity
    return humid
}

getBaroAlti(){
    activate baroPin sensor
    alti = read altitude (pressure at sea level)
    return alti
}

print getBaroTemp()
print getBaroPress()
print getBaroHumid()
print getBaroAlti()
    
end procedure
```



## Development

### 1.1

In this part of the cycle, I wanted to get used to programming the Particle Argon, and set up a basic program that can collect data from its surroundings.&#x20;

<figure><img src="../.gitbook/assets/Argon cycle 1.jpg" alt=""><figcaption><p>Particle Argon basic setup</p></figcaption></figure>

I first set up the particle argon and attached it to a grove shield board. I then attached a sensor to the 12C\_2 pin, and connected it to a lithium battary so I could test the device wirelessly.

## Data structures

| Structure Name | Structure types  | Description                                                                                               |
| -------------- | ---------------- | --------------------------------------------------------------------------------------------------------- |
| temp           | Integer Variable | Stores the temperature value that is output by the sensor (in degrees celcius)                            |
| humidity       | Integer Variable | Stores the humidity value that is output by the sensor (as a percentage)                                  |
| pressure       | Integer Variable | Stores the atmospheric pressure value that is output by the sensor (in Pascals)                           |
| altitude       | Integer Variable | Stores the approximate altitude based on the value output for pressure and sea level pressure (1013.25Pa) |

The program in C++ that I used to collect data from the BME280 sensor can be seen below.

```cpp
#include "Adafruit_BME280.h"

Adafruit_BME280 bme;

//initialising variables for BME280 sensor
int temp = 0;
int humidity = 0;
int pressure = 0;
int altitude = 0;

void setup() {
  bme.begin();

  //set variables of particle dashboard
  Particle.variable("temp", temp);
  Particle.variable("humidity", humidity);
  Particle.variable("pressure", pressure);
  Particle.variable("altitude", altitude);

  Particle.publish("Weather Station Online :)");
}


void loop() {
  delay(1000)//wait one second between readings
  temp = (int)bme.readTemperature();
  humidity = (int)bme.readHumidity();
  pressure = (int)bme.readPressure();
  altitude = (int)bme.readAltitude(1013.25); //sea level pressure, estimates altitude

  Particle.publish("testing");
  
  

}
```

<figure><img src="../.gitbook/assets/Screenshot 2023-06-04 at 19.11.46.png" alt=""><figcaption><p>Values recieved from sensors</p></figcaption></figure>

When I ran the program, I was sucessfully able to recieve readings for temperature, humidity, pressure and altitude, so the code passed the test. However, I decided to make some slight alterations to the code so that sensor readings can be displayed on the serial monitor.

To set up serial communication, I added the following line to the setup() function.

```cpp
Serial.begin(9600);
```

I then added the following lines of code at the end of loop().

```cpp
Serial.print("Temperature: ");
Serial.println(temp);

Serial.print("Humidity: ");
Serial.println(humidity);

Serial.print("Pressure: ");
Serial.println(pressure);

Serial.print("Altitude: ");
Serial.println(altitude);
```

Once again, the program successful displayed the following values, this time on the serial monitor.

<figure><img src="../.gitbook/assets/Screenshot 2023-06-04 at 20.30.16.png" alt=""><figcaption></figcaption></figure>

### 1.2

In this part of the cycle, I added the air quality monitoring sensor and dust sensor to the weather station. I started by separating each sensor into a different function, so each could be tested individually. After checking that the program for the Barometer Sensor still passed  the test, I moved on to programming the other components.&#x20;

The code for the air quality monitoring and dust level sensor can be seen below:

````cpp
#include "Air_Quality_Sensor.h"
#include <math.h>

void getAirQualityReadings(){
  //Collects readings from sensors
}

void getDustSensorReadings(){
  //Collects readings from sensors
}
```
````

Now I am going to add the light sensor.



### 1.3

To ensure that my weather station works correctly, in this section I am going to check the accuracy of the sensor readings I have collected.

### Outcome

The completed code written that passed this cycle can be seen below.

```
// Some code
```

### Testing

The table below outlines a summary of the results from my testing.

| Test | Instructions                                                                                                                 | What I expect                                                                               | What actually happens                                                                                         | Pass/Fail |
| ---- | ---------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------- | --------- |
| 1    | Compile and flash code to Argon device ([See 1.1](cycle-1.md#1.1))                                                           | Code should compile and flash without errors. Device should then reconnect to the internet. | The program was sucessfully loaded onto the particle device. The Argon connected to the internet as expected. | Pass      |
| 2    | Design a program that collects readings for the barometer sensor (BME280) ([See 1.1](cycle-1.md#1.1), [1.2](cycle-1.md#1.2)) | Data collected should be displayed in serial monitor/ particle console variables            | Recieved readings for temperature, pressure and humidity                                                      | Pass      |
| 3    | Implement other sensors into the program ([See 1.2](cycle-1.md#1.2))                                                         | Collect readings for air quality, dust levels, and light.                                   |                                                                                                               |           |
| 4    | Check that the values measured on sensors are correct ([See 1.3](cycle-1.md#1.3))                                            | Data readings that are close to the manually measured value                                 |                                                                                                               |           |



### Challenges

Description of challenges

### Evidence
