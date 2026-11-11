# 📓 Diseño de Circuitos — Cuaderno digital

**Nombre:** Andy Vladimir Ordoñez Namcela
**Unidad 1:** Lógica combinacional con diseño clásico · **Semanas 1–4**
Carrera de Computación · UNL · 2.º ciclo A · Ing. Luis Darío Sinche Cueva, Mg.
**Resultado de aprendizaje R1:** diseña e implementa circuitos combinacionales básicos con técnicas de diseño clásico.

## Índice

- [Semana 1 — Sistemas numéricos, códigos y compuertas](#semana-1--sistemas-numéricos-códigos-y-compuertas)
- [Semana 2 — Álgebra de Boole y formas canónicas](#semana-2--álgebra-de-boole-y-formas-canónicas)
- [Semana 3 — Mapas de Karnaugh, indiferentes y NAND/NOR](#semana-3--mapas-de-karnaugh-indiferentes-y-nandnor)
- [Semana 4 — Circuitos aritméticos](#semana-4--circuitos-aritméticos)
- [Mapa de la unidad](#mapa-de-la-unidad)
- [Referencias](#referencias)

---

## Semana 1 — Sistemas numéricos, códigos y compuertas

### 1. Representación binaria
- Un transistor es un interruptor controlado por voltaje: 2 estados (0 y 1) son fáciles de distinguir aun con ruido.
- Un "1" y un "0" son **rangos de voltaje**, no valores exactos (familia 74LS):

| Nivel | Salida (promete) | Entrada (exige) |
|---|---|---|
| 1 | ≥ 2,7 V | ≥ 2,0 V |
| 0 | ≤ 0,5 V | ≤ 0,8 V |

- Entre 0,8 V y 2,0 V es la **zona indefinida** (causa típica: entrada sin conectar).
- **Margen de ruido:** alto = 2,7 − 2,0 = 0,7 V · bajo = 0,8 − 0,5 = 0,3 V.

### 2. Sistemas numéricos y conversiones
- Sistema posicional: cada posición vale una potencia de la base. Bit = dígito binario; byte = 8 bits.
- Potencias de 2: `128 64 32 16 8 4 2 1`.
- **Decimal → binario:** dividir entre 2 y leer los restos de abajo hacia arriba. Ej.: 25 = `11001₂`.
- **Hexadecimal:** grupos de 4 bits (A=10 … F=15). **Octal:** grupos de 3 bits.
- Ej.: 157 = `10011101₂` = `9D₁₆` = `235₈`.

### 3. Aritmética binaria y complemento a dos
- Suma: `0+0=0`, `0+1=1`, `1+1=10` (escribo 0, llevo 1), `1+1+1=11`.
- **Complemento a dos:** negativo = invertir bits + 1. El bit izquierdo es el signo.
- Rango con n bits: −2ⁿ⁻¹ a +2ⁿ⁻¹ − 1 (4 bits: −8 a +7 · 8 bits: −128 a +127).
- **Resta = suma del negativo:** `5 − 3 = 0101 + 1101 = 1 0010 → 0010 = 2` (el bit sobrante se descarta).
- **Desbordamiento:** el resultado no cabe en los bits (`0111 + 0001 = 1000 → −8 ✗`). Igual que `int` en Java.

### 4. Códigos
| Código | Idea | Uso |
|---|---|---|
| **BCD** | Cada dígito decimal en 4 bits; 1010–1111 no se usan | Displays, relojes, contadores |
| **Gray** | Entre vecinos cambia un solo bit (método del espejo) | Encoders, sensores, **mapas de Karnaugh** |
| **ASCII** | Tabla de 7 bits (128 caracteres); "A" = 65 = `01000001` | Texto (ñ y tildes → UTF-8) |

### 5. Compuertas lógicas

| Compuerta | Expresión | Sale 1 cuando… |
|---|---|---|
| NOT | `A'` | invierte la entrada |
| AND | `A·B` | **todas** valen 1 |
| OR | `A+B` | **al menos una** vale 1 |
| NAND | `(A·B)'` | no todas valen 1 |
| NOR | `(A+B)'` | ninguna vale 1 |
| XOR | `A ⊕ B` | las entradas son **distintas** |
| XNOR | `(A ⊕ B)'` | las entradas son **iguales** |

- Normas de símbolos: ANSI (la del curso) e IEC (rectángulos).
- **Circuito integrado 74LS08** (4 AND, DIP-14): VCC en pin 14, GND en pin 7; pin 1 junto a la muesca, conteo antihorario.
- Código impreso: `SN 74 LS 08 N` = fabricante · rango comercial · familia (LS=TTL, HC=CMOS) · función · encapsulado.

---

## Semana 2 — Álgebra de Boole y formas canónicas

### 1. Circuito ⇄ expresión
| Circuito | Expresión |
|---|---|
| Cable de entrada | Variable |
| AND / OR / NOT | Producto `·` / Suma `+` / Complemento `'` |

**Precedencia:** NOT → AND → OR. Ejemplo: `Y = A·B + C'`.

### 2. Postulados y dualidad
- Identidad: `X+0=X` · `X·1=X`
- Complemento: `X+X'=1` · `X·X'=0`
- Conmutativa y distributiva (**dual:** `X+YZ = (X+Y)(X+Z)`).
- **Dualidad:** intercambiar `+ ↔ ·` y `0 ↔ 1` (sin complementar variables) mantiene válida la identidad.

### 3. Teoremas (tabla de consulta)
| Teorema | Forma OR | Forma AND |
|---|---|---|
| Elemento nulo | `X+1=1` | `X·0=0` |
| Idempotencia | `X+X=X` | `X·X=X` |
| Involución | `(X')'=X` | |
| Absorción | `X+XY=X` | `X(X+Y)=X` |
| Abs. del complemento | `X+X'Y=X+Y` | `X(X'+Y)=XY` |
| Adyacencia | `XY+XY'=X` | `(X+Y)(X+Y')=X` |
| Consenso | `XY+X'Z+YZ=XY+X'Z` | `(X+Y)(X'+Z)(Y+Z)=(X+Y)(X'+Z)` |

### 4. De Morgan
> **Rompe la barra, cambia el operador.**
> `(AB)' = A' + B'` · `(A+B)' = A'B'` · generaliza a n variables: `(A+B+C)' = A'B'C'`.

### 5. Minitérminos y maxitérminos
- **mᵢ** (producto, vale 1): variable en 1 → directa; en 0 → complementada. Ej.: m₅ (101) = `AB'C`.
- **Mᵢ** (suma, vale 0): variable en 0 → directa; en 1 → complementada. Ej.: M₅ = `A'+B+C'`.
- Relación: `Mᵢ = (mᵢ)'`.
- **SOP canónica:** Σm de las filas donde F = 1 (unidas con OR).
- **POS canónica:** ΠM de las filas donde F = 0 (unidas con AND).
- Los índices de ΠM son los que **faltan** en Σm.

### 6. ¿SOP o POS?
- Pocos 1 → SOP · Pocos 0 → POS.
- Solo NAND → SOP (NAND-NAND, 7400) · Solo NOR → POS (NOR-NOR, 7402).
- Criterio final: **costo** (compuertas, CI, niveles lógicos).

### 7. Ejemplo resuelto: alarma de 3 sensores
Sirena `F` suena si **al menos dos** sensores se activan (A=puerta, B=ventana, C=movimiento).

- `F = Σm(3,5,6,7) = ΠM(0,1,2,4)`
- Simplificando con adyacencia: **`F = AB + AC + BC`** (POS: `(A+B)(A+C)(B+C)`)
- Costo: canónica = 4 CI (7404 + 2×7411 + 7432) vs. simplificada = **2 CI** (7408 + 7432).
- ⚠️ "Exactamente dos" (Σm(3,5,6)) ≠ "al menos dos": hay que interpretar bien el enunciado.

### 8. Procedimiento de simplificación algebraica
1. Pares adyacentes `XY+XY'=X` → 2. Duplicar `X=X+X` si hace falta → 3. Absorción → 4. Absorción del complemento → 5. Consenso.

Ejemplo: `A + A'B + AB'C = A + B`. **No hay algoritmo que garantice el mínimo** → por eso existen los mapas de Karnaugh.

---

## Semana 3 — Mapas de Karnaugh, indiferentes y NAND/NOR

### Sesión 1 · Mapas de Karnaugh

**Idea clave:** el teorema de adyacencia `XY + XY' = X`. Dos minitérminos que difieren en una variable → esa variable desaparece.

**Orden Gray (00, 01, 11, 10)** para que cada celda sea vecina lógica de la siguiente (y la última de la primera).

```
Mapa de 3 variables        Mapa de 4 variables
A\BC 00 01 11 10           AB\CD 00 01 11 10
  0   0  1  3  2            00     0  1  3  2
  1   4  5  7  6            01     4  5  7  6
                            11    12 13 15 14
                            10     8  9 11 10
```

**Reglas para agrupar**
1. Tamaño **2ᵏ** (1, 2, 4, 8, 16): un grupo de 2ᵏ elimina k variables.
2. **Rectangular** (sin L ni diagonales).
3. **Lo más grande posible.**
4. **El mapa se enrolla:** bordes opuestos y las 4 esquinas son vecinos.

**Implicantes**
- *Implicante:* cualquier grupo válido de 1 (o X).
- *Implicante primo:* grupo que ya no se puede agrandar.
- *Primo esencial:* el único primo que cubre algún 1 → debe estar en la solución.
- Solución mínima = primos esenciales + los primos mínimos para cubrir el resto.

**Procedimiento SOP mínima:** colocar 1/0/X → agrupar los más grandes → elegir esenciales → cubrir los 1 restantes → leer un término por grupo.
**Lectura:** variable constante en 1 → `A` · en 0 → `A'` · si cambia → desaparece.

**POS:** se agrupan los **0** y se lee invertido (0 → `A`, 1 → `A'`), cada grupo da una suma.

**Ejemplos resueltos**
| Función | Resultado |
|---|---|
| `Σm(0,2,4,5,6)` (3 var.) | `F = C' + AB'` |
| `Σm(0,1,2,5,8,9,10)` | SOP: `B'C' + B'D' + A'C'D` (7 literales) · POS: `(A'+B')(B'+D)(C'+D')` (6 literales) |
| `Σm(0,2,5,7,8,10,13,15)` | `F = B'D' + BD` (esquinas + bloque central; es una **XNOR** de B y D) |

Se elige SOP o POS según el menor costo en literales, compuertas y CI.

### Sesión 2 · Condiciones indiferentes y NAND/NOR

**Condiciones indiferentes (X):** combinaciones que nunca ocurren (ej.: BCD usa solo 0–9; 10–15 son X).
- **Como 1:** si agrandan un grupo, se incluyen.
- **Como 0:** si no ayudan, se ignoran (ninguna X está obligada a cubrirse).
- **Nunca solas:** un grupo formado solo por X es un término inútil.
- Notación: `F = Σm(...) + d(...)`.

**Ejemplo — dígito BCD ≥ 5:** `F = Σm(5,6,7,8,9) + d(10..15)`

| | Expresión | Literales |
|---|---|---|
| Sin usar X | `AB'C' + A'BD + A'BC` | 9 |
| Con X | **`A + BD + BC`** | 5 |

**Universalidad de NAND y NOR**

| Función | Solo NAND | Solo NOR |
|---|---|---|
| NOT | `(A·A)'` | `(A+A)'` |
| AND | `((AB)')'` | `(A'+B')'` |
| OR | `(A'·B')'` | `((A+B)')'` |

- **SOP mínima → NAND-NAND · POS mínima → NOR-NOR.**
- Conversión SOP → NAND-NAND: doble negación → De Morgan sobre la negación interna → cada paréntesis es una NAND.
  `A + BD + BC = NAND( A', NAND(B,D), NAND(B,C) )`
- El literal que aparece solo entra **complementado** (NAND usada como inversor).
- Mismo retardo que AND-OR (2 niveles) y una sola familia en el inventario.

**Costo en CI (ejemplo BCD ≥ 5):** sin X = 3 CI · con X AND-OR = 2 CI (7408+7432) · con X solo NAND = 2 CI (7400+7410). *El ahorro viene de las X, no de la NAND.*

**Proyecto integrador (3 entregas):** S6 bloque combinacional en Logisim-evolution → S11 se agrega memoria (flip-flops) → S16 descrito en VHDL sobre **Basys 3**. 8 equipos, 4 roles rotativos (Especificación, Diseño, Verificación, Documentación). Basys 3: 16 interruptores, 5 pulsadores, 16 LED, 4 displays de 7 segmentos.

---

## Semana 4 — Circuitos aritméticos

### 1. Semisumador (HA) — solo columna 0
| A | B | C | S |
|---|---|---|---|
| 0 | 0 | 0 | 0 |
| 0 | 1 | 0 | 1 |
| 1 | 0 | 0 | 1 |
| 1 | 1 | 1 | 0 |

`S = A ⊕ B` (XOR) · `C = A·B` (AND). No admite acarreo de entrada.

### 2. Sumador completo (FA) — columnas 1, 2, 3…
Truco: **contar los unos** de la fila y escribirlo en binario `Cout S`.

- **S = 1** si hay un número **impar** de unos → `S = A ⊕ B ⊕ Cin` (en el K-map: tablero de ajedrez, sin grupos).
- **Cout = 1** si hay **al menos dos** unos → `Cout = AB + A·Cin + B·Cin` (tres grupos de 2).

Flujo para no memorizar: tabla de verdad → K-map → expresión → lectura en palabras.

### 3. Sumador de 4 bits (ripple carry)
- 4 sumadores en cadena: `Cout` de uno → `Cin` del siguiente.
- El acarreo avanza como una ola → el resultado se completa de derecha a izquierda.
- **Retardo ≈ 2 · n · 10 ns** (≈2 compuertas por etapa): 4 bits ≈ 80 ns · 16 bits ≈ 320 ns · 32 bits ≈ 640 ns.
- Más bits → más lento.

### 4. Resta con el mismo sumador
`A − B = A + (−B)`, con `−B` = invertir B + 1.
- Una **XOR** actúa como inversor controlado: `B ⊕ M`.
- `M = 0` → suma · `M = 1` → resta (B invertido y `M` también entra a `Cin₀` → aporta el +1).
- Ejemplo: `0110 − 0011 = 0110 + 1101 = 1 0011 → 3`.

### 5. Overflow (con signo)
- 4 bits con signo: −8 a +7. Ej.: `0101 + 0100 = 1001 → −7 ✗`.
- **`V = C3 ⊕ C4`** (acarreo que entra a la última etapa ≠ el que sale).
- `Cout = 1` **no** significa error por sí solo.
- Ej.: `0101 + 0011 = 1000` → sin signo: correcto (8) · con signo: **overflow**.

### 6. Comparador de magnitud
- **A = B:** XNOR bit a bit + AND de todas.
- **A > B:** de izquierda a derecha, el primer bit distinto decide. (Integrado **7485**.)

### 7. Hacia la ALU
Sumador/restador + comparador + AND/OR/XOR + multiplexor (semana 5) → resultado de la instrucción.

**Taller:** `1011 + 0110` con acarreos · `0110 − 0011` con M=1 · retardo de 16 bits · comparar A=10 y B=01.

---

## Mapa de la unidad

```
Sem 1: números, códigos, compuertas
   ↓
Sem 2: álgebra de Boole → tabla → Σm / ΠM → simplificar
   ↓
Sem 3: Karnaugh (SOP/POS) + indiferentes + NAND/NOR
   ↓
Sem 4: aplicar todo → semisumador, sumador completo, resta, comparador
```

**Fórmulas de bolsillo**
- XOR: `A ⊕ B = A'B + AB'`
- Sumador completo: `S = A⊕B⊕Cin` · `Cout = AB + ACin + BCin`
- Complemento a dos: `−B = B' + 1`
- Overflow: `V = C3 ⊕ C4`

---


## Referencias
- Carpinelli (2023). Texto base: mapas de Karnaugh, indiferentes y circuitos aritméticos.
- Garza, J. Á. (2025). *Electrónica digital I*.
- Diapositivas de la clase, Ing. Luis D. Sinche Cueva, UNL.
