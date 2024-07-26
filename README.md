# SQL-Server-clase-2

## Creación de tabla con PK

    USE Empresa
    GO
    IF EXISTS
    (
        SELECT *
        FROM sys.schemas s
        INNER JOIN sys.tables t
        ON s.schema_id = t.schema_id
        WHERE
                s.name = 'Personal'
            AND
                t.name = 'Rol'
    )
    DROP TABLE Personal.rol
    GO
    CREATE TABLE Personal.rol
    (
        cod_rol_in    INT PRIMARY KEY,
        nom_rol_vc VARCHAR(20)
    )
    GO
    INSERT INTO Personal.rol(cod_rol_in, nom_rol_vc) VALUES (8, 'Ventas');
    INSERT INTO Personal.rol(cod_rol_in, nom_rol_vc) VALUES (3, 'Almacén');
    INSERT INTO Personal.rol(cod_rol_in, nom_rol_vc) VALUES (4, 'Facturación');
    GO
    SET SHOWPLAN_ALL ON
    GO
    SELECT * FROM Personal.rol
    GO
    SET SHOWPLAN_ALL OFF
    GO

![image](https://github.com/user-attachments/assets/33579a7c-cda5-43c0-8ab4-2735ff105d52)

## Estrategias de Consulta empleado por SQL Server

**1) TABLE SCAN**

El TABLE SCAN se refiere a la forma a la que el motor de base de datos procesa una consulta SELECT. **Esto ocurre cuando no hay índices disponibles o cuando el motor de consultas determina escaneo que el escaneo completo de la tabla es la mejor estrategia de ejecución**.

Cuando se realiza "Table Scan", el motor de SQL Server lee todas las filas de la tabla, una por una. **Esta operación puede ser costosa (tiempo de procesamiento), cuando se trata de tablas grandes**

**CARACTERÍSTICAS DEL TABLE SCAN:**

**RENDIMIENTO: Los table Scan suelen ser lentos para tablas grandes**

**CONSULTAS SENCILLAS: Suele ser eficiente para consultas sencillas que necesitan procesar la mayoría de filas de la tabla, como consultas SELECT sin WHERE**

**ÍNDICES: Se recomienda crear índices en las columnas que se utilizan con mayor frecuencia en clausulas WHERE, ORDER BY y JOIN. Esto permite que el motor de consultas utilice índices en lugar de "Table Scan"**

**2) CLUSTERED INDEX SCAN**

Estrategia de ejecución de consultas donde el motor de consultas realiza un escaneo completo de un índice clúster, en vez de buscar datos específicos en la tabla.

El índice clúster es un tipo especial de índice en el que los datos de la tabla se ordenan físicamente en el mismo orden que el índice. Esto significa que los datos de la tabla se almacenan en páginas de datos ordenadas según la clave del índice clúster.

**CARACTERÍSTICAS DEL CLUSTERED INDEX SCAN:**

**RENDIMIENTO: Suele ser más eficiente que un "Table Scan" cuando se necesita procesar la mayor parte de datos de una tabla. Esto porque el motor lee los datos en base al orden del índice clúster. Reduce la cantidad de accesos aleatorios al disco**

**ÍNDICE ORDENADO: El motor de búsqueda recorre los datos de manera secuencial, por lo que hay mayor eficiencia (menor tiempo de procesamiento)**

**CONSULTA DE RANGO: Eficiente para consultas que buscan un rango de valores, como WHERE, FECHA BETWEEN. El motor de base de datos puede recorrer los datos ordenados de manera eficiente**

**COLUMNAS INCLUIDAS: Se pueden incluir otras columnas de la tabla en el índice. Esto mejora el rendimiento ya que no necesariamente se acceden a todas las columnas de la tabla en sí. Por ejemplo "SELECT id, nombre". Es decir hay rendimiento tanto para no recorrer todas las columnas ni todas las filas necesariamente**

## PRIMARY KEY - PK

Debido a que la información debe ser única, entonces cada fila de información debe tener un identificador único.

**CARACTERÍSTICAS DE LA PK**

**IDENTIFICADOR ÚNICO: Tiene un valor único**

**NO NULABILIDAD: Por defecto no deben ser nulos**

**INDEXACIÓN AUTOMÁTICA: Cuando se define la PK en una tabla, el motor de base de datos crea automáticamente un índice clúster en esa clave. Esto optimiza las operaciones de CRUD**

**INTEGRIDAD REFERENCIAL: La PK es clave para la integridad de tablas. Cuando se establece una relación entre tablas la PK se utiliza como clave foránea (FK) en otra tabla**

**RENDIMIENTO: Muy buen rendimiento**

**SELECCIÓN DE LA CLAVE PRIMARIA: Se deben emplear con las siguientes características:**

    **ÚNICA: Los valores son únicos para cada fila**

    **ESTABLE: Los valores deben ser estables y no cambiar a largo plazo**

    **MÍNIMA: La PK debe contener la mínima cantidad de columnas necesarias para poder identificar las filas de la tabla. Puede ser de 2 o más pero su busca la de menor cantidad**

**CREACIÓN DE UNA TABLA CON PRIMARY KEY**

    USE Empresa
    GO
    IF EXISTS
    (
        SELECT *
        FROM sys.schemas s
        INNER JOIN sys.tables t
        ON s.schema_id = t.schema_id
        WHERE
                s.name = 'Personal'
            AND
                t.name = 'Rol'
    )
    DROP TABLE Personal.rol
    GO
    CREATE TABLE Personal.rol
    (
        cod_rol_in    INT
        CONSTRAINT PK_personal_rol_cod_rol_in PRIMARY KEY NOT NULL,
        nom_rol_vc VARCHAR(20)
    )
    GO
    INSERT INTO Personal.rol(cod_rol_in, nom_rol_vc) VALUES (8, 'Ventas');
    INSERT INTO Personal.rol(cod_rol_in, nom_rol_vc) VALUES (3, 'Almacén');
    INSERT INTO Personal.rol(cod_rol_in, nom_rol_vc) VALUES (4, 'Facturación');
    GO
    --SET SHOWPLAN_ALL ON
    GO
    SELECT * FROM Personal.rol
    GO
    --SET SHOWPLAN_ALL OFF
    GO
**Otra forma de crear una tabla con PK**

(**Más empleada**: Se definen todas las columnas y el tipo de dato y al final se coloca cuál será la PK mediante un CONSTRAINT)

    USE Empresa
    GO
    IF EXISTS
    (
        SELECT *
        FROM sys.schemas s
        INNER JOIN sys.tables t
        ON s.schema_id = t.schema_id
        WHERE
                s.name = 'Personal'
            AND
                t.name = 'Rol'
    )
    DROP TABLE Personal.rol
    GO
    CREATE TABLE Personal.rol
    (
        cod_rol_in    INT
        CONSTRAINT pk_personal_rol_cod_rol_in   
        PRIMARY KEY NOT NULL,
        nom_rol_vc VARCHAR(20)
    )
    GO
    INSERT INTO Personal.rol(cod_rol_in, nom_rol_vc) VALUES (8, 'Ventas');
    INSERT INTO Personal.rol(cod_rol_in, nom_rol_vc) VALUES (3, 'Almacén');
    INSERT INTO Personal.rol(cod_rol_in, nom_rol_vc) VALUES (4, 'Facturación');
    GO
    SELECT * FROM Personal.rol
    GO


## IDENTITY (valor_inicial, incremento)

El atributo IDENTITY sirve para generar valores únicos para una columna de una tabla. Se emplea **COMÚNMENTE** en las columnas PK para que haya autoincremento.

Ejemplo: si se escribe IDENTITY(100,1): Empieza en 100 y el incremento es de 1

Ejemplo 2: si se escribe IDENTITY(200,4): El valor inicial es 200 y el incremento es 4


