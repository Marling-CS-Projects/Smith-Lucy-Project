# 2.2.1 Cycle 1: Data Collection

## Design

In this first cycle, I have decided to use the Particle Argon (a Wi-Fi enabled development board) to program data collection for my personal weather station. I will use Visual Studio Code as an IDE and the Particle Workbench extension to create my project.&#x20;

I aim to learn how the Particle Argon communicates with and takes readings from sensors to take measurements from the environment. This will include measurements of pressure, humidity, temperature and air quality. Once I have tested how these different sensors work individually, I will design a program that takes readings from these sensors simultaneously.

### Objectives

* [ ] Set up basic project with Particle Argon
* [ ] Design a program that communicates with sensors using C++

### Usability Features

In this cycle, I need to ensure that my weather station can easily be used and navigated by a stakeholder. The program should collect accutrate data from sensors at regular intervals. It must then be able to upload this data over Wi-Fi. I also need to try to minimise the storage required to run the program.

### Tests I need to conduct

<table><thead><tr><th width="69">ID</th><th>Test</th><th>Description</th><th>Pass Criteria</th></tr></thead><tbody><tr><td>1.1</td><td>Set up a basic program that takes readings from a sensor</td><td>I will use the Barometer Sensor (BME200) to take inputs of temperature, pressure and humidity. The device can be used to estimate altitude. I will write the program using C++</td><td>The program should run on the Particle device and I should recieve values on Particle dashboard.</td></tr><tr><td>1.2</td><td>Incoperate other sensors into the program</td><td>Similar to before, I will add other sensor readings to my program and compare their results.</td><td>All of the values should be output onto the Particle dashboard</td></tr><tr><td>1.3</td><td>Check that the values measured on sensors are correct</td><td>In this test I will compare the values from the sensors with manual measurements to check their accuracy.</td><td>All the values should be the same to manual measurements</td></tr></tbody></table>

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

### Outcome

### Challenges

Description of challenges

## Testing

Evidence for testing

### Tests

| Test | Instructions  | What I expect     | What actually happens | Pass/Fail |
| ---- | ------------- | ----------------- | --------------------- | --------- |
| 1    | Run code      | Thing happens     | As expected           | Pass      |
| 2    | Press buttons | Something happens | As expected           | Pass      |

### Evidence
