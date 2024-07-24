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
