# Capítulo 4: Cómo Almacenar y Usar Bitcoins

> Traducción al español del capítulo 4 del borrador **Bitcoin and Cryptocurrency Technologies**  
> Arvind Narayanan, Joseph Bonneau, Edward Felten, Andrew Miller, Steven Goldfeder (Draft — Feb 9, 2016)

---

## 4.0 Introducción

Hasta ahora hemos visto cómo funcionan las transacciones y cómo los mineros aseguran el consenso.  
En este capítulo exploramos un aspecto práctico: **¿cómo almacenan y usan bitcoins los usuarios?**

En Bitcoin, **poseer monedas = controlar claves privadas**.  
La seguridad del sistema depende de cómo los usuarios generen, almacenen y usen esas claves.  

---

## 4.1 Carteras (Wallets)

Una **cartera Bitcoin** es un software (o hardware) que gestiona claves y transacciones.  
Funciones principales:

- Generar pares de claves (privada/pública).  
- Almacenar claves privadas de forma segura.  
- Crear transacciones y firmarlas.  
- Conectarse a la red para enviar/recibir pagos.  

Tipos de carteras:
- **Carteras de software:** apps en computadoras o móviles.  
- **Carteras web:** controladas por terceros (menos seguras).  
- **Carteras de hardware:** dispositivos dedicados que aíslan claves privadas.  
- **Carteras de papel:** claves impresas físicamente, fuera de línea.  

---

## 4.2 Formatos de direcciones

En Bitcoin no usamos directamente claves públicas, sino **direcciones** derivadas de ellas:

1. Clave pública → hash → dirección base58.  
2. Ejemplo: `1A1zP1eP5QGefi2DMPTfTL5SLmv7DivfNa` (dirección de Satoshi).  

Razones:
- Hash más corto y fácil de manejar.  
- Verificación de errores (checksum).  
- Oculta la clave pública hasta que se gasta, añadiendo seguridad.  

---

## 4.3 Copias de seguridad y recuperación

Perder la clave privada = perder acceso a los bitcoins asociados.  
Por eso, las carteras implementan mecanismos de respaldo:

- **Seeds deterministas (BIP32/BIP39):**  
  A partir de una sola semilla aleatoria se generan muchas claves.  
  → Con guardar 12 o 24 palabras, puedes recuperar toda la cartera.  

- **Exportar claves:** guardar manualmente cada clave (menos práctico).  

**Recomendación:**  
- Guardar la semilla en papel o metal resistente al fuego/agua.  
- Nunca fotografiar ni almacenar en la nube sin cifrado.  

---

## 4.4 Seguridad de las claves privadas

Principales riesgos:
- **Malware en la computadora/móvil.**  
- **Phishing y robo en línea.**  
- **Pérdida accidental (borrado, fallo de hardware).**  

Buenas prácticas:
- Usar carteras de hardware para grandes cantidades.  
- Mantener copias seguras de las semillas.  
- Usar multifirma para custodias compartidas.  
- Separar fondos en “cartera caliente” (uso diario) y “cartera fría” (almacenamiento a largo plazo).

---

## 4.5 Transacciones prácticas

Algunos detalles prácticos de cómo se usan transacciones en la vida real:

- **Cambio:** si no usas toda una salida, debes crear una salida de “cambio” de vuelta a ti mismo.  
- **Comisiones:** si entradas > salidas, la diferencia se convierte en comisión para el minero.  
- **Consolidación:** se pueden juntar varias salidas pequeñas en una transacción con múltiples entradas.  
- **Batching:** un pago puede tener múltiples destinatarios en una sola transacción.  

---

## 4.6 Anonimato y privacidad

Bitcoin **no es anónimo**, sino **seudónimo**:
- Las direcciones no están vinculadas a identidades legales.  
- Pero todas las transacciones son públicas en la blockchain.  
- Con análisis, es posible rastrear patrones y vincular direcciones a usuarios.  

Mejoras de privacidad:
- Generar una nueva dirección para cada pago recibido.  
- Usar mezcladores o CoinJoin.  
- Usar redes adicionales (Tor, VPN).  
- Criptomonedas alternativas enfocadas en privacidad (ej. Monero, Zcash).  

---

## 4.7 Multicuentas y multifirma

Bitcoin permite construir condiciones de gasto más sofisticadas:

- **Multifirma (m-de-n):** se requieren varias firmas para gastar.  
  Ejemplo: 2 de 3 claves deben firmar.  
  Usos: custodias compartidas, fondos de empresas, herencias.  

- **Contratos inteligentes simples:**  
  - Bloqueos por tiempo.  
  - Pagos condicionales.  

---

## 4.8 Resumen del capítulo

- Una **cartera** gestiona claves privadas, direcciones y transacciones.  
- La pérdida de una clave privada implica la pérdida definitiva de los bitcoins.  
- Las **semillas deterministas** simplifican copias de seguridad y recuperación.  
- La seguridad depende de proteger claves contra robo y pérdida.  
- Bitcoin ofrece privacidad limitada; hay técnicas para mejorarla.  
- Multifirma y contratos sencillos permiten usos más complejos y seguros.

---

## 4.9 Preguntas de reflexión (autoestudio)

1. ¿Por qué las direcciones se derivan como un hash de la clave pública en lugar de usar la clave directamente?  
2. Explica cómo una semilla de 12 palabras puede recuperar miles de claves privadas.  
3. Diseña un esquema de multifirma 2-de-3 para una empresa con tres socios.  
4. ¿Qué riesgos tiene almacenar copias de seguridad de una cartera en un servicio de nube sin cifrado?  
5. Da ejemplos de técnicas para mejorar la privacidad al usar Bitcoin.  

---
