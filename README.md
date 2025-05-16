# Cuaderno-de-Ejercicios-2-Parte

1. **Explicar sobre el siguiente grafo de red los conceptos de circuito virtual entre el nodo 10 y el 6, y datagrama entre el nodo 3 y el 6.**  
   Asumir arcos bidireccionales.

   ### Circuito Virtual entre los nodos 10 y 6

Un **circuito virtual** es un camino lógico preestablecido entre dos nodos dentro de una red de conmutación de paquetes. Antes de iniciar la transmisión de datos, se establece este camino que **permanece fijo** durante toda la comunicación, simulando una conexión dedicada.

#### Características del circuito virtual:

- Garantiza el **orden de entrega** de los paquetes.
- Implica una **menor sobrecarga de control** durante la transmisión.
- Los **nodos intermedios mantienen el estado** de la conexión mediante tablas de encaminamiento.

#### Posibles rutas en la red (asumiendo enlaces bidireccionales):

Desde el **nodo 10 hasta el nodo 6**, se pueden establecer dos rutas equivalentes en longitud:

1. `10 → 5 → 7 → 4 → 6`  
2. `10 → 5 → 2 → 4 → 6`

Ambas opciones implican caminos válidos, diferenciándose únicamente en el nodo intermedio utilizado para llegar desde el nodo 5 al nodo 4 (vía nodo 2 o nodo 7).

---

### Datagrama entre los nodos 3 y 6

En contraste, un **datagrama** es una unidad de información enviada de forma independiente a través de una red tipo IP (sin conexión previa). Cada paquete se enruta de forma autónoma, sin garantías sobre el orden o la entrega.

#### Características del modelo de datagramas:

- **No se establece conexión** previa entre origen y destino.
- Cada paquete puede **seguir una ruta diferente**.
- **No se garantiza el orden** de llegada ni la entrega final.
- Los **nodos intermedios no mantienen estado** de sesión; solo consultan sus tablas de enrutamiento dinámicamente.

#### Posibles rutas del nodo 3 al nodo 6:

Existen múltiples rutas posibles entre el nodo 3 y el nodo 6, como por ejemplo:

1. `3 → 10 → 5 → 2 → 4 → 6`
2. `3 → 10 → 5 → 7 → 4 → 6`

Dado que los enlaces son bidireccionales y la red opera bajo el modelo de datagramas, **cada paquete podría tomar cualquiera de estas rutas** de forma independiente, alternando caminos sin afectar la red.


2. **Partiendo de la anterior red, generar las tablas de cada uno de los nodos de un circuito virtual entre los nodos 3 y 4.**  
   Tener en cuenta que los CV creados del anterior ejercicio siguen presentes.  
   Asumir arcos bidireccionales.

## Establecimiento de un Circuito Virtual entre los nodos 3 y 4

### Contexto

En esta red con enlaces bidireccionales, se quiere establecer un **circuito virtual (CV)** entre los nodos **3** y **4**.  
La ruta más corta seleccionada es:

3 → 10 → 5 → 2 → 4

No hay circuitos virtuales activos actualmente, por lo que todos los nodos e interfaces están disponibles. Se asigna al nuevo circuito el identificador **101**.

---

### Encaminamiento del Circuito Virtual 101

- **Nodo 3** inicia la transmisión del circuito virtual 101, enviando los paquetes hacia el nodo 10. Asocia este circuito al enlace de salida hacia el nodo 10.

- **Nodo 10**, al recibir un paquete con el CV 101 proveniente del nodo 3, sabe que debe reenviarlo hacia el nodo 5, manteniendo el mismo identificador de circuito.

- **Nodo 5**, al recibir el CV 101 desde el nodo 10, lo reencamina hacia el nodo 2, sin modificar el identificador del circuito.

- **Nodo 2**, al recibir el paquete del nodo 5, lo envía al nodo 4, respetando la misma etiqueta del circuito virtual.

- Finalmente, **Nodo 4** recibe el paquete con el identificador 101 desde el nodo 2, y al ser el nodo de destino, lo entrega a su capa superior para su procesamiento.


3. **Aplicar los algoritmos indicados en los siguientes grafos, tomando como partida el nodo 5.**  
   Indicar iteración a iteración lo que ocurre. Asumir arcos bidireccionales.  
   a) Algoritmo de Dijkstra.  
   b) Algoritmo de inundación hasta el nodo 6. Contar el total de paquetes generados.

   ### a) Algoritmo de Dijkstra (Ruta más corta)

Se aplica el algoritmo de Dijkstra en tres grafos distintos, comenzando desde el nodo 5. A continuación, se indican las distancias totales y los caminos más cortos encontrados hacia cada nodo destino:

#### Grafo 1 (arriba a la derecha)

| Nodo destino | Distancia total | Camino más corto       |
|--------------|-----------------|-----------------------|
| 10           | 90              | 5 → 10                |
| 9            | 32              | 5 → 9                 |
| 7            | 37              | 5 → 7                 |
| 6            | 75 (37 + 38)    | 5 → 7 → 6             |
| 1            | 71 (32 + 39)    | 5 → 9 → 1             |
| 2            | 67 (32 + 35)    | 5 → 9 → 2             |

---

#### Grafo 2 (arriba a la izquierda)

| Nodo destino | Distancia total | Camino más corto          |
|--------------|-----------------|--------------------------|
| 7            | 34              | 5 → 7                    |
| 8            | 30              | 5 → 8                    |
| 6            | 73 (34 + 39)    | 5 → 7 → 6                |
| 3            | 64 (30 + 34)    | 5 → 8 → 3                |
| 4            | 69 (30 + 39)    | 5 → 8 → 4                |
| 2            | 103 (30 + 39 + 34)| 5 → 8 → 4 → 2          |

---

#### Grafo 3 (abajo)

| Nodo destino | Distancia total | Camino más corto               |
|--------------|-----------------|-------------------------------|
| 7            | 34              | 5 → 7                         |
| 2            | 32              | 5 → 2                         |
| 8            | 69 (34 + 35)    | 5 → 7 → 8                     |
| 6            | 72 (32 + 40)    | 5 → 2 → 6                     |
| 10           | 109 (34 + 35 + 40)| 5 → 7 → 8 → 10              |
| 9            | 139 (34 + 35 + 40 + 30)| 5 → 7 → 8 → 10 → 9      |

---

### b) Algoritmo de inundación hasta el nodo 6

Se realiza el proceso de inundación para propagar un paquete desde el nodo 5 hasta alcanzar el nodo 6 en cada grafo, contando el número total de paquetes generados.

#### Grafo 1 (arriba a la derecha)

- El nodo 5 envía el paquete a todos sus vecinos: nodos 7, 9 y 10.  
- En la siguiente ronda, los nodos 7 y 10 reenvían el paquete:  
  - Nodo 7 lo envía al nodo 6, alcanzando el destino.  
  - Nodo 10 también lo envía al nodo 6, llegando a destino.

**Total de paquetes generados:** Paquetes iniciales + reenvíos (7, 9, 10) + reenvíos (7 y 10).

---

#### Grafo 2 (arriba a la izquierda)

- Nodo 5 envía el paquete a sus vecinos: nodos 7 y 8.  
- Luego, nodos 7 y 8 reenvían el paquete:  
  - Nodo 7 lo envía directamente al nodo 6, llegando a destino.  
  - Nodo 8 lo envía al nodo 4, que a su vez lo reenvía al nodo 6, completando la ruta.

---

#### Grafo 3 (abajo)

- Nodo 5 envía el paquete a nodos 7 y 2.  
- A continuación, nodos 7 y 2 reenvían el paquete:  
  - Nodo 7 envía el paquete al nodo 6, alcanzando el destino.  
  - Nodo 2 también envía el paquete al nodo 6, llegando a destino.

4. **Diseñar las tablas de rutado jerárquico de la siguiente red.**

## Diseño de las tablas de enrutamiento jerárquico de la red

Se han creado cuatro subredes (A, B, C y D), cada una con su propia tabla de rutas. Las rutas se clasifican en dos tipos: locales (dentro de la misma subred) y jerárquicas (hacia otras subredes).

---

### Subred A (arriba izquierda)

- Para la comunicación local dentro de la subred A, las rutas son:
  - De A1 a A2: se envía directamente de A1 a A2.
  - De A3 a A4: se envía directamente de A3 a A4.
  - De A2 a A3: el paquete sigue la ruta A2 → A5 → A3.
  - De A1 a A4: se envía por la ruta A1 → A5 → A4.
  - De A1 a A3: la ruta es directa, A1 → A3.
  - De A2 a A4: se envía directo A2 → A4.
  - Para llegar a A5 desde A1 o A2 o A3 o A4, se utiliza el camino directo correspondiente (por ejemplo, A1 → A5).

- Para acceder a otras subredes:
  - Para la subred B, los paquetes salen por A4 hacia B3.
  - Para la subred C, la salida es desde A4 hacia C2.
  - Para la subred D, el paquete pasa de A4 a C2 y luego a D1.

---

### Subred B (arriba derecha)

- Rutas locales dentro de B:
  - B1 a B2: comunicación directa.
  - B2 a B4: enlace directo.
  - B3 a B4 y B3 a B1 también son enlaces directos.
  
- Para salir hacia otras subredes:
  - Para llegar a la subred A, se pasa por B3 hacia A4.
  - Para la subred D, la ruta es B4 → D2.
  - Para la subred C, los paquetes van de B3 a A4 y luego a C2.

---

### Subred C (abajo izquierda)

- Rutas locales dentro de C:
  - C1 a C2, C1 a C3 y C1 a C4 tienen rutas directas.
  - C2 a C3 y C2 a C4 también son directas, al igual que C3 a C4.
  
- Para acceder a otras subredes:
  - Para llegar a la subred A, se sale de C2 hacia A4.
  - Para la subred B, el camino es C2 → A4 → B3.
  - Para la subred D, se utiliza la ruta directa C2 → D1.

---

### Subred D (abajo derecha)

- Rutas locales dentro de D incluyen:
  - Comunicaciones directas entre D1-D2, D1-D3, D2-D4, D3-D4, y también D5 con D2, D3 y D4.
  
- Para salir hacia otras subredes:
  - Para la subred B, se pasa de D2 a B4.
  - Para la subred C, la ruta es D1 → C2.
  - Para la subred A, el paquete se envía de D1 a C2 y luego a A4.


5. **En un sistema con las siguientes características, indicar las máscaras utilizadas, la primera y última dirección de los equipos, y los elementos/dispositivos necesarios:**
   - 7900 profesionales con datos sensibles.
   - 54 subredes para sensorización (100 sensores por subred).
   - 1290 tomas Ethernet.
   - 75000 clientes externos que deberán tener acceso a Internet por wifi.
   - Switches de máximo 24 bocas.

# Diseño de Red: Subredes, Direcciones y Equipamiento

## Requisitos del sistema

| Categoría                                | Cantidad                  |
|------------------------------------------|---------------------------|
| Profesionales con datos sensibles        | 7900                      |
| Subredes de sensores (100 sensores c/u)  | 54 subredes               |
| Tomas Ethernet para red cableada         | 1290                      |
| Clientes externos con acceso por WiFi    | 75.000                    |
| Capacidad máxima de switches             | 24 puertos por switch     |

---

## 1. Red para profesionales con datos sensibles

**Necesidad:** 7900 dispositivos → se requiere una única red grande

### Cálculo de subred

Buscamos una subred que albergue al menos 7900 hosts:

- Una subred `/19` permite: 2⁽³²⁻¹⁹⁾ − 2 = 8192 − 2 = **8190 hosts**

**Máscara utilizada:** `/19` → 255.255.224.0

- **Primera dirección útil:** 192.168.0.1  
- **Última dirección útil:** 192.168.31.254  
- **Broadcast:** 192.168.31.255

---

## 2. Subredes para sensores (54 subredes × 100 sensores)

Cada subred debe permitir al menos 100 sensores.

### Cálculo por subred:

- Una subred `/25` ofrece: 2⁽³²⁻²⁵⁾ − 2 = 128 − 2 = **126 hosts**

**Máscara utilizada:** `/25` → 255.255.255.128

- Se necesitan 54 subredes → total de 54 bloques `/25`

Ejemplo para primera subred:

- **Primera dirección útil:** 192.168.32.1  
- **Última dirección útil:** 192.168.32.126  
- **Broadcast:** 192.168.32.127

(El resto iría en bloques de 128 direcciones sucesivas, hasta cubrir las 54 subredes)

---

## 3. Red para 1290 tomas Ethernet

### Cálculo de subred:

- 1290 dispositivos → `/21` = 2⁽³²⁻²¹⁾ − 2 = 2048 − 2 = **2046 hosts**

 **Máscara utilizada:** `/21` → 255.255.248.0

- **Primera dirección útil:** 192.168.64.1  
- **Última dirección útil:** 192.168.71.254  
- **Broadcast:** 192.168.71.255

---

## 4. Red para 75.000 clientes WiFi

###  Cálculo:

- Se requieren múltiples subredes → usar bloques `/16` (65534 hosts c/u)

Usamos 2 subredes `/16` para cubrir 75.000 usuarios:

- `/16` → 2⁽³²⁻¹⁶⁾ − 2 = 65536 − 2 = **65534 hosts**

 **Máscara utilizada:** `/16` → 255.255.0.0

Ejemplos:

- **Subred 1 (192.168.128.0/16):**
  - Primera dirección útil: 192.168.128.1
  - Última dirección útil: 192.168.255.254

- **Subred 2 (192.169.0.0/16):**
  - Primera dirección útil: 192.169.0.1
  - Última dirección útil: 192.169.255.254

---

## 5. Switches necesarios (24 bocas máx)

### Cálculo:

| Uso                          | Dispositivos | Switches (24 puertos) |
|------------------------------|--------------|------------------------|
| Profesionales                | 7900         | 7900 / 24 ≈ **330**    |
| Tomas Ethernet               | 1290         | 1290 / 24 ≈ **54**     |
| Sensores (5400 en total)     | 54×100 = 5400| 5400 / 24 ≈ **225**    |
| WiFi (no requieren switches) | 75000        | 0                      |

**Total switches necesarios:** **~609**

---


6. **Calcular la eficiencia de un sistema basado en UDP/IP desde un mensaje de la capa de aplicación de 1000000 bytes, sabiendo que la longitud máxima del campo de datos es:**
   - UDP: 65527 bytes. Cabecera de 8 bytes.
   - IP: 65535 bytes. Cabecera de 4 bytes.
   - Ethernet: 1500 bytes. Cabecera de 8 bytes.
  
### Análisis de Transmisión de Datos y Eficiencia

**Objetivo:** Calcular la cantidad de tramas necesarias para transmitir 1.000.000 bytes de datos utilizando el protocolo UDP sobre Ethernet, considerando las limitaciones de tamaño de trama y la sobrecarga por cabeceras.

---

### Tamaño útil de datos por trama

La trama máxima de Ethernet permite transportar hasta **1500 bytes de datos** sin incluir la cabecera Ethernet.

Dado que estamos trabajando con UDP sobre IP, debemos restar las cabeceras correspondientes:

- Cabecera IP: 4 bytes  
- Cabecera UDP: 8 bytes  

**Tamaño útil para datos por trama:**

$$1500 - 4 (IP) - 8 (UDP) = 1488 bytes$$

---

### Cálculo del número de tramas necesarias

Para transmitir **1.000.000 bytes** de datos:

$$1.000.000 / 1488 ≈ 671.14 → se necesitan 672 tramas$$

- 671 tramas completas de 1488 bytes → 671 × 1488 = 999.648 bytes
- 1 trama final con los **352 bytes restantes**

---

### Sobrecarga por cabeceras

- **Primera trama:**
  - Incluye cabeceras: UDP (8 bytes) + IP (4 bytes) + Ethernet (8 bytes) = **20 bytes**

- **Restantes 671 tramas:**
  - Cada una con cabecera IP (4 bytes) + Ethernet (8 bytes) = **12 bytes**

**Cálculo total de la sobrecarga:**

$$20 + (671 × 12) = 20 + 8052 = 8072 bytes de sobrecarga total$$

---

### Cálculo de la eficiencia

La eficiencia se calcula como:

$$Eficiencia = Datos útiles / (Datos útiles + Sobrecarga)$$


Sustituyendo valores:

$$Eficiencia = 1.000.000 / (1.000.000 + 8072) ≈ 0.992$$

7. **Con los datos del ejercicio anterior, calcular la eficiencia de un sistema RTP/UDP, con cabecera RTP de 12 bytes y 65535 bytes de datos.**

### Tamaño útil por trama

Tamaño disponible para datos por cada trama Ethernet:

$$1500 - IP (4) - UDP (8) - RTP (12) = 1476 bytes útiles por trama$$

---

### Número de tramas necesarias

Dividimos los **65.535 bytes** en bloques de **1476 bytes**:

$$65.535 / 1476 ≈ 44.39 → Se necesitan 45 tramas$$

- 44 tramas completas de 1476 bytes → 44 × 1476 = 64.944 bytes
- 1 última trama con: 65.535 - 64.944 = **591 bytes restantes**

---

### Cálculo de sobrecarga

#### Primera trama (incluye todas las cabeceras):
- Ethernet (8) + IP (4) + UDP (8) + RTP (12) = **32 bytes**

#### Resto de tramas (excluyendo RTP ya incluida una vez):
Como RTP se aplica por **paquete RTP**, en la práctica **cada fragmento incluye cabecera RTP**, por lo que:

**Todas las tramas** incluyen 12 bytes RTP  
Cabecera por cada trama = Ethernet (8) + IP (4) + UDP (8) + RTP (12) = **32 bytes por trama**

Entonces:

$$45 tramas × 32 bytes = 1440 bytes de sobrecarga total$$

---

### Cálculo de eficiencia

La eficiencia se calcula como:

$$Eficiencia = Datos útiles / (Datos útiles + Sobrecarga)$$

$$Eficiencia = 65.535 / (65.535 + 1440) ≈ 0.9785$$

---

### Resultado final

- **Número de tramas necesarias:** 45
- **Sobrecarga total:** 1440 bytes
- **Eficiencia del sistema RTP/UDP:** **≈ 97.85%**

Esto indica un sistema altamente eficiente, aunque con una ligera pérdida adicional comparado con UDP puro, debido a la cabecera RTP incluida en cada trama.


8. **Realizar el pseudocódigo de un cliente y servidor UDP/TCP que devuelva la hora de una región determinada.**

## Servidor TCP/UDP

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

## Cliente TCP/UDP

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
