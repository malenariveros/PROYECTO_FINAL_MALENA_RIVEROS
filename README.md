# PROYECTO_FINAL_MALENA_RIVEROS
Entrega del proyecto final.


# CRECIÓN DE OBJETOS:

# TABLAS:
  - clientes.
  - reservas.
  - comandas.
  - menu.
  - platos.
  - bebidas.
  - postres.
  - detalle_comandas.
  - facturas_clientes.
  - detalle_facturas_clientes.
  - encuestas_satisfaccion.
  - proveedores.
  - pedidos_proveedores.
  - detalle_pedidos_proveedores.
  - facturas_proveedores.
  - detalle_facturas_proveedores.
  - empleados.
  - turnos_empleados.
  - empleados_despedidos.
  - ventas.
  - detalle_ventas.
  
Total 21 tablas.

  # CREACIÓN DE VISTAS, FUNCIONES, PROCEDURES Y TIRGGERS: 

# VISTAS: 
  - vista_reservas:
    Nos devuelve el total de reservas, a través de una consulta que une los datos de las tablas "reservas" y clientes", mediante la sentencia "INNER JOIN", que nos mostrará
    además la fecha y hora de cada reserva, cantidad de comensales y nombre de quién realiza la reserva.
    
  - vista_estado_mesas:
    Nos devuelve el estado de las mesas, si están libres o si cuentan con una reserva en la fecha en la que se realice la consulta. Une los datos de las tablas "mesas" y
    "reservas" mediante la sentencia "LEFT JOIN", donde se verán todas las mesas del restaurante, y su estado por un lado, y por el otro, las reservas a la fecha en caso de
    que hubieran, caso contratio, devolverá "NULL".

  - vista_facturas_pendientes:
    Nos devuelve las facturas pendientes de pago de los clientes. Uniendo los datos de las tablas "facturas_clientes" y "clientes", mediante un "INNER JOIN" que nos dará
    como resultado el monto de la factura, su fecha y los nombres de los clientes.

  - vista_ventas_por_dia:
    Nos devuelve la cantidad de ventas realizadas por cada fecha, realizando una consulta a la tabla venta.
    
  - vista_empleados_por_turno:
    Nos devuelve la cantidad de turnos por empleados, mediante la unión de las tablas "empleados" y "turnos_empleados", con la sentencia "INNER JOIN" que nos mostrará
    los nombre de cada empleado, hora de inicio y fin de su jornada laboral.
    
# FUNCIONES: 

  - calcular_edad_promedio_cliente:
    Calcula la edad promedio de los clientes del restaurante para ayudarnos a orientar las ventas y servicio del mismo. Toma las fechas de nacimiento de cada comensal
    a través de la tabla "clientes", obtiene el promedio de edades de los mismos y la devuelve en un solo resultado.

  - calcular_propina:
    Calcula el 10% del monto total de cada factura de los clientes, consultando la tabla "facturas_clientes". A través del select podemos ver el id de la factura, el monto
    total y el porcentaje de propina por factura.

# STORED PROCEDURES: 
  - ver_detalle_comanda:
    Nos devuelve el detalle de una comanda específica, consultada por su id.
    
  - generar_factura_cliente:
    Genera una factura para un cliente en específico, utiliza la tabla "facturas_clientes" para, a través de su id, consultar el monto total de la factura.
    
# TRIGGERS: 
  - actualizar_estado_mesas:
    Se activa después de la inserción de que se realiza una inserción en la tabla "reservas". La tabla "mesas" se actualiza y muestra el cambio de estado sobre la mesa
    en la que se realizó la reserva.

  - bloquear_eliminacion_empleados:
  Se activa antes de que se elimine un registro de la tabla "empleados", impidiendo así que se pierda información importante.





# ANÁLISIS DE DATOS: 

# Análisis de ingresos mensuales.

SELECT DATE_FORMAT(fecha, '%Y-%m') AS mes, SUM(monto_total) AS ingresos
FROM facturas_clientes
GROUP BY mes
ORDER BY mes;

# objetivos: 
  Analizar los ingresos mensuales del restaurante. Se realiza una suma de los montos de todas los registros de la tabla "facturas_clientes", por mes, para obtener el ingreso
  mensual.
  

# Análisis de productos más vendidos del restaurante.

SELECT
    categoria,
    nombre,
    cantidad_vendida,
    ROUND((cantidad_vendida / (SELECT SUM(cantidad_platos) + SUM(cantidad_bebidas) + SUM(cantidad_postres) 
                               FROM detalle_comandas)) * 100, 2) AS porcentaje_vendido
FROM (
    SELECT 'Platos' AS categoria, p.nombre, SUM(dc.cantidad_platos) AS cantidad_vendida
    FROM detalle_comandas dc
    JOIN platos p ON dc.id_platos = p.id_platos
    GROUP BY p.nombre
    UNION
    SELECT 'Bebidas' AS categoria, b.nombre, SUM(dc.cantidad_bebidas) AS cantidad_vendida
    FROM detalle_comandas dc
    JOIN bebidas b ON dc.id_bebidas = b.id_bebidas
    GROUP BY b.nombre
    UNION
    SELECT 'Postres' AS categoria, po.nombre, SUM(dc.cantidad_postres) AS cantidad_vendida
    FROM detalle_comandas dc
    JOIN postres po ON dc.id_postres = po.id_postres
    GROUP BY po.nombre
) AS ventas_totales
ORDER BY porcentaje_vendido DESC;


# objetivo: 

Identifica los productos más vendidos del restaurante en las tres categorías consultadas: "platos", "bebidas" y "postres", a través de la tabla "detalle_comandas". Se accede a las cantidades vendidas de cada producto y se calcula un porcentaje de venta por cada producto en base al total de ventas de su categoría. Luego se calcula el porcentaje de venta para cada producto individual dividiendo su cantidad vendida por el total de ventas de su respectiva categoría, posteriormente multiplicando por 100 para expresarlo como un porcentaje.

Este análisis nos acerca a las preferencias de los clientes y nos ayuda a tomar decisiones más acertadas en cuanto a la compra de materia prima para la producción y elaboración de los productos más solicitados, así como a la optimización del menú y a mejorar la experiencia de los clientes teniendo en cuenta sus preferencias.
