# **Estándar ZigBee**

ZigBee es un estándar de comunicación inalámbrica basado en **IEEE 802.15.4**, diseñado para redes de baja potencia, bajo costo y bajo ancho de banda. Es especialmente útil en aplicaciones de domótica, automatización industrial, sensores, y dispositivos del Internet de las Cosas (IoT).

---

### **Características principales:**

1. **Arquitectura:**
   - **Capa física y de enlace (IEEE 802.15.4):**
     - Define la transmisión de datos en bandas de frecuencia como:
       - **2.4 GHz:** Frecuencia global, con una tasa de datos de hasta **250 kbps**.
       - **868 MHz:** Utilizada en Europa, con una tasa de datos más baja (20 kbps).
       - **915 MHz:** Usada en América, con una tasa de hasta **40 kbps**.
   - **Capas superiores (ZigBee):**
     - Gestionan funciones como el enrutamiento, la seguridad, y las aplicaciones.

2. **Topologías de red:**
   ZigBee soporta diversas topologías, como:
   - **Estrella:** Un coordinador central conectado a varios dispositivos finales.
   - **Árbol:** Jerárquica, con routers extendiendo el alcance de la red.
   - **Malla:** Permite que cualquier nodo retransmita datos, aumentando la fiabilidad y el alcance.

3. **Clasificación de dispositivos en la red:**
   - **Coordinador:** Configura y controla la red.
   - **Router:** Retransmite datos para extender el alcance.
   - **Dispositivo final:**  
     - Enfocado en tareas específicas, como medir temperatura, encender luces, o detectar movimiento.  
     - **Sí puede enviar y recibir datos**, pero no retransmite mensajes de otros dispositivos.  
     - Ejemplo: Un sensor de temperatura (dispositivo final) envía lecturas al coordinador, mientras una lámpara inteligente puede recibir instrucciones de encendido/apagado.  
     - Este diseño ayuda a minimizar el consumo de energía, permitiendo que los dispositivos finales funcionen durante largos períodos con baterías pequeñas.

4. **Consumo de energía bajo:**
   - Ideal para dispositivos que funcionan con baterías durante largos períodos (como sensores).

5. **Capacidad de nodos:**
   - Una red ZigBee puede soportar hasta **65,000 nodos**, dependiendo de la configuración.

---

### **Ventajas de ZigBee:**

1. **Bajo consumo de energía:**  
   Dispositivos pueden durar años con una batería pequeña.
   
2. **Redes de malla confiables:**  
   Los nodos pueden reenviar datos, asegurando comunicación incluso si algún nodo falla.

3. **Costo reducido:**  
   Diseñado para ser asequible, lo que lo hace ideal para aplicaciones masivas como IoT.

4. **Interoperabilidad:**  
   Los dispositivos ZigBee de diferentes fabricantes pueden trabajar juntos si cumplen con el estándar.

5. **Alcance aceptable:**  
   Un nodo ZigBee tiene un alcance típico de 10-100 metros, ampliable con routers en una red de malla.

---

### **Desventajas de ZigBee:**

1. **Bajo ancho de banda:**  
   No es adecuado para aplicaciones que requieren transmitir grandes cantidades de datos, como video en tiempo real.
   
2. **Competencia de frecuencia:**  
   La banda de 2.4 GHz puede tener interferencias por otras tecnologías como Wi-Fi y Bluetooth.

3. **Limitación en la transmisión:**  
   No es ideal para largas distancias sin usar routers adicionales.

---

### **Aplicaciones de ZigBee:**

1. **Domótica:**
   - Control de luces, termostatos, cerraduras, y electrodomésticos.
   
2. **Automatización industrial:**
   - Supervisión y control de procesos en fábricas.

3. **Medición inteligente:**
   - Sensores para medir consumo eléctrico, agua y gas.

4. **Redes de sensores:**
   - Sensores de temperatura, humedad, o detección de movimiento.

5. **Aplicaciones IoT:**
   - Conexión de dispositivos inteligentes en hogares y empresas.

---

### **Comparación con otras tecnologías:**

| **Aspecto**        | **ZigBee**          | **Wi-Fi**            | **Bluetooth**        |
|---------------------|---------------------|-----------------------|----------------------|
| **Consumo de energía** | Muy bajo           | Alto                  | Moderado             |
| **Rango típico**    | 10-100 metros       | 50-200 metros         | 1-10 metros          |
| **Tasa de datos**   | Hasta 250 kbps      | Hasta 1-10 Gbps       | Hasta 3 Mbps         |
| **Redes soportadas**| Malla, estrella, árbol | Infraestructura       | Punto a punto o malla |
| **Aplicaciones típicas** | IoT, sensores      | Transmisión de video  | Dispositivos personales |

---

### **Resumen:**
ZigBee es una tecnología ideal para aplicaciones IoT que requieren bajo consumo de energía, escalabilidad y comunicación confiable. Los dispositivos finales pueden enviar y recibir datos para cumplir con tareas específicas, pero no retransmiten información, optimizando así el uso de energía en la red. Su diseño basado en redes de malla y su enfoque en eficiencia lo hacen una opción clave para la automatización y el monitoreo en entornos domésticos e industriales.

