# Cuaderno-de-Ejercicios-2-Parte

1. **Explicar sobre el siguiente grafo de red los conceptos de circuito virtual entre el nodo 10 y el 6, y datagrama entre el nodo 3 y el 6.**  
   Asumir arcos bidireccionales.

2. **Partiendo de la anterior red, generar las tablas de cada uno de los nodos de un circuito virtual entre los nodos 3 y 4.**  
   Tener en cuenta que los CV creados del anterior ejercicio siguen presentes.  
   Asumir arcos bidireccionales.

3. **Aplicar los algoritmos indicados en los siguientes grafos, tomando como partida el nodo 5.**  
   Indicar iteración a iteración lo que ocurre. Asumir arcos bidireccionales.  
   a) Algoritmo de Dijkstra.  
   b) Algoritmo de inundación hasta el nodo 6. Contar el total de paquetes generados.

4. **Diseñar las tablas de rutado jerárquico de la siguiente red.**

5. **En un sistema con las siguientes características, indicar las máscaras utilizadas, la primera y última dirección de los equipos, y los elementos/dispositivos necesarios:**
   - 7900 profesionales con datos sensibles.
   - 54 subredes para sensorización (100 sensores por subred).
   - 1290 tomas Ethernet.
   - 75000 clientes externos que deberán tener acceso a Internet por wifi.
   - Switches de máximo 24 bocas.

6. **Calcular la eficiencia de un sistema basado en UDP/IP desde un mensaje de la capa de aplicación de 1000000 bytes, sabiendo que la longitud máxima del campo de datos es:**
   - UDP: 65527 bytes. Cabecera de 8 bytes.
   - IP: 65535 bytes. Cabecera de 4 bytes.
   - Ethernet: 1500 bytes. Cabecera de 8 bytes.

7. **Con los datos del ejercicio anterior, calcular la eficiencia de un sistema RTP/UDP, con cabecera RTP de 12 bytes y 65535 bytes de datos.**

8. **Realizar el pseudocódigo de un cliente y servidor UDP/TCP que devuelva la hora de una región determinada.**

## 🖥️ Servidor TCP/UDP

### Entrada:
- Puerto de escucha
- Protocolo (TCP o UDP)

### Comportamiento:
- Espera conexiones o datagramas
- Recibe una solicitud con el nombre de la región horaria
- Devuelve la hora actual de esa región

### Pseudocódigo:

INICIAR servidor(protocolo, puerto)

SI protocolo == "TCP":
CREAR socket TCP en puerto
ESCUCHAR conexiones entrantes
MIENTRAS VERDADERO:
ACEPTAR conexión de cliente
RECIBIR nombre_region
OBTENER hora_actual = obtener_hora_por_region(nombre_region)
ENVIAR hora_actual al cliente
CERRAR conexión
SINO SI protocolo == "UDP":
CREAR socket UDP en puerto
MIENTRAS VERDADERO:
RECIBIR nombre_region desde cliente (direccion, puerto_cliente)
OBTENER hora_actual = obtener_hora_por_region(nombre_region)
ENVIAR hora_actual a direccion:puerto_cliente
FIN

FUNCION obtener_hora_por_region(region):
SI region ES válida:
hora_local = obtener_fecha_y_hora(region)
RETORNAR hora_local como string
SINO:
RETORNAR "Región no válida"

---

## 💻 Cliente TCP/UDP

### Entrada:
- Dirección del servidor
- Puerto
- Nombre de la región

### Comportamiento:
- Envía la región al servidor
- Recibe la hora actual de esa región
- La muestra por pantalla

### Pseudocódigo:

INICIAR cliente(protocolo, direccion_servidor, puerto, region)

SI protocolo == "TCP":
CREAR socket TCP
CONECTAR a direccion_servidor:puerto
ENVIAR region
RECIBIR hora
MOSTRAR hora
CERRAR conexión
SINO SI protocolo == "UDP":
CREAR socket UDP
ENVIAR region a direccion_servidor:puerto
RECIBIR hora desde servidor
MOSTRAR hora
FIN

9. **Indicar el procedimiento a seguir mediante el protocolo DNS para obtener la dirección IP de la siguiente URL:** `iuj.ac.jp`

    ## Procedimiento DNS paso a paso

1. **Consulta del cliente (resolutor DNS local)**
   - El cliente (por ejemplo, tu navegador o sistema operativo) solicita la dirección IP de `iuj.ac.jp`.
   - Esta solicitud se dirige al **resolver DNS configurado localmente** (normalmente el DNS del ISP o un servidor como 8.8.8.8 de Google).

2. **Verificación de caché local**
   - El resolutor comprueba si la IP de `iuj.ac.jp` ya está en **caché**.
   - Si está en caché y no ha expirado (TTL), se devuelve la IP directamente.
   - Si **no está en caché**, se continúa con la resolución.

3. **Consulta al servidor raíz**
   - El resolutor envía una consulta a un **servidor raíz DNS**.
   - El servidor raíz no conoce la IP de `iuj.ac.jp`, pero responde con la dirección de los **servidores TLD (Top-Level Domain)** para `.jp`.

4. **Consulta al servidor TLD (`.jp`)**
   - El resolutor ahora consulta al servidor DNS TLD para `.jp`.
   - Este servidor responde con la dirección del **servidor autoritativo** para el dominio `ac.jp`.

5. **Consulta al servidor autoritativo (`ac.jp`)**
   - El resolutor consulta al servidor autoritativo para `ac.jp`.
   - Este responde con la dirección del **servidor autoritativo para `iuj.ac.jp`** (o directamente con el registro A si lo tiene).

6. **Consulta final al servidor autoritativo de `iuj.ac.jp`**
   - El resolutor pregunta directamente al servidor DNS autoritativo de `iuj.ac.jp`.
   - Este responde con el **registro A** (IPv4) o **AAAA** (IPv6), que contiene la **dirección IP** de `iuj.ac.jp`.

7. **Entrega de la IP al cliente**
   - El resolutor entrega la IP al cliente original.
   - El cliente puede ahora iniciar la conexión (por ejemplo, HTTP o HTTPS) al servidor web de `iuj.ac.jp`.

8. **Almacenamiento en caché**
   - El resolutor y el sistema local **almacenan en caché** la respuesta por un tiempo definido (TTL) para acelerar futuras consultas.

---

10. **Indicar el intercambio de mensajes completo que se produce desde que se introduce la URL www.twitch.tv hasta que empieza a visualizarse una transmisión en directo.**

## 1. Entrada de la URL

**Acción:** El usuario introduce `www.twitch.tv` en el navegador y presiona Enter.

---

## 2. Resolución DNS

**Protocolo:** DNS (UDP/53 o TCP/53)

1. El navegador consulta la caché del sistema operativo.
2. Si no está resuelta:
   - Se consulta al **resolver DNS local**.
   - Este realiza consultas recursivas (a raíz, TLD `.tv`, y servidores autoritativos de `twitch.tv`).
3. El **registro A (IPv4)** o **AAAA (IPv6)** de `www.twitch.tv` es devuelto con la dirección IP del servidor web.

---

## 3. Establecimiento de conexión TCP y negociación TLS

**Protocolos:**
- TCP (puerto 443)
- TLS/SSL (para HTTPS)

1. El navegador abre una conexión **TCP (3-way handshake)** con el servidor:
   - SYN → 
   - SYN-ACK ← 
   - ACK →

2. Luego se establece una conexión segura **TLS** (por ejemplo, TLS 1.3):
   - Se intercambian certificados, claves y algoritmos.
   - Se verifica la identidad del servidor (`twitch.tv`).

---

## 4. Petición HTTP(S)

**Protocolo:** HTTPS (HTTP/2 o HTTP/3 sobre QUIC)

1. El navegador envía una **solicitud GET** a `https://www.twitch.tv/`:
   ```http
   GET / HTTP/2
   Host: www.twitch.tv
   User-Agent: ...
   Accept: text/html, application/json
