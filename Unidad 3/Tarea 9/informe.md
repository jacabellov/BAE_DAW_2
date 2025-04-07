* Obtener el nombre del alumno y el nombre de la clase en la que está inscrito.

```sql
sqlite> select a.nombre as alumno, c.nombre as clase from inscripciones i join alumnos a on i.id_alumno = a.id join clases c on i.id_clase = c.id order by alumno;
+--------+------------------------+
| alumno |         clase          |
+--------+------------------------+
| Ana    | Derecho Penal          |
| Carlos | Economía Internacional |
| Juan   | Matemáticas 101        |
| Juan   | Historia Antigua       |
| Laura  | Arte Contemporáneo     |
| Laura  | Inglés Avanzado        |
| María  | Literatura Moderna     |
| María  | Biología Avanzada      |
| Pedro  | Química Orgánica       |
| Pedro  | Física Cuántica        |
+--------+------------------------+
sqlite>
```

* Obtener el nombre del alumno y la materia de las clases en las que está inscrito

```sql
sqlite> select a.nombre as alumno, c.materia from inscripciones i join alumnos a on i.id_alumno = a.id join clases c on i.id_clase = c.id order by alumno;
+--------+-------------+
| alumno |   materia   |
+--------+-------------+
| Ana    | Derecho     |
| Carlos | Economía    |
| Juan   | Matemáticas |
| Juan   | Historia    |
| Laura  | Arte        |
| Laura  | Idiomas     |
| María  | Literatura  |
| María  | Biología    |
| Pedro  | Química     |
| Pedro  | Física      |
+--------+-------------+
sqlite>
```

* Obtener el nombre del alumno, la edad y el nombre del profesor de las clases en las que está inscrito.

```SQL
sqlite> select a.nombre as alumno, a.edad, c.profesor from inscripciones i join alumnos a on i.id_alumno = a.id join clases c on i.id_clase = c.id order by alumno;
+--------+------+------------+
| alumno | edad |  profesor  |
+--------+------+------------+
| Ana    | 19   | Profesor Q |
| Carlos | 20   | Profesor R |
| Juan   | 20   | Profesor X |
| Juan   | 20   | Profesor Y |
| Laura  | 22   | Profesor T |
| Laura  | 22   | Profesor S |
| María  | 21   | Profesor Z |
| María  | 21   | Profesor W |
| Pedro  | 19   | Profesor V |
| Pedro  | 19   | Profesor U |
+--------+------+------------+
sqlite>
```

* Obtener el nombre del alumno y la dirección de las clases en las que está inscrito.

```SQL
sqlite> select a.nombre as alumno, a.direccion from inscripciones i join alumnos a on i.id_alumno = a.id join clases c on i.id_clase = c.id order by alumno;
+--------+-----------+
| alumno | direccion |
+--------+-----------+
| Ana    | Calle F   |
| Carlos | Calle E   |
| Juan   | Calle A   |
| Juan   | Calle A   |
| Laura  | Calle D   |
| Laura  | Calle D   |
| María  | Calle B   |
| María  | Calle B   |
| Pedro  | Calle C   |
| Pedro  | Calle C   |
+--------+-----------+
sqlite>
```

* Obtener el nombre del alumno y el nombre de la clase junto con el profesor.

```SQL
sqlite> select a.nombre as alumno, c.nombre as clase, c.profesor from inscripciones i join alumnos a on i.id_alumno = a.id join clases c on i.id_clase = c.id order by alumno;
+--------+------------------------+------------+
| alumno |         clase          |  profesor  |
+--------+------------------------+------------+
| Ana    | Derecho Penal          | Profesor Q |
| Carlos | Economía Internacional | Profesor R |
| Juan   | Matemáticas 101        | Profesor X |
| Juan   | Historia Antigua       | Profesor Y |
| Laura  | Arte Contemporáneo     | Profesor T |
| Laura  | Inglés Avanzado        | Profesor S |
| María  | Literatura Moderna     | Profesor Z |
| María  | Biología Avanzada      | Profesor W |
| Pedro  | Química Orgánica       | Profesor V |
| Pedro  | Física Cuántica        | Profesor U |
+--------+------------------------+------------+
sqlite>
```

* Obtener el nombre del alumno, la materia y el nombre del profesor de las clases en las que está inscrito.

```SQL
sqlite> select a.nombre as alumno, c.materia, c.profesor from inscripciones i join alumnos a on i.id_alumno = a.id join clases c on i.id_clase = c.id order by alumno;
+--------+-------------+------------+
| alumno |   materia   |  profesor  |
+--------+-------------+------------+
| Ana    | Derecho     | Profesor Q |
| Carlos | Economía    | Profesor R |
| Juan   | Matemáticas | Profesor X |
| Juan   | Historia    | Profesor Y |
| Laura  | Arte        | Profesor T |
| Laura  | Idiomas     | Profesor S |
| María  | Literatura  | Profesor Z |
| María  | Biología    | Profesor W |
| Pedro  | Química     | Profesor V |
| Pedro  | Física      | Profesor U |
+--------+-------------+------------+
sqlite>
```

* Obtener el nombre del alumno, la edad y la materia de las clases en las que está inscrito.

```SQL
sqlite> select a.nombre as alumno, a.edad, c.materia from inscripciones i join alumnos a on i.id_alumno = a.id join clases c on i.id_clase = c.id order by alumno;
+--------+------+-------------+
| alumno | edad |   materia   |
+--------+------+-------------+
| Ana    | 19   | Derecho     |
| Carlos | 20   | Economía    |
| Juan   | 20   | Matemáticas |
| Juan   | 20   | Historia    |
| Laura  | 22   | Arte        |
| Laura  | 22   | Idiomas     |
| María  | 21   | Literatura  |
| María  | 21   | Biología    |
| Pedro  | 19   | Química     |
| Pedro  | 19   | Física      |
+--------+------+-------------+
sqlite>
```

* Obtener el nombre del alumno, la dirección y el profesor de las clases en las que está inscrito.

```SQL
sqlite> select a.nombre as alumno, a.direccion, c.profesor from inscripciones i join alumnos a on i.id_alumno = a.id join clases c on i.id_clase = c.id order by alumno;
+--------+-----------+------------+
| alumno | direccion |  profesor  |
+--------+-----------+------------+
| Ana    | Calle F   | Profesor Q |
| Carlos | Calle E   | Profesor R |
| Juan   | Calle A   | Profesor X |
| Juan   | Calle A   | Profesor Y |
| Laura  | Calle D   | Profesor T |
| Laura  | Calle D   | Profesor S |
| María  | Calle B   | Profesor Z |
| María  | Calle B   | Profesor W |
| Pedro  | Calle C   | Profesor V |
| Pedro  | Calle C   | Profesor U |
+--------+-----------+------------+
sqlite>
```

* Obtener el nombre del alumno y la materia de las clases en las que está inscrito, ordenado por el nombre del alumno.

```SQL
sqlite> select a.nombre as alumno, c.materia from inscripciones i join alumnos a on i.id_alumno = a.id join clases c on i.id_clase = c.id order by a.nombre;
+--------+-------------+
| alumno |   materia   |
+--------+-------------+
| Ana    | Derecho     |
| Carlos | Economía    |
| Juan   | Matemáticas |
| Juan   | Historia    |
| Laura  | Arte        |
| Laura  | Idiomas     |
| María  | Literatura  |
| María  | Biología    |
| Pedro  | Química     |
| Pedro  | Física      |
+--------+-------------+
sqlite>
```

* Contar cuántos alumnos están inscritos en cada clase.

```SQL
sqlite> select c.nombre as clase, count(i.id_alumno) as total_alumnos from clases c left join inscripciones i on c.id = i.id_clase group by c.id order by total_alumnos desc;
+------------------------+---------------+
|         clase          | total_alumnos |
+------------------------+---------------+
| Matemáticas 101        | 1             |
| Historia Antigua       | 1             |
| Literatura Moderna     | 1             |
| Biología Avanzada      | 1             |
| Química Orgánica       | 1             |
| Física Cuántica        | 1             |
| Arte Contemporáneo     | 1             |
| Inglés Avanzado        | 1             |
| Economía Internacional | 1             |
| Derecho Penal          | 1             |
+------------------------+---------------+
sqlite>
```