<h1 align="center">CVDS-lab-6</h1>

# Despliega mi primera aplicación en Azure

## Mi primer despliegue en la nube

# Parte I - Despliegue app React (frontend) en Azure

1) Busca Azure for Students en tu buscador de preferencias e ingresa con el correo institucional.

[Link Azure](https://portal.azure.com/?Microsoft_Azure_Education_correlationId=89dd6cdf-bcc8-4952-b538-bb29a0596354&Microsoft_Azure_Education_newA4E=true&Microsoft_Azure_Education_asoSubGuid=7d8f91e4-d9b4-4183-9a02-fb42ebadf187#home)

2) Crea un budget de 1 dólar para la cuenta

<p align="center">Creación presupuesto de un dolar.</p>

![image](https://github.com/JuanDpr99/CVDS-lab-6/assets/77819591/a5bddeda-0366-4a12-a634-ca3a156dab54)


# Parte II - Despliegue app web spring MVC (o spring-boot backend)
1) Inicie [Azure Cloud Shell](https://docs.microsoft.com/en-in/azure/cloud-shell/overview) desde el portal. Para implementar en un grupo de recursos, ingrese el siguiente comando
```shell
az group create --name MyResourceGroup --location westus
```

<p align="center">Creación grupo de recursos</p>

![image](https://github.com/JuanDpr99/CVDS-lab-6/assets/77819591/6443c499-41c6-4788-90a5-f31b9aa6e1d5)


2) Para crear un plan de servicio de aplicaciones (App service plan)
```shell
az appservice plan create --resource-group MyResourceGroup --name MyPlan --sku F1
```

![image](https://github.com/JuanDpr99/CVDS-lab-6/assets/77819591/9d01f363-59e1-4ab6-967a-b72071117a4b)


3) Finalmente, cree el servidor MySQL con un nombre de servidor único.
```shell
az mysql flexible-server create --resource-group MyResourceGroup --name mysqldbserver --admin-user mysqldbuser --admin-password P2ssw0rd@123 --sku-name B_Standard_B1ms
```

![image](https://github.com/JuanDpr99/CVDS-lab-6/assets/77819591/d67c16fb-4361-45fb-926d-a3d20b006f9b)


![image](https://github.com/JuanDpr99/CVDS-lab-6/assets/77819591/71241581-c9c5-42ce-892e-71d180adb113)

5) Seleccione **Properties**. Guarde el **Server name** y el **Server admin login name** en un bloc de notas.

![image](https://github.com/JuanDpr99/CVDS-lab-6/assets/77819591/776e9804-0407-47aa-8089-639a7b7433b1)


6) Seleccione **Connection security**. Habilite la opción **Allow access to Azure services** y guarde los cambios. Esto proporciona acceso a los servicios de Azure para todas las bases de datos de su servidor MySQL.

![image](https://github.com/JuanDpr99/CVDS-lab-6/assets/77819591/e52f0190-3a8b-4c1d-a9e5-1178ed3019db)


## Ejercicio 2: actualización de la configuración de la aplicación web
A continuación, navegue hasta la aplicación web que ha creado. Mientras implementa una aplicación Java, debe cambiar el contenedor web de la aplicación web a Apache Tomcat.
1) Seleccione **Configuration**. Establezca **Stack settings** como se muestra en la imagen a continuación y haga clic en Guardar.

![image](https://github.com/JuanDpr99/CVDS-lab-6/assets/77819591/081c516f-19af-4790-b158-feba5f5e7230)

![image](https://github.com/JuanDpr99/CVDS-lab-6/assets/77819591/ccd53a79-07b8-47cd-a36e-434d4ea003ce)


2) Seleccione Overview y click en Browse.

![image](https://github.com/JuanDpr99/CVDS-lab-6/assets/77819591/179aa633-6dde-4af6-b31a-726aaf764cc7)

3) La página web se verá como la imagen de abajo.

![image](https://github.com/JuanDpr99/CVDS-lab-6/assets/77819591/49caf315-ff3d-4709-8d5a-664b6efa1b53)

A continuación, debe actualizar las cadenas de conexión para que la aplicación web se conecte correctamente a la base de datos. Hay varias formas de hacerlo, pero para los fines de esta práctica de laboratorio, adoptará un enfoque simple actualizándolo directamente en Azure Portal.

4) Desde Azure Portal, seleccione la aplicación web que aprovisionó. Ir a Configuración | Configuración de la aplicación | Cadenas de conexión y haga clic en + Nueva cadena de conexión.

![image](https://github.com/PDSW-ECI/labs/assets/4140058/cccc9ce8-c19a-40c1-80b7-d82d278cc8db)

5) En la ventana Agregar/Editar cadena de conexión, agregue una nueva cadena de conexión MySQL con MyDatabase como nombre, pegue la siguiente cadena para el valor y reemplace MySQL Server Name, su nombre de usuario y su contraseña con los valores apropiados. Haga clic en Actualizar.
```java
jdbc:mysql://{cvdsdb.mysql.database.azure.com}:3306/alm?useSSL=true&requireSSL=false&autoReconnect=true&user={mysqldbuser}&password={P2ssw0rd@123}
```

![image](https://github.com/JuanDpr99/CVDS-lab-6/assets/77819591/556fc106-9234-4cbf-a8ed-60d0ce287967)


- Nombre del servidor MySQL: Valor que copió previamente de las Propiedades del servidor MySQL.
- su nombre de usuario: Valor que copió previamente de las Propiedades del servidor MySQL.
- su contraseña: valor que proporcionó durante la creación del servidor de base de datos MySQL.

![image](https://github.com/JuanDpr99/CVDS-lab-6/assets/77819591/97619f8b-9cdc-4d44-88d8-34701c5fc644)


7) Haga clic en Guardar para guardar la cadena de conexión.
> Nota: Las cadenas de conexión configuradas aquí estarán disponibles como variables de entorno, con el prefijo del tipo de conexión para aplicaciones Java (también para aplicaciones PHP, Python y Node). En el archivo src/main/resources/application.properties, recuperamos la cadena de conexión reemplazando el siguiente código:
```java
# ORM
# next line deletes the database on startup or shutdown
# spring.jpa.hibernate.ddl-auto=create-drop
# next line updates the database on startup
spring.jpa.hibernate.ddl-auto=update
spring.datasource.url=${MYSQLCONNSTR_MyDatabase}
#spring.datasource.username=root
#spring.datasource.password=my-secret-pw
spring.datasource.driverClassName=com.mysql.cj.jdbc.Driver
spring.jpa.show-sql=true
```
Ahora ha instalado y configurado todos los recursos necesarios para implementar y ejecutar la aplicación.

 
