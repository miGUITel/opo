# DNS GUÍA BIND9

### **Guía para configurar BIND9 como servidor secundario y ejemplos de registros avanzados**

Esta guía incluye la configuración básica y cómo configurar un servidor DNS secundario. Además, se añaden ejemplos de otros tipos de registros (SRV, TXT) y la función del símbolo `@` en los archivos de zona.

---

[oppciones `/etc/bind/named.conf.options`](#2-etcbindnamedconfoptions-arriba) [más](#4-configuración-de-forwarders-etcbindnamedconfoptions-arriba)

[declaración de zonas `/etc/bind/named.conf.local`](#3-etcbindnamedconflocal-arriba) [más](#1-archivo-etcbindnamedconflocal-arriba)

[configuración de cada zona `/etc/bind/db.midominio`](#5-etcbinddbmidominio-arriba) [más](#2-archivo-de-zona-directa-arriba)

### Archivos principales de configuración de BIND9:
##### 1. **`/etc/bind/named.conf`**
   - **AGRUPA EL RESTO DE ARCHIVOS**
   - Archivo principal de configuración de BIND9.
   - Incluye otros archivos de configuración mediante directivas `include`.
   - Es el punto de entrada para toda la configuración.

##### 2. **`/etc/bind/named.conf.options`** [arriba](#dns-guía-bind9)
   - Archivo donde se configuran las opciones globales.
   - Aquí se definen aspectos como:
     - **Forwarders**.
     - Opciones de caché.
     - DNSSEC.
     - Interfaces de escucha.
   - Es donde normalmente configuras los servidores DNS alternativos.

##### 3. **`/etc/bind/named.conf.local`** [arriba](#dns-guía-bind9)
   - **Aquí se definen las zonas que habrá en el servidor en los ejercicios.**
   - Archivo para configuraciones locales específicas.
   - Se utiliza para definir **zonas locales** o personalizadas, como zonas *directas* o *inversas*.
   - Por ejemplo puedo decir que voy a tener una zona *midominio.com* y otra zona *extra.es*

##### 4. **`/etc/bind/named.conf.default-zones`**
   - **NORMALMENTE NO SE TOCA**
   - Archivo que contiene las configuraciones de zonas predeterminadas.
   - Define las zonas para resolver LOS SERVIDORES RAIZ, nombres locales como `localhost` y direcciones como `127.0.0.1`.

### Archivos de zonas:
##### 5. **`/etc/bind/db.midominio`** [arriba](#dns-guía-bind9)
   - **SOA, NS, A, AAAA**
   - Un archivo para cada zona
   - Archivo que contiene la configuración de la zona para `localhost`.
   - Es un archivo de ejemplo de una zona directa.

### **1. Archivo `/etc/bind/named.conf.local`** [arriba](#dns-guía-bind9)
Este archivo define las zonas que manejará el servidor (midominio.com). Aquí configuraremos una **zona primaria** y una **zona secundaria**.

**Ubicación:**  
`/etc/bind/named.conf.local`

#### **Ejemplo de configuración con comentarios:**

```bash
# Configuración de una zona primaria (servidor maestro)
zone "example.com" {
    type master;                # Este servidor es el maestro para la zona
    file "/etc/bind/db.example.com";  # Archivo de registros de la zona directa
};

# Configuración de una zona secundaria (servidor esclavo)
zone "secondary.com" {
    type slave;                 # Este servidor es el secundario para la zona
    masters { 192.168.1.10; };  # Dirección IP del servidor maestro
    file "/var/cache/bind/db.secondary.com";  # Archivo donde se almacenan los datos replicados
};

# Zona inversa primaria para resolución IP -> nombre
zone "1.168.192.in-addr.arpa" {
    type master;
    file "/etc/bind/db.192.168.1";
};

# Zona inversa secundaria
zone "2.168.192.in-addr.arpa" {
    type slave;
    masters { 192.168.1.10; };  # Dirección IP del servidor maestro
    file "/var/cache/bind/db.192.168.2";  # Archivo donde se replican los datos
};
```

---

### **2. Archivo de zona directa** [arriba](#dns-guía-bind9)
El archivo de zona directa contiene los registros que permiten resolver nombres de dominio en direcciones IP.

**Ubicación:**  
`/etc/bind/db.example.com`

#### **Ejemplo de configuración con comentarios:**

```bash
$TTL 86400                     ; Tiempo de vida predeterminado para los registros en segundos (24 horas)

@       IN  SOA  ns1.example.com. admin.example.com. (
              2025012101       ; Número de serie (año, mes, día, iteración)
              3600             ; Refresh: Tiempo para sincronizar secundarios (1 hora)
              1800             ; Retry: Tiempo de espera para reintento (30 min)
              1209600          ; Expire: Tiempo antes de invalidar la zona (14 días)
              86400            ; Minimum TTL: Tiempo de vida para respuestas negativas (24 horas)
)

; Definición de servidores de nombres
@       IN  NS   ns1.example.com.      ; Servidor de nombres primario
@       IN  NS   ns2.example.com.      ; Servidor de nombres secundario

; Registros A (asociación de nombres a direcciones IP)
@       IN  A    192.168.1.10          ; Dirección IP de example.com
ns1     IN  A    192.168.1.10          ; Dirección IP del servidor DNS primario
ns2     IN  A    192.168.1.11          ; Dirección IP del servidor DNS secundario
www     IN  A    192.168.1.20          ; Dirección IP del servidor web
mail    IN  A    192.168.1.30          ; Dirección IP del servidor de correo

; Registros A para los nombres del SRV
ldap    IN A 192.168.1.40      ; Dirección IP del servidor LDAP
sip     IN A 192.168.1.50      ; Dirección IP del servidor SIP

; Registro MX (correo electrónico) ****MX Y TXT SE APLICAN A TODO EL DOMINIO, POR ESO EMPIEZAN POR @****
@       IN  MX   10 mail.example.com.  ; Prioridad 10 para el servidor de correo

; Registro TXT (texto descriptivo o verificaciones)
@       IN  TXT  "v=spf1 mx ~all"      ; Política SPF para evitar correos no autorizados

; Registro SRV (servicio específico)
_ldap._tcp IN  SRV 10 5 389 ldap.example.com. ; Servidor LDAP en el puerto 389
_sip._udp  IN  SRV 10 5 5060 sip.example.com. ; Servidor SIP para VoIP
```

---

### **Función del símbolo `@`:**

El símbolo `@` se utiliza para representar el **nombre del dominio principal de la zona** (definido en el archivo de configuración de la zona). En este ejemplo, **`@`** equivale a **`example.com`**.

Por ejemplo:
- `@ IN A 192.168.1.10` → Define la dirección IP principal para `example.com`.
- `@ IN MX 10 mail.example.com.` → Define el servidor de correo para `example.com`.

Esto permite evitar escribir el nombre completo repetidamente, facilitando la lectura y mantenimiento.

---

### **3. Archivo de zona inversa** [arriba](#dns-guía-bind9)
El archivo de zona inversa permite resolver direcciones IP en nombres de dominio.

**Ubicación:**  
`/etc/bind/db.192.168.1`

#### **Ejemplo de configuración con comentarios:**

```bash
$TTL 86400                     ; Tiempo de vida predeterminado para los registros

@       IN  SOA  ns1.example.com. admin.example.com. (
              2025012101       ; Número de serie
              3600             ; Refresh: 1 hora
              1800             ; Retry: 30 minutos
              1209600          ; Expire: 14 días
              86400            ; Minimum TTL: 24 horas
)

; Servidores de nombres para la zona inversa
@       IN  NS   ns1.example.com.
@       IN  NS   ns2.example.com.

; Registros PTR (IP -> nombre de dominio)
10      IN  PTR  example.com.          ; 192.168.1.10 → example.com
20      IN  PTR  www.example.com.      ; 192.168.1.20 → www.example.com
30      IN  PTR  mail.example.com.     ; 192.168.1.30 → mail.example.com
```

---

### **4. Configuración de forwarders `/etc/bind/named.conf.options`** [arriba](#dns-guía-bind9)

```bash
options {
    directory "/var/cache/bind";

    // Forwarders: servidores DNS externos a los que se reenviarán consultas
    forwarders {
        8.8.8.8;    // Google Public DNS
        1.1.1.1;    // Cloudflare DNS
        9.9.9.9;    // Quad9 DNS
    };

    // Usar únicamente los forwarders para la resolución de nombres, 
    forward only; //borrar para que solo se usen en caso de no tener la respuesta
};
```

### **4. Configuración como servidor secundario** [arriba](#dns-guía-bind9)
Para configurar el servidor como secundario:
1. Añade las zonas secundarias en el archivo `/etc/bind/named.conf.local`.
2. Asegúrate de que el servidor maestro permita transferencias de zona:
   - En el maestro, en el archivo `/etc/bind/named.conf.local`, añade:
     ```bash
     allow-transfer { 192.168.1.11; };  # Permitir transferencias al servidor secundario
     ```
3. Asegúrate de que el archivo del secundario (`/var/cache/bind/db.secondary.com`) exista o se cree automáticamente tras la transferencia.

---

### **5. Verificación y reinicio** [arriba](#dns-guía-bind9)

1. Verifica la configuración:
   ```bash
   sudo named-checkconf
   sudo named-checkzone example.com /etc/bind/db.example.com
   sudo named-checkzone 1.168.192.in-addr.arpa /etc/bind/db.192.168.1
   ```

2. Reinicia el servicio:
   ```bash
   sudo systemctl restart bind9
   ```