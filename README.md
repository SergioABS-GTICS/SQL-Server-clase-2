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

**USO DEL IDENTITY EN EL CREADO DE UNA TABLA CON PK**

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
        cod_rol_in    INT IDENTITY(1,1) NOT NULL,
        nom_rol_vc VARCHAR(20),
        CONSTRAINT pk_personal_rol_cod_rol_in   
        PRIMARY KEY (cod_rol_in)
    )
    GO
    INSERT INTO Personal.rol(nom_rol_vc) VALUES ('Ventas');
    INSERT INTO Personal.rol(nom_rol_vc) VALUES ('Almacén');
    INSERT INTO Personal.rol(nom_rol_vc) VALUES ('Facturación');
    GO
    SELECT * FROM Personal.rol
    GO

**CREACIÓN DE TABLA CON PK USANDO ALTER**

ALTER sirve para modificar TABLAS, BASE DE DATOS Y SCHEMAS

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
        cod_rol_in    INT IDENTITY(1,1) NOT NULL,
        nom_rol_vc VARCHAR(20),
    )
    GO
    INSERT INTO Personal.rol(nom_rol_vc) VALUES ('Ventas');
    INSERT INTO Personal.rol(nom_rol_vc) VALUES ('Almacén');
    INSERT INTO Personal.rol(nom_rol_vc) VALUES ('Facturación');
    
    --Modificamos la TABLA con Alter
    ALTER TABLE Personal.rol
    ADD CONSTRAINT pk_personal_rol_cod_rol_in   
    PRIMARY KEY (cod_rol_in)
    
    GO
    SELECT * FROM Personal.rol
    GO

**USO DEL ORDER BY [Numero de columna] ASC O DESC**

![image](https://github.com/user-attachments/assets/9a69bd1f-ec20-4635-a29c-960b56ef9122)


## ALTERNATE KEY (CLAVE ALTERNATIVA)

**CARACTERÍSTICAS**

**UNICIDAD: Valores únicos**

**INDEXACIÓN: Cuando se crean las claves alternas, el motor de SQL crea un índice no-clúster en esas columnas. Esto optimiza las operaciones de búsqueda**

**INTEGRIDAD REFERENCIAL: Las claves alternas pueden ser usadas como Foreign Keys (FK) en otras tablas.**

**MÚLTIPLES CLAVES ALTERNAS: Pueden haber muchas claves alternas en una tabla**

**SELECCIÓN DE LA CLAVE ALTERNA: Se debe seleccionar considerando que sea estable a largo plazo, mínima cantidad de columnas y única (mismas restricciones de la PK)**

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
        cod_rol_in    INT IDENTITY(1,1)
        CONSTRAINT pk_personal_rol_cod_rol_in
        PRIMARY KEY NOT NULL,
        --ALTER KEY
        cod_rol_ch    CHAR(4)
        CONSTRAINT ak_personal_rol_cod_rol_in UNIQUE NOT NULL,
        --
        nom_rol_vc VARCHAR(20),
    )
    GO
    INSERT INTO Personal.rol(cod_rol_ch,nom_rol_vc) VALUES ('R001','Ventas');
    INSERT INTO Personal.rol(cod_rol_ch,nom_rol_vc) VALUES ('R002','Almacén');
    INSERT INTO Personal.rol(cod_rol_ch,nom_rol_vc) VALUES ('R003','Facturación');
    GO
    SELECT * FROM Personal.rol
    GO

**UPDATE con función LTRIM y RIGHT**

LTRIM: Convierte un entero a Character y elimina los espacios en blanco que existen en la izquierda.

RIGHT: Comienza desde el caracter pegado a la derecha y cuenta la cantidad de caracteres que se solicita. Ejemplo: Right("Sergio",5) = "ergio"

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
        cod_rol_in        INT IDENTITY(1, 1) NOT NULL,
        cod_rol_ch        CHAR(4) NULL,
        nom_rol_vc        VARCHAR(20) NOT NULL,
    
        CONSTRAINT pk_personal_rol_cod_rol_in
        PRIMARY KEY(cod_rol_in),
    )
    GO
    INSERT INTO Personal.rol(nom_rol_vc)
    SELECT name
    FROM sys.schemas
    WHERE principal_id != schema_id
    ORDER BY name
    GO
    SELECT * FROM Personal.rol
    GO
    UPDATE Personal.rol
    SET cod_rol_ch = 'R'+RIGHT('000'+LTRIM(cod_rol_in),3) FROM Personal.rol
    GO
    
    ALTER TABLE Personal.rol
    ADD CONSTRAINT ak_personal_cod_rol_ch
    UNIQUE (cod_rol_ch)
    
    SELECT * FROM Personal.rol

## ÍNDICES

Son estructuras de datos que mejoran el rendimiento de las consultas, para que el motor de SQL no busque info en toda la tabla.

**TIPOS DE ÍNDICES**

**ÍNDICE CLÚSTER (PK): Ordena los datos de la tablaen el orden definido por las columnas del índice. Solo se puede tener 1 índice clúster. La PK es definida como índice clúster**

**ÍNDICES NO CLÚSTER: Apunta a los datos de la tabla, pero no los organiza físicamente. Una tabla puede tener varios índices no clúster. Son eficientes para consultas que emplean columnas indexadas**

**ÍNDICES ÚNICOS: Garantizan que los valores en una columna sean únicos. Pueden ser clúster o no clúster**

**ÍNDICES ESPACIALES: Optimizan las consultas con datos geográficos y geométricos. Permiten estas búsquedas basadas en proximidad espacial**

**ÍNDICES FILTRADOS: Indexan un subcojunto de filas de una tabla, basado en condición específica. Pueden mejorar el rendimiento cuando las consultas son para subconjuntos de datos de una tabla**

**CREACIÓN DE ÍNDICE ASIGNADO A UN CAMPO DE UNA TABLA**

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
        cod_rol_in        INT IDENTITY(1, 1) NOT NULL,
        cod_rol_ch        CHAR(4) NULL,
        nom_rol_vc        VARCHAR(20) NOT NULL,
    
        CONSTRAINT pk_personal_rol_cod_rol_in
        PRIMARY KEY(cod_rol_in),
    )
    GO
    INSERT INTO Personal.rol(nom_rol_vc)
    SELECT name
    FROM sys.schemas
    WHERE principal_id != schema_id
    ORDER BY name
    GO
    SELECT * FROM Personal.rol
    GO
    UPDATE Personal.rol
    SET cod_rol_ch = 'R'+RIGHT('000'+LTRIM(cod_rol_in),3) FROM Personal.rol
    GO
    
    ALTER TABLE Personal.rol
    ADD CONSTRAINT ak_personal_cod_rol_ch
    UNIQUE (cod_rol_ch)
    GO
    --Crear índice a un campo
    CREATE INDEX ix_personal_rol_nom_rol_vc
    ON Personal.rol(nom_rol_vc)
    GO
    SELECT * FROM Personal.rol
    GO

**PARA HACER CONSULTAS A UNA DB CON EL MOUSE**

![image](https://github.com/user-attachments/assets/c99e6f52-443d-4e0f-9716-dffbfe6be13c)

Para extraer datos de otra db y setearlas a otra, simplemente se usa INSERT a la DB que quiero colocarle y un SELECT a la otra DB para extraer data. [Ver sintaxis]
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
                t.name = 'Empleado'
    )
    DROP TABLE Personal.Empleado
    GO
    CREATE TABLE Personal.Empleado
    (
        cod_emp_in        INT IDENTITY(1, 1) NOT NULL,
    	cod_emp_ch		  CHAR(4)  NULL,
        pat_emp_vc        VARCHAR(40) NOT NULL,
    	mat_emp_vc        VARCHAR(40) NOT NULL,
    	nom_emp_vc        VARCHAR(35) NOT NULL,
    	dni_emp_vc        CHAR(8) NOT NULL,
    	pwd_emp_vb        VARBINARY(128) NULL, --Debe ir encriptado, si no cae AUDITORÍA!!
    	fec_nac_emp_da	  DATE NOT NULL,
    	cod_sex_bit		  BIT,
    	cod_rol_in INT NULL,
    
        CONSTRAINT pk_personal_empleado_cod_emp_in
        PRIMARY KEY(cod_emp_in),
    )
    GO
    INSERT INTO Personal.Empleado(              
        pat_emp_vc,       
    	mat_emp_vc,        
    	nom_emp_vc,        
    	dni_emp_vc,        
    	fec_nac_emp_da,	  
    	cod_sex_bit		  
    
    )
    SELECT [pat_ben_vc]
          ,[mat_ben_vc]
          ,[nom_ben_vc]
          ,[dni_ben_ch]
          ,[fec_nac_ben_da]
          ,[cod_sex_bi]
      FROM [pvl].[dbo].[Beneficiario]
      WHERE LEN(TRIM(dni_ben_ch)) = 8
    
    GO
    SELECT * FROM  [Empresa].[Personal].Empleado
    
    UPDATE Personal.Empleado
    SET cod_emp_ch = 'E' +  RIGHT('000' + LTRIM(cod_emp_in),3)
    
    SELECT * FROM Personal.Empleado

**HACIENDO UPDATE**

![image](https://github.com/user-attachments/assets/6370e2e1-cb81-4519-a08c-939260d314a6)

## FOREIGN KEY (FK)

Las FK son las columnas que hacen referencia a las PK de otras tablas, es decir pueden haber varias FK en una tabla cuando esta se relaciona con las demás.

**Integridad referencial: Las claves foráneas garantizan que los valores de las columnas en una tabla tengan una correspondencia válida en otra**

**Relación entre tablas: Establecen relaciones entre tablas**

**Restricciones de referencia: Imponen restricciones de referencia, no se pueden insertar ni actualizar así porque sí**

## Asignar permiso para modificar las tablas en entorno gráfico

![image](https://github.com/user-attachments/assets/8c7d1a98-6399-4ba4-a740-36f10216c2a4)

![image](https://github.com/user-attachments/assets/a1eefb3b-3f03-45d0-9edd-491a81992f3c)

**Ahora sí podemos verlo en entorno gráfico**

![image](https://github.com/user-attachments/assets/fe63c352-a00b-4811-b184-ed475a60a231)
