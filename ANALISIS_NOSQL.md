# Análisis NoSQL - Bestiario de Azeroth del wow

**Autor:** Ronald Puruncajas 
**Fecha:** 22/10/25

---

## 1. ¿Por qué MongoDB es mejor que SQL para este bestiario?

### Esquema
Cada criatura de WoW tiene atributos únicos:
- **Deathwing** tiene `placas de elementium`
- **Murloc** tiene `sonido_caracteristico`
- **Illidan** tiene `armas` y `frase_iconica`
- **Arthas** tiene `arma_legendaria`

En SQL necesitaríamos:
- Una tabla `criaturas` con muchas columnas NULL
- Tablas adicionales para habilidades, armas, frases
- Múltiples JOINs para consultar

En MongoDB:
- Un solo documento por criatura
- Cada documento tiene solo los campos que necesita
- Sin JOINs ni tablas adicionales

### Objetos Anidados
El campo `estadisticas` es un objeto:
estadisticas: {
  salud: 500000,
  poder: 85000,
  armadura: 95
}
 

En SQL requeriría una tabla `criaturas_estadisticas` separada.

### Arrays Simples
Las `habilidades` son un array simple:
habilidades: ["Cataclismo", "Aliento de Lava"]
 

En SQL requeriría una tabla `criaturas_habilidades` con relación muchos-a-muchos.

---

## 2. Otros Tipos de NoSQL

### CLAVE VALOR

**Uso:** Caché de jugadores online en WoW

 
Clave: "jugador:123456"
Valor: { nombre: "Thrall", nivel: 70, online: true }
 

**Ventaja:** Velocidad extrema (< 1ms) para datos temporales como:
- Lista de jugadores en línea
- Sesiones activas
- Posiciones en tiempo real

**Por qué no MongoDB:** Redis es 10x más rápido para datos simples que cambian constantemente.

---

### Neo4j para su uso con grafos

**Uso:** Red de relaciones entre personajes de WoW

 
(Arthas)-[:MATÓ]->(Uther)
(Arthas)-[:FUE_ALUMNO_DE]->(Uther)
(Illidan)-[:HERMANO_DE]->(Malfurion)
(Illidan)-[:AMA]->(Tyrande)
(Tyrande)-[:PAREJA_DE]->(Malfurion)
 

**Consulta:** "¿Quiénes son amigos de amigos de Thrall que lucharon contra la Legión?"

**Por qué no MongoDB:** MongoDB requiere múltiples consultas para seguir relaciones. Neo4j está optimizado para esto.

---

## 3. Casos de Estudio: Blizzard y MongoDB

### Aplicación: Hearthstone

**Por qué usan MongoDB:**

1. **Cartas con atributos variables**
   - Algunas cartas tienen `combo`
   - Otras tienen `grito de batalla`
   - Otras tienen `última voluntad`
   - MongoDB permite esto sin problema

2. **Mazos de jugadores**
   - Cada mazo es un array de 30 cartas
   - Fácil de almacenar y consultar

3. **Partidas en tiempo real**
   - Estado del juego cambia constantemente
   - MongoDB actualiza rápido con `updateOne()`

4. **Escalabilidad**
   - Millones de usuarios
   - MongoDB distribuye datos en múltiples servidores