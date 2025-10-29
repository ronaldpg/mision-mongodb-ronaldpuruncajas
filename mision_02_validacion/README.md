# Misión 02: Validación de Datos con JSON Schema en MongoDB

Autor: Ronald Puruncajas  
Fecha: 29/10/25  
---

## Descripción

Este proyecto implementa un sistema robusto de validación de datos para una base de datos de guardianes y criaturas mágicas utilizando JSON Schema en MongoDB. El objetivo principal es proteger la integridad de los datos mediante reglas de validación estrictas que previenen la inserción de información inválida o incompleta.

### Objetivos Cumplidos

- Implementar validaciones JSON Schema para colecciones MongoDB  
- Crear relaciones entre colecciones (1-1 y 1-N)  
- Validar campos obligatorios y tipos de datos  
- Aplicar restricciones con enum, regex y rangos numéricos  
- Probar inserciones válidas e inválidas  
- Documentar el proceso con capturas de pantalla

---

## Estructura del Proyecto

```
mision_02_validacion/                    # Carpeta principal
│
├── 01_definicion_guardianes.mongodb     # Creación del schema para guardianes
├── 02_definicion_criaturas.mongodb      # Creación del schema para criaturas
├── 03_pruebas_insercion.mongodb         # Pruebas válidas e inválidas de inserción
├── ANALISIS_VALIDACION.md               # Análisis teórico de las validaciones y relaciones
├── README.md                            # Descripción general de la misión
└── Imagenes/                            # Capturas de pantalla del proyecto
    ├── Imagen1_preparacion_entorno.png
    ├── Imagen2_Crear_Coleccion_guardianes.png
    ├── Imagen3_preparacion_bd_criaturas.png
    ├── Imagen4_crear_coleccion_criaturas.png
    ├── Imagen5_Insercion_dato_correcto.png
    ├── Imagen6_Insercion_datos_incorrectos.png
    ├── Imagen7_Insercion_novalida_criatura.png
    └── Imagen8_Insercion_criatura_valida.png
```

---

## Tecnologías Utilizadas

- MongoDB: Base de datos NoSQL orientada a documentos
- JSON Schema: Sistema de validación de documentos
- MongoDB Playground: Entorno de desarrollo para VS Code

---

## Esquemas de Validación

### Colección guardianes

Almacena información de los guardianes encargados de cuidar las criaturas.

Campos y Validaciones:

| Campo | Tipo | Validación | Descripción |
|-------|------|------------|-------------|
| `nombre` | String | Obligatorio | Nombre del guardián |
| `rango` | String | Enum: ["Aprendiz", "Maestro", "Gran Maestro"] | Nivel jerárquico |
| `password_acceso` | String | Regex: min 8 caracteres, 1 mayúscula, 1 número | Contraseña de acceso |
| `nivel` | Integer | Rango: 1-99 | Nivel de experiencia |
| `inventario` | Array | Objetos con {nombre_item, cantidad >= 1} | Items del guardián |

Ejemplo de Documento Válido:
```javascript
{
  nombre: "Rendolf",
  rango: "Gran Maestro",
  password_acceso: "Segura123",
  nivel: 85,
  inventario: [
    { nombre_item: "Poción de curación", cantidad: 5 },
    { nombre_item: "Espada mágica", cantidad: 1 }
  ]
}
```

---

### Colección criaturas

Registra las criaturas mágicas bajo cuidado de los guardianes.

Campos y Validaciones:

| Campo | Tipo | Validación | Descripción |
|-------|------|------------|-------------|
| `nombre` | String | Obligatorio | Nombre de la criatura |
| `habitat` | String | Obligatorio | Lugar donde habita |
| `nivel_peligro` | Integer | Rango: 1-10, Obligatorio | Grado de peligrosidad |
| `es_legendaria` | Boolean | Obligatorio | Indica si es legendaria |
| `habilidades` | Array | Min 1 string único | Lista de habilidades |
| `ficha_veterinaria` | Object | Subdocumento obligatorio | Datos de salud |
| `ficha_veterinaria.salud` | String | Enum: ["Excelente", "Buena", "Regular", "Crítica"] | Estado de salud |
| `ficha_veterinaria.ultima_revision` | Date | Obligatorio | Fecha de última revisión |
| `id_guardian` | ObjectId | Referencia a guardianes | Guardian asignado |

Ejemplo de Documento Válido:
```javascript
{
  nombre: "Troll de Montaña",
  habitat: "Cuevas alpinas",
  nivel_peligro: 7,
  es_legendaria: false,
  habilidades: ["Fuerza sobrehumana", "Regeneración", "Resistencia al frío"],
  ficha_veterinaria: {
    salud: "Buena",
    ultima_revision: new Date("2025-10-20")
  },
  id_guardian: ObjectId("...")
}
```

---

## Pruebas Realizadas

### Inserciones Válidas

Guardián - Caso Exitoso:
```javascript
// Inserción del guardián "Rendolf"
{
  nombre: "Rendolf",
  rango: "Gran Maestro",
  password_acceso: "Segura123",
  nivel: 85,
  inventario: [
    { nombre_item: "Poción de curación", cantidad: 5 }
  ]
}
// Resultado: Inserción exitosa
```

Criatura - Caso Exitoso:
```javascript
// Inserción de criatura "Troll" vinculada a Rendolf
{
  nombre: "Troll de Montaña",
  habitat: "Cuevas alpinas",
  nivel_peligro: 7,
  es_legendaria: false,
  habilidades: ["Fuerza sobrehumana", "Regeneración"],
  ficha_veterinaria: {
    salud: "Buena",
    ultima_revision: new Date("2025-10-20")
  },
  id_guardian: ObjectId("...")
}
// Resultado: Inserción exitosa
```

---

### Inserciones Inválidas

Guardián - Caso Fallido:
```javascript
// Intento de inserción con datos inválidos
{
  nombre: "Guardian Invalido",
  rango: "Novato",              // Rango no permitido
  password_acceso: "debil",     // Sin mayúscula ni número
  nivel: 50
}
// Error: Document failed validation (code 121)
```

Criatura - Caso Fallido:
```javascript
// Intento de inserción sin campos obligatorios
{
  nombre: "Criatura Inválida",
  habitat: "Bosque",
  nivel_peligro: 5,
  es_legendaria: true,
  habilidades: [],              // Array vacío (mínimo 1 requerido)
  ficha_veterinaria: {
    ultima_revision: new Date()  // Falta campo "salud"
  }
}
// Error: Document failed validation (code 121)
```

---

## Cómo Ejecutar el Proyecto

Prerrequisitos:
- MongoDB instalado o Docker con contenedor MongoDB
- VS Code con extensión MongoDB for VS Code
- Conexión a base de datos bestiario

Pasos de Ejecución:

1. Conectar a MongoDB
   ```
   mongodb://localhost:27017
   ```

2. Ejecutar los scripts en orden:
   - 01_definicion_guardianes.mongodb para crear la colección con validaciones
   - 02_definicion_criaturas.mongodb para crear la colección de criaturas
   - 03_pruebas_insercion.mongodb para ejecutar las pruebas de validación

3. Verificar resultados:
   - Las inserciones válidas deben completarse exitosamente
   - Las inválidas deben retornar error código 121

---

## Relaciones entre Colecciones

Relación 1-N: Guardian a Criaturas
- Un guardián puede cuidar múltiples criaturas
- Cada criatura tiene referencia a un único guardián mediante id_guardian
- Se utiliza ObjectId para la referencia

Relación 1-1: Criatura a Ficha Veterinaria
- Cada criatura tiene un documento embebido con su ficha veterinaria
- La ficha contiene estado de salud y fecha de última revisión

---

## Documentación Visual

Para ver las capturas de pantalla del proceso completo, se puede consultar el archivo ANALISIS_VALIDACION.md que incluye la siguiente documentación visual:

- Preparación del entorno
- Creación de colección de guardianes
- Creación de colección de criaturas
- Ejemplos de inserciones válidas e inválidas
- Mensajes de error de validación

---

## Commits Semánticos Utilizados

```
feat: agrega schema de guardianes con validaciones JSON Schema
feat: implementa validación para criaturas con relación 1-1 y 1-N
fix: corrige regex de password en guardianes
test: agrega inserciones válidas e inválidas para verificar reglas
docs: completa análisis teórico de validación y relaciones
```

---

## Aprendizajes Clave

Durante el desarrollo de este proyecto se aprendieron varios conceptos importantes:

- Implementación de JSON Schema en MongoDB
- Uso de validadores como enum, regex, ranges y arrays
- Manejo de relaciones entre colecciones
- Diferencias entre documentos embebidos y referencias
- Manejo de errores de validación con el código 121
- Buenas prácticas en diseño de esquemas NoSQL