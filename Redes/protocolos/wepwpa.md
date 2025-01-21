# Tabla **WPA2** y **WPA3**:

| **Característica**          | **WEP**                        | **WPA**                         | **WPA2**                        | **WPA3**                        |
|-----------------------------|---------------------------------|----------------------------------|----------------------------------|----------------------------------|
| **Introducción**            | 1997                           | 2003                            | 2004                            | 2018                            |
| **Cifrado principal**        | RC4 con clave estática         | RC4 con TKIP (clave dinámica)   | AES con CCMP                    | AES con GCMP-256                |
| **Protocolo de seguridad**  | Ninguno                        | TKIP                            | CCMP (más seguro que TKIP)       | SAE (Simultaneous Authentication of Equals) |
| **Seguridad**               | Muy débil                      | Moderada, pero obsoleta         | Alta, pero ya vulnerable a ataques avanzados | Muy alta, resistente a ataques modernos |
| **Estado actual**           | Obsoleto, no seguro            | Reemplazado por WPA2            | Aún usado, pero desplazado por WPA3 | Recomendado para redes modernas |
| **Características clave**   | Clave estática y débil         | Claves dinámicas, compatible con WEP | Soporte para AES y claves robustas | Mejoras en autenticación, cifrado más fuerte y mayor protección contra ataques offline |

---

### **Explicación adicional de WPA2 y WPA3:**

1. **WPA2 (Wi-Fi Protected Access 2):**
   - Introdujo **AES (Advanced Encryption Standard)** como estándar de cifrado, reemplazando RC4.
   - Utiliza **CCMP (Counter Mode with Cipher Block Chaining Message Authentication Code Protocol)** para proteger la integridad y confidencialidad de los datos.
   - A pesar de ser seguro, es vulnerable a ataques avanzados como **KRACK (Key Reinstallation Attack)** si no se actualiza correctamente.

2. **WPA3:**
   - Diseñado para abordar las debilidades de WPA2.
   - Introduce **SAE (Simultaneous Authentication of Equals)**, que reemplaza la autenticación basada en PSK (Pre-Shared Key), haciendo más difícil descifrar contraseñas mediante ataques de fuerza bruta.
   - Ofrece **Forward Secrecy**, asegurando que los datos capturados no puedan descifrarse incluso si se compromete la clave privada más tarde.
   - Mayor resistencia a ataques offline y configuración simplificada para dispositivos IoT con **Wi-Fi Easy Connect**.

---

### **Conclusión:**
- **WPA3** es el estándar recomendado actualmente por su mayor seguridad y resistencia a ataques modernos.
- **WPA2** sigue siendo ampliamente utilizado, pero es importante actualizar los dispositivos para mitigar vulnerabilidades.
- **WEP** y **WPA** son obsoletos y no deben usarse bajo ninguna circunstancia. 
