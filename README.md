# UNIDAD-3-BASE-DE-DATOS-EJERCICIO-11
Este diagrama entidad-relación (o modelo relacional) representa un **sistema de gestión para la venta de entradas de conciertos**, diseñado para administrar eventos, recintos, clientes, sectores, tickets y transacciones de compra.

A continuación se detalla qué representa cada tabla y cómo se relacionan entre sí:

### 1. Entidades Principales (Tablas)

* **`RECINTO`**: Almacena la información de los lugares o estadios donde se realizan los eventos.
* *Campos:* `ID_RECINTO` (Clave Primaria), `CAPACIDAD`, `DIRECCION`, `NOMBRE` y `CIUDAD`.


* **`SECTOR`**: Define las zonas o ubicaciones dentro de un recinto (por ejemplo: *Platea, Campo, Pullman*).
* *Campos:* `ID_SECTOR` (Clave Primaria), `CAPACIDAD`, `PRECIO_TICKET`, `NOMBRE` y `ID_RECINTO` (Clave Foránea que indica a qué recinto pertenece).


* **`CONCIERTO`**: Registra los eventos musicales programados.
* *Campos:* `ID_EVENTO` (Clave Primaria), `HORA`, `FECHA`, `TITULO_GIRA` y `ID_RECINTO` (Clave Foránea que indica dónde se realiza).


* **`CLIENTE`**: Contiene los datos de los usuarios registrados en la plataforma.
* *Campos:* `ID_CLIENTE` (Clave Primaria), `DNI`, `CORREO`, `NOMBRE` y `CONTRASENA`.


* **`COMPRA`**: Registra las transacciones o facturas globales realizadas por los clientes al adquirir entradas.
* *Campos:* `ID_COMPRA` (Clave Primaria), `MONTO` (total pagado), `FECHA`, `HORA` y `ID_CLIENTE` (Clave Foránea que identifica quién hizo la compra).


* **`TICKET`**: Representa cada entrada o boleto individual vendido. Vincula al cliente, el concierto, la compra y la ubicación exacta.
* *Campos:* `ID_TICKET` (Clave Primaria), `NUMERO_BUTACA`, y varias Claves Foráneas (`ID_COMPRA`, `ID_EVENTO`, `ID_CLIENTE`, `ID_SECTOR`) para mantener la trazabilidad de a qué compra, evento, usuario y sector pertenece ese boleto específico.



---

### 2. Relaciones Principales

* **Un recinto tiene varios sectores y hospeda varios conciertos:** Un recinto se divide en múltiples sectores (relación 1 a N con `SECTOR`) y en él se pueden llevar a cabo diferentes conciertos a lo largo del tiempo (relación 1 a N con `CONCIERTO`).
* **Un concierto pertenece a un recinto:** Cada evento musical se celebra en un único recinto específico.
* **Un cliente realiza compras:** Un usuario registrado (`CLIENTE`) puede realizar una o más transacciones de compra (`COMPRA`).
* **Una compra contiene tickets:** Una transacción de compra genera uno o más boletos (`TICKET`) para un concierto y sector determinado, los cuales quedan asociados al cliente comprador.
<img width="1042" height="712" alt="EJERCICIO 11-MR drawio" src="https://github.com/user-attachments/assets/cbd67c29-b944-40f7-8c53-65072abadbe5" />

Este diagrama muestra el **proceso completo de normalización de bases de datos** aplicado al sistema de venta de entradas para conciertos, transformando una tabla inicial desnormalizada (Forma Normal 0) hasta llegar al diseño relacional optimizado en Tercera Forma Normal (3FN).

A continuación se detalla cada una de las fases representadas:

### 1. Fase Normal 0 (FN0) — Tabla No Normalizada

* **Qué se hace:** Se parte de una **única tabla gigante y plana** que contiene todos los datos mezclados (datos de los clientes, las compras, los tickets, los conciertos, los recintos y los sectores) con múltiples valores repetidos e información anidada.
* **Problema:** Genera una gran cantidad de redundancia de datos (por ejemplo, los datos del recinto y del concierto se repiten en cada ticket) y es propensa a anomalías de inserción, actualización y borrado.

---

### 2. Primera Forma Normal (1FN)

* **Qué se hace:** Se elimina la redundancia y los grupos repetitivos asegurando que **todos los atributos sean atómicos** (cada celda contiene un único valor) y que cada registro tenga una clave primara identificable (`ID_CLIENTE`, `ID_COMPRA`, `ID_TICKET`).
* **Resultado:** Se mantiene una estructura tabular plana pero con los campos estrictamente atómicos.

---

### 3. Segunda Forma Normal (2FN)

* **Qué se hace:** Se parte de la 1FN y se eliminan las **dependencias parciales**. Esto significa que los atributos que dependen únicamente de una parte de la clave compuesta deben separarse en sus propias tablas.
* **Resultado:** La información se divide en dos tablas principales independientes para evitar duplicidades innecesarias:
* **`TABLA CLIENTE`**: Aislando la información personal del usuario (`ID_CLIENTE`, `DNI`, `CORREO`, `NOMBRE`, `CONTRASEÑA`).
* **`TABLA COMPRA`**: Registrando las transacciones asociadas a cada cliente.



---

### 4. Tercera Forma Normal (3FN)

* **Qué se hace:** Se eliminan las **dependencias transitivas**, es decir, aquellos atributos que no dependen de la clave principal sino de otro campo no clave.
* **Resultado:** El sistema se desglosa en tablas normalizadas y altamente relacionadas mediante claves foráneas (FK), garantizando la integridad referencial y la optimización del almacenamiento:
* **`TABLA RECINTO`**: Contiene la ubicación y capacidad de los lugares.
* **`TABLA CONCIERTO`**: Registra los eventos musicales vinculados a su respectivo recinto.
* **`TABLA SECTOR`**: Define las zonas y precios dentro de cada recinto.
* **`TABLA CLIENTE`**: Mantiene los perfiles de usuario.
* **`TABLA COMPRA`**: Almacena el historial y los montos transaccionales por cliente.
* **`TABLA TICKET`**: Funciona como la tabla relacional final que une de forma limpia y eficiente el boleto específico con su respectiva compra, evento, cliente y sector.
<img width="4262" height="3427" alt="EJERCICIO 11-FASES DE NORMALIZACION drawio" src="https://github.com/user-attachments/assets/2ccb4f19-de66-40ba-b382-8426f966df58" />
