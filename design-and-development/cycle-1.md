# 2.2.1 Cycle 1: Data Collection

## Design

In this first cycle, I have decided to use the Particle Argon (a Wi-Fi enabled development board) to program data collection for my personal weather station. I will use Visual Studio Code as an IDE and the Particle Workbench extension to create my project.&#x20;

I aim to learn how the Particle Argon communicates with and takes readings from sensors to take measurements from the environment. This will include measurements of pressure, humidity, temperature and air quality. Once I have tested how these different sensors work individually, I will design a program that takes readings from these sensors simultaneously.

### Objectives

* [x] Set up basic project with Particle Argon
* [x] Design a program that communicates with sensors using C++
* [x] Design weather station that can upload readings to particle cloud

### Usability Features

In this cycle, I need to ensure that my weather station can easily be used and navigated by a stakeholder. The program should collect accurate data from sensors at regular intervals. It must then be able to upload this data over Wi-Fi. I also need to try to minimise the storage required to run the program.

### Tests I need to conduct

<table><thead><tr><th width="69">ID</th><th>Test</th><th>Description</th><th>Pass Criteria</th></tr></thead><tbody><tr><td>1.1</td><td>Set up a basic program that takes readings from a sensor</td><td>I will use the Barometer Sensor (BME280) to take inputs of temperature, pressure and humidity. The device can be used to estimate altitude. I will write the program using C++.</td><td>The program should run on the Particle device and I should recieve values on Particle dashboard.</td></tr><tr><td>1.2</td><td>Incoperate other sensors into the program</td><td>Similar to before, I will add other sensor readings to my program and compare their results.</td><td>All of the values should be output onto the Particle dashboard</td></tr><tr><td>1.3</td><td>Check that the values measured on sensors are correct</td><td>In this test I will compare the values from the sensors with manual measurements to check their accuracy.</td><td>All the values should be the same to manual measurements</td></tr></tbody></table>

### Pseudocode

In this cycle, my overall aim is to design a program for the Particle Argon that takes readings using sensors from the environment and prints them in the console. This program is vital to the functioning of the weather monitoring and forecasting system as it will allow current conditions to be displayed on the user interface, and predictions to be made using this data. The basic pseudocode for this algorithm can be found below.

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
print soundVal
    
end procedure
```



## Data structures

| Structure Name | Structure Type    | Description                                                                                               |
| -------------- | ----------------- | --------------------------------------------------------------------------------------------------------- |
| temp           | Integer Variable  | Stores the temperature value that is output by the sensor (in degrees celcius)                            |
| humidity       | Integer Variable  | Stores the humidity value that is output by the sensor (as a percentage)                                  |
| pressure       | Integer Variable  | Stores the atmospheric pressure value that is output by the sensor (in Pascals)                           |
| altitude       | Integer Variable  | Stores the approximate altitude based on the value output for pressure and sea level pressure (1013.25Pa) |
| airQuality     | String Variable   | Stores current air quality as either "Fresh Air", "Low Pollution", "High Pollution" or "Dangerous Level"  |
| airQualityVal  | Integer Variable  | Stores the actual reading from the air quality sensor                                                     |
| lastCheck      | Unsigned Long     | Stores a value in miliseconds for when the dust sensor was last checked                                   |
| lpo\_val       | Unsigned Long     | Stores a running total of the duration readings from the dust sensor                                      |
| duration       | Unsigned Long     | Stores how long it takes for the sensor to change between digital LOW to HIGH in microseconds             |
| ratio          | Double Variable   | Stores the percentage of dust filled air that the sensor detects                                          |
| concentration  | Double Variable   | Stores the concentration of dust in the air                                                               |
| lightVal       | Integer Variable  | Stores the light reading from the light sensor                                                            |
| soundVal       | Integer Variable  | Stores the sound reading from the sound sensor                                                            |

## Development

### 1.1

In this part of the cycle, I wanted to get used to programming the Particle Argon, and set up a basic program that can collect data from its surroundings.&#x20;

<figure><img src="../.gitbook/assets/Argon cycle 1.jpg" alt=""><figcaption><p>Particle Argon basic setup</p></figcaption></figure>

I first set up the particle argon and attached it to a grove shield board. I then attached a sensor to the 12C\_2 pin, and connected it to a lithium battary so I could test the device wirelessly. After looking at the basic documentation for using the air quality monitoring sensors, I then designed my own program to collect readings from the environment [(docs.particle.io, 2017)](../evaluation/reference-list.md).&#x20;

When setting up Visual Studio Code, I used the Particle Workbench extension pack. The program in C++ that I used to collect data from the BME280 sensor can be seen below. I used the Adafruit\_BME280 library to help me collect data from these sensors.

<pre class="language-cpp"><code class="lang-cpp">// Temperature, humidity, air pressure and altitude sensor library
<strong>#include "Adafruit_BME280.h"
</strong>
Adafruit_BME280 bme;

//Initialising variables for BME280 sensor
int temp = 0;
int humidity = 0;
int pressure = 0;
int altitude = 0;

void setup() {
  bme.begin();

  //Set variables of particle dashboard
  Particle.variable("temp", temp);
  Particle.variable("humidity", humidity);
  Particle.variable("pressure", pressure);
  Particle.variable("altitude", altitude);

  Particle.publish("Weather Station Online :)");
}


void loop() {
  delay(1000)//Wait one second between readings
  //Assigns temp variable to temperature, collected by sensor
  temp = (int)bme.readTemperature();
  
  //Assigns humidity variable to temperature, collected by sensor
  humidity = (int)bme.readHumidity();
  
  //Assigns pressure variable to temperature, collected by sensor
  pressure = (int)bme.readPressure();
  
  //Assigns altitude variable to temperature, collected by sensor
  altitude = (int)bme.readAltitude(1013.25); //sea level pressure, estimates altitude

  Particle.publish("testing");

}
</code></pre>

<figure><img src="../.gitbook/assets/Screenshot 2023-06-04 at 19.11.46.png" alt=""><figcaption><p>Values recieved from sensors</p></figcaption></figure>

When I ran the program, I was successfully able to receive readings for temperature, humidity, pressure and altitude, so the code passed the test. However, I decided to make some slight alterations to the code so that sensor readings can be displayed on the serial monitor.

To set up serial communication, I added the following line to the setup() function.

```cpp
Serial.begin(9600);
```

I then added the following lines of code at the end of loop().

<pre class="language-cpp"><code class="lang-cpp"><strong>//Prints temperature reading
</strong><strong>Serial.print("Temperature: ");
</strong>Serial.println(temp);

//Prints humidity reading
Serial.print("Humidity: ");
Serial.println(humidity);

//Prints pressure reading
Serial.print("Pressure: ");
Serial.println(pressure);

//Prints altitude reading
Serial.print("Altitude: ");
Serial.println(altitude);
</code></pre>

Once again, the program successfully displayed the following values, this time on the serial monitor.

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

The next sensor I programmed was the air quality sensor, which takes a reading from the environment and assigns the current air quality to either 'Fresh Air',  'Low Pollution', 'High Pollution', or 'Dangerous Level'. This will allow me to provide the stakeholder with information about the afety and air qualitiy of their environment.  I used the Air quality sensor library for this. The code for the air quality monitoring can be seen below.&#x20;

```cpp
//Library for air quality sensor
#include "Air_Quality_Sensor.h"

//Pin for air quality sensor
#define AQS_PIN D2
AirQualitySensor aqSensor(AQS_PIN);

//Initiates variable airQuality
String airQuality = "Loading";

//Function that collects readings from AQ sensor

void getAirQualityReadings(){

  //Gets sensor reading
  int airQualityVal = aqSensor.slope();

  //Assigns air quality
  if (airQualityVal == AirQualitySensor:: FORCE_SIGNAL) {
    //Assigns air quality to dangerous level
    airQuality = "Dangerous Level";
  }
  else if (airQualityVal == AirQualitySensor:: HIGH_POLLUTION) {
    //Assigns air quality to high pollution
    airQuality = "High Pollution";
  }
  else if (airQualityVal == AirQualitySensor:: LOW_POLLUTION) {
    //Assigns air quality to low pollution 
    airQuality = "Low Pollution";
  }
  //Assigns air quality to fresh air
  else if (airQualityVal == AirQualitySensor:: FRESH_AIR) {
    airQuality = "Fresh Air";
  }
  else {
    //A validation check for when the reading for air quality isn't successful
    airQuality = "Reading Unsuccessful";
  }

}

```

After running the code, I was successfully able to get an output for the air quality on the particle console, which can be seen below. The other sensors also continued to work as expected.

<figure><img src="../.gitbook/assets/Screenshot 2023-06-07 at 16.49.28.png" alt=""><figcaption></figcaption></figure>

#### Dust Concentration

The next sensor I will set up is the dust sensor, as it will provide the user with more detailed information about the concentration of dust and pollution in the atmosphere surrounding their local area. According to the particle argon documentation, this sensor requires several formulas to calculate dust concentration, which are found in the code below. I used the math.h library to assist these calculations. The dust concentration is measured by using infrared light at constant intervals. I decided to use an interval of 30000 milliseconds.

<pre class="language-cpp" data-full-width="false"><code class="lang-cpp">// Adds math library for pow()
#include &#x3C;math.h>
// Sets dust sensor to a pin and sets the interval between readings
#define DUST_SENSOR_PIN A2
#define SENSOR_READING_INTERVAL 30000

//Variables used in program
//The miliseconds since the last readings check
unsigned long lastCheck;
//Stores the value for lowpulseoccupancy
unsigned long lpo_val = 0;
//Stores the value for the last lowpulseoccupancy
unsigned long last_lpo = 0;
unsigned long duration;

double ratio = 0;
//Concentration of dust in atmosphere
double concentration = 0;


//Function of collect sensor readings
void getDustSensorReadings(){
  //Prevents the value on the dust sensor from being 0
  if (lpo_val == 0){
    lpo_val = last_lpo;
  }
  else{
    last_lpo = lpo_val;
  }
  
  //Calculates the concentration of dust
  ratio = lpo_val / (SENSOR_READING_INTERVAL * 10.0);
  concentration = 1.1 * pow(ratio, 3) - 3.8 * pow(ratio, 2) + 520 * ratio + 0.62;
}

void setup() {
  
  pinMode(DUST_SENSOR_PIN, INPUT);
  lastCheck = millis();
  
  //Uploads sensor values to particle cloud
<strong>  Particle.variable("lpo", lpo_val);
</strong>  Particle.variable("ratio", ratio);
  Particle.variable("conc", concentration);
}

void loop() {
  duration = pulseIn(DUST_SENSOR_PIN, LOW);
  lpo_val = lpo_val + duration;
  
  //Collects sensor readings at pre-set intervals
  if ((millis() - lastCheck) > SENSOR_READING_INTERVAL){
    getDustSensorReadings(); //Calls dust sensor reading function

    lpo_val = 0;
    lastCheck = millis();
  }
}
</code></pre>

While this program successfully compiled and ran on the Argon device, I had some issues when trying to access readings from sensors on the particle cloud. After going through my code, I identified one line of code in particular which was preventing variables from being accessed.

```cpp
duration = pulseIn(DUST_SENSOR_PIN, LOW);

```

This line of code measures how long it takes for the sensor to change between digital LOW to HIGH. However, when this line of code runs, the rest of the program stops and waits for the sensor to receive an input or timeout. This interrupts the particle console's ability to retrieve values of the variables. To confirm this is the case, I added the following lines of code to output readings in the serial monitor.

```cpp
Particle.variable("lpo", lpo_val);
Particle.variable("ratio", ratio);
Particle.variable("conc", concentration);
```

<figure><img src="../.gitbook/assets/Screenshot 2023-06-09 at 18.04.35.png" alt=""><figcaption></figcaption></figure>

As this was an error with the particle cloud and not the program itself, I ran the same code again and received the following outputs. As expected, I received readings for concentration of dust as well as other sensors. To overcome the issue of uploading to the particle cloud, I instead published data values as events, which can be seen below. I used the Json Parser Generator Library for this. This now passed the test.&#x20;

```cpp
#include "JsonParserGeneratorRK.h"

void loop() {
  //Build JSON object to publish to cloud
  JsonWriterStatic<256> jw;

  {
    JsonWriterAutoObject obj(&jw);

    // Add various types of data
    jw.insertKeyValue("temp", temp);
    jw.insertKeyValue("humidity", humidity);
    jw.insertKeyValue("pressure", pressure);
    jw.insertKeyValue("altitude", altitude);
    jw.insertKeyValue("airQual", airQuality);
    jw.insertKeyValue("lpo_val", lpo_val);
    jw.insertKeyValue("dust_ratio", ratio);
    jw.insertKeyValue("dust_conc", concentration);

  }
  Particle.publish("weatherStationData", jw.getBuffer(), PRIVATE);
}
```

<figure><img src="../.gitbook/assets/Screenshot 2023-06-09 at 18.37.12.png" alt=""><figcaption></figcaption></figure>

#### Light

The next stage to programming my weather station is designing a program that can measure light readings. Light intensity is also another factor that can be monitored in the environment. I decided to include this sensor in my project as it provides the user with personalised information about light which would not be available on a usual monitoring system. This would be especially useful to stakeholders in the agricultural industry. The program for this can be seen below.

<pre class="language-cpp"><code class="lang-cpp">// Set light pin to A0
int lightPin = A0;

// Initilise lightVal variable
int lightVal = 0;

void getLightReadings() {
  //collects light reading from sensors
  lightVal = analogRead(lightPin);
}

void setup() {
  //Setup serial monitor
<strong>  Serial.begin(9600);
</strong><strong>  
</strong>  //Setup light sensor
  pinMode(lightPin, INPUT);
}

void loop() {
  //call function to get light readings
  getLightReadings();
  
  //Print light values in serial monitor
  Serial.print("Light: ");
  Serial.println(lightVal);
}
</code></pre>

After checking that I was receiving readings, I compared sensor readings in dark and light conditions to ensure the sensor was working correctly.

<figure><img src="../.gitbook/assets/Screenshot 2023-06-09 at 19.22.27.png" alt=""><figcaption></figcaption></figure>

#### Sound

The next aspect of my weather station which a stakeholder may wish to monitor in their local environment is sound. I attached this sensor to the A4 pin and added the following code below to collect these readings.

```cpp
// SOUND SENSOR

//Sets sound pin to A4
int soundPin = A4;

//Initilise soundVal variable
int soundVal;

void getSoundReadings() {
  // Collects sound readings from sensor
  soundVal = analogRead(soundPin);
}

void setup() {
  //Setup serial monitor
  Serial.begin(9600);
  
  // Setup sound sensor
  pinMode(soundPin, INPUT);
}

void loop() {
  // Calls sound reading function
  getSoundReadings();
  
  //Print light values in serial monitor
  Serial.print("Sound: ");
  Serial.println(soundVal);
}
```

After running this code, I received a value for sound in the serial monitor. I then compared sensor readings in loud and quiet areas to ensure the sensor was working correctly. These outputs can be seen below.

<figure><img src="../.gitbook/assets/Screenshot 2023-06-10 at 15.19.06.png" alt=""><figcaption></figcaption></figure>

### 1.3

In this section I am going to check the accuracy of readings from the sensors, and ensure that my program works correctly. I am going to test that the values for sound, light, temperature, humidity, pressure, altitude, air quality and dust concentration to ensure they are correct and accurate. For this test, I kept my program on the argon the same and viewed the sensor readings in the serial monitor.

To test that the sound, altitude and light sensors were working correctly, I compared their readings in different environments.

<figure><img src="../.gitbook/assets/Screenshot 2023-06-21 at 14.03.12.png" alt=""><figcaption><p>Passed Test</p></figcaption></figure>

To test for sound, I compared the readings when in constant, quiet conditions, with the changing volumes of music. Readings stayed constant when recording sound in an unchanging environment, while readings rise and fell to different volumes of sound in the music. These readings can be seen below.

<figure><img src="../.gitbook/assets/Screenshot 2023-06-21 at 14.15.40.png" alt=""><figcaption><p>Passed test</p></figcaption></figure>

To test for altitude, I compared the readings in different locations. As there was only a small change in altitude, I wasn't expecting a large difference to be detected by the sensors. However, my weather station was able to detect the differences between the two environments, which can be seen below.

<figure><img src="../.gitbook/assets/Screenshot 2023-06-21 at 14.24.44.png" alt=""><figcaption><p>Passed test</p></figcaption></figure>

In the final part of my accuracy testing, I compared the readings from the barometer temperature sensor with the ones I manually measured.&#x20;

<figure><img src="../.gitbook/assets/Screenshot 2023-06-21 at 14.47.25.png" alt=""><figcaption><p>Passed Test</p></figcaption></figure>

### Outcome

Throughout this cycle, I have successfully managed to design a program that takes several readings from the environment, including:

* Temperature
* Humidity
* Pressure
* Altitude
* Air quality
* Dust concentration
* Light
* Sound

After testing all the sensors individually, I combined the programs to produce one program that can take readings from the attached sensors and upload the data values wirelessly to the particle cloud.&#x20;

In terms of hardware, the layout of my weather station can be seen below.&#x20;

<figure><img src="../.gitbook/assets/Screenshot 2023-06-10 at 16.05.55.png" alt=""><figcaption><p>Personal Weather Station Design at the end of cycle 1</p></figcaption></figure>

### Testing

The table below outlines a summary of the results from my testing.

| Test | Instructions                                                                                                                 | What I expect                                                                               | What actually happens                                                                                                      | Pass/Fail |
| ---- | ---------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------- | --------- |
| 1    | Compile and flash code to Argon device ([See 1.1](cycle-1.md#1.1))                                                           | Code should compile and flash without errors. Device should then reconnect to the internet. | The program was sucessfully loaded onto the particle device. The Argon connected to the internet as expected.              | Pass      |
| 2    | Design a program that collects readings for the barometer sensor (BME280) ([See 1.1](cycle-1.md#1.1), [1.2](cycle-1.md#1.2)) | Data collected should be displayed in serial monitor/ particle console variables.           | Recieved readings for temperature, pressure and humidity.                                                                  | Pass      |
| 3    | Implement other sensors into the program ([See 1.2](cycle-1.md#1.2))                                                         | Collect readings for air quality, dust levels, light and sound.                             | I managed to sucessfully publish readings to the particle events log.                                                      | Pass      |
| 4    | Check that the values measured on sensors are correct ([See 1.3](cycle-1.md#1.3))                                            | Data readings that are close to the manually measured value.                                | When comparing readings in different conditions and using manually measured values, readings had a high degree of accuracy | Pass      |



### Challenges

In this cycle I was successfully able to design a program that is able to take readings from the surrounding environment. One of the main challenges I faced was ensuring that the code in C++ was syntactically correct before compiling and flashing it to the particle device. I initially had some trouble when incorporating the dust levels sensor into my program, as waiting to receive readings from sensors interrupted the connection with the particle cloud. However, I was able to resolve this issue by outputting data into the particle events console in the JSON parse format.

Another issue that I have had with this cycle is connecting the particle device to the Wi-Fi, especially when working in different locations. As the other aspects of my project are dependent on this system working correctly, I need to ensure I am able to run this program with minimal errors.

{% embed url="https://github.com/Lucyhub30/cycle1weatherstation" %}
Link to github page
{% endembed %}
