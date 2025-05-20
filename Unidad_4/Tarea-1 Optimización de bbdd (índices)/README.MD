# Tarea 1 de trabajo con índices

Un instituto de enseñanza guarda los siguientes datos de sus alumnos:
 - número de inscripción, comienza desde 1 cada año,
 - año de inscripción,
 - nombre del alumno,
 - documento del alumno,
 - domicilio,
 - ciudad,
 - provincia,
 - clave primaria: número de inscripto y año de inscripción.

Se pide: 
- Elimine la tabla "alumno" si existe. 

```SQL
DROP TABLE IF EXISTS alumno;
```

<IMG SRC=IMG/IMG1.PNG>

- Cree la tabla definiendo una clave primaria compuesta (año de inscripción y número de 
inscripción).

```SQL
CREATE TABLE alumno (numero_inscripcion INT NOT NULL, año_inscripcion INT NOT NULL, nombre VARCHAR(100), documento VARCHAR(20), domicilio VARCHAR(150), ciudad VARCHAR(100), provincia VARCHAR(100), PRIMARY KEY (año_inscripcion, numero_inscripcion));
```

<img src=IMG/IMG2.PNG>

- Define los siguientes indices:
   - Un índice único por el campo "documento" y un índice común por ciudad y provincia.

```SQL
   -- Índice único por documento (para evitar documentos duplicados)
CREATE UNIQUE INDEX idx_documento ON alumno (documento);

    -- Índice común por ciudad y provincia (para mejorar búsquedas geográficas)
CREATE INDEX idx_ciudad_provincia ON alumno (ciudad, provincia);
```
    - Vea los índices de la tabla.
        >__Nota__:_Muestra el comando y la salida __"show index"___.

<IMG SRC=IMG/IMG3.PNG>

- Intente ingresar un alumno con clave primaria repetida.
    >__Nota__:_Muestra el comando y la salida_.

```SQL
INSERT INTO alumno VALUES (1, 2024, 'Ana Ruiz', '12345678', 'Calle A 123', 'Córdoba', 'Córdoba');

INSERT INTO alumno VALUES (1, 2024, 'Luis Gómez', '87654321', 'Calle B 456', 'Rosario', 'Santa Fe');
```
<IMG SRC=IMG/IMG4.PNG>

- Intente ingresar un alumno con documento repetido.
    >__Nota__:_Muestra el comando y la salida_.

```SQL
INSERT INTO alumno VALUES (2, 2024, 'Carla Fernández', '12345678', 'Calle C 789', 'Salta', 'Salta');
```

<IMG SRC=IMG/IMG5.PNG>

- Ingrese varios alumnos de la misma ciudad y provincia.
    >__Nota__:_Muestra el comando y la salida_.

```SQL
INSERT INTO alumno VALUES 
(2, 2024, 'Sofía Blanco', '11111111', 'Av. Siempreviva 1', 'Mendoza', 'Mendoza'),
(3, 2024, 'Tomás Pérez', '22222222', 'Av. Siempreviva 2', 'Mendoza', 'Mendoza'),
(4, 2024, 'Lucía López', '33333333', 'Av. Siempreviva 3', 'Mendoza', 'Mendoza');
```

- Elimina los indices creados, y muestra que ya no se encuentran.
    >__Nota__:_Muestra el comando y la salida_.

```SQL
DROP INDEX idx_documento ON alumno;
DROP INDEX idx_ciudad_provincia ON alumno;
```

<IMG SRC=IMG/IMG6.PNG>
