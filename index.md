# Revisión del programa y migración a cloud de Enterprise X

## Objetivo

El objetivo de este documento es mostrar brevemente el estado de la empresa Enterprise X hasta la fecha y un programa de migración a cloud que le permita a la empresa ser más eficiente, flexible y ágil. El estudio revisa:

### Aplicaciones

- Estilo de arquitectura de aplicaciones
- Estructura de aplicaciones
- Persistencia de datos
- Repositorio de código fuente
- Integración de código y despliegue de aplicaciones
- Repositorio de componentes
- Documentación
- Retroalimentación de clientes y usuarios finales

### Infraestructura

- Plataforma de Cómputo
- Almacenamiento
- Despliegue de cargas de trabajo
- Uso de Nube
- Conectividad
- Monitoreo
- Soporte

El reporte está estructurado en tres partes principales:

- **¿ Qué es Enterprise X ?**
- **Hasta hoy**: Breve panorama del estado actual
- **Futuro**: Visión a futuro y propuesta de programa de migración a cloud

<br/><br/><br/>

## **_¿ Qué es Enterprise X ?_**

Es una compañía global dedicada a ofrecer los productos favoritos de las comunidades. Con presencia en 18 países, ofrece productos de calidad en diversas categorías y precios.

<br/>

![numbers](./img/numbers-es.png "Enterprise X en números")

<br/>

Todo esto está soportado por un conjunto de aplicaciones, entre las que destacan:

| Aplicación        |                                             Función                                             |
| ----------------- | :---------------------------------------------------------------------------------------------: |
| SAP ERP           | Soporta operación en general en cuanto producción, materiales, facturación, datos maestros, etc |
| SAP CRM           |                                   Soporta operación comercial                                   |
| SAP Commerce      | Soporta operación en general en cuanto producción, materiales, facturación, datos maestros, etc |
| Integration Bus   | Soporta operación en general en cuanto producción, materiales, facturación, datos maestros, etc |
| WMS               |                                   Soporta operación almacenes                                   |
| App de Venta      |                                   Soporta operación comercial                                   |
| App HH (obsoleto) |                                   Soporta operación comercial                                   |

Todo esto está soportado por la infraestructura informática, compuesta por:

- 20+ Centros de datos
- 3+ nubes privadas y 2 nubes públicas
- 600+ servidores
- Sistemas de almacenamiento
- Sistemas de redes informáticas

<br/><br/><br/>

## **_Hasta hoy_**

<br/><br/>

### **_Estilo de arquitectura de aplicaciones_** <br/>

El estilo de arquitectura de aplicaciones que más aparece en el ecosistema actual es el de Cliente-Servidor. Las aplicaciones de este tipo tienen las siguientes características:

- No se separa lógica de negocio de repositorio de datos
- Por lo general estas aplicaciones se construyen manera monolítica
- Un cambio erróneo puede afectar a todo el sistema fácilmente
- Su tamaño en cuanto a líneas de código y funcionalidades las hace altamente complejas y difíciles de mantener.

<br/>

Ejemplos de aplicaciones con este patrón:

- SAP ERP
- SAP CRM

<br/><br/>

### **_Estructura de aplicaciones_** <br/>

De manera general las aplicaciones hoy están compuestas por 2 elementos

- Aplicación cliente (lógica de presentación de la información)
- Servidor / Base de datos (lógica de negocio almacenamiento de datos)

Toda la lógica de la aplicación se integra en un solo bloque (monolítico). Esto provoca que sea dificil escalar, que la interacción entre aplicaciones no se base en un estandard y que se tengan que desarrollar interfaces ad-hoc para cubrir estas necesidades y que ante un incidente se vea afectada la aplicación en su totalidad.

<br/><br/>

### **_Construcción de aplicaciones_** <br/>

Actualmente se utilizan alrededor de 10 lenguajes sobre al menos 13 frameworks para desarrollo de aplicaciones en Enterprise X

| Lenguaje   |     Plataforma      |                         Función                         |
| ---------- | :-----------------: | :-----------------------------------------------------: |
| ABAP       |         SAP         |                       BAPI, RFCs                        |
| C#         | .Net 1.x (obsoleto) | Web apps, servicios Windows, aplicaciones de escritorio |
| VB         | .Net 1.x (obsoleto) | Web apps, servicios Windows, aplicaciones de escritorio |
| C#         | .Net 2.x (obsoleto) | Web apps, servicios Windows, aplicaciones de escritorio |
| VB         | .Net 2.x (obsoleto) | Web apps, servicios Windows, aplicaciones de escritorio |
| C#         | .Net 3.x (obsoleto) | Web apps, servicios Windows, aplicaciones de escritorio |
| C#         |      .Net 4.x       | Web apps, servicios Windows, aplicaciones de escritorio |
| C#         |    .Net core 2.x    |                      Web apps, API                      |
| C#         |    .Net core 3.x    |                      Web apps, API                      |
| Java       |       JDK 8.0       |       Web apps, serivicios, flujos de integración       |
| Java       |      JDK 11.0+      |       Web apps, serivicios, flujos de integración       |
| JavaScript |       NodeJS        |                           API                           |
| JavaScript |       Angula        |                         We apps                         |
| Xamarin/C# |     .Net (mono)     |                       Mobile Apps                       |
| PHP        |         PHP         |                        Web apps                         |
| Python 3   |        Flask        |                 Analítica de datos, API                 |
| R          |          R          |            Analítica de datos, API, Web Apps            |

<br/>
Esto provoca una gran complejidad para mantener o modificar aplicaciones existentes pues se tiene que contar con o contratar personal con dominio o 
conocimiento de tecnologías muy diversas.

No existe un estándar en cuanto herramientas de desarrollo y cada proyecto escoge las tecnologías que se van a utilizar.

<br/><br/>

### **_Persistencia de datos_** <br/>

Actualmente las aplicaciones almacenan sus datos en forma de archivos texto que se traspasan de un servidor a otro mediante una variedad de vías. Las formas de almacenamiento más comunes son:

- Carpetas compartidas en servidores Windows.
- Uso de FTP, SFTP y similares.
- Uso de almacenamiento local

Particularmente el uso de almacenamiento local en los servidores impide el escalamiento horizontal de las aplicaciones.

Para persistencia en general de datos transaccionales se utilizan sistemas de bases de datos relacionales en su mayoría alguna versión on-premise de SQL Server (2000, 2005, 2008, etc.) y en páginas web de marcas se utiliza MySQL Server.

<br/><br/>

### **_Repositorio de código fuente_** <br/>

Actualmente algunas aplicaciones tienen su código fuente en un servidor de Team Foundation Service (TFS) tecnología que el propio fabricante está dejando en favor de repositorios distribuidos Git

El resto de las aplicaciones se almacenan en carpetas en los servidores dedicados a la aplicación o en las computadoras personales de quienes participaron en el proyecto. Esto provoca que se tenga poco control y conocimiento de esta base de código fuente, incluso pudiera conducir a la pérdida por completo de este.

<br/><br/>

### **_Integración de código y despliegue de aplicaciones_** <br/>

Para las aplicaciones alojadas en el servidor de Team Foundation Services el código se actualiza actualmente de manera manual y se generan versiones de los cambios. Las aplicaciones no alojadas en algún repositorio de código fuente se actualizan directamente en la carpeta de la aplicación y las versiones se llevan mediante archivos zip o copias de las carpetas, en el peor de los casos solo se sobrescribe perdiendo así la historia de cambios y haciendo más complicado un regreso a una versión anterior

Los desarrollos en las plataformas SAP se realizan a través del procedimiento propietario de transportes entre ambientes con aprobaciones manuales

El resto de los desarrollos se despliegan actualmente de manera manual, sobre servidores destinados a soportar los ambientes lo cual es complejo e involucra a personal de varias áreas. Esto lleva a que no sean tan frecuentes los despliegues, los cambios potencialmente más grandes y por tanto con un mayor riesgo de introducir algún error en producción. Adicionalmente esta dependencia 1:1 entre servidores y aplicaciones evita la flexibilidad y el escalado de las aplicaciones

<br/><br/>

### **_Repositorio de componentes_** <br/>

Actualmente no existe un repositorio de componentes comunes que se puedan reutilizar en las diferentes aplicaciones por lo que se duplica esfuerzo y lógicas de negocio por cada aplicación. Solamente unas pocas aplicaciones de Enterprise X comparten componentes que evitan la reimplementación.

<br/><br/>

### **_Documentación_**

Actualmente de las aplicaciones que cuentan con documentación, esta se encuentra esparcida por diferentes repositorios o carpetas. No existe un repositorio central de documentación.

<br/><br/>

### **_Retroalimentación de clientes y usuarios finales_**

Actualmente la retroalimentación de usuarios finales se realiza a través de correo electrónico solamente en el caso donde ocurre un incidente en ambientes productivos o mediante requerimientos de clientes internos para mejoras o creación de aplicaciones.

Por otro lado, se liberan funcionalidades sin tener retroalimentación de los usuarios finales para conocer si es algo que realmente resuelve un problema que tengan o si aporta valor, lo cual puede llevar a una menor adopción de las aplicaciones.

<br/><br/>

### **_Plataforma de cómputo_** <br/>

<br/>

Como Plataforma de Cómputo se utiliza mayormente servidores virtualizados sobre tecnología VMWare ESX o en nube de Azure, y en menor medida servidores físicos o bare-metal. Se uiliza para sistemas "NO-SAP" o "Wintel" y otras aplicaciones

- Flujos de aprobación
- Sitios Web
- Microservicios
- Clúster SQL Server
- File Servers / FTP Servers
- Servicio de impresión

Como Plataforma de Cómputo para sistemas UNIX se utiliza servidores virtualizados sobre tecnología IBM Power (VIO Server). Esta plataforma brinda gran estabilidad y los CPU más potentes del mercado. Como desventaja tiene altos costos de hardware, software y mantenimientos, así como también alta complejidad de operación y poca compatibilidad con el universo de aplicaciones. Se utiliza para sistemas core como:

- SAP ERP
- SAP CRM
- SAP BW
- WMS

La mayoría de los servidores utilizan versiones de sistemas operativos con licencias para su uso (Windows, AIX, Suse Linux, RedHat Linux) y solamente una minoría (menos del 10%) utiliza sistemas operativos Open-Source. Esto debido entre otras cosas a los requerimientos de las cargas de trabajo y a que el proveedor de servicios administrados (X-Systems) tiene como requerimiento el soporte de fabricante para incluir un servidor en su línea base de soporte. La siguiente figura muestra una distribución aproximada:

![Distribución de servidores por sistema operativo](./img/os-chart.png "Distribución de servidores por sistema operativo")

La ventaja de esto es que ante un incidente se tiene el soporte de fabricante del sistema operativo con SLAs de alrededor de 2 horas de solución.

La desventaja de esto es que anualmente se paga una cantidad importante en costos de licenciemiento solo de los sistemas operativos y tiene consecuencias como límites al crecimiento o vulnerabilidades ante auditorías de los fabricantes.

<br/><br/>

### **_Almacenamiento_** <br/>

Acutalmente los datos se persisten en sistemas de almacenamiento alojados físicamente en los centros de datos on-premise. En el centro de datos Central se utiliza mayormente sistemas de almacenamiento tipo SAN basados en tecnología Flash de la marca IBM. En los centros de datos regionales (Plantas, Almacenes, Corporativos) se utilizan sistemas de almacenamiento de tipo DAS basados en tecnología de discos magnéticos de 7200 rpm.

<br/><br/>

### **_Despliegue de cargas de trabajo_** <br/>

En la actualidad el despliegue de cargas de trabajo se realiza de manera manual durante eventos de despliegue de nueva versión regidos por el proceso de Control de Cambios vigente (los cambios se anuncian con 1-2 semanas de anticipación). Por ejemplo:

- SAP ERP
- SAP CRM
- Flujos de aprobación
- WMS

Algunas cargas de trabajo se depliegan automáticamente a través de pipelines a sus ambientes de ejecución. Ejemplo de esto:

- SAP Commerce (SAP Cloud)
- App Portal Enterprise X
- Plataforma comercial (API y Web)
- Páginas de Marcas (X01, miniX, eXists,etc.)

<br/><br/>

### **_Uso de Nube_** <br/>

#### **_Nube Privada_** <br/>

Los sistemas más críticos se ejecutan principalmente sobre la nube privada de Enterprise X en el centro de datos Central en Ciudad X.

#### **_Nube Pública_** <br/>

Actualmente se tiene subscripción en las siguientes nubes:

- Azure: IaaS, Bases de datos (PaaS), Almacenamiento en nube, Sitios web, Datalake, Escritorios virtuales (VDI).
- Google Cloud (GCP): Pricipalmente para acceso a APIs de Google Maps, Google Abnalitycs y mensajes móviles tipo Push (Firebase).
- SAP Cloud (HEC): SAP Commerce y SAP CPI

#### **_Nube Híbrida_** <br/>

Algunas cargas de trabajo se corren en un esquema de nube híbrida donde participan el centro de datos central en Ciudad X y la nube de Azure. Esta forma tiene la ventaja de poder colocar las cargas de trabajos donce más convenga por criterios como cercanía a los datos o capacidad de enlace de salida o alta disponibilidad.

Este esquema es sobre todo utilizado por las aplicaciones con sistemas de registros en centro de datos central en Ciudad X y por aplicaciones de IoT que trabajen con equipos en sitios y oficinas de Enterprise X (Plantas, Almacenes, etc.). Por ejemplo:

- App Portal Enterprise X
- Plataforma comercial (API y Web)

<br/><br/>

### **_Conectividad_** <br/>

Internamente Enterprise X interconecta mediante su proveedor X-Networks sus sitios: Datacenters, Plantas, Almacenes, Oficinas de Venta, Corporativos, etc..., basada en enlaces MPLS de diferentes capacidades, entre 10 - 1000 MBps.

La conexión del datacenter principal en Ciudad X con la nube de Azure es a través de un enlace directo (Azure ExpressRoute) de 1000 MBps. Con los demás servicos de nubes, la comunicación se realiza a través de VPN Site-to-Site.

El acceso de los usurios fuera de sitios de Enterprise X a la red interna es a través de VPN Point-to-Site.

<br/><br/>

### **_Monitoreo_** <br/>

Actualmente el monitoreo de cargas de trabajo se realiza a través de los sistemas que tiene implementado el proveedor de servicios administrados (X-Systems). Este monitoreo se realiza sobre los principales recursos de los servidores (CPU, memoria RAM, espacio en disco, etc.) y sobre la disponibilidad de puertos de red específicos. Con esto se monitorea solamente una parte del escenario pues no incluye correlación con el desempeño de las aplicaciones o cargas de trabajo, datos que en caso de que se tengan son parte de sistemas distintos, lo cual hace más complejo tener una visión integral del estado de la infraestructura informática y el funcionamiento de las aplicaciones.

<br/><br/>

### **_Soporte_** <br/>

El soporte y atención a incidentes es proporcionado por el proveedor de servicios administrados (X-Systems). Los procesos están diseñados sobre las bases de metodologías de gestión como _ITIL_ o frameworks como _COBIT_. Este servicio de soporte a operación mantiene la estabilidad de manera robusta pero no es ágil.

El soporte está estructurado por equipos de especialistas en diferentes areas: Servidores Windows, Servidores Linux, Servidores AIX, Almacenamiento, SAP Basis, Middleware, etc. Esta organización tiene el inconveniente de que para la resolución de un incidente debe implicarse a varios equipos los cuales no trabajan de forma conjunta, sino más bien en silos lo cual hace que tarde la solución.

Los cambios están sujetos a un proceso donde los ordinarions por lo general toman entre 1 semana o 2 para su implementación y los cambios expeditos pueden ser ejecutados incluso el mismo día mediante un proceso adiconal de aprobaciones, estos últimos deben ser la excepción de la regla y estar bien fundamentados.

La atención de incidentes se basa en un sistema/proceso de tickets los cuales se mantienen abiertos durante todo el ciclo de resolución. El SLA crítco de es de 1 hora para respuesta y 2 para solución.

<br/><br/><br/><br/>

## Futuro

<br/><br/>

### **_Estilo de arquitectura de aplicaciones_** <br/>

Se recomienda principalmente el uso de Microservicios, que es un estilo de arquitectura de aplicaciones que divide una solución como una colección de servicios, lo cauales si se diseñan correctamente son:

- Altamente sustentables
- Fáciles de probar
- Levemente acoplados (Loosely coupled)
- Desplegados de forma independiente
- Organizados por dominios del negocio
- Atendidos por pequeños equipos

<br/>

![Microservicios](./img/microservice.png "Microservicios")

<br/>

Este estilo permite el despliegue rápido, frecuente y confiable de aplicaciones complejas y de gran tamaño. También permite a la Organización evolucionar su arquitectura tecnológica. Además, se va creando un ecosistema de servicios disponibles para su uso en aplicaciones futuras

<br/><br/>

### **_Estructura de aplicaciones_** <br/>

Para crear aplicaciones más flexibles, adaptables y resistentes a fallas se recomienda que separen en tres componentes:

- Frontend (Aplicaciones Web y móviles): Lógica de experiencia de usuarios y presentación de datos.
- Middleware (APIs o servicios web): Lógica de flujos de negocio.
- Backend (bases de datos y sistemas core): Persistencia de datos y flujos de negocio legacy.

<br/>

#### **_Frontend_** <br/>

Como premisa este tipo de componente de la estructura de aplicaciones encapsula toda la funcionalidad relacionada con la experiencia del usuario final y la presentación de datos. Depende totalmente de los sistemas de tipo middleware para su el acceso a datos almacenados en sistemas backend.

Están totalmente separados de los sistemas de tipo middleware y backend, por lo que se puede siempre desplegar en una plataforma de infraestructura diferente para mayor flexibilidad y escalabilidad.

Ejemplos de sistemas Frontend:

- Aplicación web
- Aplicación móvil
- Bot
- Paquetes, controles o componentes reutilizables.
- Presentación de datos, Dashboards

<br/>

#### **_Middleware_** <br/>

Como premisa este tipo de componente de la estructura de aplicaciones encapsula toda la lógica de negocio y abstrae a los sistemas de tipo frontend de la complejidad de los sistemas de backend. Es el punto de acceso a los datos almacenados en los sistemas de tipo backend.

Ejemplos de sistemas Middleware:

- API REST
- Serverless
- Sistema de mensajería ([Azure EventGrid](https://azure.microsoft.com/es-mx/services/event-grid/), [Azure Logic Apps](https://azure.microsoft.com/es-mx/services/logic-apps/), [SAP CPI](https://www.sap.com/latinamerica/products/integration-suite.html), etc ...)

<br/>

#### **_Backend_** <br/>

Como premisa este tipo de componente de la estructura de aplicaciones se encarga de la persistencia, lectura y manipulación final de los datos en general. No deben exponer interfaces directas a los sistemas de tipo frontend, sino a través de sistemas de tipo middleware.

Son sistemas críticos, por tanto, debe garantizarse la integridad y calidad de los datos almacenados.

Ejemplo de sistema Backend:

- Base de datos relacional o no relacional
- Data Lake
- SAP ERP
- SAP CRM

Para bases de datos relacional o no relacional se puede utilizar cualquier opción de la siguiente lista:

#### Bases relacionales (RDBMS)

| **Bases Relacionales**                                    | **Version** | **Vendor**                               |
| --------------------------------------------------------- | ----------- | ---------------------------------------- |
| [SQL Server](https://www.microsoft.com/es-mx/sql-server/) | 2016+       | [Microsoft](https://www.microsoft.com)   |
| [MySQL](https://www.mysql.com/)                           | 8+          | [Oracle](https://www.oracle.com)         |
| [PostgreSQL](https://www.postgresql.org/)                 | 13+         | [PosgreSQL](https://www.postgresql.org/) |

#### Bases no relacionales (NoSQL)

Una base de datos NoSQL (o "no relacional") proporciona un mecanismo para el almacenamiento y recuperación de datos que se modela en medios distintos a las relaciones tabulares utilizadas en las bases de datos relacionales. Estas bases de datos han existido desde finales de la década de 1960, pero no obtuvieron el apodo de "NoSQL" hasta que se popularizaron a principios del siglo XXI, impulsadas por las necesidades de la Web 2.0. Las bases de datos NoSQL se utilizan cada vez más en escenarios de Big Data y aplicaciones web en tiempo real [(NoSQL)](https://en.wikipedia.org/wiki/NoSQL)

| **Bases NoSQL**                                               | **Version**         | **Vendor**                             |
| ------------------------------------------------------------- | ------------------- | -------------------------------------- |
| [MongoDB](https://www.mongodb.com/)                           | 4+                  | [MogoDB](https://www.mongodb.com)      |
| [CosmosDB](https://docs.microsoft.com/en-us/azure/cosmos-db/) | N/A (Cloud Service) | [Microsoft](https://www.microsoft.com) |

<br/><br/>

### **_Construcción de aplicaciones_** <br/>

Para poder enfocar mejor los esfuerzos se recomienda concentrar el conocimiento en los siguientes lenguajes de programación.

| Lenguaje   |  Framework    |                  Función                  |
| ---------- | :-----------: | :---------------------------------------: |
| ABAP       |      SAP      |                BAPI, RFCs                 |
| C#         |   .Net 6.0+   | Web apps, API, Mobile Apps (iOS, Android) |
| Javascript |    NodeJs     |         Web apps, API, Serveless          |
| TypeScript | Angular/React |         Web apps, API, Serveless          |
| Python     |   Python 3+   |            Analítica de datos             |
| R          |       R       |            Analítica de datos             |

<br/>

Como herramienta para desarrollo se recomienda el uso de [Visual Studio Code](https://code.visualstudio.com/). El cual además de ser uno de los IDE más utilizados en la actualidad, es una herramienta Open-Source gratuita que no requiere inversión en licenciamiento y cuenta con un sistema de extensiones que hace que se pueda cubrir fácilmente diferentes tecnologías. Adicionalmente la interfaz es la base para el futuro [Github Codespaces](https://github.com/features/codespaces) que nos va a permitir desplegar ambientes de desarrollo en nube listos para usar desde cualquier dispositivo.

<br/><br/>

### **_Persistencia de datos_** <br/>

Como premisa se debe evitar el uso de:

- Carpetas compartidas en servidores Windows.
- Sitios FTP, SFTP y similares.
- Almacenamiento local, excepto para el caso de procesamiento temporal.

Se recomienda para persistencia de archivos a largo plazo.

- Servicios de nube (Azure Storage Account, OneDrive, SharePoint Online, etc ...)
- Data Lake

Para persistencia en general de datos transaccionales se recomienda uso de sistemas de bases de datos, tanto relacionales como no relacionales en dependencia de las necesidades de la aplicación.

<br/><br/>

### **_Repositorio de código fuente_** <br/>

Como repositorio central de código fuente se recomienda [Azure Repos](https://azure.microsoft.com/es-mx/services/devops/repos/) el cual es parte de la plataforma [Azure DevOps](https://azure.microsoft.com/es-mx/services/devops/) y está basado en [Git](https://git-scm.com/) que es el estándar mundial para repositorios de código distribuidos

<br/><br/>

### **_Integración de código y despliegue de aplicaciones_** <br/>

Como premisa la integración de código y despliegue de aplicaciones debe ocurrir de manera automática y como un proceso ágil que se puede reproducir cuantas veces se desee sin necesidad de movilizar personal para que ocurran.

Como plataforma para implementar estos procesos de integración y despliegue automatizado se recomienda [Azure Pipelines](https://azure.microsoft.com/es-es/services/devops/pipelines/) el cual es parte de la plataforma [Azure DevOps](https://azure.microsoft.com/es-mx/services/devops/) y cuenta con integración a la mayoría de plataformas de nube e infraestructura, además soporta una amplia variedad de tecnologías y cuenta con la posibilidad de extender la funcionalidad en caso que se necesite.

Para el encapsulamiento de aplicaciones se recomienda el uso de contenedores para que nos permitan desplegar la aplicación de manera escalable y flexible en cualquier tipo de infraestructura: Nube privada, púbñica o híbrida. Recomendamos el uso de [Docker](https://www.docker.com/) para la creación de estos contenedores
<br/><br/>

### **_Repositorio de componentes_** <br/>

Para evitar la duplicación de desarrollos y potenciar la reusabilidad y la estandarización de flujos de negocio se propone como premisa encapsular la lógica funcional desarrollada en componentes o paquetes y publicarlos para que esté disponibles para el uso de toda la Organización.

Ejemplo de funcionalidades que se pueden convertir en componentes:

- Login
- UI general de las aplicaciones
- Sistema de logs
- Controles como tablas, botones, filtros de datos, etc.
- Vías de acceder a datos

Como repositorio de componentes se recomienda el uso de [Azure Artifacts](https://azure.microsoft.com/es-es/services/devops/artifacts/) el cual es parte de la plataforma [Azure DevOps](https://azure.microsoft.com/es-mx/services/devops/) y soporta las tecnologías más importantes de manejo de paquetes (componentes): [Nuget](https://www.nuget.org/), [NPM](https://www.npmjs.com/), [Maven](https://maven.apache.org/index.html), etc.

<br/><br/>

### **_Documentación_**

Se recomienda el uso de un repositorio de documentación centralizado y accesible para toda la Organización y un adicionalmente se debe implementar un catálogo de aplicaciones de preferencia enlazado o como parte del repositorio de código.

Para esto proponemos el uso de la heramienta [Backstage](https://backstage.io) que permite el uso de pácticas de "Doc-as-Code" y mantener actualizada de forma automática la documentación de las aplicaciones.

<br/><br/>

### **_Retroalimentación de clientes y usuarios finales_**

Se recomienda la implementación de una plataforma para obtener feedback que se pueda incorporar en las demás aplicaciones en forma de widget o componente y así facilitar a los usuarios finales proponer nuevas ideas, expresar su opinión y mejorar las soluciones.

Adicionalmente se recomienda realizar los despliegues de nuevas versiones utilizando técnicas de [A/B testing](https://es.wikipedia.org/wiki/Prueba_A/B) para determinar lo que realmente tiene valor para los usuarios finales

<br/><br/>

### **_Plataforma de cómputo_** <br/>

<br/>

Como Plataforma de Cómputo para sistemas x86 se recomienda el uso de pools de servidores x86 virtuales, con sistemas de manejo de contenedores como [Docker](https://www.docker.com/), [Containerd](https://containerd.io/) o [CRI-O](https://cri-o.io/) y orquestados de manera autónama por algún sistema como [Kubenetes](https://kubernetes.io/). Esto con el fin de maximizar las capacidades de hardware y romper la tradicional dependencia 1:1 que existe entre aplicaciones y servidores asignados (pet servers) . La ventaja directa de esto es la disminución del número total de servidores, lo que incide directamente en el costo total de operacón. El esquema general de este escenario se representa en la siguiente figura: <br/><br/>


![Esquema aplicaciones sobre contenedors](./img/kubernetes.jpg "Esquema aplicaciones sobre contenedors")

<br/><br/>

Este esquema lo están siguiendo empresas como SAP cuya arquitectura actual se basa en desplegar cargas de trabajo en nube pública sobre servidores x86 con sistemas operativos Open-Source los cuales tienen instalados Docker y Kubernetes. Ejemplo de esto es la plataforma de SAP Commerce.

Sería factible revisar la posibilidad de migrar las cargas de trabajo de tipo core de AIX a x86 con contenedors y kubernetes. Teniendo en cuenta que esta es el stack tecnológico que usa el fabricante de estos sistemas (SAP) para brindar servicios de tipo Sofware-as-a-Service (SaaS) y Platform-as-a-Service (PaaS) a sus clientes. Este movimiento pudiera generar ahorros y aumentar la flexibilidad de los ambientes.

Se recomienda la migración paulatina de sistemas operativos con licenciamiento a sistemas operativos como [Ubuntu Server 2004 (LTS)](https://ubuntu.com/server) para minizar el costos de licencias y mantenimiento. Este fabricante brinda planes de soporte flexibles para esquemas On-Premise y Nube. Con esto se estaría cumpliendo tambíen con el requisito del proveedor de Servicios Administrados (X-Systems).

<br/><br/>

### **_Almacenamiento_** <br/>

Para servicios almacenamiento en general se recomiendo el uso de servicos PaaS en nube como [Azure Blob Storage](https://azure.microsoft.com/es-mx/services/storage/blobs/), [Azure Data Lake](https://azure.microsoft.com/es-mx/services/storage/data-lake-storage/), [OneDrive](https://www.microsoft.com/es-es/microsoft-365/onedrive/onedrive-for-business) o [Azure Files](https://azure.microsoft.com/es-mx/services/storage/files/).

Para las cargas de trabajo que se deben seguir ejecutando en los centros de datos on-premise se recomienda el uso de sistemas de almacenamiento virtualizados tipo SAN basados en tecnología Flash. Esto garantiza la baja latencia de respuesta y maximizar las capacidades al multiplica por al menos por tres el tamaño real de disco adquirido.

Para cargas de trabajo que se deban ejecutar en on-premise sitios remotos se recomienda el uso de una mezcla entre discos tipo Flash/SSD y disco mecánicos para alcanzar la mejor relación redimiento-costos.

<br/><br/>

### **_Despliegue de cargas de trabajo_** <br/>

Se recomienda continuar mejorando los proceso de depliegue automático a través de pipelines a sus ambientes de ejecución de las aplicaiones que ya lo realiza.

Adicionalmente se debe contar con una manera de desplegar infraestructura de manera automática y on-demmand utilizando herrmientas como [Ansible](https://www.ansible.com/) o [Terraform](https://www.terraform.io/). De esta manera se puede llevar un control de versiones aplicando prácticas de [Infraestructura como Código](https://es.wikipedia.org/wiki/Infraestructura_como_c%C3%B3digo) (IaC) y automatizar los procesos de operaciones de TI.

<br/><br/>

### **_Uso de Nube_** <br/>

#### **_Nube Privada_** <br/>

Se recomienda su uso para:

- Cargas críticas que por razones legales o de cumplimiento no puedan ejecutarse en una nube pública como AWS, Azure o Google Cloud.
- Cargas que tengan que ejecutarse en sitios remotos IoT, control de maquinaria, etc

No se recomienda su uso para cualquier otro tipo de cargas de trabajo.

#### **_Nube Pública_** <br/>

Se recomienda su uso para sistemas que requieran alta disponibilidad, alta trasferencia de volumenes de datos, escalar con facilidad o poder desplegarse de manera global en diferentes geografías (**Recomendada**). Se espera que en esta modalidad se encuentren la mayoría de las cargas de trabajo.

#### **_Nube Híbrida_** <br/>

Se recomienda su uso para sistema que requieran acceder de manera directa y segura a sistemas y datos que se estén ejecutando en nube privada. Esto permite tener las ventajas de la nube privada y la nube pública.

En general se recomienda como estrategia disminuir la inversión en compra de hardware para nube privada que puede quedar obsoleto en algunos años (3-5 años generalmente) y aumentar la inversión en nube pública y nube híbrida. Esto con el fin de potenciar una operación de TI global en Enterprise X. También empezar a aplicar técnicas de multinube para no depender de un único proveedor Cloud.

<br/><br/>

### **_Conectividad_** <br/>

Es necesario evolucionar de una arquitectura basada en silos por unidad de negocio a una arquitectura globalmente distribuida que nos permita desplegar una misma aplicación en diferentes geografías. Para esto se recomienda:

- Crear un centro de datos en nube en la regiones cercanas a los sitios de Enterprise X

- Agregar conexiones de ExpressRoute hasta las regiones cercanas a los sitios de Enterprise X

- Establecer conexiones Site-to-Site entre todos los centros de datos en nube.
  <br/><br/>

### **_Monitoreo_** <br/>

Se recomienda el uso de plataformas que monitorean desde los servidores que ejecutan las cargas de trabajo hasta las acciones que realizan los usuarios con las aplicaciones. Se recomiendan herramientas como [Dynatrace](https://www.dynatrace.com/) o [Application Insights](https://docs.microsoft.com/en-us/azure/azure-monitor/app/app-insights-overview)/[Azure Monitor](https://azure.microsoft.com/es-mx/services/monitor/)

Las alarmas deben integrase de manera automática con la plataforma de manejo de incidentes que se utiliza en Enterprise X.

<br/><br/>

### **_Soporte_** <br/>

Se recomienda buscar con el proveedor de Servicios administrados maneras menos rígidas que se adapten mejor a las prácticas de agilidad. Con esquemas basados en métricas diferentes a las actuales, por ejemplo en cuanto "requests" o "page views" de los sistemas soportados en lugar de cantidad de servidores. Buscar un equipo de soporte versátil que puede atender un caso de extremo a extremo sin necesidad de hacer pases entre torres.

<br/><br/><br/>

## **_Conclusión_**

En conclusión, el entorno actual, volátil, incierto, complejo y ambiguo está llevando a las organizaciones a que sean más ágiles, felxibles y resistentes a fallas. A continuación algunas de las razones principales para migrar a la nube:

- Mejorar la respuesta al mercado actual
- Reducir costos de operación
- Mejorar la escalabilidad
- Mejorar la seguridad
- Mejorar del rendimiento de las aplicaciones

En conjunto, esto permite a las organizaciones equilibrar la estabilidad, el dinamismo y prosperar. Pero al final depende de las organizaciones si quieren ser “organismos vivos” o “máquinas”, al cabo “… el cambio no es necesario, la supervivencia es opcional …”

