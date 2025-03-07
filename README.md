# Proyecto DB001

Este es un proyecto para un curso de bases de datos que incluye la configuración de PostgreSQL, creación de tablas, conexión a un backend y el uso de Docker para el despliegue.

---

## Pasos del Proyecto

### 1. Conectar PostgreSQL de Render a PgAdmin 4

1. **Configura la conexión en PgAdmin 4**:
   - **Hostname**: `oregon-postgres.render.com`
   - Los demás datos de conexión están disponibles en la página de configuración de Render.

2. **Crear las tablas necesarias en PostgreSQL**:

```sql
-- Tabla de usuarios
CREATE TABLE usuarios (
    id SERIAL PRIMARY KEY,
    nombre_usuario VARCHAR(50) NOT NULL UNIQUE,
    correo VARCHAR(100) NOT NULL UNIQUE,
    contrasena VARCHAR(255) NOT NULL,
    fecha_registro TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Tabla de publicaciones
CREATE TABLE publicaciones (
    id SERIAL PRIMARY KEY,
    usuario_id INT NOT NULL,
    contenido TEXT NOT NULL,
    fecha_publicacion TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (usuario_id) REFERENCES usuarios (id) ON DELETE CASCADE
);

-- Tabla de comentarios
CREATE TABLE comentarios (
    id SERIAL PRIMARY KEY,
    publicacion_id INT NOT NULL,
    usuario_id INT NOT NULL,
    contenido TEXT NOT NULL,
    fecha_comentario TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (publicacion_id) REFERENCES publicaciones (id) ON DELETE CASCADE,
    FOREIGN KEY (usuario_id) REFERENCES usuarios (id) ON DELETE CASCADE
);

-- Tabla de relaciones (seguimientos)
CREATE TABLE relaciones (
    id SERIAL PRIMARY KEY,
    seguidor_id INT NOT NULL,
    seguido_id INT NOT NULL,
    fecha_seguimiento TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (seguidor_id) REFERENCES usuarios (id) ON DELETE CASCADE,
    FOREIGN KEY (seguido_id) REFERENCES usuarios (id) ON DELETE CASCADE,
    UNIQUE (seguidor_id, seguido_id) -- Evita duplicados
);

CREATE TABLE  likes (
	id serial primary key,
	usuario_id INT not null,
	publicacion_id int not null,
	comentario_id int not null,
	fecha TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
	CONSTRAINT fk_usuario FOREIGN KEY (usuario_id) references usuarios (id) ON DELETE CASCADE,
	CONSTRAINT fk_publicacion FOREIGN KEY (publicacion_id) REFERENCES publicaciones (id) ON DELETE CASCADE,
    CONSTRAINT fk_comentario FOREIGN KEY (comentario_id) REFERENCES comentarios (id) ON DELETE CASCADE
);

```

---

### 2. Conectar PostgreSQL al Backend

1. **Configura la conexión en el archivo `main.py`**:
   - Modifica la línea `DATABASE_URL` para agregar la URL de la base de datos externa proporcionada por Render.

```python
DATABASE_URL = "aqui va el External Database URL"
```

### 3. Levantar el Proyecto con Docker

1. Usa el siguiente comando para construir y levantar los servicios:

```bash
docker-compose up -d --build
```

---

---
### 4. Entrar a docs de fast api para probar los endpoints

- dentro de la url puedes entrar a tu-url/docs
- agregar docs a la url que entrega codespaces 
- `https://effective-sniffle-657p7gjvpj4h4vp-80.app.github.dev/`

---

## Información Adicional

### Requisitos Previos

- Tener instalado `Docker` y `Docker Compose`.
- Disponer de acceso a la base de datos en Render.
- Contar con PgAdmin 4 para gestionar PostgreSQL.

### Funcionalidades del Proyecto

- **Tablas:**
  - `usuarios`: para gestionar los usuarios.
  - `publicaciones`: para almacenar publicaciones.
  - `comentarios`: para almacenar comentarios.
  - `relaciones`: para almacenar relaciones.
- **Backend:**
  - Conexión a PostgreSQL.
  - API funcional con endpoints para CRUD.
- **Despliegue:**
  - Contenedor de Docker para simplificar el entorno de desarrollo y producción.


---

levantar grafana con docker 
`docker run -d -p 3000:3000 --name=grafana grafana/grafana-oss`
 -    -usuario : admin
 -    -password: admin
---
## Links Importantes (Contenidos a Revisar)

- **Documentación de FastAPI**: [FastAPI](https://fastapi.tiangolo.com/)  
- **Canal de Programación CodelyTV**: [CodelyTV en YouTube](https://www.youtube.com/@CodelyTV/)  
- **Curso de SQL**: [Curso de SQL - Hola Mundo](https://www.youtube.com/watch?v=uUdKAYl-F7g&t=1667s&ab_channel=HolaMundo)  
- **Grafana**: [Grafana - Página Oficial](https://grafana.com/)
