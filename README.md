# Consultas multitabla

1. Obtén un listado con el nombre de cada cliente y el nombre y apellido de su representante de ventas.

    ```sql
    SELECT c.nombre_cliente, CONCAT(e.nombre,' ', e.apellido1) AS nombre_rep_ventas FROM cliente c, empleado e WHERE c.codigo_empleado_rep_ventas = e.codigo_empleado;
    -- segunda opción
    SELECT c.nombre_cliente, CONCAT(e.nombre,' ', e.apellido1) AS nombre_rep_ventas FROM cliente c INNER JOIN empleado e ON c.codigo_empleado_rep_ventas = e.codigo_empleado;
    ```

2. Muestra el nombre de los clientes que hayan realizado pagos junto con el nombre de sus representantes de ventas.

    ```sql
    SELECT DISTINCT c.nombre_cliente, e.nombre FROM cliente c INNER JOIN pago p ON p.codigo_cliente = c.codigo_cliente INNER JOIN empleado e ON c.codigo_empleado_rep_ventas = e.codigo_empleado;
    ```

3. Muestra el nombre de los clientes que no hayan realizado pagos junto con el nombre de sus representantes de ventas.

    ```sql
    SELECT c.nombre_cliente, e.nombre FROM cliente c LEFT JOIN pago p ON p.codigo_cliente = c.codigo_cliente INNER JOIN empleado e ON c.codigo_empleado_rep_ventas = e.codigo_empleado WHERE p.codigo_cliente IS NULL;
    ```

4. Devuelve el nombre de los clientes que han hecho pagos y el nombre de sus representantes junto con la ciudad de la oficina a la que pertenece el representante.

    ```sql
    SELECT DISTINCT c.nombre_cliente, e.nombre, o.ciudad FROM cliente c INNER JOIN pago p ON p.codigo_cliente = c.codigo_cliente INNER JOIN empleado e ON c.codigo_empleado_rep_ventas = e.codigo_empleado INNER JOIN oficina o ON e.codigo_oficina = o.codigo_oficina;
    ```

5. Devuelve el nombre de los clientes que no hayan hecho pagos y el nombre de sus representantes junto con la ciudad de la oficina a la que pertenece el representante.

    ```sql
    SELECT c.nombre_cliente, e.nombre, o.ciudad FROM cliente c LEFT JOIN pago p ON p.codigo_cliente = c.codigo_cliente INNER JOIN empleado e ON c.codigo_empleado_rep_ventas = e.codigo_empleado INNER JOIN oficina o ON e.codigo_oficina = o.codigo_oficina WHERE p.codigo_cliente IS NULL;
    ```

6. Lista la dirección de las oficinas que tengan clientes en Fuenlabrada.

    ```sql
    SELECT CONCAT(o.linea_direccion1,' ',o.linea_direccion2) 'dirección oficinas en Fuenlabrada' FROM cliente c INNER JOIN empleado e ON c.codigo_empleado_rep_ventas = e.codigo_empleado INNER JOIN oficina o ON e.codigo_oficina = o.codigo_oficina WHERE c.ciudad = 'Fuenlabrada';
    ```

7. Devuelve el nombre de los clientes y el nombre de sus representantes junto con la ciudad de la oficina a la que pertenece el representante.

    ```sql
    SELECT c.nombre_cliente, e.nombre, o.ciudad FROM cliente c INNER JOIN empleado e ON c.codigo_empleado_rep_ventas = e.codigo_empleado INNER JOIN oficina o ON e.codigo_oficina = o.codigo_oficina;
    ```

8. Devuelve un listado con el nombre de los empleados junto con el nombre de sus jefes.

    ```sql
    SELECT emp.nombre 'nombre empleado', jef.nombre 'nombre del jefe' FROM empleado emp INNER JOIN empleado jef ON emp.codigo_jefe = jef.codigo_empleado;
    -- segunda opción (salen los que no tienen jefe especificado)
    SELECT emp.nombre 'nombre empleado', jef.nombre 'nombre del jefe' FROM empleado emp LEFT JOIN empleado jef ON emp.codigo_jefe = jef.codigo_empleado; 
    ```

9. Devuelve un listado que muestre el nombre de cada empleados, el nombre de su jefe y el nombre del jefe de sus jefe.

    ```sql
    SELECT emp.nombre 'nombre empleado', emp.codigo_jefe, jef.nombre 'nombre del jefe', jef.codigo_jefe, jefdjef.nombre 'nombre del jefe del jefe' FROM empleado emp LEFT JOIN empleado jef ON emp.codigo_jefe = jef.codigo_empleado LEFT JOIN empleado jefdjef ON jef.codigo_jefe = jefdjef.codigo_empleado;
    ```

10. Devuelve el nombre de los clientes a los que no se les ha entregado a tiempo un pedido.

    ```sql
    SELECT DISTINCT c.nombre_cliente FROM cliente c LEFT JOIN pedido p ON c.codigo_cliente = p.codigo_cliente WHERE p.fecha_entrega > p.fecha_esperada;
    ```

11. Devuelve un listado de las diferentes gamas de producto que ha comprado cada cliente.

    ```sql
    SELECT  DISTINCT c.nombre_cliente, g.gama FROM cliente c INNER JOIN pedido p ON c.codigo_cliente = p.codigo_cliente INNER JOIN detalle_pedido d ON p.codigo_pedido = d.codigo_pedido INNER JOIN producto pro ON d.codigo_producto = pro.codigo_producto INNER JOIN gama_producto g ON pro.gama = g.gama;
    ```