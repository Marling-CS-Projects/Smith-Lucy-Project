# 2.2.1 Cycle 1: Data Collection

## Design

In this first cycle, I have decided to use the Particle Argon (a Wi-Fi enabled development board) to program data collection for my personal weather station. I will use Visual Studio Code as an IDE and the Particle Workbench extension to create my project.&#x20;

I aim to learn how the Particle Argon communicates with and takes readings from sensors to take measurements from the environment. This will include measurements of pressure, humidity, temperature and air quality. Once I have tested how these different sensors work individually, I will design a program that takes readings from these sensors simultaneously.

### Objectives

* [x] Set up basic project with Particle Argon
* [x] Design a program that communicates with sensors using C++
* [ ] Design weather station that can upload readings to particle cloud

### Usability Features

In this cycle, I need to ensure that my weather station can easily be used and navigated by a stakeholder. The program should collect accutrate data from sensors at regular intervals. It must then be able to upload this data over Wi-Fi. I also need to try to minimise the storage required to run the program.

### Tests I need to conduct

<table><thead><tr><th width="69">ID</th><th>Test</th><th>Description</th><th>Pass Criteria</th></tr></thead><tbody><tr><td>1.1</td><td>Set up a basic program that takes readings from a sensor</td><td>I will use the Barometer Sensor (BME280) to take inputs of temperature, pressure and humidity. The device can be used to estimate altitude. I will write the program using C++</td><td>The program should run on the Particle device and I should recieve values on Particle dashboard.</td></tr><tr><td>1.2</td><td>Incoperate other sensors into the program</td><td>Similar to before, I will add other sensor readings to my program and compare their results.</td><td>All of the values should be output onto the Particle dashboard</td></tr><tr><td>1.3</td><td>Check that the values measured on sensors are correct</td><td>In this test I will compare the values from the sensors with manual measurements to check their accuracy.</td><td>All the values should be the same to manual measurements</td></tr></tbody></table>

### Pseudocode

```
int temp = 0
int humidity = 0
int pressure = 0
int altitude = 0
float concentration = 0
int light = 0
int sound = 0

getBarometerReadings(){
    activate baroPin sensor
    temp = read temperature
    humidity = read humidity
    pressure = read pressure
    altitude = calculate altitude
}
getAirQualityReadings(){
    activate air quality sensor
    airQuality = read air quality
    
}
getDustSensorReadings(){
    activate dust sensor
    concentration = read dust
}

getLightSensorReadings(){
    activate light sensor
    light = read light
}

getSoundSensorReadings(){
    activate sound sensor
    sound = read sound
    

print temp
print humidity
print pressure
print altitude
print airQuality
print concentration
print lightVal
    
end procedure
```



## Data structures

| Structure Name | Structure types  | Description                                                                                               |
| -------------- | ---------------- | --------------------------------------------------------------------------------------------------------- |
| temp           | Integer Variable | Stores the temperature value that is output by the sensor (in degrees celcius)                            |
| humidity       | Integer Variable | Stores the humidity value that is output by the sensor (as a percentage)                                  |
| pressure       | Integer Variable | Stores the atmospheric pressure value that is output by the sensor (in Pascals)                           |
| altitude       | Integer Variable | Stores the approximate altitude based on the value output for pressure and sea level pressure (1013.25Pa) |

## Development

### 1.1

In this part of the cycle, I wanted to get used to programming the Particle Argon, and set up a basic program that can collect data from its surroundings.&#x20;

<figure><img src="../.gitbook/assets/Argon cycle 1.jpg" alt=""><figcaption><p>Particle Argon basic setup</p></figcaption></figure>

I first set up the particle argon and attached it to a grove shield board. I then attached a sensor to the 12C\_2 pin, and connected it to a lithium battary so I could test the device wirelessly.

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

<pre class="language-cpp"><code class="lang-cpp"><strong>Serial.print("Temperature: ");
</strong>Serial.println(temp);

Serial.print("Humidity: ");
Serial.println(humidity);

Serial.print("Pressure: ");
Serial.println(pressure);

Serial.print("Altitude: ");
Serial.println(altitude);
</code></pre>

Once again, the program successful displayed the following values, this time on the serial monitor.

<figure><img src="../.gitbook/assets/Screenshot 2023-06-04 at 20.30.16.png" alt=""><figcaption></figcaption></figure>

### 1.2

In this part of the cycle, I added the air quality monitoring sensor and dust sensor to the weather station. I decided to use procedures to separate each sensor. This will make it easier to test components individually and debug my code without affecting the other data readings.

#### Temperature, Humidity, Pressure and Altitude

```cpp
void getBarometerReadings() {
  //Collects readings from sensors
  temp = (int)bme.readTemperature();
  humidity = (int)bme.readHumidity();
  pressure = (int)bme.readPressure();
  altitude = (int)bme.readAltitude(1013.25);
}
```

After checking that the program for the Barometer Sensor still passed, I moved on to programming the other components.

#### Air Quality

The next sensor I programmed was the air quality sensor, which takes a reading from the environment and assigns the current air quality to either 'Fresh Air',  'Low Polution', 'High Polution', or 'Dangerous Level'. The code for the air quality monitoring can be seen below.&#x20;

```cpp
//Library for air quality sensor
#include "Air_Quality_Sensor.h"

//Pin for air quality sensor
#define AQS_PIN A2
AirQualitySensor aqSensor(AQS_PIN);

//Initiates variable airQuality
String airQuality = "Loading";

//Function that collects readings from AQ sensor

void getAirQualityReadings(){

  //Gets sensor reading
  int airQualityVal = aqSensor.slope();

  //Assigns air quality
  if (airQualityVal == AirQualitySensor:: FORCE_SIGNAL) {
    airQuality = "Dangerous Level";
  }
  else if (airQualityVal == AirQualitySensor:: HIGH_POLLUTION) {
    airQuality = "High Polution";
  }
  else if (airQualityVal == AirQualitySensor:: LOW_POLLUTION) {
    airQuality = "Low Polution";
  }
  else if (airQualityVal == AirQualitySensor:: FRESH_AIR) {
    airQuality = "Fresh Air";
  }
  else {
    airQuality = "Reading Unsuccessful";
  }

}

```

After running the code, I was successfully able to get an output for the air quality on the particle console, which can be seen below. The other sensors also continued to work as expected.

<figure><img src="../.gitbook/assets/Screenshot 2023-06-07 at 16.49.28.png" alt=""><figcaption></figcaption></figure>

#### Dust Concentration

<pre class="language-cpp"><code class="lang-cpp">// Adds math library for pow()
#include &#x3C;math.h>
// Sets dust sensor to a pin and sets the interval between readings
#define DUST_SENSOR_PIN A2
#define SENSOR_READING_INTERVAL 30000

//variables used in program
unsigned long lastCheck;
unsigned long lowpulseoccupancy = 0;
unsigned long last_lpo = 0;
unsigned long duration;

double ratio = 0;
double concentration = 0;


//function of collect sensor readings
void getDustSensorReadings(){
  //prevents the value on the dust sensor from being 0
  if (lowpulseoccupancy == 0){
    lowpulseoccupancy = last_lpo;
  }
  else{
    last_lpo = lowpulseoccupancy;
  }
  
  //Calculates the concentration of dust
  ratio = lowpulseoccupancy / (SENSOR_READING_INTERVAL * 10.0);
  concentration = 1.1 * pow(ratio, 3) - 3.8 * pow(ratio, 2) + 520 * ratio + 0.62;
}

void setup() {
  
  pinMode(DUST_SENSOR_PIN, INPUT);
  lastCheck = millis();
  
  //Uploads sensor values to particle cloud
<strong>  Particle.variable("lpo", lowpulseoccupancy);
</strong>  Particle.variable("ratio", ratio);
  Particle.variable("conc", concentration);
}

void loop() {
  duration = pulseIn(DUST_SENSOR_PIN, LOW);
  lowpulseoccupancy = lowpulseoccupancy + duration;
  
  //Collects sensor readings at pre-set intervals
  if ((millis() - lastCheck) > SENSOR_READING_INTERVAL){
    getDustSensorReadings(); //Calls dust sensor reading function

    lowpulseoccupancy = 0;
    lastCheck = millis();
  }
}
</code></pre>

While this program sucessfully compiled and ran on the Argon device, I had some issues when trying to access readings from sensors on the particle cloud. After going through my code, I identified one line of code in particular which was preventing variables from being accessed.

```cpp
duration = pulseIn(DUST_SENSOR_PIN, LOW);
```

This line of code measures how long it takes for the sensor to change between digital LOW to high. However, when this line of code runs, the rest of the program stops and waits for the sensor to recive and input or time out. This interrupts the particle console's ability to retrieve values of the variables. To confirm this is the case, I added the following lines of code to output readings in the serial monitor.

```cpp
Particle.variable("lpo", lowpulseoccupancy);
Particle.variable("ratio", ratio);
Particle.variable("conc", concentration);
```

/Image of serial monitor...

As this was an error with the particle cloud and not the program itself, I ran the same code again and recieved the following outputs. As expected, I recived readings for concentration of dust as well as other sensors. To overcome the issue of uploading to the particle cloud, I instead published data values as events, which can be seen below. This now passed the test.&#x20;

/Image of particle cloud events log...

#### Light

The next stage to programming my weather station is designing a program that can measure light readings.

#### Sound

The final sensor I am adding to my weather station is for sound.



### 1.3

To ensure that my weather station works correctly, in this section I am going to check the accuracy of the sensor readings I have collected.

### Outcome

Throughout this cycle, I have successfully managed to design a program that takes several readings from the enrivoment, including:

* Temperature
* Humidity
* Pressure
* Altitude
* Air quality
* Dust concentration
* Light
* Sound

After testing all the sensors individually, I combinded the programs to produce one program that can take readings from the attached sensors and upload the data values wirelessly to the particle cloud. The final design for the device itself can be seen below.&#x20;

The completed code written that passed this cycle can be seen below.

```cpp
// Some code
```

### Testing

The table below outlines a summary of the results from my testing.

| Test | Instructions                                                                                                                 | What I expect                                                                               | What actually happens                                                                                                   | Pass/Fail |
| ---- | ---------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------- | --------- |
| 1    | Compile and flash code to Argon device ([See 1.1](cycle-1.md#1.1))                                                           | Code should compile and flash without errors. Device should then reconnect to the internet. | The program was sucessfully loaded onto the particle device. The Argon connected to the internet as expected.           | Pass      |
| 2    | Design a program that collects readings for the barometer sensor (BME280) ([See 1.1](cycle-1.md#1.1), [1.2](cycle-1.md#1.2)) | Data collected should be displayed in serial monitor/ particle console variables.           | Recieved readings for temperature, pressure and humidity.                                                               | Pass      |
| 3    | Implement other sensors into the program ([See 1.2](cycle-1.md#1.2))                                                         | Collect readings for air quality, dust levels, light and sound.                             | After having a slight issue with the dust sensor, I managed to sucessfully publish readings to the particle events log. | Pass      |
| 4    | Check that the values measured on sensors are correct ([See 1.3](cycle-1.md#1.3))                                            | Data readings that are close to the manually measured value.                                |                                                                                                                         |           |



### Challenges

Description of challenges

### Evidence
