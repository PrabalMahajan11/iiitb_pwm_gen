# iiitb_pwm_gen -> Pulse Width Modulated Wave Generator with Variable Duty Cycle
This project simulates the designed Pulse Width Modulated Wave Generator with Variable Duty Cycle. We can generate PWM wave and varry its DUTYCYCLE in steps of 10%.

## 1. Introduction to PWM Generator
Pulse Width Modulation is a famous technique used to create modulated electronic pulses of the desired width. The duty cycle is the ratio of how long that PWM signal stays at the high position to the total time period.![pwm](https://github.com/PrabalMahajan11/iiitb_pwm_gen/assets/100370090/8c7a5a06-e3de-422c-9d5a-798512dbfe89)

## 2. Applications
Pulse Width Modulated Wave Generator can be used to
- control the brightness of the LED
- drive buzzers at different loudnes
- control the angle of the servo motor
- encode messages in telecommunication
- used in speed controlers of motors

## 3. Block Diagram of PWM Generator
This PWM generator generates 10Mhz signal. We can control duty cycles in steps of 10%. The default duty cycle is 50%. Along with clock signal we provide another two external signals to increase and decrease the duty cycle.

![BasicBlockedDeagram](https://github.com/PrabalMahajan11/iiitb_pwm_gen/assets/100370090/af440fa6-4220-4390-a286-0ae447277f97)

In this specific circuit, we mainly require a n-bit counter and comparator. Duty given to the comparator is compared with the current value of the counter. If current value of counter is lower than duty then comparator results in output high. Similarly, If current value of counter is higher than duty is then comparator results in output low. As counter starts at zero, initially comparator gives high output and when counter crosses duty it becomes low. Hence by controlling duty, we can control duty cycle.
![BlockDiagram](https://github.com/PrabalMahajan11/iiitb_pwm_gen/assets/100370090/f6cb3183-715a-4a01-b97d-2b901fa5bf08)
As the comparator is a combinational circuit and the counter is sequential, while counting from 011 to 100 due to improper delays there might be an intermediate state like 111 which might be higher or lower than duty. This might cause a glitch. To avoid these glitches output of the comparator is passed through a D flipflop.

## 4. Functional Simulation
### 4.1 About iVerilog
Icarus Verilog is an implementation of the Verilog hardware description language.

### 4.2 About GTKWave
GTKWave is a fully featured GTK+ v1. 2 based wave viewer for Unix and Win32 which reads Ver Structural Verilog Compiler generated AET files as well as standard Verilog VCD/EVCD files and allows their viewing

### 4.3 Installing iVerilog and GTKWave
#### For Ubuntu
Open your terminal and type the following to install iverilog and GTKWave

```
$   sudo apt-get update
$   sudo apt-get install iverilog gtkwave
```
