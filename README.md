 # EXPERIMENT-03-INTERFACING-DIGITAL-SENSOR-WITH-EDGE-DEVELOPMENT-BOARD-

---

### **NAME: ARJUN K 
### **DEPARTMENT: CSE
### **ROLL NO: 212224040028
### **DATE OF EXPERIMENT: 10.03.2025  

---

## **AIM:**  
To interface an **MPU6050 6-Axis Accelerometer & Gyroscope Sensor** with the **Raspberry Pi Pico** and display the sensor readings using MicroPython.

---

## **APPARATUS REQUIRED:**  
1. Raspberry Pi Pico  
2. MPU6050 6-Axis Accelerometer & Gyroscope Sensor  
3. Jumper Wires  
4. Breadboard  
5. USB Cable  
6. Computer with Thonny IDE  

---

## **THEORY:**  
### **About Raspberry Pi Pico:**  
Raspberry Pi Pico is a microcontroller board based on the **RP2040 chip**. It features:  
- Dual-core ARM Cortex-M0+ processor  
- 26 multi-function GPIO pins  
- Supports **MicroPython** and **C/C++**  
- Interfaces like **I2C, SPI, UART, and PWM**  
- Low power consumption, ideal for **IoT applications**  

### **About MPU6050 Sensor:**  
The **MPU6050** is a **6-Axis Inertial Measurement Unit (IMU)** that includes:  
- **3-axis accelerometer** and **3-axis gyroscope**  
- **I2C communication protocol** for easy interfacing  
- **Operating Voltage:** 3.3V – 5V  
- **Accelerometer Range:** ±2g, ±4g, ±8g, ±16g  
- **Gyroscope Range:** ±250°/s, ±500°/s, ±1000°/s, ±2000°/s  

The **accelerometer** measures linear acceleration in **X, Y, Z axes**, while the **gyroscope** measures rotational velocity. The sensor communicates with the Raspberry Pi Pico via **I2C protocol**.

---

## **WORKING PRINCIPLE:**  
1. The **MPU6050 sensor** is connected to the **Raspberry Pi Pico** using the **I2C communication protocol**.  
2. The **Pico reads acceleration and gyroscope values** from the sensor registers.  
3. The data is **processed and displayed on the serial monitor**.  
4. The readings can be used for **motion tracking, tilt sensing, or gesture recognition**.

---

## **CIRCUIT DIAGRAM:
![Screenshot 2025-03-24 105240](https://github.com/user-attachments/assets/e1d30bb9-181f-4367-ac08-0733c82ceced)

### **Connections:

| MPU6050 Pin | Raspberry Pi Pico Pin |
|------------|----------------------|
| VCC | 3.3V |
| GND | GND |
| SDA | GP20 |
| SCL | GP21 |

---

## **PROGRAM (MicroPython)**  
```
from machine import Pin, I2C
import utime

# MPU6050 I2C address
MPU6050_ADDR = 0x68

# MPU6050 Registers
PWR_MGMT_1 = 0x6B
ACCEL_XOUT_H = 0x3B
GYRO_XOUT_H = 0x43

# INITIALIZE I2C
sda = Pin(0)  # Define your SDA pin
scl = Pin(1)  # Define your SCL pin
i2c = I2C(0, scl=scl, sda=sda, freq=400000)

def mpu6050_init():
    i2c.writeto_mem(MPU6050_ADDR, PWR_MGMT_1, b'\x00')  # Corrected method name

def read_raw_data(reg):
    data = i2c.readfrom_mem(MPU6050_ADDR, reg, 2)
    value = (data[0] << 8) | data[1]
    if value > 32767:
        value -= 65536
    return value

def get_sensor_data():
    accel_x = read_raw_data(ACCEL_XOUT_H) / 16384.0
    accel_y = read_raw_data(ACCEL_XOUT_H + 2) / 16384.0
    accel_z = read_raw_data(ACCEL_XOUT_H + 4) / 16384.0

    gyro_x = read_raw_data(GYRO_XOUT_H) / 131.0
    gyro_y = read_raw_data(GYRO_XOUT_H + 2) / 131.0
    gyro_z = read_raw_data(GYRO_XOUT_H + 4) / 131.0

    return (accel_x, accel_y, accel_z, gyro_x, gyro_y, gyro_z)

# Initialize MPU6050
mpu6050_init()

while True:
    ax, ay, az, gx, gy, gz = get_sensor_data()
    print(f"Accel: X={ax:.2f}g, Y={ay:.2f}g, Z={az:.2f}g | Gyro: X={gx:.2f}°/s, Y={gy:.2f}°/s, Z={gz:.2f}°/s")
    utime.sleep(1)
```

## **OUTPUT:**  
When the above program is executed, the output on the serial monitor will display real-time acceleration and gyroscope values, such as:
```
Accel: X=0.02g, Y=-0.01g, Z=1.00g | Gyro: X=0.05°/s, Y=-0.02°/s, Z=0.01°/s
Accel: X=0.03g, Y=-0.02g, Z=1.01g | Gyro: X=0.06°/s, Y=-0.03°/s, Z=0.02°/s
...
```
output 1:
![Screenshot 2025-03-10 113200](https://github.com/user-attachments/assets/f24bbdcc-2383-4c65-a00c-c9aa1c854051)
output 2:
![Screenshot 2025-03-10 113430](https://github.com/user-attachments/assets/90c2df57-4066-44d4-b952-a386c5d624b7)
output 3:
![Screenshot 2025-03-10 113538](https://github.com/user-attachments/assets/c48a4655-939f-4133-b4fb-87188940f6ee)h
output 4:
![Screenshot 2025-03-10 113608](https://github.com/user-attachments/assets/e18eb5db-5116-47f6-aa12-2fe7a25699da)
---

## **RESULT:**  
The **MPU6050 sensor** was successfully interfaced with the **Raspberry Pi Pico**, and real-time **acceleration and gyroscope data** were read and displayed. The sensor values can be used for **motion tracking, tilt detection, and gesture control applications**.

---

