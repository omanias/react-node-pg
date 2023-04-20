# Conexion de ReactJs, NodeJs y Postgres

Para conectar una aplicación de React con un backend de Node.js, generalmente se utiliza una API, como Express.js, que facilita la comunicación entre ambas partes. Aquí te muestro los pasos básicos para hacerlo:

1. Crea tu proyecto de React y Node.js:
   Si no has creado tus proyectos todavía, crea un proyecto de React usando `create-react-app` y un proyecto de Node.js utilizando `npm init`.

2. Instala Express.js y otras dependencias en tu proyecto de Node.js:
   En la carpeta de tu proyecto de Node.js, ejecuta los siguientes comandos:

```
npm install express
npm install cors
```

Esto instalará Express.js y la librería CORS, que es necesaria para permitir solicitudes entre diferentes orígenes.

3. Crea una API básica en Node.js utilizando Express.js:
   En tu proyecto de Node.js, crea un archivo llamado app.js y escribe el siguiente código:

```javascript
const express = require("express");
const cors = require("cors");
const app = express();

app.use(cors());
app.use(express.json());

app.get("/api/data", (req, res) => {
  res.json({ message: "¡Hola desde Node.js!" });
});

const port = process.env.PORT || 5000;
app.listen(port, () => {
  console.log(`Servidor ejecutándose en el puerto ${port}`);
});
```

Este código crea un servidor Express.js básico que escucha en el puerto 5000 y expone un endpoint GET en /api/data.

---

4. Consume la API desde tu aplicación de React:
   En tu proyecto de React, instala axios para hacer solicitudes HTTP a tu API de Node.js:

```
npm install axios
```

Luego, en un componente de React, realiza una solicitud a la API utilizando axios. Por ejemplo, crea un archivo llamado App.js con el siguiente contenido:

```javascript
import React, { useEffect, useState } from "react";
import axios from "axios";

function App() {
  const [data, setData] = useState(null);

  useEffect(() => {
    async function fetchData() {
      try {
        const response = await axios.get("http://localhost:5000/api/data");
        setData(response.data.message);
      } catch (error) {
        console.error("Error al obtener datos de la API:", error);
      }
    }

    fetchData();
  }, []);

  return (
    <div className="App">
      {data ? <h1>{data}</h1> : <p>Cargando datos...</p>}
    </div>
  );
}

export default App;
```

Este código realiza una solicitud GET a la API y muestra el mensaje recibido en un elemento.

5. Ejecuta ambos proyectos:

- En la carpeta de tu proyecto de Node.js, ejecuta node app.js para iniciar el servidor.
- En la carpeta de tu proyecto de React, ejecuta npm start para iniciar la aplicación de React.

_Ahora, la aplicación de React debería estar conectada con tu API de Node.js y mostrar el mensaje "¡Hola desde Node.js!" en la página._

---

Para conectar una base de datos PostgreSQL con Node.js, puedes utilizar la biblioteca pg (node-postgres), que es un cliente de PostgreSQL para Node.js. Aquí te muestro cómo hacerlo:

1. Instala `pg` en tu proyecto de Node.js:
   En la carpeta de tu proyecto de Node.js, ejecuta el siguiente comando:

```
npm install pg
```

2. Configura la conexión a la base de datos:
   En tu proyecto de Node.js, crea un archivo llamado `db.js` y escribe el siguiente código, reemplazando las credenciales de la base de datos con las tuyas:

```javascript
const { Pool } = require("pg");

const pool = new Pool({
  host: "localhost",
  port: 5432,
  user: "your_username",
  password: "your_password",
  database: "your_database_name",
});

module.exports = {
  query: (text, params) => pool.query(text, params),
};
```

Este código crea un `Pool` de conexiones a la base de datos, que permite realizar consultas de forma eficiente y manejar múltiples conexiones simultáneamente.

3. Realiza consultas a la base de datos:
   Ahora que tienes una conexión configurada, puedes realizar consultas a la base de datos utilizando el objeto `query` exportado en `db.js`. Por ejemplo, en tu archivo `app.js` donde creaste el servidor Express.js, puedes agregar lo siguiente:

```javascript
const db = require("./db");

app.get("/api/users", async (req, res) => {
  try {
    const result = await db.query("SELECT * FROM users");
    res.json(result.rows);
  } catch (err) {
    console.error("Error al realizar consulta:", err);
    res.status(500).json({ error: "Error interno del servidor" });
  }
});
```

Este código agrega un nuevo endpoint `/api/users` que realiza una consulta para obtener todos los registros de la tabla `users` y devuelve los datos en formato JSON.

Asegúrate de haber creado previamente la tabla `users` en tu base de datos PostgreSQL para que este ejemplo funcione correctamente.

4. Ejecuta tu proyecto:
   En la carpeta de tu proyecto de Node.js, ejecuta `node app.js` para iniciar el servidor.

Ahora, tu servidor Node.js está conectado a una base de datos PostgreSQL y puedes realizar consultas y modificaciones utilizando el objeto `query` exportado en `db.js`.
