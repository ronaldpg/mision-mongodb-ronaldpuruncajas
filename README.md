# Bestiario de Azeroth - Implementación con MongoDB
**Autor:** Ronald Puruncajas  
**Fecha:** 22 de octubre de 2025  

## 1. Introducción

El proyecto denominado "Bestiario de Azeroth" tiene como objetivo desarrollar una base de datos documental para gestionar y almacenar información sobre criaturas del universo World of Warcraft (WoW). La implementación se realizó utilizando MongoDB, una base de datos NoSQL orientada a documentos que facilita el modelado de estructuras de datos complejas y heterogéneas con flexibilidEl sistema registra criaturas emblemáticas como Deathwing, Arthas Menethil, Illidan Tempestira y Sylvanas Windrunner, así como criaturas comunes, entre ellas Murlocs y Hogger. Cada registro contiene información detallada sobre habilidades, estadísticas, localización y nivel de peligro, estructurada en documentos JSON dentro de una colección denominada "criaturas".turas".

## 2. Tecnologías Utilizadas

- MongoDB Atlas: Plataforma de base de datos NoSQL en la nube utilizada para la gestión del entorno y la persistencia de los datos.  

- Visual Studio Code: Entorno de desarrollo empleado para la escritura y ejecución de los scripts.  

- Extensión MongoDB para VS Code: Herramienta que facilita la conexión directa con MongoDB Atlas y la ejecución de consultas desde el editor.  

- Lenguaje JavaScript: Lenguaje utilizado para definir las operaciones CRUD (Create, Read, Update, Delete) y la manipulación de los datos en MongoDB.  

## 3. Estructura del Proyecto

├── misiones.mongodb → Contiene las operaciones CRUD implementadas

├── ANALISIS_NOSQL.md → Análisis comparativo entre MongoDB y otros modelos NoSQL

└── README.md → Documento descriptivo del proyecto

## 4. Procedimiento de Ejecución

La ejecución del sistema requiere iniciar sesión en MongoDB Atlas y copiar la cadena de conexión para vincular MongoDB Atlas con Visual Studio Code (VS Code). Tras establecer la conexión, es posible ejecutar los archivos necesarios para el funcionamiento de la base de datos.

## 5. Operaciones CRUD Implementadas

### 5.1. Operaciones de Inserción (CREATE)

- insertOne(): Inserta un documento individual, utilizado para el registro de Deathwing.  

- insertMany(): Permite la inserción masiva de múltiples criaturas, como Arthas Menethil, Illidan Tempestira, Sylvanas Windrunner, Hogger y Murloc.  

### 5.2. Operaciones de Consulta (READ)

Se implementaron consultas para:  

- Obtener todas las criaturas registradas.  

- Filtrar por localización geográfica.  

- Seleccionar criaturas con nivel de peligro superior a 8.  

- Mostrar únicamente los Raid Bosses.  

- Consultar criaturas con determinadas estadísticas específicas, como salud superior a 400,000 puntos. 

### 5.3. Operaciones de Actualización (UPDATE)

- updateOne(): Agrega nuevas habilidades a Deathwing y registra la fecha de actualización.  

- updateMany(): Incrementa el nivel de peligro de todas las criaturas del Bosque de Elwynn y asocia un evento denominado "Invasión de la Legión Ardiente".  

- updateMany(): Adicionalmente, se catalogan todas las criaturas agregando campos de auditoría (catalogado, fecha_catalogo). 
 
## 6. Estructura de los Documentos

Cada criatura registrada posee los siguientes atributos principales:  

- nombre (String)  

- raza (String)  

- facción (String)  

- nivel_peligro (Número entero de 1 a 10)  

- localización (String)  

- habilidades (Array de Strings)  

- estadísticas (Objeto anidado con salud, poder, armadura)  

- atributos únicos (dependientes de la criatura)  

**Ejemplo de documento:**  
```json
{
  "nombre": "Deathwing",
  "raza": "Dragón Negro",
  "faccion": "Aspecto de la Muerte",
  "nivel_peligro": 10,
  "localizacion": "Espina Negra",
  "habilidades": ["Cataclismo", "Aliento de Lava"],
  "estadisticas": { "salud": 500000, "armadura": 95 }
}
```

## 7. Análisis Comparativo NoSQL

### 7.1. Ventajas de MongoDB frente a SQL

**Estructura flexible:**
Cada criatura posee campos únicos. En bases de datos relacionales, esto implicaría múltiples tablas y columnas nulas, así como operaciones JOIN adicionales.
En MongoDB, cada documento contiene únicamente los campos que necesita, sin requerir relaciones explícitas.

**Objetos anidados:**
El subdocumento estadisticas permite agrupar atributos como salud, poder y armadura dentro de un mismo campo, evitando la necesidad de crear tablas auxiliares.

**Arreglos:**
MongoDB admite directamente arrays, como el caso del campo habilidades, que en un sistema relacional requeriría una tabla intermedia para representar relaciones de tipo muchos-a-muchos.

### 7.2. Comparación con otros modelos NoSQL

**Modelo Clave-Valor (Redis):**
Utilizado principalmente para el almacenamiento temporal de datos, como sesiones de jugadores o listas de usuarios en línea. Redis ofrece una latencia menor a 1 ms, siendo ideal para operaciones en tiempo real.

**Modelo de Grafos (Neo4j):**
Adecuado para representar relaciones entre personajes, por ejemplo:

(Arthas) → [MATÓ] → (Uther)
(Illidan) → [HERMANO_DE] → (Malfurion)

MongoDB puede representar estas relaciones, pero no está optimizado para consultas de grafos complejos, donde Neo4j ofrece un mejor rendimiento y expresividad semántica.

## 8. Caso de Estudio: Blizzard Entertainment

La empresa Blizzard Entertainment, desarrolladora de World of Warcraft y Hearthstone, emplea MongoDB en varios de sus proyectos debido a las siguientes razones:

- **Estructuras de datos dinámicas:** Las cartas de Hearthstone presentan atributos variables, que son fácilmente modeladas mediante documentos en MongoDB.

- **Almacenamiento de mazos:** Cada mazo de jugador puede representarse como un arreglo de cartas, lo que simplifica su consulta y actualización.

- **Procesamiento en tiempo real:** Las partidas requieren actualizaciones constantes del estado del juego, que MongoDB maneja eficientemente mediante operaciones como `updateOne()`.

- **Escalabilidad horizontal:** MongoDB permite distribuir los datos entre varios nodos, garantizando alta disponibilidad ante grandes volúmenes de jugadores activos.

## 9. Conclusiones

- El proyecto Bestiario de Azeroth demuestra la versatilidad y eficiencia de MongoDB en la representación de información compleja y heterogénea, como la asociada a criaturas con atributos variables.

- El modelo documental de MongoDB simplifica el almacenamiento, reduce la redundancia y elimina las limitaciones de los modelos relacionales.

- La integración de MongoDB con herramientas como MongoDB Atlas y Visual Studio Code proporciona un entorno de desarrollo ágil y escalable, adecuado tanto para aplicaciones académicas como para la industria de los videojuegos.