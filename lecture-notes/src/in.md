# Introducción

## Temas del curso

Curso básico de devops que cubre varias temáticas, herramientas y tecnologías:

1. Linux, vagrant, networking
2. Cloud Computing in AWS
3. Git, maven, Jenkins, CICD, Sonarqube, Pipelines as a Code
4. Bash and python scripting
5. Ansible
6. More AWS
7. Docker, Kubernetes
8. Terraform

# Prerequisitos y set-up

## Información sobre los prerequisitos

Se necesitan las siguientes herramientas y tecnologías:

- Repo de GitHub de referencia [https://github.com/devopshydclub/vprofile-project](https://github.com/devopshydclub/vprofile-project)
- Instalar software.
    - Virtual Box
    - Git bash
    - Vagrant
    - Chocolatey/Brew (Opcional)
    - JDK8
    - IntelliJ
    - Sublime Text Editor
    - AWS Cli
- Crear/acceder a algunas cuentas.
    - GitHub
    - DockerHub
    - Godaddy (comprar un dominio)
    - Sonarcloud
- Configurar una cuenta de AWS.


## Instalando el software

Consultar la rama `prereqs` del repo de referencia.

Instalar el software detallado en caso que no se diponga de el. De momento, no se instalan los ides/editoresv de código.

## Creando y configurando cuentas

- Crear una cuenta de GitHub en caso de no disponer de una.
- Crear una cuenta de DockerHub.
- Comprar un dominio. El autor propone comprarl en Godaddy.
- Acceder a [https://sonarcloud.io/login](https://sonarcloud.io/login) utilizando las credendiales de GitHub. Se pueden importar los repos de los que se disponga. Se utiliza el plan free.

## AWS Setup

**Aviso** Estoy utilizando una cuenta AWS en la que borré, en la mayoría de las regiones, algunos recursos de `EC2`, etc que venían por defecto. En la región `Canada (Central)` no borré nada.

Esta es la parte más tediosa del setup inicial necesario para empezar el curso. Incluye:

- Free Tier Account
- IAM with MFA
- Billing alarm
- Certificate setup

En los que sigue, hay algunos pasos que no se detallan. Pueden consultarse en la sección **[Kubernetes en la nube][AWS. Crear y configurar una cuenta]** de los apuntes siguientes

> `20220711-kubernetes-for-the-absolute-beginners`

### Free Tier Account

Crear un cuenta de AWS asocidada a una cuenta de correo. Este paso no tiene mayor complicación. El usuario de esta cuenta recibe el nombre de `root user`.

Pasos a dar:

- Activar el MFA para el root user

Está documentado en el v

### IAM with MFA

Crear un usuario IAM y activar el MFA.

### Billing alarm

Nota inicial: lo siguiente hay que hacerlo con el root user o con un iam user que tenga permisos para ver la información del billing.

Se deben de hacer dos cosas:

1. Navegar a `Billing - Billing preferences` y activar dos opciones: la de recibir emails de alerta si se sobrepasan los límites del free tier y la de recibir Billings Alerts. Guardar los cambios. 
2. Navegar a `CloudWatch`. Hay que situarse en la región `US East (N. Virginia)`, que es donde se almacenan las métricas de billing. Navegamos a `Alarms - All alarms` y creamos una nueva alarma seleccionando la métrica **Billing/Total Estimated Charge/USD**. 
    - Asignar 5 como threshold (avisos al superar los 5 dólares).
    - Notification. Crear un nuevo SNS topic llamado `MonitoringTeam`, seleccionarlo e introducir una dirección de correo. (entrar al correo para confirmar esta subscripción).
    - Add name and description.  `AWSBillingAlert`

### Certificate setup

En AWS, con el usuario root, navegar hasta `Certificate Manager` y utilizar la opción, que será gratuíta `Request a certificate`:

Se hace clic en `Request a public certificate`.

Se introduce el nombre del dominio que hemos comprado con el siguiente formato:

```bash
*.nombredeldominio.com
```

Se utilizará validación DNS y se pone el siguiente tag

```bash
Name: nombredeldominio.com
```

El estado del certificado aparecera en estado pendiente de validar. Accedemos al certificado para obtener estos dos datos:

- `CNAME name`
- `CNAME value`

En el caso de haber comprado el dominio en GoDaddy. accedemos a su portal web y, en el domio que hemos comprado, buscamos la opción `Administrar dominio - Administrar DNS`.

Se añadirá un nuevo registro DNS (DNS Record) de tipo CNAME:

- Colocar el name y el valur obtenidos del certificado de AWS eliminando el punto del final.
- TTL default.
- Resolver con el nombre más corto, sin repetir el nombre del dominio.

Tras esperar 5-10 minutos, en raras ocasiones 1 hora, el estado del certificado de AWS cambia a `Issued`.

