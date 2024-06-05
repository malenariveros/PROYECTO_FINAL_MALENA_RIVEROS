# PROYECTO_FINAL_MALENA_RIVEROS
Entrega del proyecto final.


# CRECIÓN DE OBJETOS:

# TABLAS:
  - clientes(
	id_clientes: identificador de cliente. int AI PK 
	nombre: varchar(50) 
	apellido: varchar(50) 
	telefono: varchar(15) 
	email: varchar(45) 
	fecha_nacimiento: date)
  - reservas(
    	id_reserva: identificador de reserva. int AI PK 
	hora: time 
	fecha: date 
	cantidad_pax: int 
	id_mesas: int 
	id_clientes: int)
  - comandas(
    	id_comandas: identificador de comandas. int PK 
	hora: time 
	fecha: date 
	id_clientes: int FK
	id_mesas: int FK)
  - menu(
	id_menu: identificador de menu. int AI PK 
	nombre: varchar(50) 
	descripcion: varchar(100) 
	precio: decimal(10,2) 
	tipo: varchar(50) 
	id_comandas: int FK)
  - platos(
	id_platos: identificador de plato. int AI PK 
	nombre: varchar(50) 
	descripcion: varchar(100) 
	categoria: varchar(50) 
	tiempo_preparacion: time 
	disponible: tinyint -indica disponibilidad en la carta.
	precio: decimal(10,2)
	id_menu: int  FK)
  - bebidas(
	id_bebidas: identificador de bebidas. int AI PK 
	nombre: varchar(50) 
	descripcion: varchar(100) 
	categoria: varchar(50) 
	disponible: tinyint - indica disponibilidad en la carta.
	precio: decimal(10,2)
	id_menu: int  FK)
  - postres(
    	id_postres: identificador de postre. int AI PK 
	nombre: varchar(100) 
	descripcion: varchar(100) 
	disponible: tinyint -indica disponibilidad en la carta.
	categoria: varchar(50) 
	tiempo_preparacion: time 
	precio: decimal(10,2)
	id_menu: int FK)
  - detalle_comandas(
	id_detalle_comandas: identificador detalle de comandas. int AI PK 
	id_comandas: int FK
	id_platos: int FK
	id_bebidas: int FK
	id_menu: int FK
	observaciones text 
	id_postres int FK
	cantidad_platos int 
	cantidad_bebidas int 
	cantidad_postres int)
  - facturas_clientes(
	id_facturas: identificador de facturas.: int AI PK 
	monto_total: int 
	fecha: date 
	pagada: tinyint -indica pago realizado/pendiente.
	id_clientes: int FK
	id_comandas: int FK)
  - detalle_facturas_clientes(
	id_clientes: int FK 
	id_facturas int FK
	id_detalle_comandas: int FK)
  - encuestas_satisfaccion(
	id_encuestas: identificador encuestas. int AI PK 
	comentarios: text 
	fecha: date 
	hora: time 
	id_clientes: int FK)
  - proveedores(
	id_proveedores: identificador proveedor. int AI PK 
	nombre: varchar(50) 
	telefono: varchar(15) 
	direccion: varchar(50))
  - pedidos_proveedores(
	id_pedidos_proveedores: identificador pedido. int AI PK 
	detalle: text 
	fecha: date 
	id_proveedores: int FK)
  - detalle_pedidos_proveedores(
	id_detalle: identificador de detalle pedido. int AI PK 
	productos text 
	cantidad: int
	id_proveedores: int FK
	id_pedidos_proveedores: int FK)
  - facturas_proveedores(
	id_facturas_proveedores: identificador facturas. int AI PK 
	detalle: text 
	fecha: date 
	monto: int 
	id_proveedores: int FK
	id_pedidos_proveedores: int FK)
  - detalle_facturas_proveedores(
	id_detalle: identificador detalle de factura. int AI PK 
	productos: text 
	cantidad: int
	precio: int
	id_proveedores: int FK
	id_pedidos_proveedores: int FK)
  - empleados(
	id_empleados: identificador empleados. int AI PK 
	nombre: varchar(50) 
	apellido: varchar(50) 
	fecha_nacimiento: date 
	tipo_documento: varchar(15) 
	numero_documento: int 
	direccion: varchar(100) 
	email: varchar(50) 
	telefono: varchar(15) 
	fecha_contratacion: date 
	puesto: varchar(50) 
	alergias: text)
  - turnos_empleados(
	id_turnos: identificador turnos. int AI PK 
	tipo: varchar(50) 
	fecha: date 
	hora_inicio: time 
	hora_fin: time 
	puesto: varchar(50) 
	observaciones: varchar(50) -horas extras, suspenciones, ect.
	id_empleados: int fk)
  - empleados_despedidos(
	id_empleados_despedidos: identificador despidos. int AI PK 
	nombre: varchar(50) 
	apellido: varchar(50) 
	fecha_contratacion: date 
	fecha_despido: date 
	motivo_despido: text 
	puesto: varchar(45) 
	id_empleados: int FK)
  - ventas(
	id_ventas: identificador de venta. int AI PK 
	fecha: date)
  - detalle_ventas(
	id_detalle: identificador detalle. int AI PK 
	id_menu: int 
	cantidad_platos_vendidos: int 
	cantidad_bebidas_vendidas: int 
	cantidad_postres_vendidos: int 
	cantidad_menu_pasos_vendidos: int
	id_ventas: int FK
	id_platos: int FK
	id_bebidas: int FK
	id_postres: int FK)
  
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
  

# Análisis de eficiencia de mesas.

CREATE VIEW uso_de_mesas AS
SELECT m.id_mesas, COUNT(c.id_comandas) AS veces_utilizada
FROM mesas m
LEFT JOIN comandas c ON m.id_mesas = c.id_mesas
GROUP BY m.id_mesas
ORDER BY veces_utilizada DESC;

SELECT * FROM uso_mesas;

# objetivos: 
	Analizar el uso de las mesas, más y menos utilizadas del restaurante para mejorar la experiencia del cliente en base a sus preferencias.

