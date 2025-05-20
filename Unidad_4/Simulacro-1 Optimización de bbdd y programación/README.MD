## 🎯 Objetivo

Consolidar el uso de los principales componentes de MySQL:

- Índices
- Vistas
- Funciones
- Procedimientos

---

## 🧱 Enunciado del ejercicio

La empresa "TecnoMarket" quiere analizar las ventas realizadas por sus clientes. Para ello, necesita organizar la información en su base de datos y optimizar el rendimiento de las consultas.

## 📘 Parte 1: Creación de tablas

```sql
CREATE TABLE clientes (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(100),
    ciudad VARCHAR(50)
);

CREATE TABLE productos (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(100),
    precio DECIMAL(10, 2)
);

CREATE TABLE ventas (
    id INT AUTO_INCREMENT PRIMARY KEY,
    cliente_id INT,
    producto_id INT,
    fecha DATE,
    cantidad INT,
    FOREIGN KEY (cliente_id) REFERENCES clientes(id),
    FOREIGN KEY (producto_id) REFERENCES productos(id)
);
INSERT INTO productos (id, nombre, precio) VALUES
(1, 'Laptop', 1200.00),
(2, 'Teclado', 50.00),
(3, 'Monitor', 300.00);

INSERT INTO ventas (id, cliente_id, producto_id, fecha, cantidad) VALUES
(1, 1, 1, '2024-05-01', 1),
(2, 1, 2, '2024-05-12', 2),
(3, 2, 3, '2024-05-13', 1),
(4, 3, 2, '2024-05-14', 1);
```

<img src=img/img1.png>

## 🔍 Parte 2: Creación de índices

Crea los siguientes clientes:

- **idx_ciudad** sobre la tabla clientes y el campo ciudad.
- **idx_fecha** sobre la tabla ventas, y el campo fecha.

```sql
CREATE INDEX idx_ciudad ON clientes(ciudad);

CREATE INDEX idx_fecha ON ventas(fecha);
```
### ¿Preguntas?

- Crea los indices, muestra su rendimiento, y explica si son óptimos y por qué?.

# INDICES CREADOS

<img src=img/img2.png>

<img src=img/img3.png> 

# RENDIMIENTO DE ESOS INDICES

<img src=img/img4.png>

<img src=img/img5.png>

# EXPLICA SI SON OPTIMOS Y POR QUE?

Estos índices son óptimos porque permiten que MySQL acceda más rápido a los datos sin tener que revisar todas las filas de la tabla, lo que mejora mucho el rendimiento de las consultas. Además, como estas tablas se usan principalmente para consultar y no tanto para modificar, el uso de índices no afecta negativamente el rendimiento general.

---

## 👁️ Parte 3: Crear una vista

- **Obtén, a través de una vista**, la siguiente información detallada de cada venta:

- ID de la venta
- Nombre del cliente
- Producto vendido
- Fecha de la venta
- Cantidad comprada
- Total gastado (precio × cantidad)

La vista **vista** que consolida los datos de las tablas `ventas`, `clientes` y `productos`, permitiendo consultar fácilmente el historial de ventas detallado.

> **RECUERDA**: Una vista es una **consulta sql** encapsulada en una tabla ficticia.

### 📊 Resultado esperado

| venta_id | cliente     | producto | fecha       | cantidad | total   |
|----------|-------------|----------|-------------|----------|---------|
| 1        | Ana Pérez   | Laptop   | 2024-05-01  | 1        | 1200.00 |
| 2        | Ana Pérez   | Teclado  | 2024-05-12  | 2        | 100.00  |
| 3        | Luis Gómez  | Monitor  | 2024-05-13  | 1        | 300.00  |
| 4        | Carla Ruiz  | Teclado  | 2024-05-14  | 1        | 50.00   |

RESPUESTA

# CREACIOND DE LA VISTA

```SQL
CREATE VIEW vista_ventas_detalladas AS
SELECT
    v.id AS venta_id,
    c.nombre AS cliente,
    p.nombre AS producto,
    v.fecha,
    v.cantidad,
    p.precio * v.cantidad AS total
FROM ventas v
JOIN clientes c ON v.cliente_id = c.id
JOIN productos p ON v.producto_id = p.id;
```

# CONSULTANDO LA VISTA

```SQL
SELECT * FROM vista_ventas_detalladas;
```
# RESULTADO FINAL

<img src=img/img6.png>

---

## 🧮 Parte 4: Crear una función

Crea una **función almacenada** en MySQL llamada `calcular_total` que reciba los siguientes parámetros:

- `precio`: un valor decimal con dos decimales (precio del producto)
- `cantidad`: un número entero que representa las unidades compradas

La función debe devolver el resultado de multiplicar ambos valores, es decir, el **total a pagar** por esa línea de venta.

```sql
SELECT calcular_total(1200.00, 2);
```

| calcular_total(1200.00, 2) |
|----------------------------|
| 2400.00                    |

# FUNCION

```SQL
CREATE FUNCTION CALCULAR_TOTAL(PRECIO DECIMAL(10,2), CANTIDAD INT) RETURNS DECIMAL(10,2) DETERMINISTIC RETURN PRECIO * CANTIDAD;
```
# EJECUCION

<img src=img/img7.png>
---

## ⚙️ Parte 5: Crear un procedimiento

Crea un procedimiento llamado `resumen_cliente` que reciba como parámetro el **ID de un cliente** (`cliente_id`), y que devuelva el **historial de ventas** de dicho cliente.  
El procedimiento debe mostrar los siguientes datos por cada venta realizada por ese cliente:

- El **nombre del cliente**
- La **fecha de la venta**
- El **nombre del producto**
- La **cantidad comprada**
- El **total de la línea de venta**, calculado como `precio * cantidad`

💡 **Sugerencia:** Puedes reutilizar una función existente (como `calcular_total`) o calcular el total directamente en la consulta.

```sql
CALL resumen_cliente(1);
```

Con el siguiente resultado: 

| nombre     | fecha       | producto   | cantidad | total   |
|------------|-------------|------------|----------|---------|
| Ana Pérez | 2024-05-01  | Laptop     | 1        | 1200.00 |
| Ana Pérez | 2024-05-12  | Teclado    | 2        | 100.00  |

> *Este resultado depende de los datos que se hayan insertado en la base de datos.*

# CREACION DEL PROCEDIMIENTO

CREATE PROCEDURE RESUMEN_CLIENTE(IN CLIENTE_ID INT) BEGIN SELECT C.NOMBRE AS NOMBRE, V.FECHA, P.NOMBRE AS PRODUCTO, V.CANTIDAD, P.PRECIO * V.CANTIDAD AS TOTAL FROM VENTAS V JOIN CLIENTES C ON V.CLIENTE_ID = C.ID JOIN PRODUCTOS P ON V.PRODUCTO_ID = P.ID WHERE C.ID = CLIENTE_ID; END;

# RESULTADO DEL PROCEDIMIENTO

```SQL
CALL resumen_cliente(1);
```
<IMG SRC='IMG/img8.png'>

---

## ❓ Preguntas teóricas

1. ¿Qué ventajas ofrece el uso de una vista en lugar de una consulta con múltiples `JOIN`?

Usar una vista facilita el trabajo porque encapsula una consulta compleja en una "tabla virtual"

2. ¿Por qué es importante declarar una función como `DETERMINISTIC`?

Es importante porque indica que la función siempre devuelve el mismo resultado si se le dan los mismos valores.

3. ¿Cuál es la diferencia entre una función y un procedimiento?

La función devuelve un único valor y se usa dentro de consultas (SELECT calcular_total(...)). El procedimiento puede devolver resultados más complejos (como tablas) y no se usa dentro de un SELECT, sino que se ejecuta con CALL. Además, la función no puede modificar datos, pero el procedimiento sí.

4. ¿Qué impacto tienen los índices sobre el rendimiento de una base de datos?

Los índices mejoran mucho la velocidad de búsqueda, ordenamiento y filtrado de datos. Sin ellos, MySQL tiene que revisar toda la tabla fila por fila.

5. ¿Cuándo se recomienda usar un trigger en lugar de un procedimiento?

Se recomienda usar un trigger cuando quiero que una acción ocurra automáticamente como reacción a un evento (como insertar, actualizar o borrar una fila).

---

## 📝 Preguntas prácticas

1. Modifica el procedimiento para filtrar también por un rango de fechas.

```SQL
CREATE PROCEDURE RESUMEN_CLIENTE(IN CLIENTE_ID INT, IN FECHA_INICIO DATE, IN FECHA_FIN DATE)
BEGIN
    SELECT 
        C.NOMBRE AS NOMBRE, 
        V.FECHA, 
        P.NOMBRE AS PRODUCTO, 
        V.CANTIDAD, 
        P.PRECIO * V.CANTIDAD AS TOTAL
    FROM VENTAS V
    JOIN CLIENTES C ON V.CLIENTE_ID = C.ID
    JOIN PRODUCTOS P ON V.PRODUCTO_ID = P.ID
    WHERE C.ID = CLIENTE_ID AND V.FECHA BETWEEN FECHA_INICIO AND FECHA_FIN;
END;
```

2. Crea un índice sobre la columna `producto_id` en la tabla `ventas`.

```SQL
CREATE INDEX IDX_PRODUCTO ON VENTAS(PRODUCTO_ID);
```

3. ¿Qué ocurre si insertas una venta con un `cliente_id` inexistente?

Dará un error, porque la columna cliente_id en ventas tiene una clave foránea (FOREIGN KEY) que impide insertar un valor que no exista en la tabla clientes.

4. Modifica la vista para incluir también el nombre de la ciudad del cliente.

```SQL
CREATE VIEW VISTA_VENTAS_DETALLADAS AS 
SELECT 
    V.ID AS VENTA_ID, 
    C.NOMBRE AS CLIENTE, 
    C.CIUDAD, 
    P.NOMBRE AS PRODUCTO, 
    V.FECHA, 
    V.CANTIDAD, 
    P.PRECIO * V.CANTIDAD AS TOTAL
FROM VENTAS V
JOIN CLIENTES C ON V.CLIENTE_ID = C.ID
JOIN PRODUCTOS P ON V.PRODUCTO_ID = P.ID;
```

5. Agrega una validación en el procedimiento para evitar resultados si el cliente no existe.

```SQL
CREATE PROCEDURE RESUMEN_CLIENTE(IN CLIENTE_ID INT) BEGIN IF EXISTS (SELECT 1 FROM CLIENTES WHERE ID = CLIENTE_ID) THEN SELECT C.NOMBRE AS NOMBRE, V.FECHA, P.NOMBRE AS PRODUCTO, V.CANTIDAD, P.PRECIO * V.CANTIDAD AS TOTAL FROM VENTAS V JOIN CLIENTES C ON V.CLIENTE_ID = C.ID JOIN PRODUCTOS P ON V.PRODUCTO_ID = P.ID WHERE C.ID = CLIENTE_ID; ELSE SELECT 'CLIENTE NO EXISTE' AS MENSAJE_*_
```
---