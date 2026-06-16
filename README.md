# ComercioTech — Implementación segura de Ubuntu Server y MongoDB

> Proyecto académico de **Bases de Datos No Estructuradas (TI3032)** orientado al diseño e implementación de una solución **NoSQL documental, segura y escalable** para la empresa **ComercioTech**.

## Descripción general

Este proyecto aborda el diagnóstico inicial, la selección del entorno técnico y la implementación segura de una base de datos para **ComercioTech**, una empresa en crecimiento que necesita administrar **clientes, productos y pedidos** de forma robusta, flexible y escalable.

La solución propuesta se basa en:

- **DBMS:** MongoDB Community Server
- **Sistema operativo:** Ubuntu Server
- **Virtualización:** Oracle VM VirtualBox
- **Enfoque:** solución NoSQL documental, segura y escalable

---

## Objetivo del proyecto

Configurar un entorno virtualizado con Ubuntu Server y MongoDB para **ComercioTech**, aplicando medidas de seguridad y dejando evidencia técnica de los procedimientos más relevantes.

---

## Contexto del caso

**ComercioTech** se encuentra en una etapa de expansión y requiere modernizar su infraestructura de datos. El sistema debe permitir:

- Registrar, consultar, actualizar y eliminar clientes.
- Administrar productos, categorías, precios y stock.
- Crear pedidos con uno o más productos asociados.
- Mantener seguridad, rendimiento y disponibilidad.
- Escalar sin rediseñar completamente la base de datos.

Además, el proyecto considera un crecimiento progresivo del negocio y la necesidad de soportar datos actuales e históricos.

---

## Avance 1 — Diagnóstico, requisitos y selección de entorno

### 1) Necesidades del negocio

El análisis inicial identifica que la empresa trabaja con una base de datos heredada que ya no responde bien al crecimiento del negocio. Esto genera riesgos de lentitud, dificultades para incorporar nuevas funciones y problemas al manejar mayores volúmenes de clientes y transacciones.

### 2) Volumen de datos estimado

Se definieron proyecciones realistas para dimensionar la solución:

| Elemento | Situación actual estimada | Proyección año 1 | Proyección año 2 |
|---|---:|---:|---:|
| Clientes | 5.000 | 6.500 | 8.450 |
| Productos | 2.000 | 2.600 | 3.380 |
| Pedidos mensuales | 10.000 | 13.000 | 16.900 |
| Crecimiento usado | Base actual | 30% anual | 30% anual |

Estas proyecciones justifican el uso de una base de datos flexible y preparada para incrementar carga de trabajo.

### 3) Requisitos de rendimiento, escalabilidad y disponibilidad

#### Rendimiento
Se requiere optimizar consultas frecuentes mediante índices en campos relevantes como correo del cliente, nombre o categoría del producto, identificador del cliente y fecha del pedido.

#### Escalabilidad
El proyecto considera inicialmente escalabilidad vertical, aumentando RAM, CPU y almacenamiento de la máquina virtual. Como evolución futura, podría evaluarse replicación o particionamiento.

#### Disponibilidad
Se contemplan medidas como inicio automático del servicio, respaldos periódicos y monitoreo del estado del servidor.

### 4) Requisitos de seguridad y cumplimiento

La solución debe proteger datos sensibles de clientes, incluyendo nombres, correos, teléfonos y direcciones. Por ello se definieron controles como:

- Autenticación con usuarios y contraseñas.
- Autorización basada en roles.
- Restricción de red mediante `bindIp` y firewall.
- Uso de contraseñas robustas.
- Respaldos periódicos.
- Revisión de logs y auditoría básica.

### 5) Requisitos técnicos de MongoDB

Se estableció como base técnica:

| Elemento | Recomendación |
|---|---|
| Sistema operativo | Ubuntu Server 22.04 LTS / posteriormente implementado sobre Ubuntu 24.04.4 LTS |
| Procesador | 2 vCPU mínimo |
| Memoria RAM | 4 GB mínimo |
| Almacenamiento | 40 GB mínimo |
| Red | NAT o puente |
| Software adicional | MongoDB Shell, herramientas de respaldo y Python/PyMongo |
| Seguridad | Autenticación, roles, control de red y respaldos |

### 6) Comparación de sistemas operativos

Se compararon **Ubuntu Server**, **Rocky Linux** y **Windows Server** considerando compatibilidad, rendimiento, administración, seguridad y costo. La alternativa seleccionada fue **Ubuntu Server**, por su compatibilidad con MongoDB, bajo consumo de recursos, facilidad de administración y ausencia de coste de licencia.

### 7) Justificación técnica del sistema operativo seleccionado

Se seleccionó **Ubuntu Server** por ser una distribución estable, gratuita, ampliamente utilizada en servidores y adecuada para un entorno académico y técnico. También facilita la documentación del procedimiento mediante comandos claros.

### 8) Selección de plataforma de virtualización

La plataforma elegida fue **Oracle VM VirtualBox**, por ser gratuita, simple de usar y suficiente para instalar Ubuntu Server dentro de una máquina virtual con recursos configurables.

Configuración propuesta para la VM:

| Parámetro | Valor recomendado |
|---|---|
| Nombre de la VM | `ComercioTech-DB` |
| Sistema operativo invitado | Ubuntu Server |
| CPU | 2 núcleos |
| Memoria RAM | 4096 MB mínimo |
| Disco | 40 GB dinámico |
| Red | NAT o puente |
| Usuario administrador | `admincomerciotech` |

---

## Avance 2 — Implementación segura de Ubuntu Server y MongoDB

### Resumen de la implementación

La implementación real quedó construida con la siguiente configuración:

| Componente | Configuración implementada |
|---|---|
| Máquina virtual | 2 CPU, 4 GB RAM, disco VDI dinámico de 40 GB y red NAT |
| Servidor | Ubuntu Server 24.04.4 LTS |
| Hostname | `comerciotech-db` |
| Red | Interfaz `enp0s3` con IP `10.0.2.15/24` |
| Base de datos | MongoDB Community Server 8.0.26 |
| Acceso MongoDB | Local mediante `127.0.0.1:27017` |
| Seguridad del servidor | UFW activo, OpenSSH habilitado y usuarios separados |
| Control de acceso | Roles `admin`, `readWrite` y `read` |
| Continuidad | Logs revisados, respaldo con `mongodump` y copia cifrada con GPG |

### 1) Implementación del sistema operativo

Durante la instalación se utilizó Ubuntu Server **24.04.4 LTS de 64 bits**. Aunque inicialmente se había considerado Ubuntu Server 22.04 LTS, el entorno implementado terminó usando **Ubuntu 24.04.4 LTS**, y el informe técnico se ajustó a esa versión final.

### 2) Recursos asignados a la máquina virtual

| Recurso | Configuración |
|---|---|
| Procesador | 2 CPU virtuales |
| Memoria RAM | 4 GB |
| Almacenamiento | 40 GB VDI dinámico |
| Arquitectura | x86_64 |
| Particionado | LVM |

Esta configuración fue suficiente para instalación, pruebas, operaciones CRUD académicas, revisión de logs y respaldo de la base de datos.

### 3) Red y exposición controlada

La máquina virtual se configuró con adaptador **NAT**, lo que permite acceso a Internet para descargar paquetes sin exponer directamente el servidor a toda la red local. MongoDB quedó limitado a acceso local mediante `127.0.0.1:27017`.

### 4) Hardening del servidor Ubuntu

Se aplicaron varias medidas de endurecimiento del sistema:

- Uso de usuario con privilegios **sudo** en lugar de trabajar como `root`.
- Creación de una cuenta adicional de soporte.
- Activación de **UFW** con política de entrada denegada por defecto.
- Habilitación de **OpenSSH** como acceso remoto controlado.
- Mantenimiento del puerto de MongoDB sin exposición externa.

### 5) Instalación de MongoDB

MongoDB se instaló desde el repositorio oficial para Ubuntu 24.04 y quedó operativo como servicio del sistema.

Versiones verificadas:

- **MongoDB Server:** 8.0.26
- **MongoDB Shell (mongosh):** 2.8.3

### 6) Autenticación, roles y permisos

Se habilitó el control de acceso en MongoDB y se definieron cuentas separadas según función:

| Usuario | Rol | Uso definido |
|---|---|---|
| `adminComercioTech` | Administración | Gestión de usuarios, bases y configuración |
| `appComercioTech` | `readWrite` | Cuenta de aplicación para leer y modificar datos |
| `auditorComercioTech` | `read` | Cuenta de revisión con acceso de solo lectura |

Esta separación implementa el principio de **mínimo privilegio** y evita otorgar permisos completos a todas las cuentas.

### 7) Política de acceso

La política aplicada establece que:

- Cada administrador o servicio debe usar una cuenta propia.
- Las credenciales no deben compartirse.
- Las contraseñas deben ser robustas y no quedar escritas en el código fuente.
- MongoDB debe mantenerse en localhost o en IP autorizadas.
- Los logs y las cuentas deben revisarse periódicamente.
- Los respaldos deben protegerse y comprobarse antes de recuperar información.

### 8) Cifrado, auditoría y continuidad

En este avance, el cifrado se aplicó al respaldo de la base de datos:

1. Se generó una copia con `mongodump`.
2. Se comprimió el respaldo.
3. Se creó una copia cifrada con **GPG**.

Además, se revisaron los registros de Ubuntu y MongoDB para comprobar eventos del servicio, conexiones, desconexiones, checkpoints y posibles errores. Como mejora futura para un entorno productivo, se menciona la conveniencia de implementar **TLS**, automatización de respaldos, monitoreo continuo, replicación y pruebas periódicas de restauración.

---

## Principales avances logrados

- Se definió una arquitectura técnica coherente con el caso de estudio.
- Se justificó el uso de una base de datos NoSQL documental.
- Se seleccionó Ubuntu Server como sistema operativo base.
- Se implementó una máquina virtual funcional en VirtualBox.
- Se instaló y verificó MongoDB Community Server 8.0.26.
- Se habilitó autenticación y control de acceso por roles.
- Se aplicaron medidas de hardening del sistema y de la base de datos.
- Se revisaron logs del servicio y se generó un respaldo cifrado.
- Se documentaron procedimientos y evidencias relevantes del proceso.

---

## Tecnologías utilizadas

- **Ubuntu Server 24.04.4 LTS**
- **MongoDB Community Server 8.0.26**
- **MongoDB Shell (mongosh) 2.8.3**
- **Oracle VM VirtualBox**
- **UFW**
- **OpenSSH**
- **GPG**
- **mongodump**

---

## Próximos pasos sugeridos

Con el entorno preparado, el proyecto queda listo para continuar con tareas como:

- Creación definitiva de la base de datos operativa.
- Desarrollo del CRUD para clientes, productos y pedidos.
- Automatización de respaldos.
- Pruebas de restauración.
- Implementación de TLS en un entorno más cercano a producción.
- Monitoreo continuo y posible replicación.

---

## Conclusión

El proyecto permitió construir una base técnica sólida para **ComercioTech** mediante herramientas gratuitas, compatibles y adecuadas para un entorno académico. La combinación de **Ubuntu Server**, **MongoDB** y **VirtualBox** permitió implementar una solución funcional con medidas básicas de seguridad, separación de roles, control de acceso, respaldo y documentación.

En conjunto, los avances demuestran una evolución desde el diagnóstico y la selección de entorno hasta una implementación segura y verificable, alineada con las necesidades del caso.
