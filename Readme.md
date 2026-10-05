 # Actividad #1: Sistema de Control de Rescate y Adopcion Animal

## Informacion academica

**Asignatura:** Desarrollo de Sistemas Informaticos  
**Unidad:** Unidad 1 - Desarrollo de software, introduccion y paradigmas de diseno  
**Institucion:** Universidad Tecnica de Manabi  
**Disponibilidad:** 22/09/2026 al 17/10/2026  
**Fecha limite:** 17/10/2026  
**Ponderacion:** 10 puntos

## Descripcion

Este proyecto propone la arquitectura logica de un sistema para gestionar el rescate, atencion veterinaria y adopcion responsable de animales. La plataforma permite registrar casos de rescate, identificar animales, asignar responsables, controlar tratamientos, administrar solicitudes de adopcion y realizar seguimientos posteriores.

El diseno contempla perros, gatos y fauna silvestre mediante la entidad `especies`, por lo que puede crecer sin modificar la estructura principal del sistema.

## Objetivo

Disenar una solucion escalable, reutilizable y mantenible que aplique patrones de diseno de software, principios SOLID y un flujo profesional de trabajo con Gitflow.

## Problema identificado

Los refugios necesitan centralizar la informacion de los animales y controlar cada etapa de su atencion. Cuando los reportes, asignaciones, expedientes veterinarios y adopciones se manejan de forma separada, se dificulta conocer el estado real de cada caso y mantener la trazabilidad de las decisiones.

Este sistema organiza el proceso completo:

1. Un ciudadano reporta un animal en situacion de riesgo.
2. El administrador valida y asigna el caso a un rescatista.
3. El rescatista registra el rescate y traslada el animal a un refugio.
4. El veterinario crea el expediente y registra tratamientos.
5. El animal se publica cuando esta disponible para adopcion.
6. Un adoptante envia una solicitud que es evaluada por el refugio.
7. Si la solicitud es aprobada, se registra la adopcion y se realizan seguimientos.

## Actores principales

| Actor | Responsabilidad |
| --- | --- |
| Ciudadano o reportante | Informa un posible caso de rescate y consulta su estado. |
| Rescatista o voluntario | Atiende el caso, registra el rescate y apoya el traslado. |
| Veterinario | Registra el expediente, diagnostico, estado de salud y tratamientos. |
| Administrador del refugio | Gestiona usuarios, casos, refugios, solicitudes y adopciones. |
| Responsable de refugio | Registra el ingreso, cuidado y disponibilidad de los animales. |
| Adoptante | Consulta animales y envia una solicitud de adopcion. |

## Requerimientos funcionales

- Registrar usuarios, roles y estados de usuario.
- Crear casos de rescate con ubicacion, descripcion y nivel de urgencia.
- Asignar un rescatista o voluntario a cada caso.
- Registrar animales y clasificarlos por especie.
- Registrar el rescate y el refugio de destino.
- Crear expedientes veterinarios.
- Registrar tratamientos, vacunas, esterilizacion y estado de salud.
- Publicar animales disponibles para adopcion.
- Recibir y evaluar solicitudes de adopcion.
- Formalizar adopciones y registrar el contrato.
- Programar y documentar seguimientos posteriores.
- Enviar notificaciones a los usuarios involucrados.

## Requerimientos no funcionales

- **Escalabilidad:** permitir nuevas especies, estados, refugios y tipos de usuario.
- **Mantenibilidad:** separar responsabilidades mediante MVC y principios SOLID.
- **Trazabilidad:** registrar estados, fechas, responsables y observaciones.
- **Reutilizacion:** aplicar patrones de diseno para evitar codigo repetido.
- **Disponibilidad:** conservar la informacion de los casos y expedientes en una base centralizada.
- **Seguridad:** controlar el acceso de acuerdo con el rol del usuario.
- **Usabilidad:** mostrar un flujo claro para reportar, rescatar y adoptar.

## Patrones de diseno

### Singleton

Se propone una instancia unica para el gestor central de casos y la conexion a datos. Esto evita crear varias conexiones innecesarias y permite coordinar el acceso a la informacion del sistema.

**Aplicacion:** `GestorCasos` o `ConexionBD`.  
**Problema que resuelve:** controlar un recurso compartido y garantizar un unico punto de acceso.

### Observer

Se utiliza para notificar automaticamente a rescatistas, veterinarios, administradores y adoptantes cuando cambia el estado de un caso, se asigna una tarea o se aprueba una solicitud.

**Aplicacion:** `Notificacion` relacionada con `casos_rescate`, `adopciones` y `seguimientos_adopcion`.  
**Problema que resuelve:** desacoplar la actualizacion de los usuarios de la logica que modifica un caso.

### Factory Method

Se propone una fabrica de expedientes que crea el tipo de expediente adecuado segun la especie: perro, gato o fauna silvestre.

**Aplicacion:** `ExpedienteFactory`, `ExpedientePerro`, `ExpedienteGato` y `ExpedienteFaunaSilvestre`.  
**Problema que resuelve:** centralizar la creacion de objetos y facilitar la incorporacion de nuevas especies.

### MVC

El sistema se organiza en tres capas:

- **Modelo:** entidades como `Animal`, `CasoRescate`, `ExpedienteVeterinario`, `SolicitudAdopcion` y `Adopcion`.
- **Vista:** formularios y pantallas para reportes, gestion veterinaria, refugios y adopciones.
- **Controlador:** coordina las acciones del usuario y aplica las reglas del proceso.

**Problema que resuelve:** separar la interfaz, la logica de negocio y los datos para facilitar las pruebas y el mantenimiento.

## Arquitectura propuesta

```text
Vista
	|
	v
Controladores
	|
	v
Servicios y reglas de negocio
	|
	+--> Gestor de notificaciones (Observer)
	+--> Fabrica de expedientes (Factory Method)
	+--> Gestor central y conexion (Singleton)
	|
	v
Modelo y repositorios
	|
	v
Base de datos
```

## Modelo de datos

La base de datos se encuentra en [db.dbml](db.dbml). Sus entidades principales son:

- `usuarios`, `roles_usuario` y `estados_usuario`.
- `animales`, `especies` y `estados_animal`.
- `casos_rescate`, `rescates` y `refugios`.
- `expedientes_veterinarios` y `tratamientos`.
- `solicitudes_adopcion`, `adopciones` y `seguimientos_adopcion`.
- `notificaciones`.

Las relaciones `Ref` del archivo DBML representan las claves foraneas entre estas entidades.

## Diagramas UML

Los diagramas se encuentran en [uml.md](uml.md) e incluyen:

- Diagrama de casos de uso.
- Diagrama de clases con atributos, operaciones y relaciones.
- Diagrama de secuencia del reporte y rescate.
- Diagrama de actividad del proceso de adopcion.
- Diagrama de estados del animal.

El flujo general del sistema se encuentra en [lucid.md](lucid.md).

## Flujo de trabajo Gitflow

El repositorio debe utilizar las siguientes ramas:

| Rama | Uso |
| --- | --- |
| `master` | Version estable y entregable del proyecto. |
| `develop` | Integracion de los avances aprobados. |
| `feature/nombre-funcionalidad` | Desarrollo aislado de cada funcionalidad. |

Flujo recomendado:

```bash
git checkout master
git pull origin master
git checkout -b develop
git push -u origin develop

git checkout develop
git checkout -b feature/modelo-datos
git add .
git commit -m "feat: agregar modelo de datos del sistema"
git push -u origin feature/modelo-datos
```

Al finalizar una funcionalidad, se crea un Pull Request desde la rama `feature` hacia `develop`. Cuando el sistema esta revisado y listo para entregar, `develop` se integra en `master`.

## Repositorio y evidencias

**Repositorio de GitHub:** [Agregar aqui el enlace publico del repositorio]

Se deben agregar como evidencias:

- Captura de las ramas `master`, `develop` y `feature`.
- Captura del historial de commits.
- Captura de los Pull Requests o fusiones realizadas.
- Captura de los diagramas UML.

## Estructura del proyecto

```text
.
|-- db.dbml       # Modelo de base de datos
|-- lucid.md      # Diagrama de flujo general
|-- uml.md        # Diagramas UML del sistema
|-- Readme.md     # Documentacion de la actividad
```

## Conclusion

El sistema propuesto cubre el ciclo completo de atencion animal, desde el reporte de un rescate hasta el seguimiento despues de la adopcion. La separacion por responsabilidades, el uso de patrones de diseno y la estrategia Gitflow permiten construir una solucion organizada, extensible y facil de mantener.
