# Portfolio-SQL04

**•	NOTA:** Esta base de datos, hace parte de los Ejercicios: Realización de consultas SQL Apuntes de BD para DAW, DAM y ASIR José Juan Sánchez Hernández Curso 2023/2024. Disponibles en el [link](https://josejuansanchez.org/bd/ejercicios-consultas-sql/index.html#jardiner%C3%ADa). Se utilizo con el objeto de poder practicar consultas del lenguaje SQL.

**•	TITULO: JARDINERIA.**  

**•	DESCRIPCION DEL PROYECTO:** Esta base de datos es un modelo relacional para una empresa que se dedica a la comercialización y distribución de productos de jardineria. Su diseño permite gestionar y rastrear transacciones comerciales, desde la venta de productos hasta el registro de pagos y la organización del personal de ventas. El sistema está compuesto por las siguientes entidades principales: Productos: Información sobre los artículos vendidos, como nombre, descripción, precio de compra y venta, y cantidad en stock. Clientes: Datos de los clientes, incluyendo detalles de contacto, dirección y límite de crédito. Pedidos: Registro de las órdenes realizadas por los clientes, con detalles sobre productos incluidos y cantidades. Pagos: Gestión de los pagos realizados por los clientes, con información sobre la forma de pago y fechas de las transacciones. Empleados: Información sobre el personal de ventas y su relación con oficinas específicas. Oficinas: Datos sobre las sucursales de la empresa, como ubicación y contacto. Gamas de Productos: Clasificación de los productos en categorías para facilitar su gestión.

**•	OBJETIVOS:**
Optimizar la gestión de ventas y pedidos: Facilitar el registro, seguimiento y control de los pedidos realizados por los clientes, integrando información sobre productos, detalles del pedido y estados de entrega para garantizar un proceso eficiente y organizado.

Mejorar la administración de clientes y finanzas: Centralizar la información de los clientes, incluyendo sus datos de contacto, historial de compras y pagos, para ofrecer un servicio más personalizado, mantener un control financiero adecuado y asegurar un flujo constante de ingresos.

Fortalecer la gestión de inventarios y recursos empresariales: Implementar un sistema que permita supervisar el inventario de productos, gestionar el desempeño de los empleados y organizar las actividades de las oficinas, asegurando una operación empresarial más eficiente y competitiva.

**•	HERRAMIENTAS O LENGUAJES:** Básicamente para este proyecto utilizamos la herramienta Mysql, en donde hicimos creacion de schema y creacion de tablas, insercion de datos, consultas, subconsultas y joins.

**•	CONJUNTO DE DATOS:** Esta base de datos, se compone de ocho tablas que son: gama_producto, producto, detalle_pedido, pedido, cliente, pago, empleado y oficina. Gracias a esta informacion se espera, una mejora en la planificación y control de inventarios, Incremento en la eficiencia del manejo de pedidos y pagos. Mayor satisfacción del cliente al garantizar disponibilidad de productos con un seguimiento personalizado, por ultimo una optimización del rendimiento de empleados y oficinas.

**•	DESARROLLO-EJECUCION:**

![Der](https://github.com/pocolus/Portfolio-SQL04/blob/main/Der.png)

**1. CREAMOS LA BASE DE DATOS**
```sql
DROP DATABASE IF EXISTS Portfolio_Jardineria;
Create schema Portfolio_Jardineria;
Use Portfolio_Jardineria;
```

**2. CREACION DE TABLAS**
```sql
-- 2.1 CREAMOS LA TABLA OFICINA
DROP TABLE IF exists oficina;
CREATE TABLE oficina (
  codigo_oficina VARCHAR(10) NOT NULL,
  ciudad VARCHAR(30) NOT NULL,
  pais VARCHAR(50) NOT NULL,
  region VARCHAR(50) DEFAULT NULL,
  codigo_postal VARCHAR(10) NOT NULL,
  telefono VARCHAR(20) NOT NULL,
  linea_direccion1 VARCHAR(50) NOT NULL,
  linea_direccion2 VARCHAR(50) DEFAULT NULL,
  PRIMARY KEY (codigo_oficina)
);

-- 2.2 CREAMOS LA TABLA EMPLEADO
DROP TABLE IF exists empleado;
CREATE TABLE empleado (
  codigo_empleado INTEGER NOT NULL,
  nombre VARCHAR(50) NOT NULL,
  apellido1 VARCHAR(50) NOT NULL,
  apellido2 VARCHAR(50) DEFAULT NULL,
  extension VARCHAR(10) NOT NULL,
  email VARCHAR(100) NOT NULL,
  codigo_oficina VARCHAR(10) NOT NULL,
  codigo_jefe INTEGER DEFAULT NULL,
  puesto VARCHAR(50) DEFAULT NULL,
  PRIMARY KEY (codigo_empleado),
  FOREIGN KEY (codigo_oficina) REFERENCES oficina (codigo_oficina),
  FOREIGN KEY (codigo_jefe) REFERENCES empleado (codigo_empleado)
);

-- 2.3 CREAMOS LA TABLA GAMA_PRODUCTO
DROP TABLE IF exists gama_producto;
CREATE TABLE gama_producto (
  gama VARCHAR(50) NOT NULL,
  descripcion_texto TEXT,
  descripcion_html TEXT,
  imagen VARCHAR(256),
  PRIMARY KEY (gama)
);

-- 2.4 CREAMOS LA TABLA CLIENTE
DROP TABLE IF exists cliente;
CREATE TABLE cliente (
  codigo_cliente INTEGER NOT NULL,
  nombre_cliente VARCHAR(50) NOT NULL,
  nombre_contacto VARCHAR(30) DEFAULT NULL,
  apellido_contacto VARCHAR(30) DEFAULT NULL,
  telefono VARCHAR(15) NOT NULL,
  fax VARCHAR(15) NOT NULL,
  linea_direccion1 VARCHAR(50) NOT NULL,
  linea_direccion2 VARCHAR(50) DEFAULT NULL,
  ciudad VARCHAR(50) NOT NULL,
  region VARCHAR(50) DEFAULT NULL,
  pais VARCHAR(50) DEFAULT NULL,
  codigo_postal VARCHAR(10) DEFAULT NULL,
  codigo_empleado_rep_ventas INTEGER DEFAULT NULL,
  limite_credito NUMERIC(15,2) DEFAULT NULL,
  PRIMARY KEY (codigo_cliente),
  FOREIGN KEY (codigo_empleado_rep_ventas) REFERENCES empleado (codigo_empleado)
);

-- 2.5 CREAMOS LA TABLA PEDIDO
DROP TABLE IF exists pedido;
CREATE TABLE pedido (
  codigo_pedido INTEGER NOT NULL,
  fecha_pedido date NOT NULL,
  fecha_esperada date NOT NULL,
  fecha_entrega date DEFAULT NULL,
  estado VARCHAR(15) NOT NULL,
  comentarios TEXT,
  codigo_cliente INTEGER NOT NULL,
  PRIMARY KEY (codigo_pedido),
  FOREIGN KEY (codigo_cliente) REFERENCES cliente (codigo_cliente)
);

-- 2.6 CREAMOS LA TABLA PRODUCTO
DROP TABLE IF exists producto;
CREATE TABLE producto (
  codigo_producto VARCHAR(15) NOT NULL,
  nombre VARCHAR(70) NOT NULL,
  gama VARCHAR(50) NOT NULL,
  dimensiones VARCHAR(25) NULL,
  proveedor VARCHAR(50) DEFAULT NULL,
  descripcion text NULL,
  cantidad_en_stock SMALLINT NOT NULL,
  precio_venta NUMERIC(15,2) NOT NULL,
  precio_proveedor NUMERIC(15,2) DEFAULT NULL,
  PRIMARY KEY (codigo_producto),
  FOREIGN KEY (gama) REFERENCES gama_producto (gama)
);

-- 2.7 CREAMOS LA TABLA DETALLE_PEDIDO
DROP TABLE IF exists detalle_pedido;
CREATE TABLE detalle_pedido (
  codigo_pedido INTEGER NOT NULL,
  codigo_producto VARCHAR(15) NOT NULL,
  cantidad INTEGER NOT NULL,
  precio_unidad NUMERIC(15,2) NOT NULL,
  numero_linea SMALLINT NOT NULL,
  PRIMARY KEY (codigo_pedido, codigo_producto),
  FOREIGN KEY (codigo_pedido) REFERENCES pedido (codigo_pedido),
  FOREIGN KEY (codigo_producto) REFERENCES producto (codigo_producto)
);

-- 2.8 CREAMOS LA PAGO
DROP TABLE IF exists pago ;
CREATE TABLE pago (
  codigo_cliente INTEGER NOT NULL,
  forma_pago VARCHAR(40) NOT NULL,
  id_transaccion VARCHAR(50) NOT NULL,
  fecha_pago date NOT NULL,
  total NUMERIC(15,2) NOT NULL,
  PRIMARY KEY (codigo_cliente, id_transaccion),
  FOREIGN KEY (codigo_cliente) REFERENCES cliente (codigo_cliente)
);
```

**3. INSERCION DE ARCHIVOS**














