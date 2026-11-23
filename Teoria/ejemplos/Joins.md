![logotipo de The Bridge](https://user-images.githubusercontent.com/27650532/77754601-e8365180-702b-11ea-8bed-5bc14a43f869.png  "logotipo de The Bridge")

# **BOOTCAMP FULLSTACK**

## Fullstack FT Vitoria Octubre 2026

---

# TALLER BBDD - SQL

## 📊 SQL Joins (Uniones en SQL)

Las **uniones** (Joins) en SQL se utilizan para combinar filas de dos o más tablas, basándose en un campo común entre ellas.

---

## 🧪 Ejemplo

La base de datos **Northwind** tiene las tablas `Orders` y `Customers`.  
Estas pueden unirse a través de la columna `CustomerID` (clave foránea en Orders).

Si queremos conocer el nombre de la empresa asociada a cada pedido:

```sql
SELECT Orders.OrderID, Customers.CompanyName, Orders.OrderDate
FROM Customers
	INNER JOIN Orders ON Customers.CustomerID = Orders.OrderID
ORDER BY Customers.CompanyName; 
```

---

## 🔄 INNER JOIN (Unión Interna)

- Es el tipo de unión más común.
- Devuelve todas las filas de ambas tablas donde exista coincidencia en las columnas especificadas.

### Sintaxis:

```sql
SELECT nombre_columna(s)
FROM tabla1
    INNER JOIN tabla2 ON tabla1.columna = tabla2.columna;
```
o:

```sql
SELECT nombre_columna(s)
FROM tabla1
    JOIN tabla2 ON tabla1.columna = tabla2.columna;
```

**Nota:** `INNER JOIN` y `JOIN` son equivalentes.

### Diagrama de Venn de INNER JOIN
![](img/sql_innerjoin.jpg)

---

## 🡸 LEFT JOIN (Unión Izquierda)

- Devuelve **todas las filas de la tabla izquierda** y las coincidencias de la tabla derecha.
- Si no hay coincidencia, devuelve NULL en las columnas de la tabla derecha.

### Sintaxis:

```sql
SELECT nombre_columna(s)
FROM tabla1
    LEFT JOIN tabla2 ON tabla1.columna = tabla2.columna;
```
o:

```sql
SELECT nombre_columna(s)
FROM tabla1
    LEFT OUTER JOIN tabla2 ON tabla1.columna = tabla2.columna;
```

**Nota:** En algunas bases de datos `LEFT JOIN` se llama `LEFT OUTER JOIN`.

### Ejemplo:

```sql
-- Obtener todos los clientes y sus pedidos
SELECT Orders.OrderID, Customers.CompanyName, Orders.OrderDate
FROM Customers
	LEFT OUTER JOIN Orders ON Customers.CustomerID = Orders.CustomerID
ORDER BY Customers.CompanyName;
```

### Diagrama de Venn de LEFT JOIN
![](img/sql_leftjoin.jpg)

---

## 🡺 RIGHT JOIN (Unión Derecha)

- Devuelve **todas las filas de la tabla derecha** con las coincidencias de la izquierda.
- Si no hay coincidencia, devuelve NULL en las columnas de la tabla izquierda.

### Sintaxis:

```sql
SELECT nombre_columna(s)
FROM tabla1
    RIGHT JOIN tabla2 ON tabla1.columna = tabla2.columna;
```
o:

```sql
SELECT nombre_columna(s)
FROM tabla1
    RIGHT OUTER JOIN tabla2 ON tabla1.columna = tabla2.columna;
```

### Ejemplo:

```sql
-- Obtener todos los pedidos y los clientes asociados
SELECT Orders.OrderID, Customers.CompanyName, Orders.OrderDate
FROM Orders
	RIGHT OUTER JOIN Customers ON Customers.CustomerID = Orders.CustomerID
ORDER BY Customers.CompanyName;
```

### Diagrama de Venn de RIGHT JOIN
![](img/sql_rightjoin.jpg)

---

## 🔄 FULL OUTER JOIN (Unión Externa Completa)

- Devuelve **todas las filas** de ambas tablas.
- Devuelve NULL en los lados donde no hay coincidencia.

### Sintaxis:

```sql
SELECT nombre_columna(s)
FROM tabla1
    FULL OUTER JOIN tabla2 ON tabla1.columna = tabla2.columna;
```

### Ejemplo:

```sql
-- Obtener todos los pedidos y todos los clientes, combinados
SELECT Orders.OrderID, Customers.CompanyName, Orders.OrderDate
FROM Orders
	FULL OUTER JOIN Customers ON Customers.CustomerID = Orders.OrderID
ORDER BY Customers.CompanyName;
```

### Diagrama de Venn de FULL JOIN
![](img/sql_fulljoin.jpg)

---

## 📚 GROUP BY (Agrupación)

Se usa junto a funciones de agregación para agrupar resultados por una o más columnas.

### Sintaxis:

**Una columna:**
```sql
SELECT columna, función_agregada(columna2)
FROM tabla
WHERE condición
GROUP BY columna;
```

**Varias columnas:**
```sql
SELECT columna1, columna2, función_agregada(columna3)
FROM tabla
WHERE condición
GROUP BY columna1, columna2;
```

### Ejemplo:

```sql
-- ¿Cuántos pedidos ha realizado cada cliente del Reino Unido?
SELECT Customers.CompanyName, COUNT(Orders.OrderID)
FROM Customers	
	LEFT JOIN Orders ON Customers.CustomerID = Orders.CustomerID
WHERE Customers.Country = 'UK'
GROUP BY Customers.CompanyName;
```

### Ejemplo con más columnas:

```sql
-- ¿Cuántos objetos ha pedido cada cliente del Reino Unido por año?
SELECT Customers.CompanyName, YEAR(Orders.OrderDate), SUM([Order Details].Quantity)
FROM Customers	
	INNER JOIN Orders ON Customers.CustomerID = Orders.CustomerID
	INNER JOIN [Order Details] ON Orders.OrderID = [Order Details].OrderID
WHERE Customers.Country = 'UK'
GROUP BY Customers.CompanyName, YEAR(Orders.OrderDate)
ORDER BY Customers.CompanyName, YEAR(Orders.OrderDate);
```

---

## 🏷️ ALIAS en SQL

Se usan para renombrar temporalmente columnas o tablas.

### Alias de columnas:

```sql
SELECT columna AS alias
FROM tabla;
```

### Alias de tablas:

```sql
SELECT columnas
FROM tabla AS alias;
```

### Ejemplo:

```sql
-- ¿Cuántos objetos ha pedido cada cliente del Reino Unido cada año y cuánto ha pagado?
SELECT C.CompanyName AS [Nombre Empresa], 
		YEAR(O.OrderDate) AS [Año Pedido], 
		SUM(OD.Quantity) AS [Cantidad Total], 
		SUM(OD.Quantity * OD.UnitPrice * (1 - OD.Discount)) AS [Ingresos Totales]
FROM Customers AS C
	INNER JOIN Orders AS O ON C.CustomerID = O.CustomerID
	INNER JOIN [Order Details] AS OD ON O.OrderID = OD.OrderID
WHERE C.Country = 'UK'
GROUP BY C.CompanyName, YEAR(O.OrderDate)
ORDER BY C.CompanyName, YEAR(O.OrderDate);
```

---

## ➕ INSERT INTO (Insertar datos)

Inserta registros nuevos en una tabla.

### Sintaxis:

**Insertando en todas las columnas:**

```sql
INSERT INTO tabla
VALUES (valor1, valor2, ...);
```

**Insertando columnas específicas:**

```sql
INSERT INTO tabla (col1, col2, ...)
VALUES (val1, val2, ...);
```

### Ejemplo:

```sql
INSERT INTO Suppliers (CompanyName, ContactName, Address, City, PostalCode, Country)
VALUES ('Cardinal', 'Tom B. Erichsen', 'Skagen 21', 'Stavanger', '4006', 'Norway');
```

---

## 🗑️ DELETE (Eliminar registros)

Elimina una o más filas de una tabla.

### Sintaxis:

```sql
DELETE FROM tabla
WHERE columna = valor;
```

⚠️ Si omites el `WHERE`, se eliminarán **todos los registros**.

### Ejemplo:

```sql
-- Eliminar todos los proveedores con nombre 'Cardinal'
DELETE FROM Suppliers
WHERE CompanyName = 'Cardinal';
```

---

## 🛠️ UPDATE (Actualizar registros)

Actualiza registros existentes.

### Sintaxis:

```sql
UPDATE tabla
SET columna1 = valor1, columna2 = valor2
WHERE columna = valor;
```

⚠️ Si omites `WHERE`, se actualizarán **todas las filas**.

### Ejemplo:

```sql
-- Actualizar el teléfono de todas las empresas llamadas 'Cardinal'
UPDATE Suppliers
SET Phone = '(0)2-953010'
WHERE CompanyName = 'Cardinal';
```

---

## 🔁 TRANSACCIONES

Permiten agrupar múltiples operaciones SQL como una única unidad lógica.

Puedes **revertir** los cambios con `ROLLBACK` o **confirmarlos** con `COMMIT`.

### Ejemplo:

```sql
-- Cambiar todos los países de clientes a 'Greece' y luego revertir
BEGIN TRANSACTION;

UPDATE Customers
SET Country = 'Greece';

SELECT Country, * FROM Customers;

ROLLBACK;

SELECT Country, * FROM Customers;
```

---

## 🧠 Consejos y Trucos

- Usa **alias** para acortar nombres de tabla (ej. `Customers AS C`)
- Usa **DISTINCT** para obtener valores únicos
- Usa **BEGIN TRANSACTION...ROLLBACK** para probar `INSERT`, `UPDATE`, `DELETE`
- Usa **@@ROWCOUNT** para saber cuántas filas afectó una consulta
- Siempre incluye cláusula **WHERE** en `DELETE` y `UPDATE`
- Evita `CURSORS` y bucles `WHILE` si es posible
- Prefiere **variables de tabla** frente a tablas temporales (`#temp`)

