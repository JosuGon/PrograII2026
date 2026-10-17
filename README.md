# Proyecto base: Express + Sequelize + PostgreSQL + JWT

Este proyecto ya viene con el flujo de autenticación completo y funcionando
(usuarios, roles, login, JWT) y una entidad de ejemplo (`estudiantes`) con
CRUD protegido. Está probado y verificado — solo falta conectarlo a tu propio
PostgreSQL local.

## 1. Instalar dependencias

```bash
npm install
```

## 2. Configurar variables de entorno

Copia `.env.example` a `.env` y ajusta los valores según tu instalación de
PostgreSQL:

```bash
cp .env.example .env
```

```
DB_NAME=mi_base
DB_USER=postgres
DB_PASSWORD=tu_password
DB_HOST=127.0.0.1
DB_PORT=5432
JWT_SECRET=una_clave_super_secreta_y_larga_12345
PORT=8010
```

## 3. Crear la base de datos y las tablas

Crea la base de datos vacía (el nombre debe coincidir con `DB_NAME`):

```bash
createdb mi_base
```

Luego aplica el script que crea las tablas `roles`, `usuarios` y
`estudiantes`, con los roles básicos ya insertados:

```bash
psql -d mi_base -f server/schema.sql
```

## 4. Levantar el servidor

```bash
npm run dev
```

Si todo está bien configurado, verás en la terminal:

```
Conexión a PostgreSQL establecida correctamente.
Servidor corriendo en http://localhost:8010
```

## 5. Endpoints ya disponibles

| Método | Ruta                     | Protegida | Descripción                          |
|--------|--------------------------|-----------|---------------------------------------|
| POST   | /api/auth/registrar      | No        | Crea un usuario (nombre, email, password, rol_id) |
| POST   | /api/auth/login          | No        | Devuelve el token JWT                 |
| GET    | /api/ejemplo             | Sí        | Ruta de prueba para validar el token  |
| GET    | /api/estudiante          | No        | Lista todos los estudiantes           |
| GET    | /api/estudiante/:id      | No        | Obtiene un estudiante por id          |
| POST   | /api/estudiante          | Sí        | Crea un estudiante                    |
| PUT    | /api/estudiante/:id      | Sí        | Actualiza un estudiante               |
| DELETE | /api/estudiante/:id      | Sí        | Elimina un estudiante                 |

Las rutas protegidas requieren el header:
```
Authorization: Bearer <token>
```

## 6. Continuar con el laboratorio

Sequelize CLI ya está instalado y configurado (ver `.sequelizerc` y
`server/config/config.js`, apuntando ambos a la carpeta `server/`). Para
agregar tu propia entidad nueva (Curso, Profesor, etc.) sigue la guía de
automatización:

```bash
npx sequelize-cli model:generate --name Curso --attributes nombre:string,creditos:integer
npx sequelize-cli db:migrate
node generar-crud.js Curso
```

**Nota:** el modelo que genera `sequelize-cli` usa un formato distinto
(función factory) al de `server/models/Estudiante.js` (formato directo).
Ajusta el archivo generado para que siga el mismo patrón que
`Estudiante.js`, así `generar-crud.js` podrá usarlo sin cambios.

Después, conecta la ruta nueva en `app.js`, igual que se hizo con
`estudianteRoutes`.
