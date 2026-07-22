# Apuntes: SQL y Bases de Datos (Fase 1 - Track B)

---

## Índice

1. Conceptos base
2. Entrar y salir de PostgreSQL
3. Crear bases de datos y usuarios
4. `CREATE TABLE` — crear tablas
5. `INSERT INTO` — insertar datos
6. `SELECT`, `WHERE`, `ORDER BY`
7. Relaciones entre tablas y `JOIN`
8. `GROUP BY` y funciones de agregación
9. SQL vs POO — comparación conceptual
10. Herramientas visuales
11. Pendientes / próximos temas

---

## 1. Conceptos base

Una base de datos relacional se organiza en:
- **Tablas** — como hojas de Excel (ej. `estudiantes`, `materias`)
- **Columnas** — los campos de cada tabla (ej. `nombre`, `edad`)
- **Filas** — cada registro individual (ej. una persona específica)

**SQL** = *Structured Query Language* — el lenguaje para hablar con la base de datos: pedir información, insertarla, modificarla.

**PostgreSQL** — no es una sigla, es el nombre propio de un sistema gestor de bases de datos de código abierto que implementa SQL. MySQL es un competidor similar.

**Detalle de sintaxis importante:** en SQL, un comando puede escribirse en varias líneas y no se ejecuta hasta poner un punto y coma `;` al final. Si el prompt cambia de `practica_sql=>` a `practica_sql(>` o `practica_sql'>`, significa que sigue esperando que cierres algo (un paréntesis, o una comilla) — no se rompió nada, solo hay que terminar de escribir el comando o cancelar con `Ctrl+C`.

**Comillas:** las cadenas de texto en SQL se delimitan con comillas **simples** `'texto'`, no dobles. Mezclar `'texto"` da error de sintaxis o deja el prompt esperando más input.

---

## 2. Entrar y salir de PostgreSQL

```bash
sudo -u postgres psql          # entra como superusuario de PostgreSQL
psql -U giojar06 -d practica_sql -h localhost   # entra con usuario propio a una base específica
```

Para salir de la consola de PostgreSQL:
```sql
\q
```

---

## 3. Crear bases de datos y usuarios

Dentro de la consola como superusuario (`sudo -u postgres psql`):

```sql
CREATE USER giojar06 WITH PASSWORD 'tu_password_aqui';
CREATE DATABASE practica_sql OWNER giojar06;
```

Buena práctica: no trabajar siempre como el superusuario `postgres` — crear un usuario propio para el día a día.

---

## 4. `CREATE TABLE` — crear tablas

```sql
CREATE TABLE estudiantes (
    id SERIAL PRIMARY KEY,
    nombre VARCHAR(50),
    carrera VARCHAR(50),
    semestre INT
);
```

**Tipos de datos usados:**
- `SERIAL PRIMARY KEY` — número único que se autoincrementa solo para cada fila (el identificador de cada registro)
- `VARCHAR(50)` — texto de hasta 50 caracteres
- `INT` — número entero

### Relación entre tablas con clave foránea (`REFERENCES`)

```sql
CREATE TABLE materias (
    id SERIAL PRIMARY KEY,
    nombre_materia VARCHAR(50),
    estudiante_id INT REFERENCES estudiantes(id)
);
```

`estudiante_id INT REFERENCES estudiantes(id)` es una **clave foránea** — le dice a PostgreSQL que esa columna debe corresponder a un `id` que ya exista en la tabla `estudiantes`. Esto es lo que conecta ambas tablas: no se puede insertar una materia con un `estudiante_id` que no exista.

---

## 5. `INSERT INTO` — insertar datos

```sql
INSERT INTO estudiantes (nombre, carrera, semestre) VALUES ('Giovanni', 'Ingenieria en Computacion', 2);
INSERT INTO materias (nombre_materia, estudiante_id) VALUES ('Programacion I', 1);
```

El número al final de la segunda línea (`1`) es el `id` del estudiante en la tabla `estudiantes` — así es como se conectan los registros entre tablas.

---

## 6. `SELECT`, `WHERE`, `ORDER BY`

```sql
SELECT * FROM estudiantes;                    -- todas las columnas
SELECT nombre FROM estudiantes;               -- solo la columna nombre
SELECT * FROM estudiantes WHERE semestre = 2; -- filtra por condición
SELECT nombre, carrera FROM estudiantes WHERE carrera = 'Ingenieria en Computacion'; -- columnas específicas + filtro
SELECT * FROM estudiantes ORDER BY nombre;       -- ordena alfabéticamente (A-Z por defecto)
SELECT * FROM estudiantes ORDER BY nombre DESC;  -- orden descendente (Z-A)
```

**El `*` es un comodín** (mismo concepto que `ls *.txt` en terminal) — significa "todas las columnas".

**Buena práctica:** en proyectos reales se evita `SELECT *` cuando es posible, prefiriendo especificar columnas exactas. Razón: si la tabla crece con más columnas después, `SELECT *` trae datos innecesarios, haciendo la consulta más lenta y menos predecible. Para explorar/aprender, `*` está bien.

---

## 7. Relaciones entre tablas y `JOIN`

### `JOIN` (también llamado `INNER JOIN`)

```sql
SELECT estudiantes.nombre, materias.nombre_materia
FROM estudiantes
JOIN materias ON estudiantes.id = materias.estudiante_id;
```

Solo muestra filas donde **hay coincidencia en ambas tablas**. Si un estudiante no tiene ninguna materia asociada, simplemente no aparece en el resultado.

### `LEFT JOIN`

```sql
SELECT estudiantes.nombre, materias.nombre_materia
FROM estudiantes
LEFT JOIN materias ON estudiantes.id = materias.estudiante_id;
```

Muestra **TODAS las filas de la tabla izquierda** (la que va primero en `FROM`), tenga o no coincidencia en la tabla derecha. Cuando no hay coincidencia, rellena con `NULL` (se ve como espacio vacío).

### Cuándo usar cada uno

- `JOIN` normal — cuando solo interesan registros que sí tienen relación (ej. "materias por estudiante", sin importar quién no tiene materias)
- `LEFT JOIN` — cuando se necesita ver TODOS los registros de una tabla, y detectar cuáles no tienen relación en la otra (ej. encontrar estudiantes sin materias asignadas, para darles seguimiento)

---

## 8. `GROUP BY` y funciones de agregación

```sql
SELECT estudiantes.nombre, COUNT(materias.id) AS total_materias
FROM estudiantes
LEFT JOIN materias ON estudiantes.id = materias.estudiante_id
GROUP BY estudiantes.nombre;
```

- `COUNT(materias.id)` — cuenta cuántas filas de `materias` hay para cada grupo
- `AS total_materias` — alias, le da un nombre más claro a la columna calculada
- `GROUP BY estudiantes.nombre` — agrupa resultados por nombre, para que `COUNT` cuente por grupo en vez de sumar todo junto

Se usa `LEFT JOIN` aquí (no `JOIN` normal) para que estudiantes sin materias también aparezcan, con conteo `0`, en vez de desaparecer del resultado.

### Otras funciones de agregación

```sql
SELECT AVG(semestre) FROM estudiantes;  -- promedio
SELECT MAX(semestre) FROM estudiantes;  -- valor máximo
SELECT MIN(semestre) FROM estudiantes;  -- valor mínimo
```

---

## 9. SQL vs POO — comparación conceptual

Una relación entre tablas (con `REFERENCES`) **no es lo mismo** que herencia en POO, aunque se sienten parecidas.

**Herencia en POO** es sobre "es un tipo de":
```cpp
class Animal { };
class Perro : public Animal { };  // Perro ES UN Animal
```

**Relación entre tablas SQL** es sobre "pertenece a" / "está relacionado con", no "es un tipo de":
```
estudiantes (Giovanni) → tiene → materias (Programación I, Bases de datos)
```

**La comparación correcta es composición, no herencia:**
```cpp
class Estudiante {
    vector<Materia> materias;  // un Estudiante TIENE materias, no ES una materia
};
```

| Concepto SQL | Equivalente en POO |
|--------------|---------------------|
| Tabla con `REFERENCES` hacia otra tabla | Composición (una clase que contiene objetos de otra) |
| Herencia (clase padre/hija) | No tiene equivalente directo en bases de datos relacionales estándar |

---

## 10. Herramientas visuales

**DBeaver** — cliente gráfico para ver bases de datos en formato de tabla (como Excel), en vez de solo texto en terminal.

Instalación:
```bash
sudo snap install dbeaver-ce
```

Conexión a PostgreSQL en DBeaver:
- Host: `localhost`
- Port: `5432`
- Database: `practica_sql`
- Username: `giojar06`
- Password: la configurada al crear el usuario

Permite navegar `practica_sql → Schemas → public → Tables → estudiantes` y ver los datos en cuadrícula editable, además de correr queries SQL desde un editor integrado.

---

## 11. Pendientes / próximos temas

- Normalización (1FN, 2FN, 3FN)
- Diagramas Entidad-Relación (ER)
- Subconsultas (subqueries)
- Vistas (`views`)
- `HAVING` (filtrar después de un `GROUP BY`)
- Diseño de una base de datos propia más completa (ej. sistema de biblioteca)

---

## Preguntas para aclarar más adelante

-
-
-
