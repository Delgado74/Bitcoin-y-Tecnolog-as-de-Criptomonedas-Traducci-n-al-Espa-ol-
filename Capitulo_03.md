# Capítulo 3: Minería y Consenso en Bitcoin

> Traducción al español del capítulo 3 del borrador **Bitcoin and Cryptocurrency Technologies**  
> Arvind Narayanan, Joseph Bonneau, Edward Felten, Andrew Miller, Steven Goldfeder (Draft — Feb 9, 2016)

---

## 3.0 Introducción

En los capítulos anteriores vimos cómo las transacciones definen propiedad y cómo la blockchain registra el historial de transferencias.  
Ahora nos toca responder una pregunta clave:  

**¿Cómo logra Bitcoin consenso descentralizado sobre un solo historial válido de transacciones?**

La respuesta es la **minería**, un mecanismo basado en:
- **Prueba de trabajo (Proof-of-Work, PoW).**
- **Incentivos económicos.**

Los mineros compiten para añadir nuevos bloques a la blockchain, asegurando que:
1. Se proponga **solo una historia canónica** de transacciones.  
2. Los ataques de doble gasto sean **difíciles y costosos**.  

---

## 3.1 El problema del consenso sin confianza

En un sistema distribuido, cada nodo recibe mensajes en momentos distintos.  
Sin un coordinador central, ¿cómo ponerse de acuerdo en un mismo orden de transacciones?

- Solución ingenua: “la mayoría decide”.  
  Problema: cualquiera puede crear miles de identidades falsas (**ataque Sybil**).  

- Necesitamos ponderar el consenso en función de un recurso **costoso de falsificar**.  
  En Bitcoin ese recurso es la **capacidad de cómputo**.

---

## 3.2 La prueba de trabajo (PoW)

La idea básica: para proponer un bloque, el minero debe resolver un **acertijo criptográfico**:

- Buscar un valor *nonce* tal que:
  \[
  H(\text{cabecera de bloque} \,\|\, \text{nonce}) < \text{umbral}
  \]

- El umbral depende de la **dificultad**, ajustada para que un bloque válido se encuentre en promedio cada 10 minutos.

**Propiedades:**
- Difícil de encontrar (requiere fuerza bruta).  
- Fácil de verificar (un solo cálculo de hash).  
- Ajustable en dificultad.  

---

## 3.3 Incentivos y recompensas

Los mineros invierten recursos (electricidad, hardware). ¿Por qué participar?

- **Recompensa de bloque:** nuevos bitcoins emitidos con cada bloque.  
  - Comenzó en 50 BTC por bloque.  
  - Se reduce a la mitad cada 210,000 bloques (~4 años).  
  - Límite total: 21 millones de BTC.  

- **Comisiones de transacción:** pagadas por los usuarios al incluir sus transacciones.  

Estas recompensas incentivan a los mineros a:
- Competir honestamente.  
- Proteger la validez de la blockchain (su ganancia depende de que el sistema sea confiable).

---

## 3.4 La regla de la cadena más larga

Los nodos aceptan como válida la cadena con **más trabajo acumulado**.  
Esto asegura que:
- En caso de bifurcación, eventualmente una rama se volverá más larga.  
- La otra rama será abandonada.  

**Confirmaciones:**  
- Una transacción se considera más segura a medida que se acumulan bloques encima.  
- Regla práctica: esperar ~6 confirmaciones (~1 hora) para pagos grandes.

---

## 3.5 Probabilidad de un ataque de doble gasto

Imaginemos que un atacante intenta gastar dos veces la misma moneda:
1. Envía una transacción a un comerciante.  
2. Paralelamente mina en secreto una cadena con una transacción conflictiva a sí mismo.  
3. Si logra adelantar su cadena privada, puede publicarla y revertir el pago.

**Probabilidad de éxito:**  
- Depende de la proporción de poder de minado del atacante.  
- Si controla menos del 50%, la probabilidad de éxito disminuye exponencialmente con cada confirmación extra.  
- Si controla más del 50% (**ataque del 51%**), puede reescribir arbitrariamente la blockchain.

---

## 3.6 Seguridad y economía de la minería

La seguridad de Bitcoin surge de un equilibrio económico:

- **Costo del ataque:** electricidad y hardware requeridos para minar más rápido que el resto de la red.  
- **Beneficio del ataque:** ganancias potenciales de doble gasto o sabotaje.  
- Mientras el costo supere al beneficio, el sistema permanece seguro.

**Implicación:** la seguridad depende del **valor económico de Bitcoin**.  
A mayor precio, mayores incentivos para minar honestamente y mayor costo para atacantes.

---

## 3.7 Pools de minería

Dado que la minería es un proceso altamente **aleatorio**, los mineros individuales enfrentan gran varianza en sus recompensas.  
Solución: formar **pools de minería**.

- Los mineros contribuyen con poder de cómputo.  
- El pool distribuye recompensas proporcionalmente al trabajo aportado.  

**Ventaja:** ingresos más estables.  
**Riesgo:** centralización; si pocos pools dominan, se debilita el ideal de descentralización.

---

## 3.8 Ajuste de dificultad

Bitcoin ajusta automáticamente la dificultad de la prueba de trabajo cada **2016 bloques** (~2 semanas):

- Si los bloques se encuentran demasiado rápido, la dificultad aumenta.  
- Si son demasiado lentos, la dificultad baja.  

Este mecanismo mantiene un ritmo estable de producción (~10 min por bloque), sin importar el total de poder de hash en la red.

---

## 3.9 Resumen del capítulo

- El consenso en Bitcoin se logra gracias a la **prueba de trabajo** y la **regla de la cadena más larga**.  
- Los mineros compiten resolviendo acertijos y son incentivados con recompensas de bloque y comisiones.  
- La seguridad contra doble gasto depende de que ningún atacante controle más del 50% del poder de minado.  
- El ajuste de dificultad estabiliza la tasa de producción de bloques.  
- Pools de minería reducen la varianza, pero pueden introducir riesgos de centralización.

---

## 3.10 Preguntas de reflexión (autoestudio)

1. Explica por qué el ataque Sybil no funciona contra Bitcoin.  
2. ¿Por qué la prueba de trabajo debe ser difícil de encontrar pero fácil de verificar?  
3. Calcula la probabilidad de éxito de un atacante con 30% del poder de hash después de 6 confirmaciones.  
4. ¿Qué efectos tendría una caída súbita del 50% en el poder de hash global sobre el tiempo de bloque hasta el próximo ajuste de dificultad?  
5. Discute ventajas y desventajas de los pools de minería respecto a minar en solitario.  

---
