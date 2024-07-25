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

**TABLE SCAN**

El TABLE SCAN se refiere a la forma a la que el motor de base de datos procesa una consulta SELECT. **Esto ocurre cuando no hay índices disponibles o cuando el motor de consultas determina escaneo que el escaneo completo de la tabla es la mejor estrategia de ejecución**.

Cuando se realiza "Table Scan", el motor de SQL Server lee todas las filas de la tabla, una por una. **Esta operación puede ser costosa, cuando se trata de tablas grandes**
