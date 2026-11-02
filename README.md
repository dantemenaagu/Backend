# Backend de Tareas con autenticación JWT

API REST hecha con Node.js, Express y MongoDB (Mongoose). Permite registrar usuarios, iniciar sesión y obtener un token JWT. Con ese token, cada usuario puede crear, ver, modificar y eliminar únicamente sus propias tareas.

## Tecnologías

- Node.js y Express
- MongoDB Atlas con Mongoose
- JSON Web Token (jsonwebtoken)
- bcrypt para cifrar contraseñas
- dotenv, nodemon

## Instalación

1. Clona el repositorio:
   ```
   git clone https://github.com/dantemenaagu/Backend.git
   cd Backend
   ```
2. Instala las dependencias:
   ```
   npm install
   ```
3. Crea un archivo `.env` en la raíz a partir de `.env.example` y llena tus valores:
   ```
   PORT=5000
   NODE_ENV=development
   MONGO_DB=tu_cadena_de_conexion_de_mongodb
   JWT_SECRET=tu_secreto_largo_y_aleatorio
   ```

## Ejecución

Modo desarrollo (con nodemon):
```
npm run server
```

Modo normal:
```
npm start
```

El servidor queda en `http://localhost:5000`.

## Endpoints

| Método | Ruta | Descripción | ¿Requiere token? |
|--------|------|-------------|------------------|
| POST | /api/users | Registrar usuario | No |
| POST | /api/users/login | Iniciar sesión y obtener el JWT | No |
| GET | /api/users/datos | Datos del usuario dueño del token | Sí |
| GET | /api/tareas | Listar las tareas del usuario logueado | Sí |
| POST | /api/tareas | Crear una tarea | Sí |
| PUT | /api/tareas/:id | Modificar una tarea propia | Sí |
| DELETE | /api/tareas/:id | Eliminar una tarea propia | Sí |

Las rutas protegidas necesitan el header:
```
Authorization: Bearer <token>
```

## Ejemplos de body

Registro:
```json
{ "name": "Pedro", "email": "pedro@test.com", "password": "123456" }
```

Login:
```json
{ "email": "pedro@test.com", "password": "123456" }
```

Crear o modificar tarea:
```json
{ "descripcion": "Mi tarea" }
```

## Seguridad

- Las contraseñas se guardan cifradas con bcrypt.
- El middleware `protect` valida el token en todas las rutas de tareas y responde 401 si falta o es inválido.
- El id del usuario se obtiene del token, nunca del body ni de la URL.
- Cada tarea guarda el id de su dueño y todas las consultas filtran por ese id.