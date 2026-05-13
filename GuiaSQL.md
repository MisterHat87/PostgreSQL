# PostgreSQL 
Guía básica de PostgreSQL para sentirte como todo un DBA que domina el PGAdmin y trabaja para una gran empresa
---
<img width="100" height="100" alt="Chest" src="https://github.com/user-attachments/assets/b4008141-9dde-4e3e-b28c-81ab3701c642" /><img width="100" height="100" alt="Chest" src="https://github.com/user-attachments/assets/b4008141-9dde-4e3e-b28c-81ab3701c642" /><img width="100" height="100" alt="Chest" src="https://github.com/user-attachments/assets/b4008141-9dde-4e3e-b28c-81ab3701c642" /><img width="100" height="100" alt="Chest" src="https://github.com/user-attachments/assets/b4008141-9dde-4e3e-b28c-81ab3701c642" /><img width="100" height="100" alt="Chest" src="https://github.com/user-attachments/assets/b4008141-9dde-4e3e-b28c-81ab3701c642" /><img width="100" height="100" alt="Chest" src="https://github.com/user-attachments/assets/b4008141-9dde-4e3e-b28c-81ab3701c642" /><img width="100" height="100" alt="Chest" src="https://github.com/user-attachments/assets/b4008141-9dde-4e3e-b28c-81ab3701c642" /><img width="100" height="100" alt="Chest" src="https://github.com/user-attachments/assets/b4008141-9dde-4e3e-b28c-81ab3701c642" /><img width="100" height="100" alt="Chest" src="https://github.com/user-attachments/assets/b4008141-9dde-4e3e-b28c-81ab3701c642" /><img width="100" height="100" alt="Chest" src="https://github.com/user-attachments/assets/b4008141-9dde-4e3e-b28c-81ab3701c642" />

---
# Índice

- [Capitulo 1. ¿Cómo entrar a PostgreSQL?](#capitulo-1-cómo-entrar-a-postgresql)
- [Capitulo 2. La Base De Datos](#capitulo-2-la-base-de-datos)
- [Capitulo 3. Los Esquemas](#capitulo-3-los-esquemas)
- [Capitulo 4. LDD](#capitulo-4-ldd)
- [Capitulo 5. LMD](#capitulo-5-lmd)
- [Capitulo 6. Roles](#capitulo-6-roles)
- [Capitulo 6.1/2 Pruebas de Permisos](#capitulo-612-pruebas-de-permisos)
- [Capitulo 7. Generalidades](#capitulo-7-generalidades)


---

# Capitulo 1. ¿Cómo entrar a PostgreSQL?:

```bash
sudo su

-i -u postgres

psql
```

---

# Capitulo 2. La Base De Datos:

## ¿Cómo crear una Base de Datos?

```sql
CREATE DATABASE ElNombreDelaBase;
```

## Uy le puse mal el nombre, ¿Cómo lo cambio?

```sql
ALTER DATABASE ElNombreDelaBase RENAME TO ElNuevoNombreDelaBase;
```

## ¿Cómo borro una Base de Datos?

```sql
DROP DATABASE MiBaseDeDatos;
```

---

## Acceso:

### Selecciona la base de datos a usar

```sql
USE ElnombreDElaBase;
```

### Para Acceder a esta:

```sql
\c  ElnombreDElaBase;
```

---

# ¡Importante!

## Revocar permisos de creación en el esquema public:

primero conectamos con la base

```sql
\c  ElnombreDElaBase;
```

luego

```sql
REVOKE CREATE ON SCHEMA public FROM PUBLIC;
```

Nota: Nadie crea objetos en el esquema público.

```sql
REVOKE CONNECT ON DATABASE ElnombreDElaBase FROM PUBLIC;
```

Nota: Nadie se conecta sin permiso explícito.

---

# Capitulo 3. Los Esquemas:

Crea un nuevo esquema dentro de la base de datos.

```sql
\c  ElnombreDElaBase;
```

Nota: Para elegir donde lo vamos a crear

## ¿Cómo creo un esquema?

```sql
CREATE SCHEMA IF NOT EXISTS nombre_del_esquema;
```

## Otra vez mae, ¿Cómo cambio el nombre?

```sql
ALTER SCHEMA nombre_del_esquema RENAME TO nuevo_nombre_del_esquema;
```

## ¿Cómo cambio el propietario del esquema?

```sql
ALTER SCHEMA nombre_del_esquema OWNER TO nuevo_duenno;
```

## ¿Cómo borro un esquema?

```sql
DROP SCHEMA nombre_del_esquema;
```

o

```sql
DROP SCHEMA nombre_del_esquema CASCADE;
```

Nota: este borra todo dentro de el

---

## Revocar permisos:

```sql
REVOKE CREATE ON SCHEMA public FROM PUBLIC;
```

```sql
REVOKE CONNECT ON DATABASE ElnombreDElaBase FROM PUBLIC;
```

---

# Capitulo 4. LDD

## Crear la tabla

```sql
CREATE TABLE nombre_del_esquema.nombretabla (
    id SERIAL PRIMARY KEY,
    nombre Varchar(100),
    edad INTEGER(3)
);
```

---

## Tipos de datos más comunes en SQL

| Tipo | Descripción |
|---|---|
| SERIAL | Entero autoincremental |
| INTEGER | Un int de toda la vida |
| VARCHAR | Texto con tamaño máxima definido |
| TEXT | Texto sin límite de longitud |
| NUMERIC | Número con precisión exacta |
| DATE | Fecha (año, mes, día) |
| TIMESTAMP | Fecha y hora completas |
| BOOLEAN | Verdadero/Falso |

---

## Estos editan el LDD una tabla ya existente.

```sql
ALTER TABLE nombre_del_esquema.nombretabla ADD COLUMN fecha DATE;
```

Nota: Agrega una nueva columna llamada fecha.

```sql
ALTER TABLE nombre_del_esquema.nombretabla DROP COLUMN nombre;
```

Nota: Elimina la columna cliente.

---

## Eliminar por completo una tabla.

```sql
DROP TABLE nombre_del_esquema.nombretabla;
```

---

## Borra todos los registros de una tabla, pero mantiene su estructura.

```sql
TRUNCATE TABLE nombre_del_esquema.nombretabla;
```

---

# Capitulo 5. LMD

## Comandos básicos del LMD

### Consultar datos

```sql
SELECT * FROM nombre_esquema.nombre_tabla;
```

Muestra todos los registros de la tabla.

---

### Consultar columnas específicas

```sql
SELECT nombre, edad
FROM nombre_esquema.nombre_tabla;
```

Muestra únicamente las columnas indicadas.

---

### Contar registros

```sql
SELECT COUNT(*)
FROM nombre_esquema.nombre_tabla;
```

Cuenta cuántos registros existen en la tabla.

---

### Ordenar resultados

```sql
SELECT *
FROM nombre_esquema.nombre_tabla
ORDER BY edad DESC;
```

Ordena los resultados de mayor a menor según la columna edad.

```sql
SELECT id, nombre, edad
FROM nombre_del_esquema.nombretabla
WHERE edad > 67;
```

Devuelve los registros con edad mayor a 67.

---

### Insertar nuevos registros

```sql
INSERT INTO nombre_del_esquema.nombretabla (nombre, edad)
VALUES ('Artyom', 25);
```

Agrega un nuevo registro con nombre y edad.

---

### Actualizar registros existentes

```sql
UPDATE nombre_del_esquema.nombretabla
SET edad = 30
WHERE nombre = 'Artyom';
```

Cambia la edad de Artyom a 30.

---

### Eliminar registros

```sql
DELETE FROM nombre_del_esquema.nombretabla
WHERE id = 1;
```

Borra el registro con id = 1.

---

# Capitulo 6. Roles

> "Y creó Dios al hombre a su imagen, a imagen de Dios lo creó"

---

## ¿Qué es un Rol?

Un rol en PostgreSQL es una entidad que puede recibir permisos.

Puede funcionar como:

- Un grupo de permisos.
- Un usuario con capacidad de iniciar sesión.

---

## ¿Cómo creo un usuario(Rol de acceso)?

```sql
CREATE USER fulano WITH PASSWORD 'claveSegura123';
```

## Como lo edito

```sql
ALTER ROLE fulano RENAME TO fulano_de_tal;
```

```sql
ALTER USER fulano WITH PASSWORD 'nuevaClave456';
```

```sql
ALTER USER fulano WITH SUPERUSER;
```

## y como lo borro?

```sql
DROP USER fulano;
```

---

## ¿Cómo creo un ROL?

```sql
CREATE ROLE nombre_rol;
```

---

## Opciones comunes al crear Roles

| Opción | Descripción |
|---|---|
| LOGIN | Permite iniciar sesión |
| NOLOGIN | No puede iniciar sesión |
| PASSWORD 'abc' | Define contraseña |
| SUPERUSER | Acceso total al sistema |
| CREATEDB | Puede crear bases de datos |
| CREATEROLE | Puede crear otros roles |
| INHERIT | Hereda permisos de otros roles |

---
##
---

## ¿Cómo editar un usuario o rol?

### Cambiar nombre

```sql
ALTER ROLE nombre_rol RENAME TO nuevo_nombre_rol;
```

### Cambiar contraseña

```sql
ALTER USER fulano WITH PASSWORD 'nuevaClave456';
```

### Dar permisos de superusuario

```sql
ALTER USER fulano WITH SUPERUSER;
```

---

## ¿Cómo eliminar un usuario o rol?

### Eliminar usuario

```sql
DROP USER fulano_de_tal;
```

### Eliminar rol

```sql
DROP ROLE nombre_rol;
```

---

# Permisos de Conexión a la Base de Datos

## Dar permiso de conexión

```sql
GRANT CONNECT ON DATABASE ElnombreDElaBase TO nombre_rol;
```

## Solo uso del esquema

```sql
GRANT USAGE ON SCHEMA nombre_esquema TO nombre_rol;
```

## Uso + creación de objetos

```sql
GRANT USAGE, CREATE ON SCHEMA nombre_esquema TO nombre_rol;
```

---

| Permiso | Operación |
|---|---|
| SELECT | Consultar datos |
| INSERT | Insertar registros |
| UPDATE | Actualizar registros |
| DELETE | Eliminar registros |
| ALL PRIVILEGES | Todos los permisos |

---

## Solo lectura

```sql
GRANT SELECT ON ALL TABLES IN SCHEMA nombre_esquema TO nombre_rol;
```

## Lectura y escritura

```sql
GRANT SELECT, INSERT, UPDATE, DELETE
ON ALL TABLES IN SCHEMA nombre_esquema
TO nombre_rol;
```

## Todos los privilegios

```sql
GRANT ALL PRIVILEGES
ON ALL TABLES IN SCHEMA nombre_esquema
TO nombre_rol;
```

---

# Permisos para Tablas Futuras

## (DEFAULT PRIVILEGES)

### Solo lectura en tablas futuras

```sql
ALTER DEFAULT PRIVILEGES IN SCHEMA nombre_esquema
GRANT SELECT ON TABLES TO nombre_rol;
```

### Lectura y escritura en tablas futuras

```sql
ALTER DEFAULT PRIVILEGES IN SCHEMA nombre_esquema
GRANT SELECT, INSERT, UPDATE, DELETE
ON TABLES TO nombre_rol;
```

---

## Crear roles jerárquicos

```sql
CREATE ROLE empleado;

CREATE ROLE supervisor;
```

---

## Heredar permisos

```sql
GRANT empleado TO supervisor;
```

Nota: supervisor heredará automáticamente todos los permisos asignados a empleado.

---

## Verificar herencia

```sql
\du
```

Con eso se ven qué roles heredan permisos de otros

### Todos los privilegios

```sql
ALTER DEFAULT PRIVILEGES IN SCHEMA nombre_esquema
GRANT ALL PRIVILEGES ON TABLES TO nombre_rol;
```
---

# Capitulo 6.1/2 Pruebas de Permisos 

Hay que probar la vara para sacar 100%

---
## Iniciar sesión con un usuario 

```bash
psql -U fulano -d ElNombreDelaBase -h localhost -W
```

Nota: Esto permite conectarse directamente con el usuario que queremos probar.

---

## Probar lectura de datos

```sql
SELECT * FROM nombre_del_esquema.nombretabla;
```

Si el usuario tiene permiso SELECT, podrá visualizar los registros.

---

## Probar inserción de datos

```sql
INSERT INTO nombre_del_esquema.nombretabla (nombre, edad) VALUES ('Artyom', 25);
```

## Ver el usuario actual conectado

```sql
SELECT current_user;
```

Util por si tiene la memoria que yo tengo

---

## Cambiar temporalmente de rol

```sql
SET ROLE nombre_rol;
```

---

## Volver al rol original

```sql
RESET ROLE;
```


# Capitulo 7. Generalidades

Estos comandos ayudan muchísimo cuando uno se pierde o quiere revisar rápidamente qué existe dentro de PostgreSQL.

---

## Ver bases de datos

```sql
\l
```

Muestra todas las bases de datos existentes dentro del servidor PostgreSQL.
---

## Cambiar de base de datos

```sql
\c nombre_bd
```

Permite conectarse a otra base de datos desde psql.

---

## Ver esquemas

```sql
\dn
```

Muestra todos los esquemas de la base de datos actual.

---

## Ver tablas

```sql
\dt
```

Muestra todas las tablas existentes en la base de datos actual.

---

## Ver roles

```sql
\du
```

Muestra todos los usuarios y roles creados en PostgreSQL junto con sus permisos.

---

## Ver estructura de una tabla

```sql
\d nombre_tabla
```

Muestra la estructura completa de una tabla

# Gracias por leer



