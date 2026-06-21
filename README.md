# Análisis de Incidente de Ciberseguridad: Aplicación del NIST CSF

## Resumen del Incidente
Un actor malicioso ejecutó un ataque de Denegación de Servicio (DoS) sostenido contra la red interna de la empresa multimedia. El ataque consistió en una inundación masiva de peticiones ICMP (Ping Flood). Esto saturó el ancho de banda y la capacidad de procesamiento de la red, provocando una caída total de los servicios internos durante un periodo de dos horas. La interrupción impidió temporalmente que el equipo accediera a recursos críticos de red, afectando la continuidad del negocio hasta que el equipo de gestión de incidentes logró bloquear el tráfico malicioso y restaurar las operaciones.

---

## 1. Identificar 
* **Tipo de ataque:** Ataque de Denegación de Servicio (DoS) mediante una inundación ICMP (ICMP Flood).
* **Sistemas afectados:** La infraestructura de red interna completa de la empresa, incluyendo enrutadores, conmutadores (switches) y los servicios críticos que dependen de dicha conectividad para la operación diaria de los equipos de diseño y marketing.
* **Vulnerabilidad raíz:** Un cortafuegos perimetral mal configurado que permitía el paso irrestricto de tráfico ICMP entrante desde fuentes externas no confiables.

## 2. Proteger 
Para proteger los activos contra incidentes futuros similares, se deben implementar las siguientes modificaciones a la infraestructura:
* **Configuración Estricta del Cortafuegos:** Implementar una nueva regla en el cortafuegos para limitar la tasa (Rate Limiting) de los paquetes ICMP entrantes. Adicionalmente, configurar el cortafuegos para verificar la dirección IP de origen y descartar paquetes con IPs falsificadas (Spoofed IPs).
* **Principio de Mínimo Privilegio en Red:** Bloquear completamente las peticiones ICMP externas (como `ping` o `traceroute`) a menos que sean explícitamente necesarias para un servicio específico o diagnósticos autorizados.

## 3. Detectar 
Para garantizar una identificación temprana de futuras anomalías en el tráfico de red, se implementarán los siguientes métodos:
* **Sistemas IDS/IPS:** Desplegar un Sistema de Detección y Prevención de Intrusiones (IDS/IPS) perimetral configurado para filtrar e inspeccionar el tráfico entrante, buscando firmas conocidas de ataques DoS y características sospechosas en los paquetes ICMP.
* **Monitorización Continua (SIEM):** Implementar software de supervisión de red para establecer una línea base (baseline) del tráfico normal. Esto permitirá la detección automatizada y en tiempo real de picos anómalos o volúmenes masivos de tráfico que se desvíen del comportamiento esperado.

## 4. Responder 
Ante la detección de un ataque DoS en curso, el plan de respuesta inmediata debe ser:
* **Contención:** Aislar el tráfico malicioso instruyendo al cortafuegos para que bloquee (Drop) temporalmente todas las peticiones ICMP de las direcciones IP atacantes o de zonas geográficas sospechosas.
* **Mitigación Operativa:** Desconectar temporalmente de la red todos los servicios no esenciales para reducir la superficie de ataque y asegurar que el ancho de banda restante y los recursos de procesamiento se destinen a mantener los servicios críticos en línea.

## 5. Recuperar 
Una vez neutralizado el ataque, los procesos de recuperación incluirán:
* **Restauración Gradual:** Volver a poner en línea los servicios no críticos de forma progresiva, validando el rendimiento de la red y la integridad de los datos en cada etapa.
* **Auditoría Post-Incidente (Lecciones Aprendidas):** Analizar los registros (logs) del cortafuegos y del IDS/IPS para entender la magnitud del ataque, identificar fallas en los tiempos de respuesta y actualizar las políticas de seguridad (playbooks) para optimizar la defensa en futuros eventos.
