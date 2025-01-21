# Configuración global para todas las subredes
default-lease-time 600;        # Tiempo de concesión predeterminado en segundos
max-lease-time 7200;           # Tiempo máximo de concesión en segundos
authoritative;                 # Indica que este servidor DHCP es el principal en la red

# Configuración de subred para 192.168.1.0/24
subnet 192.168.1.0 netmask 255.255.255.0 {
    range 192.168.1.100 192.168.1.200;  # Rango de direcciones IP a asignar
    option routers 192.168.1.1;         # Puerta de enlace predeterminada
    option domain-name-servers 8.8.8.8, 8.8.4.4;  # Servidores DNS
    option domain-name "example.com";   # Nombre de dominio
    option broadcast-address 192.168.1.255; # Dirección de difusión
}

# Configuración de una subred adicional (por ejemplo, 192.168.2.0/24)
subnet 192.168.2.0 netmask 255.255.255.0 {
    range 192.168.2.100 192.168.2.150;  # Rango de IPs para esta subred
    option routers 192.168.2.1;
    option domain-name-servers 8.8.8.8, 8.8.4.4;
}

# Asignación estática para un cliente específico
host printer1 {
    hardware ethernet 00:11:22:33:44:55; # Dirección MAC del dispositivo
    fixed-address 192.168.1.50;          # Dirección IP asignada estáticamente
}


### **Guía para configurar ISC DHCP Server en Linux**

Esta guía describe cómo configurar un servidor DHCP utilizando **isc-dhcp-server**. El servidor DHCP asignará direcciones IP, máscara de subred, puerta de enlace, DNS, y más, a los dispositivos en una red local.

---

### **1. Instalación de ISC DHCP Server**

1. **Instalar el paquete:**
   ```bash
   sudo apt update
   sudo apt install isc-dhcp-server
   ```

2. **Verificar el estado del servicio:**
   ```bash
   sudo systemctl status isc-dhcp-server
   ```

---

### **2. Configuración del archivo principal: `/etc/dhcp/dhcpd.conf`**

Este archivo contiene las opciones principales para configurar el servidor DHCP.

**Ubicación:**  
`/etc/dhcp/dhcpd.conf`

#### **Ejemplo de configuración con comentarios:**

```bash
# Configuración global para todas las subredes
default-lease-time 600;        # Tiempo de concesión predeterminado en segundos
max-lease-time 7200;           # Tiempo máximo de concesión en segundos
authoritative;                 # Indica que este servidor DHCP es el principal en la red

# Configuración de subred para 192.168.1.0/24
subnet 192.168.1.0 netmask 255.255.255.0 {
    range 192.168.1.100 192.168.1.200;  # Rango de direcciones IP a asignar
    option routers 192.168.1.1;         # Puerta de enlace predeterminada
    option domain-name-servers 8.8.8.8, 8.8.4.4;  # Servidores DNS
    option domain-name "example.com";   # Nombre de dominio
    option broadcast-address 192.168.1.255; # Dirección de difusión
}

# Configuración de una subred adicional (por ejemplo, 192.168.2.0/24)
subnet 192.168.2.0 netmask 255.255.255.0 {
    range 192.168.2.100 192.168.2.150;  # Rango de IPs para esta subred
    option routers 192.168.2.1;
    option domain-name-servers 8.8.8.8, 8.8.4.4;
}

# Asignación estática para un cliente específico
host printer1 {
    hardware ethernet 00:11:22:33:44:55; # Dirección MAC del dispositivo
    fixed-address 192.168.1.50;          # Dirección IP asignada estáticamente
}
```

---

### **3. Archivo de configuración de la interfaz de red: `/etc/default/isc-dhcp-server`**

Este archivo define en qué interfaces de red se ejecutará el servidor DHCP.

**Ubicación:**  
`/etc/default/isc-dhcp-server`

#### **Ejemplo de configuración con comentarios:**

```bash
# Interfaces en las que el servidor DHCP escuchará y responderá
INTERFACESv4="enp0s3"  # Cambia "enp0s3" por el nombre de tu interfaz de red
INTERFACESv6=""
```

Para obtener el nombre de tu interfaz de red, ejecuta:
```bash
ip a
```

---

### **4. Comprobación de configuración**

Antes de iniciar el servicio, verifica que no haya errores en la configuración.

1. **Verificar el archivo `dhcpd.conf`:**
   ```bash
   sudo dhcpd -t -cf /etc/dhcp/dhcpd.conf
   ```

2. **Revisar permisos y rutas:**  
   Asegúrate de que el servidor tenga acceso a la interfaz de red configurada.

---

### **5. Iniciar y habilitar el servicio**

1. **Iniciar el servidor DHCP:**
   ```bash
   sudo systemctl start isc-dhcp-server
   ```

2. **Habilitar el servicio para que inicie automáticamente al arrancar:**
   ```bash
   sudo systemctl enable isc-dhcp-server
   ```

3. **Revisar el estado del servicio:**
   ```bash
   sudo systemctl status isc-dhcp-server
   ```

---

### **6. Archivos de registro**

Los registros del servidor DHCP se encuentran en:
```bash
/var/log/syslog
```
Para revisar los eventos relacionados con DHCP:
```bash
sudo tail -f /var/log/syslog
```

---

### **7. Opciones avanzadas**

#### **Asignación basada en clases de clientes:**
Puedes asignar configuraciones específicas según el tipo de cliente.
```bash
class "VoIPPhones" {
    match if substring (option vendor-class-identifier, 0, 6) = "VoIPPh";
}
subnet 192.168.3.0 netmask 255.255.255.0 {
    range 192.168.3.100 192.168.3.120;
    allow members of "VoIPPhones";
}
```

#### **Opciones específicas (PXE Boot, etc.):**
Por ejemplo, para configurar arranque por red (PXE):
```bash
filename "pxelinux.0";       # Archivo de arranque
next-server 192.168.1.2;     # Dirección IP del servidor TFTP
```

---

### **8. Solución de problemas comunes**

- **El servidor no asigna IPs:**
  - Asegúrate de que el rango IP no esté en conflicto con direcciones estáticas.
  - Verifica que el servidor esté escuchando en la interfaz correcta (`/etc/default/isc-dhcp-server`).
  - Comprueba que el firewall permita el tráfico DHCP:
    ```bash
    sudo ufw allow 67/udp
    ```

- **Conflictos de IP:**
  - Usa asignaciones estáticas para dispositivos importantes (como impresoras o servidores).

---

### **Conclusión:**
Con esta configuración, tendrás un servidor DHCP funcional utilizando **isc-dhcp-server**. Puedes personalizarlo según tus necesidades, agregar múltiples subredes o configuraciones avanzadas como PXE boot. Si necesitas ayuda con una parte específica, ¡avísame! 😊