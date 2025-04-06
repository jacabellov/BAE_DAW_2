* -- Listar los coches vendidos con sus modelos y precios, junto con los nombres de los clientes que los compraron.
  -- Cosas que debo de tener en cuenta:
    -- ¿Qué me están pidiendo?. ¿Qué es lo que no me han pedido?

```SQL
    sqlite> select c.modelo, c.precio, cl.nombre from ventas v join coches c on v.id_coche = c.id_coche join clientes cl on v.id_cliente = cl.id_cliente;
+----------------+---------+-----------------+
|     modelo     | precio  |     nombre      |
+----------------+---------+-----------------+
| Sedán 2022     | 25000.0 | Juan Pérez      |
| Hatchback 2021 | 22000.0 | María Gómez     |
| SUV 2023       | 30000.0 | Carlos López    |
| Coupé 2022     | 28000.0 | Ana Martínez    |
| Camioneta 2023 | 32000.0 | Pedro Rodríguez |
| Compacto 2021  | 20000.0 | Laura Sánchez   |
| Híbrido 2022   | 27000.0 | Miguel González |
| Deportivo 2023 | 35000.0 | Isabel Díaz     |
| Eléctrico 2021 | 40000.0 | Elena Torres    |
+----------------+---------+-----------------+
sqlite>
```
¿Qué me están pidiendo?
Mostrar solo los coches vendidos.

Mostrar modelo y precio del coche.

Mostrar nombre del cliente.

* -- Encontrar los clientes que han comprado coches con precios superiores al promedio de todos los coches vendidos.
  -- Cosas que debo de tener en cuenta:
    -- Precios superiores.
    -- Obtener la media. AVG(precio)

```SQL
    sqlite> select cl.nombre, c.modelo, c.precio from ventas v join coches c on v.id_coche = c.id_coche join clientes cl on v.id_cliente = cl.id_cliente where c.precio > (select avg(c2.precio) from ventas v2 join coches c2 on v2.id_coche = c2.id_coche);
+-----------------+----------------+---------+
|     nombre      |     modelo     | precio  |
+-----------------+----------------+---------+
| Carlos López    | SUV 2023       | 30000.0 |
| Pedro Rodríguez | Camioneta 2023 | 32000.0 |
| Isabel Díaz     | Deportivo 2023 | 35000.0 |
| Elena Torres    | Eléctrico 2021 | 40000.0 |
+-----------------+----------------+---------+
sqlite>
```
¿Qué te están pidiendo?
Clientes que han comprado coches.

Coches con precio superior al promedio.

Relacionar clientes y coches.

* -- Mostrar los modelos de coches y sus precios que no han sido vendidos aún:

  -- Cosas que debo de tener en cuenta:
    -- Coches que han sido vendidos.
    -- Quiero los coches que no han sido vendidos. NOT id_coche IN ventas

```SQL
    sqlite> select modelo, precio from coches where id_coche not in (select id_coche from ventas);
+-------------+---------+
|   modelo    | precio  |
+-------------+---------+
| Pickup 2022 | 31000.0 |
+-------------+---------+
sqlite>
```
¿Qué te están pidiendo?
Mostrar modelos y precios de coches no vendidos.

Relación solo entre la tabla coches y lo que esté o no esté en ventas.


* -- Calcular el total gastado por todos los clientes en coches:
  -- Cosas que debo de tener en cuenta:
    -- Me estan pidiendo la suma total de todos los coches vendidos, NO de aquellos que aún no se han vendido.

```SQL
    sqlite> select sum(c.precio) as total_gastado from ventas v join coches c on v.id_coche = c.id_coche;
+---------------+
| total_gastado |
+---------------+
| 259000.0      |
+---------------+
sqlite>
```
 ¿Qué debes tener en cuenta?
Solo coches vendidos.

Suma total de precios de esos coches.

* -- Listar los coches vendidos junto con la fecha de venta y el nombre del cliente, ordenados por fecha de venta de forma descendente:
  -- Cosas que debo de tener en cuenta:
    -- ¿Qué me están pidiendo?. ¿Por qué campo tengo que ordenadar. Es uno o más campos?

```SQL
    sqlite> select c.modelo, v.fecha_venta, cl.nombre from ventas v join coches c on v.id_coche = c.id_coche join clientes cl on v.id_cliente = cl.id_cliente order by v.fecha_venta desc;
+----------------+-------------+-----------------+
|     modelo     | fecha_venta |     nombre      |
+----------------+-------------+-----------------+
| Eléctrico 2021 | 2023-10-05  | Elena Torres    |
| Deportivo 2023 | 2023-08-25  | Isabel Díaz     |
| Híbrido 2022   | 2023-07-20  | Miguel González |
| Compacto 2021  | 2023-06-15  | Laura Sánchez   |
| Camioneta 2023 | 2023-05-05  | Pedro Rodríguez |
| Coupé 2022     | 2023-04-10  | Ana Martínez    |
| SUV 2023       | 2023-03-25  | Carlos López    |
| Hatchback 2021 | 2023-02-20  | María Gómez     |
| Sedán 2022     | 2023-01-15  | Juan Pérez      |
+----------------+-------------+-----------------+
sqlite>
```
¿Qué me están pidiendo?
Coches vendidos.

Mostrar:

El modelo del coche,

La fecha de venta,

El nombre del cliente.

Ordenar por la fecha de venta de forma descendente.

* -- Encontrar el modelo de coche más caro.
  -- Cosas que debo de tener en cuenta:
    -- ¿Qué me están pidiendo?. MAX

```SQL
    sqlite> select modelo from coches where precio = (select max(precio) from coches);
+----------------+
|     modelo     |
+----------------+
| Eléctrico 2021 |
+----------------+
sqlite>
```
 ¿Qué me están pidiendo?
El modelo del coche más caro.

* -- Mostrar los clientes que han comprado al menos un coche (un coche o más) y la cantidad de coches comprados.
  -- Cosas que debo de tener en cuenta:
    -- ¿Qué me están pidiendo?. COUNT

```SQL
sqlite> select cl.nombre, count(v.id_coche) as cantidad_coches_comprados from ventas v join clientes cl on v.id_cliente = cl.id_cliente group by v.id_cliente having count(v.id_coche) > 0;
+-----------------+---------------------------+
|     nombre      | cantidad_coches_comprados |
+-----------------+---------------------------+
| Juan Pérez      | 1                         |
| María Gómez     | 1                         |
| Carlos López    | 1                         |
| Ana Martínez    | 1                         |
| Pedro Rodríguez | 1                         |
| Laura Sánchez   | 1                         |
| Miguel González | 1                         |
| Isabel Díaz     | 1                         |
| Elena Torres    | 1                         |
+-----------------+---------------------------+
sqlite>
```
 ¿Qué me están pidiendo?
Clientes que han comprado al menos un coche.

Cantidad de coches comprados.

Usar COUNT() para contar los coches comprados.

* -- Encontrar los clientes que han comprado coches de la marca 'Toyota':
  -- Cosas que debo de tener en cuenta:
    -- ¿Qué me están pidiendo?. Like | regexp | =. Tabla normalizada ?.

```SQL
sqlite> select cl.nombre, c.modelo from ventas v join coches c on v.id_coche = c.id_coche join clientes cl on v.id_cliente = cl.id_cliente where c.marca like 'Toyota%';
+------------+------------+
|   nombre   |   modelo   |
+------------+------------+
| Juan Pérez | Sedán 2022 |
+------------+------------+
sqlite>
```
¿Qué me están pidiendo?
Clientes que han comprado coches de la marca 'Toyota'.

Filtrar por la marca del coche en la tabla coches y luego identificar a los clientes en la tabla ventas.

* -- Calcular el promedio de edad de los clientes que han comprado coches de más de 25,000.
  -- Cosas que debo de tener en cuenta:
    -- ¿Qué me están pidiendo?. 

```SQL
sqlite> select avg(cl.edad) as promedio_edad from ventas v join coches c on v.id_coche = c.id_coche join clientes cl on v.id_cliente = cl.id_cliente where c.precio > 25000;
+------------------+
|  promedio_edad   |
+------------------+
| 32.8333333333333 |
+------------------+
sqlite>
```
¿Qué me están pidiendo?
Calcular el promedio de edad de los clientes que han comprado coches de más de 25,000.

Edad de los clientes 

Coches de más de 25,000 

* -- Mostrar los modelos de coches y sus precios que fueron comprados por clientes mayores de 30 años.
  -- Cosas que debo de tener en cuenta:
    -- ¿Qué me están pidiendo?.

```sql
sqlite> select c.modelo, c.precio from ventas v join coches c on v.id_coche = c.id_coche join clientes cl on v.id_cliente = cl.id_cliente where cl.edad > 30;
+----------------+---------+
|     modelo     | precio  |
+----------------+---------+
| SUV 2023       | 30000.0 |
| Camioneta 2023 | 32000.0 |
| Compacto 2021  | 20000.0 |
| Deportivo 2023 | 35000.0 |
+----------------+---------+
sqlite>
```
Qué me están pidiendo?
Modelos de coches y precios de los coches que fueron comprados por clientes mayores de 30 años.

Filtrar a los clientes mayores de 30 años de la tabla clientes.

Mostrar los modelos y precios de los coches que estos clientes han comprado.

* -- Encontrar los coches vendidos en el año 2022 junto con la cantidad total de ventas en ese año.
  -- Cosas que debo de tener en cuenta:
    -- ¿Qué me están pidiendo?.

```sql
sqlite> select c.modelo, count(v.id_venta) as cantidad_ventas_2022 from ventas v join coches c on v.id_coche = c.id_coche where strftime('%Y', v.fecha_venta) = '2022' group by c.id_coche;
sqlite>
```

¿Qué me están pidiendo?
Coches vendidos en 2022.

Además de los coches, pide mostrar la cantidad total de ventas de coches en ese año. Es decir, la cantidad de veces que cada coche fue vendido en 2022.

* -- Listar los coches cuyos precios son mayores que el precio promedio de coches vendidos a clientes menores de 30 años.
  -- Cosas que debo de tener en cuenta:
    -- ¿Qué me están pidiendo?. AVG

```sql
sqlite> select c.modelo, c.precio from coches c where c.precio > (select avg(c2.precio) from ventas v2 join coches c2 on v2.id_coche = c2.id_coche join clientes cl2 on v2.id_cliente = cl2.id_cliente where cl2.edad < 30);
+----------------+---------+
|     modelo     | precio  |
+----------------+---------+
| SUV 2023       | 30000.0 |
| Camioneta 2023 | 32000.0 |
| Deportivo 2023 | 35000.0 |
| Pickup 2022    | 31000.0 |
| Eléctrico 2021 | 40000.0 |
+----------------+---------+
sqlite>
```

¿Qué me están pidiendo?
Coches cuyo precio sea mayor que el precio promedio de los coches vendidos a clientes menores de 30 años.

Calcular el precio promedio de los coches vendidos a clientes menores de 30 años.

Listar los coches cuyo precio sea mayor que ese promedio.

* -- Calcular el total de ventas por marca de coche, ordenado de forma descendente por el total de ventas:
  -- Cosas que debo de tener en cuenta:
    -- ¿Qué me están pidiendo?. COUNT| DESC|ASC precio

```sql
sqlite> select c.marca, count(v.id_venta) as total_ventas from ventas v join coches c on v.id_coche = c.id_coche group by c.marca order by total_ventas desc;
+------------+--------------+
|   marca    | total_ventas |
+------------+--------------+
| Volkswagen | 1            |
| Toyota     | 1            |
| Tesla      | 1            |
| Nissan     | 1            |
| Mazda      | 1            |
| Hyundai    | 1            |
| Honda      | 1            |
| Ford       | 1            |
| Chevrolet  | 1            |
+------------+--------------+
sqlite>
```
¿Qué me están pidiendo?
Calcular el total de ventas por marca de coche.

Ordenar de forma descendente por el total de ventas.