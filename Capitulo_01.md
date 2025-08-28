# Capítulo 1: Introducción a la Criptografía y las Criptomonedas

> Traducción al español del capítulo 1 del borrador **Bitcoin and Cryptocurrency Technologies**  
> Arvind Narayanan, Joseph Bonneau, Edward Felten, Andrew Miller, Steven Goldfeder (Draft — Feb 9, 2016)

---

## 1.0 Panorama general

Todas las monedas necesitan mecanismos para **controlar la oferta** y **hacer cumplir reglas de seguridad** que eviten el fraude.  
En las monedas fiduciarias, los **bancos centrales** controlan la emisión y el efectivo incorpora **medidas antifalsificación**. Aun así, la aplicación de la ley es quien, en última instancia, detiene a los infractores.

Las **criptomonedas** también requieren impedir que los participantes **manipulen el estado del sistema** o **se contradigan** (equivocación). Por ejemplo, si Alice convence a Bob de que le pagó una moneda digital, no debe poder convencer a Carol de que le pagó **esa misma** moneda.  
A diferencia de las monedas fiduciarias, estas reglas deben cumplirse **solo con tecnología**, sin una autoridad central.

Como su nombre sugiere, las criptomonedas dependen fuertemente de la **criptografía**, que permite codificar reglas y garantías de seguridad dentro del propio sistema. En este capítulo cubrimos dos primitivas esenciales:

- **Funciones hash criptográficas**
- **Firmas digitales**

Con ellas se construyen estructuras como **punteros hash** y **árboles de Merkle**, fundamentales para la integridad de datos y el encadenamiento de estados. Cerraremos con **mini-diseños de criptomonedas** que ilustran retos clásicos (como el doble gasto).

---

## 1.1 Funciones hash criptográficas

Una **función hash** es una función \( H(\cdot) \) con estas propiedades básicas:

1. **Entrada arbitraria:** acepta cadenas de **cualquier** tamaño.
2. **Salida de tamaño fijo:** produce, por ejemplo, **256 bits**.
3. **Eficiencia:** se calcula rápidamente (tiempo cercano a \(O(n)\) para entrada de longitud \(n\)).

Para uso **criptográfico**, además exigimos:

- **Resistencia a colisiones:** es inviable encontrar \(x \ne y\) tales que \(H(x) = H(y)\).  
  > Las colisiones *existen* (por principio del palomar), pero debe ser **computacionalmente inalcanzable** hallarlas.

- **Ocultamiento (hiding):** si elegimos un **valor secreto aleatorio** \(r\) y publicamos \(H(r \,\|\, m)\), el hash **no revela** \(m\).  
  > Útil para *compromisos* (commitments): publicar \(C = H(r \,\|\, m)\) “bloquea” \(m\) sin mostrarlo; más tarde se **abre** revelando \(r, m\).

- **Apto para acertijos (puzzle-friendliness):** dado un objetivo, es difícil encontrar una entrada que produzca un hash por debajo de cierto **umbral**.  
  > Base de los **acertijos de cómputo** (prueba de trabajo): “encuentra \(x\) tal que \(H(x)\) tenga \(k\) ceros iniciales”.

### Notas útiles

- **Preimagen:** dado un hash \(y\), es inviable encontrar \(x\) tal que \(H(x)=y\).
- **Segunda preimagen:** dado \(x\), es inviable encontrar \(x'\ne x\) con el mismo hash.
- **Cumplimiento probabilístico:** “inviable” se refiere a costos astronómicos con tecnología realista.

---

## 1.2 Punteros hash y estructuras encadenadas

Un **puntero hash** es un puntero que, además de referir una ubicación, incluye un **hash del contenido** apuntado. Si el contenido cambia, el hash **no coincide** y la referencia queda invalidada.

Con punteros hash se construyen **listas enlazadas** y **árboles** donde cada nodo “sella” a sus predecesores:

- **Lista con punteros hash:** cada bloque contiene datos, el hash del bloque previo y, opcionalmente, metadatos (p. ej., marca de tiempo).  
  > Cualquier cambio retroactivo rompe todos los hashes siguientes (*inmutabilidad por encadenamiento*).

- **Árboles de Merkle:** estructura en árbol binario donde cada nodo interno guarda el hash de la **concatenación** de los hashes de sus hijos.  
  - La **raíz de Merkle** compromete todo el conjunto.  
  - Pruebas de membresía son **logarítmicas** en el número de elementos (solo envías la ruta de hermanos).

**Usos clave:**
- Comprometer un conjunto de transacciones con una sola **raíz**.
- Enviar **pruebas ligeras** (SPV) sin revelar o almacenar todo el conjunto.

---

## 1.3 Firmas digitales

Una **firma digital** permite a un emisor demostrar, con su **clave privada**, que aprueba un mensaje; cualquiera puede verificarlo con su **clave pública**.

**Algoritmo abstracto:**
- **Gen:** produce \((sk, pk)\) (secreto y público).
- **Firmar:** \(\sigma = \text{Sign}_{sk}(m)\).
- **Verificar:** \(\text{Verify}_{pk}(m, \sigma) \to \{\text{válida}, \text{inválida}\}\).

**Propiedades:**
- **Corrección:** firmas legítimas verifican correctamente.
- **Inforgeabilidad:** sin el secreto \(sk\) es inviable producir \(\sigma\) válida sobre un mensaje no firmado por el dueño.
- **No repudio (práctico):** un firmante no puede negar creíblemente una firma válida asociada a su \(pk\).

En Bitcoin se utilizan firmas de **curvas elípticas** (ECDSA/secp256k1; hoy también se usan **Schnorr** en extensiones), pero a este nivel basta la abstracción.

**Ideas importantes para criptomonedas:**
- El “**derecho a gastar**” un valor se puede modelar como “demuestra posesión de \(sk\) cuyo \(pk\) (o hash de \(pk\)) se especifica en la condición de gasto”.
- Se pueden codificar **políticas**: N-de-M firmas, bloqueos temporales, scripts, etc.

---

## 1.4 Diseños simples de “criptomonedas” (modelos pedagógicos)

Para entender los retos, pensemos en versiones simplificadas:

### 1) Servidor central con libro mayor
- Una entidad mantiene saldos y autoriza transferencias firmadas por los dueños.
- **Ventajas:** sencillo, eficiente.
- **Problemas:** **punto único de fallo** y **confianza** total en el operador.

### 2) Fichas firmadas (estilo “moneda-mensaje”)
- Cada “moneda” es un mensaje transferible firmado por el dueño actual hacia el siguiente.
- **Problema central:** **doble gasto**. Sin un registro global del *primer* receptor válido, un emisor puede firmar dos transferencias conflictivas.

### 3) Libro mayor distribuido ingenuo
- Todos mantienen una copia del registro; se añade una entrada si la mayoría la acepta.
- **Problema:** la “mayoría” por identidades es trivial de falsificar (ataques Sybil). Se necesita un **recurso escaso** para ponderar votos.

### 4) Resolver la coordinación con acertijos (intuición de PoW)
- Obligar a que cada bloque/entrada requiera resolver un **acertijo costoso** (hash con umbral).
- La “influencia” en el consenso es proporcional al **cómputo** aportado (no a identidades).
- Dificultad ajustable: mantener ritmo objetivo (p. ej., promedio de 10 minutos por bloque).

> Este capítulo introduce las piezas criptográficas que hacen **posible** este enfoque.  
> Los detalles de la red, consenso y ajuste de dificultad se desarrollan en capítulos posteriores.

---

## 1.5 Compromisos (commitments) con hash

Un **compromiso** es como cerrar un mensaje en una caja:
- **Fase de compromiso:** publicar \(C = H(r \,\|\, m)\) con \(r\) aleatorio secreto.
- **Fase de apertura:** revelar \(r, m\); cualquiera verifica \(H(r \,\|\, m) = C\).

**Propiedades:**
- **Ocultamiento:** \(C\) no revela \(m\) antes de abrir.
- **Firmeza (binding):** tras publicar \(C\), el emisor **no puede** abrir \(C\) como dos mensajes distintos con alta probabilidad.

Usos: loterías, subastas ciegas, selección de nonce, anclaje de datos, enlaces a estados fuera de cadena, etc.

---

## 1.6 Árboles de Merkle en detalle

Dado un conjunto \(\{d_1, d_2, \dots, d_n\}\):

1. **Hoja:** \(h_i = H(d_i)\).
2. **Nivel interno:** combina de dos en dos: \(h_{i,j} = H(h_i \,\|\, h_j)\).
3. **Raíz:** repetir hasta obtener un único hash.

**Prueba de membresía:** para demostrar que \(d_k\) está incluido:
- Envías \(d_k\) y los **hermanos** de cada nivel en la ruta a la raíz.
- El verificador recalcula y compara la raíz con la comprometida.

**Ventajas:**
- Prueba de tamaño \(O(\log n)\).
- Verificación rápida, ideal para clientes ligeros (no guardan todo).

---

## 1.7 Resumen del capítulo

- **Hashes**: resistencia a colisiones, ocultamiento y dificultad de preimagen hacen posible *acertijos* y *compromisos*.
- **Punteros hash** y **Merkle**: encadenan estados y permiten pruebas compactas de integridad.
- **Firmas digitales**: vinculan autoridad de gasto con claves públicas; habilitan políticas de gasto.
- **Modelos simples** muestran el talón de Aquiles: **doble gasto** y **coordinación confiable**. La idea de ponderar por **trabajo** (acertijos) anticipa el consenso que estudiaremos más adelante.

---

## 1.8 Preguntas de reflexión (autoestudio)

1. Explica por qué una función hash **debe** tener colisiones y, aun así, puede ser **segura** contra su hallazgo práctico.  
2. ¿En qué se diferencian **preimagen** y **segunda preimagen**? Da un ejemplo de amenaza donde importe cada una.  
3. Construye un esquema de **compromiso** con hash y analiza sus propiedades de ocultamiento y firmeza.  
4. ¿Por qué los **árboles de Merkle** permiten pruebas \(O(\log n)\)? Ilustra una verificación de membresía con 8 elementos.  
5. Diseña una **moneda transferible por firmas** y muestra cómo ocurre el **doble gasto** sin un registro compartido.  
6. Describe la intuición de usar **acertijos de cómputo** para ordenar transacciones sin identidades confiables.

---
