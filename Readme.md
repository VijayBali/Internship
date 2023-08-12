# Synbiosys Internship

## Generative design assignment- Laptop stand

In order to create a laptop stand which can withstand significant force, I began creating many models using generative design. 


Generative design is an AI-driven approach that uses algorithms to explore and produce innovative design solutions, optimizing for specific criteria. The criteria includes materials, manufacturing techniques, the magnitude and direction of a force acting on the object, the set objectives and more. 

When creating designs, the AI was given the objective to minimise the quantity of material used while maintining adequate structural integrity. The weight of the laptop was also applied on each design in the form of Newtons(N).

### Prototype 1:
<img width="400" alt="image" src="https://github.com/VijayBali/Synbiosys-internship/assets/140536734/30c9cbcc-2c7d-4496-b975-b73651173ea6">

#### Strengths of the prototype: 
- The stand is very strong and can withstand great amounts of compressive force.
- The prototype is well- supportedand is thus very resistant to bending and torsion from all angles.
- The extrusion at the bottom of the stand was able to secure the laptop well
- The length of the stand is able to accommodate a variety of laptop sizes

#### Shortcomings of the prototype:
- The frame is unnecessarily thick and is a waste of material
- The prototype is overly narrow and has a small area in contact with the laptop and the ground. This causes it to be unstable.
- The branches connecting the baseand the platform are frail, as they are long and narrow, and thus, unable to withstand a powerful impact from the side. 
- The prototype is not very aesthetically appealing and does not fully take advantage of the facinating nature of generative design.

### Prototype 2:
#### Changes:
- The laptop stand is wider with both the platform and the base.
- The base was wider than the platform. This has been done in order to increase stability while preventing unnecessary contact with the base of the laptop, which could reduce the dissipation of heat in the laptop. Additionally, this has been done to create a more interesting prototype. 
- The generative design programme was given far less constraints, and more space to use in order to create a more facinating design.
- The unnused area on the base was removed to aesthetically differentiate the product.


### Prototype 3:

### Prototype 3 v2:
Generative design laptop stand link: https://www.thingiverse.com/thing:6141502




## Programming assignment
I was assigned the task of creating a portable device, which when plugged in, would graph the live temperature and humidity on a computer screen. This is in order to notice trends in the data when conducting experiments. 
To do this, I integrated both hardware and software.

### Hardware
For my task, I was given an Arduino Nano microcontroller and an Adafruit AHT20 Temperature and Humidity Sensor. 



### Software
```python
import matplotlib.pyplot as plt
import matplotlib.animation as animation
from matplotlib import style
import numpy as np
import random
import serial

ser = serial.Serial("/dev/cu.usbserial-1110", 115200, timeout=0.1)   

def readserial():

    if ser.inWaiting()>0:
        line=ser.readline().decode().strip()
        return line
    
    return None

# Create figure for plotting
# fig = plt.figure()
# ax = fig.add_subplot(1, 1, 1)
fig, ax = plt.subplots()
ax.set_ylabel('Temperature(ºC)')
ax2 = ax.twinx()
ax2.set_ylabel('Humidity(%)',c='r')
ax2.yaxis.set_label_position('right')
ax2.yaxis.set_ticks_position('right')

xs = [] #store temp trials here 
ys = [] #store temp relative frequency here

xh = [] #store humidity trials here 
yh = [] #store humidity relative frequency here

# This function is called periodically from FuncAnimation
def animate(i, xs, ys, xh, yh):

    #Aquire and parse data from serial port
    line = readserial()
    if line is not None:
        first_character = line[0]
        if first_character == 't':
            tempValue = line[1:]
            tempValue = float(tempValue)
            ys.append(tempValue)
            xs.append(len(ys))
        elif first_character == 'h':
            humidValue = line [1:]
            humidValue = float(humidValue)
            yh.append(humidValue)
            xh.append(len(yh))


    # Limit x and y lists to 30 items
    xs = xs[-30:]
    ys = ys[-30:]

    xh = xh[-30:]
    yh = yh[-30:]

    # Draw x and y lists
    ax.clear()
    ax.plot(xs, ys, label="Data line")
    ax.set_ylabel('Temperature(ºC)')

    # secondary y-axis
    ax2.clear()
    ax2.plot(xh, yh, color='r')
    ax2.set_ylabel('Humidity(%)',c='r')
    ax2.tick_params(axis='y', labelcolor='r')
    ax2.yaxis.set_label_position('right')
    ax2.yaxis.set_ticks_position('right')

    # Format plot
    plt.xticks(rotation=45, ha='right')
    plt.subplots_adjust(bottom=0.30)
    plt.title('Live data')
    plt.legend()

# Set up plot to call animate() function periodically
ani = animation.FuncAnimation(fig, animate, fargs=(xs, ys, xh, yh), interval=20)
plt.show()
```


