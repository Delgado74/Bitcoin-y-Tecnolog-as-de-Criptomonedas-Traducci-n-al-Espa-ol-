# Capítulo 9: Avances en Criptografía y Bitcoin

> Traducción al español del capítulo 9 del borrador **Bitcoin and Cryptocurrency Technologies**  
> Arvind Narayanan, Joseph Bonneau, Edward Felten, Andrew Miller, Steven Goldfeder (Draft — Feb 9, 2016)

---

## 9.0 Introducción

Bitcoin se basa en técnicas criptográficas conocidas:  
- Funciones hash.  
- Firmas digitales (ECDSA).  
- Árboles de Merkle.  

Pero su aparición ha motivado nuevas líneas de investigación en criptografía aplicada.  
Este capítulo explora innovaciones y cómo podrían transformar a Bitcoin y otras criptomonedas.

---

## 9.1 Firmas digitales avanzadas

### Firmas Schnorr
- Alternativa a ECDSA.  
- Más simples matemáticamente y más seguras bajo supuestos estándar.  
- Permiten **agregación de firmas**: múltiples firmas pueden combinarse en una sola, reduciendo tamaño de transacciones y mejorando privacidad.  
- Bitcoin aún no las incorporaba en su versión inicial, pero existe fuerte interés en adoptarlas (Taproot).

### Firmas de umbral y multifirma
- Generalización de multifirma m-de-n.  
- Las claves privadas pueden dividirse entre varias partes.  
- Mejoran la seguridad y la flexibilidad en custodia de fondos.  

---

## 9.2 Pruebas de conocimiento cero

Las **zk-SNARKs** y otros protocolos permiten demostrar que una afirmación es verdadera **sin revelar la información subyacente**.  

Aplicaciones en criptomonedas:  
- Transacciones privadas en *Zcash*.  
- Posible verificación de balances o intercambios sin revelar datos sensibles.  

Desafíos:  
- Complejidad computacional.  
- Necesidad de parámetros de confianza (trusted setup).  

---

## 9.3 Criptografía homomórfica

Permite realizar cálculos sobre datos cifrados sin necesidad de descifrarlos.  

Aplicaciones potenciales:  
- Exchanges que prueben solvencia sin exponer claves.  
- Contratos inteligentes que manejen datos privados.  

Aún es costosa en términos de eficiencia, pero los avances son prometedores.  

---

## 9.4 Pruebas de reserva y solvencia

Un reto para exchanges de criptomonedas es demostrar que tienen fondos suficientes.  

Herramientas criptográficas aplicadas:  
- Pruebas de **reserva** mediante árboles de Merkle.  
- Permiten verificar que los saldos de clientes están respaldados por fondos sin revelar identidades ni montos individuales.  

Esto incrementa la transparencia sin comprometer privacidad.  

---

## 9.5 Protocolos de mezcla y privacidad

Bitcoin es seudónimo pero trazable.  
Por eso, se han propuesto protocolos de **mezcla criptográfica**:

- **CoinJoin:** varios usuarios combinan sus transacciones en una sola.  
- **Mixnets y tumblers:** reenvían monedas a través de múltiples intermediarios.  
- **Confidential Transactions:** ocultan los montos usando criptografía homomórfica.  

Objetivo: mejorar la privacidad y la fungibilidad de las monedas digitales.  

---

## 9.6 Contratos inteligentes criptográficos

Más allá de Ethereum, la investigación busca incorporar contratos inteligentes directamente en Bitcoin mediante herramientas criptográficas avanzadas:

- **Time-lock puzzles:** fondos que solo se pueden gastar después de cierto tiempo.  
- **Hashed timelock contracts (HTLC):** base para Lightning Network y swaps atómicos.  
- **Scriptless scripts:** contratos complejos implementados mediante firmas avanzadas, sin necesidad de exponer datos en la cadena.  

---

## 9.7 Canales de pago y escalabilidad

Los canales de pago permiten transacciones fuera de la cadena:  
- Dos partes bloquean fondos en un contrato multifirma.  
- Pueden intercambiar pagos ilimitados fuera de la blockchain.  
- Solo la transacción inicial y final quedan registradas en la cadena.  

La combinación de **HTLCs** con canales forma la base de **Lightning Network**, uno de los mayores avances prácticos hacia escalabilidad.  

---

## 9.8 Criptografía poscuántica

Un desafío futuro: la llegada de computadoras cuánticas.  

- Algoritmos actuales como ECDSA serían vulnerables a ataques cuánticos.  
- Se investiga en **firmas poscuánticas** basadas en retículas (lattices), códigos y funciones hash.  
- Migrar un sistema global como Bitcoin a criptografía poscuántica será un reto técnico y social.  

---

## 9.9 Resumen del capítulo

- Bitcoin ha estimulado nuevas aplicaciones de la criptografía avanzada.  
- Firmas Schnorr y de umbral ofrecen mejoras en eficiencia y multifirma.  
- Pruebas de conocimiento cero y transacciones confidenciales amplían privacidad.  
- Pruebas de reserva pueden dar mayor confianza en exchanges.  
- Lightning Network y contratos criptográficos extienden la funcionalidad y escalabilidad.  
- La amenaza cuántica motiva investigación en nuevas primitivas criptográficas.  

---

## 9.10 Preguntas de reflexión (autoestudio)

1. ¿Qué ventajas aportan las firmas Schnorr frente a ECDSA en Bitcoin?  
2. Explica cómo funcionan las pruebas de reserva usando árboles de Merkle.  
3. ¿Qué problemas de privacidad resuelven las *Confidential Transactions*?  
4. ¿Qué relación existe entre los contratos HTLC y Lightning Network?  
5. ¿Qué desafíos implica una eventual transición de Bitcoin a criptografía poscuántica?  

---
