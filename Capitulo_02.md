# Capítulo 2: La Mecánica de Bitcoin

> Traducción al español del capítulo 2 del borrador **Bitcoin and Cryptocurrency Technologies**  
> Arvind Narayanan, Joseph Bonneau, Edward Felten, Andrew Miller, Steven Goldfeder (Draft — Feb 9, 2016)

---

## 2.0 Introducción

En este capítulo veremos **cómo funciona Bitcoin en la práctica**.  
En particular, estudiaremos la forma en que los usuarios **poseen y transfieren** bitcoins, cómo se representan las transacciones y cómo la red mantiene un **libro mayor público** llamado **blockchain**.

Bitcoin se distingue de otros sistemas de pago porque es **descentralizado**:  
- No existe un servidor central que mantenga cuentas.  
- Cualquier nodo puede participar, crear transacciones y verificar el historial.  
- La integridad del sistema surge de reglas **criptográficas** y de un protocolo de consenso basado en **prueba de trabajo**.

---

## 2.1 Qué significa poseer bitcoins

En Bitcoin, **no existen monedas físicas ni archivos digitales que representen monedas**.  
En su lugar, la propiedad se define mediante **transacciones registradas en la blockchain**.

- Cada transacción especifica **entradas** (bitcoins previamente recibidos) y **salidas** (nuevos destinatarios).  
- Una salida contiene un **script de bloqueo** (generalmente: “el dueño de tal clave pública puede gastar esto”).  
- El dueño demuestra su derecho proporcionando una **firma digital** correspondiente (script de desbloqueo).

👉 Poseer bitcoins equivale a tener la **clave privada** que permite desbloquear ciertas salidas en la blockchain.

---

## 2.2 Transacciones: estructura básica

Una **transacción** en Bitcoin incluye:

- **Entradas:** referencias a transacciones previas + pruebas de autorización (firma).  
- **Salidas:** condiciones de gasto futuras + montos de BTC.  
- **Metadatos:** versión, tiempo, etc.

Ejemplo sencillo:

1. Alice recibió 1 BTC en una transacción previa.  
2. Ahora quiere enviar 0.6 BTC a Bob y quedarse con 0.4 BTC.  
3. Construye una transacción con:  
   - Entrada: referencia a su transacción previa, con su firma.  
   - Salidas:  
     - 0.6 BTC a la clave pública de Bob.  
     - 0.4 BTC de “cambio” a su propia clave pública.

---

## 2.3 La cadena de transacciones

Cada transacción crea nuevas salidas que podrán usarse como entradas futuras.  
De este modo se forma una **cadena de posesión**:

1. Génesis: transacciones de recompensa a mineros.  
2. Alice recibe una salida (1 BTC).  
3. Alice gasta esa salida en Bob.  
4. Bob gasta en Carol…  

Toda la historia de propiedad se reconstruye siguiendo las referencias de entradas y salidas.

---

## 2.4 El libro mayor: la blockchain

La blockchain es una **lista enlazada de bloques**.  
Cada bloque contiene:

- Una colección de transacciones.  
- La raíz de un **árbol de Merkle** que compromete todas esas transacciones.  
- El hash del bloque anterior (puntero hash).  
- Un valor *nonce* que resuelve el acertijo de prueba de trabajo.

**Características clave:**
- La cadena es pública y replicada por todos los nodos.  
- El encadenamiento con hashes asegura **inmutabilidad**: modificar un bloque invalida todos los posteriores.  
- El consenso se alcanza por la regla de la **cadena más larga** (con mayor trabajo acumulado).

---

## 2.5 Scripts de Bitcoin

Bitcoin usa un lenguaje de scripting simple, basado en **pila**, para definir condiciones de gasto:

- **Pay-to-PubKey-Hash (P2PKH):**  
  “El dueño de la clave privada asociada a este hash de clave pública puede gastar”.

- **Multifirma (m-de-n):**  
  Requiere múltiples firmas válidas. Útil para carteras compartidas o contratos.  

- **Bloqueo por tiempo:**  
  Una salida no puede gastarse hasta cierto bloque o fecha.  

Aunque el lenguaje es limitado (no es Turing-completo), permite construir contratos útiles con seguridad razonable.

---

## 2.6 Ejemplo paso a paso

Supongamos:

1. **Génesis:** el minero crea 50 BTC.  
2. Alice recibe 10 BTC de un minero.  
3. Alice paga 5 BTC a Bob.  
4. Bob paga 2 BTC a Carol.  

Cada paso está representado en la blockchain como transacciones encadenadas.  
Los nodos verifican que:
- Las entradas existan y no se hayan gastado.  
- Las firmas sean válidas.  
- Las sumas cuadren (entradas = salidas + comisiones).

---

## 2.7 Nodos y validación

Todos los nodos de la red verifican las transacciones y bloques:

- **Nodos completos:** mantienen toda la blockchain y validan todas las reglas.  
- **Clientes ligeros (SPV):** descargan solo cabeceras de bloques y usan pruebas de Merkle para verificar transacciones específicas.

**Proceso de validación de una transacción:**
1. Verificar la firma.  
2. Revisar que las entradas no se hayan gastado antes (prevención de doble gasto).  
3. Chequear sumas (entradas ≥ salidas).  
4. Aceptar en la “mempool” hasta que sea incluida en un bloque.

---

## 2.8 Resumen del capítulo

- Poseer bitcoins significa controlar **claves privadas** que permiten gastar salidas específicas en la blockchain.  
- Las transacciones encadenan propiedad mediante **entradas** y **salidas**.  
- La blockchain es un libro mayor global, mantenido de forma **descentralizada** y protegido por **prueba de trabajo**.  
- Los **scripts** permiten condiciones de gasto más ricas que un simple pago directo.  
- La validación distribuida garantiza integridad y evita el doble gasto.

---

## 2.9 Preguntas de reflexión (autoestudio)

1. Explica por qué “poseer bitcoins” en realidad significa controlar ciertas **claves privadas**.  
2. Construye un ejemplo de transacción con **entrada múltiple** (juntar varias salidas).  
3. ¿Qué problemas surgirían si la blockchain no usara punteros hash para enlazar bloques?  
4. Da un ejemplo práctico donde un **contrato multifirma** sea más seguro que un pago normal.  
5. ¿Qué riesgos tendrías al usar solo un **cliente SPV** en lugar de un nodo completo?  

---
