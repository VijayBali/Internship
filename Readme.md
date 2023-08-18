# Synbiosys Internship

## Generative design assignment- Laptop stand

In order to create a laptop stand which can withstand significant force, I began creating many models using generative design. 


Generative design is an AI-driven approach that uses algorithms to explore and produce innovative design solutions, optimizing for specific criteria. The criteria includes materials, manufacturing techniques, the magnitude and direction of a force acting on the object, the set objectives and more. 

When creating designs, the AI was given the objective to minimise the quantity of material used while maintining adequate structural integrity. The weight of the laptop was also applied on each design in the form of Newtons(N).

When evaluating the prototypes, I considered a variety of factors. Firstly, I evaluated stand's ability to withstand forces (e.g. compressive, tensile, torsional, etc.) This included the properties of the materials when withstanding these forces, such as the toughness of the structure (ability to withstand impacts without cracking/breaking) and the hardness of the structure (the abilty to withstand scratching/wear).

I also considered other important factors such as aesthetics, stability, safety (influenced by factors such as the quantity of sharp edges) and ergonomics.

### Prototype 1:
<img width="400" alt="image" src="https://github.com/VijayBali/Synbiosys-internship/assets/140536734/30c9cbcc-2c7d-4496-b975-b73651173ea6">

When testing, it was found that the prototype had many benefitial physical properties. 
Firstly, the stand was very strong and was able to withstand great amounts of compressive force.
Additionally, the prototype was well- supported and was thus very resistant to bending and torsion from all angles. 
The length of the stand was also able to accommodate a variety of laptop sizes and the extrusion at the bottom of the stand was able to secure the laptop well.

Unfortunately, the product also suffered from certain flaws. Firstly, the frame was unnecessarily thick, resulting in a wasteful use of materials. Secondly, the prototype was excessively narrow, and had a small area of contact between with the ground, resulting in instability. Additionally, the branches connecting the base and the platform were frail, as they were long and narrow, and thus, unable to withstand a powerful impact from the side. Finally, the prototype was not very aesthetically appealing and did not fully take advantage of the facinating nature of generative design.

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


