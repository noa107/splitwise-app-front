# Proyecto GRI
## Módulos
- gri
  - frontend
  - [frontend](frontend/README.md)
  - backend
  - [backend](backend/README.md)
  
## Instalación en entorno DEV

Para instalar el aplicativo en un entorno de desarrollo se necesita lo siguiente:

- Weblogic 12.2.1 instalado (ver guía de instalación en proyecto Gemas-Java
- Instalación de nodejs y paquetes para react instalados (ver guía en proyecto Gemas-Admin --> frontend)

En nuestra instalación de weblogic habrá que crear un datasource, varios WorkManager y MES, y un persistence store. Para ello realizar lo siguiente:

### Creación de WorkManager y Managed Executed Services (MES)

Acceder a la consola de nuestro servidor local: http://127.0.0.1:7001/console
Navegar a estas opciones: Environment -> WorkManager -> New

![Img1](images/wls-1.png)

Setear como nombre "GemasZMQReaderWM" y scope "Global", aceptar, editarlo de nuevo y seleccionar la siguiente opción:

![Img1](images/wls-2.png)

Con esto ya tendriamos creado el workmanager. Ahora para crear el MES habría que hacer lo siguiente:

Navegar hasta Environment -> Concurrent Templates – New Managed Executer Service Template y rellenar los datos como se ve en la imagen: (dispatch policy es el nombre del workmanager creado en el paso anterior)

![Img1](images/wls-3.png)

Repertir estos pasos hasta crear las siguiente parejas de workmanger y MES

- WorkManager: GRICritWM, MES: GriCritMes
- WorkManager: GRIAlarmWM, MES: GRIAlarmMes

### Creación de datasource a base de datos

GRI precisa de una base de datos para funcionar. Cada desarrollador usará su propio esquema para no interferir en los datos generados por el aplicativo cuando está arrancado. Los datos de conexión son los siguientes:

Servidor: 172.24.57.69
Puerto: 1522
SID: reegric0

Pasos en weblogic:

![Img1](images/wls-ds-1.png)
![Img1](images/wls-ds-2.png)
![Img1](images/wls-ds-3.png)
![Img1](images/wls-ds-4.png)
![Img1](images/wls-ds-5.png)
![Img1](images/wls-ds-6.png)
![Img1](images/wls-ds-7.png)

### Creación de persistence store

Los timers necesitan un persistence store para almacenar su estado (solo sería necesario en entornos cluster, pero al ser una configuración anotada en las clases se debe crear también en el entorno de desarrollo).

Para crear seguir los siguientes pasos:

![Img1](images/wls-ps-1.png)
![Img1](images/wls-ps-2.png)
![Img1](images/wls-ps-3.png)

Volver a editar el PS y setear el logical name del mismo.

![Img1](images/wls-ps-4.png)


### Propiedades de arranque de WLS

Para que arranque correctamente el servidor se deben añadir estas propiedades de arranque al mismo editando el archivo:

%WL_HOME%\user_projects\domains\gemas\bin\setDomainEnv.cmd

y añadir la siguiente línea (línea 42 más o menos):

set JAVA_OPTIONS=-Duser.timezone=UTC -Duser.language=en -Duser.country=US





