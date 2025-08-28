# Capítulo 5: La Red P2P Descentralizada de Bitcoin

> Traducción al español del capítulo 5 del borrador **Bitcoin and Cryptocurrency Technologies**  
> Arvind Narayanan, Joseph Bonneau, Edward Felten, Andrew Miller, Steven Goldfeder (Draft — Feb 9, 2016)

---

## 5.0 Introducción

Hasta ahora hemos descrito cómo funcionan transacciones, blockchain y minería.  
Pero aún falta responder: **¿cómo circula la información en Bitcoin?**  

Bitcoin funciona sobre una red **peer-to-peer (P2P)**, descentralizada y sin servidores centrales.  
Los nodos se comunican directamente entre sí para:  
- Difundir transacciones.  
- Difundir bloques.  
- Mantener la consistencia de la blockchain.  

---

## 5.1 Topología de la red

La red Bitcoin es una red P2P no estructurada:

- Cualquier nodo puede unirse.  
- Cada nodo mantiene conexiones TCP con varios pares (típicamente entre 8 y 125).  
- No hay nodos “especiales” oficiales: todos son iguales.  
- La redundancia asegura que la red sea robusta frente a fallos o censura.  

**Implicación:**  
No existe un punto único de control. La resistencia a la censura y la robustez son inherentes al diseño.

---

## 5.2 Propagación de transacciones

Cuando un usuario crea una transacción:

1. Su cliente la transmite a algunos pares.  
2. Estos pares la reenvían a otros (flooding).  
3. Eventualmente se difunde por toda la red y llega a los mineros.  

**Verificaciones previas a la difusión:**
- Firma válida.  
- Entradas no gastadas.  
- Tamaño y formato correctos.  

De este modo, los nodos filtran transacciones inválidas antes de propagar.

---

## 5.3 Propagación de bloques

Cuando un minero encuentra un bloque válido:

1. Lo transmite a sus pares.  
2. Estos lo validan y lo propagan al resto.  
3. La mayoría de la red lo recibe en segundos.  

La latencia de propagación influye en:
- **Forks temporales:** dos bloques pueden competir si se descubren casi al mismo tiempo.  
- La ventaja de mineros bien conectados (sus bloques se difunden antes).  

---

## 5.4 Ataques en la red

La red P2P, al no tener autoridad central, es vulnerable a ataques específicos:

- **Eclipse attack:** el atacante aísla un nodo rodeándolo de conexiones controladas. Así manipula lo que ve.  
- **DoS (Denial of Service):** inundar a un nodo con tráfico para saturarlo.  
- **Propagación selectiva:** retrasar ciertos bloques o transacciones para manipular la visión de un nodo.  

Mitigaciones:
- Limitar conexiones por dirección IP.  
- Usar conexiones salientes aleatorias.  
- Implementar listas negras contra pares maliciosos.  

---

## 5.5 Escalabilidad y ancho de banda

La red debe balancear:
- **Rapidez de propagación:** bloques y transacciones deben difundirse rápido.  
- **Eficiencia de ancho de banda:** evitar redundancia excesiva.  

Estrategias:
- Difusión basada en “inv” (anuncio de identificadores antes de enviar el contenido completo).  
- Compresión de bloques (enviar solo transacciones nuevas que el receptor no conoce).  

Escalabilidad es un desafío continuo:
- Más usuarios = más transacciones.  
- Bloques grandes = más ancho de banda requerido.  

---

## 5.6 Clientes ligeros y pruebas SPV

No todos los nodos almacenan la blockchain completa.  
Los **clientes SPV (Simplified Payment Verification)** descargan solo **cabeceras de bloques**:

- Mantienen la cadena de cabeceras (32 bytes por hash).  
- Pueden verificar que una transacción está incluida en un bloque usando pruebas de Merkle.  
- Confían en que la mayoría de la red es honesta y extendió la cadena más larga.  

Ventaja: bajo consumo de recursos.  
Desventaja: mayor dependencia de la honestidad de los nodos completos.

---

## 5.7 Resistencia a la censura

El diseño P2P de Bitcoin lo hace difícil de censurar:

- No hay servidor central que pueda ser bloqueado.  
- Cualquiera puede conectarse desde cualquier parte del mundo.  
- Si un nodo o región se aísla, la red sigue operando globalmente.  

Sin embargo, gobiernos o ISPs pueden:
- Bloquear direcciones IP conocidas de nodos.  
- Inspeccionar tráfico.  

Medidas de resistencia:
- Uso de Tor o VPN.  
- Encapsulamiento de tráfico Bitcoin en protocolos comunes.  

---

## 5.8 Resumen del capítulo

- Bitcoin usa una red **P2P descentralizada** para difundir transacciones y bloques.  
- La propagación es rápida pero imperfecta, lo que permite forks temporales.  
- Existen vulnerabilidades de red (eclipse, DoS), pero mitigables.  
- Los clientes SPV permiten verificar pagos sin descargar toda la blockchain.  
- El diseño descentralizado otorga a Bitcoin una fuerte **resistencia a la censura**.  

---

## 5.9 Preguntas de reflexión (autoestudio)

1. ¿Por qué Bitcoin no necesita nodos especiales ni jerárquicos en su red?  
2. Explica cómo se propaga una transacción desde el emisor hasta un minero.  
3. ¿Qué ventajas obtiene un minero que tiene conexiones rápidas y numerosas?  
4. Describe un ataque de eclipse y sus posibles consecuencias.  
5. ¿Qué compromisos existen entre usar un cliente SPV y un nodo completo?  

---
