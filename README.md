# Sistema de Control y Medición de Nivel de Agua con IoT con ESP32 y MQTT

Sistema de instrumentación y automatización IoT para la medición continua de nivel de líquidos sin contacto, control automatizado de bombeo y visualización en tiempo real a través de una interfaz web basada en el protocolo MQTT.

---

## Descripción del proyecto

El sistema realiza el cálculo del nivel de líquidos utilizando un sensor ultrasónico **HC-SR04** y acciona una bomba DC mediante un driver **L298N**. Toda la telemetría se publica en la nube a través del protocolo **MQTT**, permitiendo el monitoreo remoto y el control manual/automático desde un dashboard en la web.

### Características principales

* **Medición sin contacto:** Medición continua de altura y porcentaje de agua.
* **Telemetría e integración IoT:** Transmisión de datos via MQTT utilizando WebSockets.
* **Dashboard web interactiva:** Interfaz gráfica responsive desarrollada en HTML5/JavaScript para visualización y control.
* **Control dual (auto/manual):** Ciclo automático de llenado con anulación manual desde la web.

---

## Arquitectura de software y hardware

### Hardware

* ESP32 (Microcontrolador principal)
* Sensor ultrasónico HC-SR04
* Módulo driver puente H L298N
* Bomba de agua 12V DC & fuente externa
* Divisor de voltaje resistivo ($1\text{ k}\Omega$ / $2\text{ k}\Omega$)

### Software & protocolos

* **Lenguajes:** C++ (arduino framework)
* **Protocolo de red:** Wi-Fi / MQTT sobre WebSockets
* **Frontend web:** HTML5, CSS3 y JavaScript (Librería `mqtt.js`)

---

## Tópicos MQTT

| Tópico | Dirección | Descripción |
| :--- | :--- | :--- |
| `proyecto/tanque/nivel` | ESP32 ---> Web | Porcentaje actual de llenado ($0\% - 100\%$) |
| `proyecto/tanque/altura` | ESP32 ---> Web | Añtura del agua calculada en centímetros |
| `proyecto/tanque/bomba` | ESP32 ---> Web | Estado de funcionamiento del actuador |
| `proyecto/tanque/comando` | ESP32 ---> Web | Comando manual de accionamiento (`ON` / `OFF`) |

## Esquema de conexiones

Enlace al diagrama de conexiones:

Fuente 12V ---> VMS (L298N)
Fuente 12V ---> GND (L298N) ---> GND (ESP32) GND unificado

[L298N 5V Out] ---> VIN / 5V (ESP32) & VCC (HC-SR04)
[L298N OUR1/2] ---> Bomba de agua DC

ESP32 GPIO 26 ---> IN1 (L298N)
ESP32 GPIO 27 ---> IN2 (L298N)
ESP32 GPIO 14 ---> ENA (L298N) [PWM]

ESP32 GPIO 05 ---> TRIG (HC-SR04)
ESP32 GPIO 18 <--- [R1:$1\text{ k}\Omega$] --- ECHO (HC-SR04) --- [R2:$2\text{ k}\Omega$] ---> GND

---

## Despliegue e instalación

1. **ESP32:** carga el código en la tarjeta configurando tus credenciales de Wi-Fi
2. **Dashboard web:** abre el archivo `index.html` en cualquier navegador web.
3. El dahsboard se conectará automáticamente al broker MQTT para empezar a recibir lecturas y enviar comandos al ESP32.

---

## Autor

* **Juan Albrin Meza Guzmán** - *Estudiante de Ingeniería Electrónica (UNAD) y ADSO (SENA).
* **GitHub:** [@Jalbrin445](https://github.com/Jalbrin445)
