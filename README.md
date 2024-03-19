<h1 align="center">CVDS-lab-6</h1>

# Despliega mi primera aplicación en Azure

## Mi primer despliegue en la nube

# Parte I - Despliegue app React (frontend) en Azure

1) Busca Azure for Students en tu buscador de preferencias e ingresa con el correo institucional.
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

 
