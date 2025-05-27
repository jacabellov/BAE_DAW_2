## 🧾 Contexto

Como analista de datos en una universidad, se te ha encargado la explotación de una base de datos que permita gestionar estudiantes, profesores, cursos y matrículas. Además, deberás demostrar habilidades en consultas SQL, índices, vistas, procedimientos y funciones. Si la base de datos no estuvira creada, a continuación tienes el [init.sql](docker-init/init.sql).

## Base de datos en docker

Crea una carpeta y añade el fichero **docker-compose.yml** y el directorio **docker-init**.

Ejecuta a continuación el siguiente comando:

```sql
docker compose up -d 
```

Obtendrar una salida similar a la siguiente:

```console
 docker compose up -d   
[+] Running 2/2
 ✔ Container adminer_container  Started                                                                                                             0.9s 
 ✔ Container mysql_container    Started    
```

A continuación ejecuta el siguiente comando:

```console
docker exec -it mysql_container mysql -u root -p
```

Indica el *password* que es **bae**.

A continuación debes de estar dentro de la consola:

```sql
....
....
....
Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql>
```

> **IMPORTANTE**: *Para salir de la consola se debe de ejecutar* ***exit***.

Verifica las bases de datos que tienes cargadas: (*SHOW DATABASES;*)

```console
SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| bae                |
| information_schema |
| mysql              |
| performance_schema |
| sys                |
| universidad        |
+--------------------+
6 rows in set (0.00 sec)
```

Usa la base de datos **universidad**: (*use universidad;*)

```console
use universidad;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
```

También podemos acceder a través del navegador. Para ello utilizaremos **Adminer** porque *simula la línea de comandos, y nos ayuda a aprender*. Una vez que accedas a [http://localhost:8099](http://localhost:8099), Adminer te pedirá los siguientes datos:

> Puedes consultar la documentación [aquí](https://www.adminer.org/en/).

- **Sistema**: `MySQL`
- **Servidor**: `db`  
  *Es el nombre del servicio del contenedor MySQL dentro del mismo `docker-compose` (Adminer y MySQL están en la misma red `db-network`).*
- **Usuario**: `bae`
- **Contraseña**: `bae`

> ***Lee atentamente cada una de los puntos y cuestiones y realiza***.

## 🔎 Parte 1: Consultas SQL

### A. Consultas Simples

1. Mostrar todos los cursos disponibles.

```sql
mysql> select * from cursos;
+----+----------------------+-------------+----------+
| id | nombre               | profesor_id | creditos |
+----+----------------------+-------------+----------+
|  1 | Álgebra Lineal      |           1 |        6 |
|  2 | Programación I      |           2 |        5 |
|  3 | Mecánica Clásica   |           3 |        6 |
|  4 | Estructuras de Datos |           2 |        5 |
|  5 | Cálculo I           |           1 |        6 |
+----+----------------------+-------------+----------+
5 rows in set (0.001 sec)
```

2. Mostrar el nombre de todos los profesores.

```sql
mysql> select nombre from profesores;
+------------------+
| nombre           |
+------------------+
| Dra. Ana Torres  |
| Dr. Luis Gómez  |
| Dra. Marta Díaz |
+------------------+
3 rows in set (0.001 sec)
```

3. Listar todas las matrículas.

```sql
mysql> select * from matriculas;
+----+---------------+----------+------------+
| id | estudiante_id | curso_id | fecha      |
+----+---------------+----------+------------+
|  1 |             1 |        1 | 2021-09-01 |
|  2 |             2 |        2 | 2022-09-01 |
|  3 |             3 |        3 | 2023-09-02 |
|  4 |             4 |        4 | 2024-09-03 |
|  5 |             1 |        5 | 2020-09-04 |
|  6 |             2 |        4 | 2022-09-05 |
|  7 |             3 |        1 | 2023-09-06 |
|  8 |             4 |        2 | 2024-09-06 |
+----+---------------+----------+------------+
8 rows in set (0.003 sec)
```

4. Ver los nombres y correos de los estudiantes.

```sql
mysql> select * from estudiantes;
+----+-------------------+----------------+-----------+
| id | nombre            | email          | ciudad    |
+----+-------------------+----------------+-----------+
|  1 | María López     | maria@uni.edu  | Madrid    |
|  2 | Juan Pérez       | juan@uni.edu   | Barcelona |
|  3 | Lucía Fernández | lucia@uni.edu  | Valencia  |
|  4 | Carlos Ruiz       | carlos@uni.edu | Sevilla   |
+----+-------------------+----------------+-----------+
4 rows in set (0.002 sec)
```

5. Ver los cursos y su número de créditos.

```sql
mysql> select nombre, creditos from cursos;
+----------------------+----------+
| nombre               | creditos |
+----------------------+----------+
| Álgebra Lineal      |        6 |
| Programación I      |        5 |
| Mecánica Clásica   |        6 |
| Estructuras de Datos |        5 |
| Cálculo I           |        6 |
+----------------------+----------+
5 rows in set (0.001 sec)
```
---

### B. Consultas con `WHERE`. Obligatorio utilizar **WHERE** donde se **combine dos o más tablas**

1. Obtener los cursos impartidos por profesores del departamento de Informática.

```sql
mysql> select cursos.nombre, profesores.nombre from cursos, profesores where cursos.profesor_id=profesores.id and profesores.departamento='Informatica';
```

2. Obtener los estudiantes que viven en Madrid.

```sql
mysql> select nombre from estudiantes where ciudad='Madrid';
+---------------+
| nombre        |
+---------------+
| María López |
+---------------+
1 row in set (0.001 sec)
```

3. Mostrar los cursos con más de 5 créditos.

```sql
mysql> select nombre, creditos from cursos where creditos>5;
+--------------------+----------+
| nombre             | creditos |
+--------------------+----------+
| Álgebra Lineal    |        6 |
| Mecánica Clásica |        6 |
| Cálculo I         |        6 |
+--------------------+----------+
3 rows in set (0.002 sec)
```

4. Ver las matrículas realizadas después del año 2022.

```sql
mysql> select * from matriculas where fecha>2022-12-31;
+----+---------------+----------+------------+
| id | estudiante_id | curso_id | fecha      |
+----+---------------+----------+------------+
|  1 |             1 |        1 | 2021-09-01 |
|  2 |             2 |        2 | 2022-09-01 |
|  3 |             3 |        3 | 2023-09-02 |
|  4 |             4 |        4 | 2024-09-03 |
|  5 |             1 |        5 | 2020-09-04 |
|  6 |             2 |        4 | 2022-09-05 |
|  7 |             3 |        1 | 2023-09-06 |
|  8 |             4 |        2 | 2024-09-06 |
+----+---------------+----------+------------+
8 rows in set, 1 warning (0.002 sec)
```

5. Ver los cursos impartidos por la profesora “Dra. Ana Torres”.

```sql
mysql> select cursos.nombre , profesores.nombre from cursos, profesores where cursos.profesor_id=profesores.id and profesores.nombre='Dra. Ana Torres';
+-----------------+-----------------+
| nombre          | nombre          |
+-----------------+-----------------+
| Álgebra Lineal | Dra. Ana Torres |
| Cálculo I      | Dra. Ana Torres |
+-----------------+-----------------+
2 rows in set (0.005 sec)
```

---

### C. Consultas con `JOIN`.  Obligatorio utilizar **JOIN** donde se **combine dos o más tablas**

> (Devuelven el mismo resultado que las anteriores, pero usando combinaciones de tablas)

1. Mostrar cursos impartidos por profesores del departamento de Informática.

```sql
mysql> select cursos.nombre, profesores.nombre from cursos join profesores on cursos.profesor_id=profesores.id where profesores.departamento='Informtica';
```

2. Obtener estudiantes que viven en Madrid.

```sql
mysql> select estudiantes.nombre, estudiantes.ciudad, cursos.nombre from estudiantes join matriculas on estudiantes.id=matriculas.estudiante_id join cursos on matricu
las.curso_id=cursos.id where estudiantes.ciudad='Madrid';
+---------------+--------+-----------------+
| nombre        | ciudad | nombre          |
+---------------+--------+-----------------+
| María López | Madrid | Álgebra Lineal |
| María López | Madrid | Cálculo I      |
+---------------+--------+-----------------+
2 rows in set (0.001 sec)
```

3. Mostrar cursos con más de 5 créditos.

```sql
mysql> select cursos.nombre, cursos.creditos, profesores.nombre from cursos join profesores on cursos.profesor_id=profesores.id where cursos.creditos > 5;
+--------------------+----------+------------------+
| nombre             | creditos | nombre           |
+--------------------+----------+------------------+
| Álgebra Lineal    |        6 | Dra. Ana Torres  |
| Mecánica Clásica |        6 | Dra. Marta Díaz |
| Cálculo I         |        6 | Dra. Ana Torres  |
+--------------------+----------+------------------+
3 rows in set (0.023 sec)
```

4. Listar matrículas realizadas después del año 2022.

```sql
mysql> SELECT estudiantes.nombre, cursos.nombre, matriculas.fecha FROM matriculas JOIN estudiantes ON matriculas.estudiante_id = estudiantes.id JOIN cursos ON matriculas.curso_id = cursos.id WHERE matriculas.fecha > '2022-12-31';
+-------------------+----------------------+------------+
| nombre            | nombre               | fecha      |
+-------------------+----------------------+------------+
| Lucía Fernández | Mecánica Clásica   | 2023-09-02 |
| Carlos Ruiz       | Estructuras de Datos | 2024-09-03 |
| Lucía Fernández | Álgebra Lineal      | 2023-09-06 |
| Carlos Ruiz       | Programación I      | 2024-09-06 |
+-------------------+----------------------+------------+
4 rows in set (0.007 sec)
```

5. Mostrar los cursos impartidos por la profesora “Dra. Ana Torres”.

```sql
mysql> SELECT cursos.nombre FROM cursos JOIN profesores ON cursos.profesor_id = profesores.id WHERE profesores.nombre = 'Dra. Ana Torres';
+-----------------+
| nombre          |
+-----------------+
| Álgebra Lineal |
| Cálculo I      |
+-----------------+
2 rows in set (0.009 sec)
```
---

### D. Consultas con Subconsultas

> (Devuelven el mismo resultado que las anteriores, pero usando subconsultas)

1. Cursos impartidos por profesores del departamento de Informática.

```sql
mysql> select * from cursos where profesor_id in (select id from profesores where departamento 
= 'Informatica');
```

2. Estudiantes que viven en Madrid.

```sql
mysql> select * from estudiantes  where id in (select id from estudiantes where ciudad = 'Madri
d');
+----+---------------+---------------+--------+
| id | nombre        | email         | ciudad |
+----+---------------+---------------+--------+
|  1 | María López | maria@uni.edu | Madrid |
+----+---------------+---------------+--------+
1 row in set (0.003 sec)
```

3. Cursos con más de 5 créditos.

```sql
mysql> select * from cursos where id in (select id from cursos where creditos > 5);
+----+--------------------+-------------+----------+
| id | nombre             | profesor_id | creditos |
+----+--------------------+-------------+----------+
|  1 | Álgebra Lineal    |           1 |        6 |
|  3 | Mecánica Clásica |           3 |        6 |
|  5 | Cálculo I         |           1 |        6 |
+----+--------------------+-------------+----------+
3 rows in set (0.002 sec)
```

4. Matrículas realizadas después del año 2022.

```sql
mysql> select * from matriculas where id in (select id from matriculas where fecha > '2022-12-3
1');
+----+---------------+----------+------------+
| id | estudiante_id | curso_id | fecha      |
+----+---------------+----------+------------+
|  3 |             3 |        3 | 2023-09-02 |
|  4 |             4 |        4 | 2024-09-03 |
|  7 |             3 |        1 | 2023-09-06 |
|  8 |             4 |        2 | 2024-09-06 |
+----+---------------+----------+------------+
4 rows in set (0.001 sec)
```

5. Cursos impartidos por la profesora “Dra. Ana Torres”.

```sql
mysql> SELECT nombre FROM cursos WHERE profesor_id = (SELECT id FROM profesores WHERE nombre = 'Dra. Ana Torres');
+-----------------+
| nombre          |
+-----------------+
| Álgebra Lineal |
| Cálculo I      |
+-----------------+
2 rows in set (0.006 sec)
```

---

## 📂 Parte 2: Índices

1. Crear un índice en la columna `ciudad` de la tabla `estudiantes`.

```sql
mysql> create index idx_ciudad on estudiantes(ciudad);
Query OK, 0 rows affected (0.201 sec)
Records: 0  Duplicates: 0  Warnings: 0
```

2. Crear un índice en la columna `departamento` de la tabla `profesores`.

```sql
mysql> create index idx_departamento on profesores(departamento);
Query OK, 0 rows affected (0.052 sec)
Records: 0  Duplicates: 0  Warnings: 0
```

3. Consultar los índices creados.

```sql
mysql> show index from estudiantes;
+-------------+------------+------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+     
| Table       | Non_unique | Key_name   | Seq_in_index | Column_name | Collation | Cardinality | Sub_part | Packed | Null | Index_type | Comment | Index_comment | Visible | Expression |     
+-------------+------------+------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+     
| estudiantes |          0 | PRIMARY    |            1 | id          | A         |           4 |     NULL |   NULL |      | BTREE      |         |               | YES     | NULL       |     
| estudiantes |          1 | idx_ciudad |            1 | ciudad      | A         |           4 |     NULL |   NULL | YES  | BTREE      |         |               | YES     | NULL       |     
+-------------+------------+------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+     
2 rows in set (0.016 sec)
```
```sql
mysql> show index from profesores;
+------------+------------+------------------+--------------+--------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
| Table      | Non_unique | Key_name         | Seq_in_index | Column_name  | Collation | Cardinality | Sub_part | Packed | Null | Index_type | Comment | Index_comment | Visible | Expression |
+------------+------------+------------------+--------------+--------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
| profesores |          0 | PRIMARY          |            1 | id           | A         |           2 |     NULL |   NULL |      | BTREE      |         |               | YES     | NULL       |
| profesores |          1 | idx_departamento |            1 | departamento | A         |           2 |     NULL |   NULL | YES  | BTREE      |         |               | YES     | NULL       |
+------------+------------+------------------+--------------+--------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
2 rows in set (0.008 sec)
```

4. Eliminar ambos índices.

```sql
mysql> DROP INDEX idx_ciudad ON estudiantes;
Query OK, 0 rows affected (0.027 sec)
Records: 0  Duplicates: 0  Warnings: 0
```
```sql
mysql> DROP INDEX idx_departamento ON profesores;
Query OK, 0 rows affected (0.014 sec)
Records: 0  Duplicates: 0  Warnings: 0
```
---

## 🪞 Parte 3: Vistas

1. Crear una vista llamada `vista_matriculas_completas` que muestre:
   - nombre del estudiante,
   - nombre del curso,
   - fecha de la matrícula.

```sql
mysql> CREATE VIEW vista_matriculas_completas AS SELECT estudiantes.nombre AS nombre_estudiante, cursos.nombre AS nombre_curso, matriculas.fecha FROM matriculas JOIN estudiantes ON matriculas.estudiante_id = estudiantes.id JOIN cursos ON matriculas.curso_id = cursos.id;
Query OK, 0 rows affected (0.013 sec)
```

2. Consultar datos desde la vista, mostrando el nombre del estudiante y la fecha de matrícula.

```sql
mysql> SELECT nombre_estudiante, fecha FROM vista_matriculas_completas;
+-------------------+------------+
| nombre_estudiante | fecha      |
+-------------------+------------+
| María López     | 2021-09-01 |
| Juan Pérez       | 2022-09-01 |
| Lucía Fernández | 2023-09-02 |
| Carlos Ruiz       | 2024-09-03 |
| María López     | 2020-09-04 |
| Juan Pérez       | 2022-09-05 |
| Lucía Fernández | 2023-09-06 |
| Carlos Ruiz       | 2024-09-06 |
+-------------------+------------+
8 rows in set (0.007 sec)
```

3. Eliminar la vista.

```sql
mysql> DROP VIEW vista_matriculas_completas;
Query OK, 0 rows affected (0.010 sec)
```

---

## ⚙ Parte 4: Procedimiento Almacenado

1. Crear un procedimiento llamado `cursos_por_profesor` que reciba el nombre del profesor como parámetro y devuelva los cursos que imparte y su número de créditos.

```sql
mysql> DELIMITER //
mysql> create procedure cursos_por_profesor (in nombre_profesor varchar(100)) begin select cursos.nombre, cursos.creditos from cursos join profesores on cursos.profesor_id=profesores.id where profesores.nombre = nombre_profesor;
    -> END//
Query OK, 0 rows affected (0.071 sec)

mysql> DELIMITER ;
```

2. Ejecutar el procedimiento con el nombre “Dr. Luis Gómez”.

```sql
mysql> CALL cursos_por_profesor('Dra. Ana Torres');
+-----------------+----------+
| nombre          | creditos |
+-----------------+----------+
| Álgebra Lineal |        6 |
| Cálculo I      |        6 |
+-----------------+----------+
2 rows in set (0.004 sec)

Query OK, 0 rows affected (0.004 sec)
```
Mi consola no toma acentos ni idea porque

3. Eliminar el procedimiento.

```sql
mysql> DROP PROCEDURE IF EXISTS cursos_por_profesor;
Query OK, 0 rows affected (0.023 sec)
```

---

## 🔢 Parte 5: Función Definida por el Usuario

1. Crear una función llamada `total_creditos_estudiante` que reciba el ID de un estudiante y devuelva el total de créditos que ha matriculado.

```sql
mysql> DELIMITER //
mysql> 
mysql> CREATE FUNCTION total_creditos_estudiante(estudianteID INT) RETURNS INT
    -> DETERMINISTIC
    -> BEGIN
    ->     DECLARE total INT;
    ->
    ->     SELECT SUM(c.creditos) INTO total
    ->     FROM matriculas m
    ->     JOIN cursos c ON m.curso_id = c.id
    ->     WHERE m.estudiante_id = estudianteID;
    ->
    ->     RETURN IFNULL(total, 0);
    -> END //
Query OK, 0 rows affected (0.047 sec)

mysql>
mysql> DELIMITER ;
```

2. Ejecutar la función para un estudiante específico.

```sql
mysql> SELECT total_creditos_estudiante(1) AS total_creditos;
+----------------+
| total_creditos |
+----------------+
|             12 |
+----------------+
1 row in set (0.040 sec)
```

3. Eliminar la función.

```sql
mysql> DROP FUNCTION IF EXISTS total_creditos_estudiante;
Query OK, 0 rows affected (0.017 sec)
```