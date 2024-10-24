
## Paso 1: Conéctate a tu Droplet

Primero, debes conectarte a tu Droplet usando SSH. En tu terminal, ejecuta el siguiente comando, reemplazando `your_droplet_ip` con la dirección IP pública de tu Droplet:

```bash
ssh root@your_droplet_ip
```

## Paso 2: Actualiza tu Droplet

Es importante asegurarte de que todo tu sistema esté actualizado antes de instalar cualquier software. Ejecuta lo siguiente:

```bash
sudo apt update && sudo apt upgrade -y
```

## Paso 3: Instala Docker en tu Droplet

Ahora instalaremos Docker siguiendo los pasos recomendados para Ubuntu. Ejecuta estos comandos uno a uno:

### 1. Instala las dependencias necesarias:

```bash
sudo apt install apt-transport-https ca-certificates curl software-properties-common
```

### 2. Agrega la clave GPG oficial de Docker:

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

### 3. Agrega el repositorio de Docker:

```bash
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

### 4. Instala Docker:

```bash
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io
```

### 5. Verifica que Docker está instalado correctamente:

```bash
sudo systemctl status docker
```

Si ves que el servicio Docker está activo y en ejecución, ¡ya tienes Docker instalado en tu Droplet!

## Paso 4: Descargar y usar una imagen específica de MongoDB

Aquí te muestro cómo descargar y usar la imagen de MongoDB versión 5.0 o latest:

### 1. Descargar la versión específica de MongoDB (5.0 o latest)

Para descargar la imagen de MongoDB, utiliza el siguiente comando para obtener la versión que deseas. Aquí te muestro cómo hacerlo tanto para la versión `5.0` como para la más reciente `latest`.

#### Descargar MongoDB versión 5.0:

```bash
sudo docker pull mongo:5.0
```

#### O, si prefieres la última versión estable:

```bash
sudo docker pull mongo:latest
```

### 2. Ejecutar MongoDB con la imagen descargada

Una vez que hayas descargado la imagen, puedes crear un nuevo contenedor de MongoDB con la imagen que acabas de descargar. A continuación te doy los comandos para ambas versiones:

#### Para MongoDB 5.0:

```bash
sudo docker run -d -p 27017:27017 --name mongodb -e MONGO_INITDB_ROOT_USERNAME=admin -e MONGO_INITDB_ROOT_PASSWORD=password mongo:5.0
```

#### Para MongoDB latest:

```bash
sudo docker run -d -p 27017:27017 --name mongodb -e MONGO_INITDB_ROOT_USERNAME=admin -e MONGO_INITDB_ROOT_PASSWORD=password mongo:latest
```

Este comando hará lo siguiente:
- `-d`: Ejecuta el contenedor en segundo plano.
- `-p 27017:27017`: Expone el puerto 27017 del contenedor (el puerto predeterminado de MongoDB) en tu Droplet.
- `--name mongodb`: Le da el nombre `mongodb` al contenedor.
- `-e MONGO_INITDB_ROOT_USERNAME=admin -e MONGO_INITDB_ROOT_PASSWORD=password`: Configura el usuario administrador `admin` y la contraseña `password` (cámbialos por algo más seguro si lo deseas).
- `mongo:5.0` o `mongo:latest`: Utiliza la versión de MongoDB que especificaste.

### 3. Verificar que MongoDB está ejecutándose

Para comprobar que tu contenedor de MongoDB está ejecutándose correctamente, usa el siguiente comando:

```bash
sudo docker ps
```

Este comando debería mostrarte una lista de los contenedores en ejecución, incluido el contenedor `mongodb` con MongoDB en el puerto 27017.

### 4. Conectar al contenedor MongoDB usando el cliente

Ahora que el servidor MongoDB está en funcionamiento, puedes conectarte a él usando el cliente `mongo`. Puedes hacerlo desde otro contenedor temporal con el cliente MongoDB o instalando el cliente en tu Droplet. Aquí están las opciones:

#### Usar un contenedor temporal con el cliente MongoDB:

Si deseas conectarte desde otro contenedor, ejecuta lo siguiente:

```bash
sudo docker run -it --rm --network container:mongodb mongo:5.0 mongo -u admin -p password --authenticationDatabase admin
```

Este comando:
- Ejecuta un contenedor temporal con el cliente `mongo`.
- Usa la misma red del contenedor MongoDB (`--network container:mongodb`).
- Conecta al servidor MongoDB en el contenedor.
```
