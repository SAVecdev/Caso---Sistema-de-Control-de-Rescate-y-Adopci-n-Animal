# UML - Sistema de Control de Rescate y Adopcion Animal

## 1. Diagrama de casos de uso

```mermaid
flowchart LR
    ciudadano[Ciudadano]
    rescatista[Rescatista]
    veterinario[Veterinario]
    administrador[Administrador]
    adoptante[Adoptante]
    refugio[Responsable de refugio]

    subgraph sistema[Sistema de Control de Rescate y Adopcion Animal]
        UC1((Reportar caso de rescate))
        UC2((Consultar estado del caso))
        UC3((Asignar rescatista))
        UC4((Gestionar rescate))
        UC5((Registrar animal))
        UC6((Gestionar refugio))
        UC7((Crear expediente veterinario))
        UC8((Registrar tratamiento))
        UC9((Publicar animal disponible))
        UC10((Enviar solicitud de adopcion))
        UC11((Evaluar solicitud))
        UC12((Registrar adopcion))
        UC13((Realizar seguimiento))
        UC14((Enviar notificaciones))
    end

    ciudadano --> UC1
    ciudadano --> UC2
    rescatista --> UC4
    rescatista --> UC5
    administrador --> UC3
    administrador --> UC6
    administrador --> UC9
    administrador --> UC11
    administrador --> UC12
    veterinario --> UC7
    veterinario --> UC8
    adoptante --> UC10
    adoptante --> UC2
    refugio --> UC6
    refugio --> UC13
    UC1 -.->|incluye| UC14
    UC3 -.->|incluye| UC14
    UC12 -.->|incluye| UC14
```

## 2. Diagrama de clases del dominio

```mermaid
classDiagram
    class RolUsuario {
        +int id_rol
        +string nombre_rol
    }

    class EstadoUsuario {
        +int id_estado
        +string nombre_estado
    }

    class Usuario {
        +int id_usuario
        +int id_rol
        +int id_estado
        +string nombres
        +string apellidos
        +string telefono
        +string email
        +string direccion
        +datetime fecha_registro
        +reportarCaso()
        +consultarCaso()
    }

    class Especie {
        +int id_especie
        +string nombre_especie
    }

    class EstadoAnimal {
        +int id_estado
        +string nombre_estado
    }

    class Animal {
        +int id_animal
        +int id_especie
        +int id_estado
        +string nombre
        +string sexo
        +int edad_aproximada
        +string raza
        +string color
        +string tamano
        +string descripcion
        +datetime fecha_ingreso
        +actualizarEstado()
        +marcarDisponible()
    }

    class Refugio {
        +int id_refugio
        +int id_usuario_responsable
        +string nombre_refugio
        +string direccion
        +string telefono
        +int capacidad
        +boolean activo
        +recibirAnimal()
        +consultarCapacidad()
    }

    class EstadoCasoRescate {
        +int id_estado
        +string nombre_estado
    }

    class CasoRescate {
        +int id_caso
        +int id_usuario_reporta
        +int id_usuario_asignado
        +int id_estado
        +int id_animal
        +string ubicacion
        +decimal latitud
        +decimal longitud
        +string descripcion
        +int nivel_urgencia
        +datetime fecha_reporte
        +datetime fecha_cierre
        +registrarCaso()
        +asignarRescatista()
        +cerrarCaso()
    }

    class Rescate {
        +int id_rescate
        +int id_caso
        +int id_refugio
        +int id_usuario_rescatista
        +datetime fecha_rescate
        +string observaciones
        +ejecutarRescate()
        +trasladarARefugio()
    }

    class ExpedienteVeterinario {
        +int id_expediente
        +int id_animal
        +int id_usuario_veterinario
        +decimal peso
        +string estado_salud
        +boolean vacunado
        +boolean esterilizado
        +string observaciones
        +datetime fecha_revision
        +registrarRevision()
        +actualizarSalud()
    }

    class Tratamiento {
        +int id_tratamiento
        +int id_expediente
        +string descripcion
        +datetime fecha_inicio
        +datetime fecha_fin
        +string estado
        +iniciarTratamiento()
        +finalizarTratamiento()
    }

    class EstadoSolicitud {
        +int id_estado
        +string nombre_estado
    }

    class SolicitudAdopcion {
        +int id_solicitud
        +int id_animal
        +int id_usuario_solicitante
        +int id_estado
        +datetime fecha_solicitud
        +string motivo
        +string observaciones
        +enviarSolicitud()
        +aprobar()
        +rechazar()
    }

    class Adopcion {
        +int id_adopcion
        +int id_solicitud
        +int id_usuario_aprobador
        +datetime fecha_adopcion
        +datetime fecha_seguimiento
        +boolean contrato_firmado
        +string observaciones
        +formalizar()
        +programarSeguimiento()
    }

    class SeguimientoAdopcion {
        +int id_seguimiento
        +int id_adopcion
        +int id_usuario_responsable
        +datetime fecha_seguimiento
        +string estado_animal
        +string observaciones
        +registrarVisita()
        +evaluarAdopcion()
    }

    class Notificacion {
        +int id_notificacion
        +int id_usuario
        +string tipo_notificacion
        +string titulo
        +string contenido
        +datetime fecha_emision
        +boolean leido
        +marcarComoLeida()
    }

    RolUsuario "1" --> "0..*" Usuario : asigna
    EstadoUsuario "1" --> "0..*" Usuario : clasifica
    Especie "1" --> "0..*" Animal : pertenece
    EstadoAnimal "1" --> "0..*" Animal : define
    Usuario "1" --> "0..*" Refugio : administra
    Usuario "1" --> "0..*" CasoRescate : reporta
    Usuario "0..1" --> "0..*" CasoRescate : atiende
    EstadoCasoRescate "1" --> "0..*" CasoRescate : define
    Animal "0..1" --> "0..*" CasoRescate : asociado
    CasoRescate "1" --> "0..1" Rescate : genera
    Usuario "1" --> "0..*" Rescate : ejecuta
    Refugio "0..1" --> "0..*" Rescate : recibe
    Animal "1" --> "0..1" ExpedienteVeterinario : tiene
    Usuario "1" --> "0..*" ExpedienteVeterinario : revisa
    ExpedienteVeterinario "1" --> "0..*" Tratamiento : contiene
    EstadoSolicitud "1" --> "0..*" SolicitudAdopcion : define
    Animal "1" --> "0..*" SolicitudAdopcion : recibe
    Usuario "1" --> "0..*" SolicitudAdopcion : solicita
    SolicitudAdopcion "1" --> "0..1" Adopcion : produce
    Usuario "1" --> "0..*" Adopcion : aprueba
    Adopcion "1" --> "0..*" SeguimientoAdopcion : recibe
    Usuario "1" --> "0..*" SeguimientoAdopcion : realiza
    Usuario "1" --> "0..*" Notificacion : recibe
```

## 3. Diagrama de secuencia: reporte y rescate

```mermaid
sequenceDiagram
    actor Ciudadano
    participant Sistema
    participant Administrador
    participant Rescatista
    participant Refugio
    participant Veterinario
    participant BD as Base de datos

    Ciudadano->>Sistema: Reportar caso con ubicacion y descripcion
    Sistema->>BD: Crear casos_rescate
    BD-->>Sistema: Caso registrado
    Sistema->>Administrador: Notificar nuevo caso
    Administrador->>Sistema: Asignar rescatista
    Sistema->>BD: Actualizar id_usuario_asignado y estado
    Sistema->>Rescatista: Enviar notificacion de asignacion
    Rescatista->>Sistema: Confirmar atencion del caso
    Rescatista->>Sistema: Registrar animal rescatado
    Sistema->>BD: Crear o actualizar animales
    Rescatista->>Sistema: Solicitar refugio disponible
    Sistema->>BD: Consultar refugios activos
    BD-->>Sistema: Refugio disponible
    Sistema->>Refugio: Registrar ingreso del animal
    Sistema->>BD: Crear rescates
    Refugio->>Veterinario: Solicitar revision
    Veterinario->>Sistema: Registrar expediente veterinario
    Sistema->>BD: Crear expedientes_veterinarios
    Sistema-->>Ciudadano: Mostrar estado actualizado del caso
```

## 4. Diagrama de actividad: proceso de adopcion

```mermaid
flowchart TD
    A([Inicio]) --> B[Consultar animales disponibles]
    B --> C[Seleccionar animal]
    C --> D[Enviar solicitud de adopcion]
    D --> E[Registrar solicitudes_adopcion]
    E --> F[Revisar datos del solicitante]
    F --> G{Solicitud completa?}
    G -- No --> H[Solicitar informacion adicional]
    H --> F
    G -- Si --> I[Evaluar condiciones de adopcion]
    I --> J{Solicitud aprobada?}
    J -- No --> K[Actualizar estado como rechazada]
    K --> L[Enviar notificacion de rechazo]
    L --> Z([Fin])
    J -- Si --> M[Crear contrato de adopcion]
    M --> N[Registrar adopciones]
    N --> O[Actualizar animal como adoptado]
    O --> P[Programar fecha de seguimiento]
    P --> Q[Registrar seguimientos_adopcion]
    Q --> R{Seguimiento satisfactorio?}
    R -- No --> S[Registrar observaciones y apoyo]
    S --> T[Programar nuevo seguimiento]
    T --> Q
    R -- Si --> U[Cerrar seguimiento]
    U --> V[Enviar notificacion de finalizacion]
    V --> Z
```

## 5. Diagrama de estados del animal

```mermaid
stateDiagram-v2
    [*] --> Reportado: caso de rescate creado
    Reportado --> EnRescate: rescatista asignado
    EnRescate --> EnRefugio: rescate completado
    EnRefugio --> EnRevision: ingreso veterinario
    EnRevision --> EnTratamiento: requiere atencion
    EnRevision --> Disponible: apto para adopcion
    EnTratamiento --> EnRevision: tratamiento en curso
    Disponible --> EnProcesoAdopcion: solicitud recibida
    EnProcesoAdopcion --> Disponible: solicitud rechazada
    EnProcesoAdopcion --> Adoptado: solicitud aprobada
    Adoptado --> EnSeguimiento: adopcion formalizada
    EnSeguimiento --> Adoptado: seguimiento satisfactorio
    EnSeguimiento --> Disponible: adopcion cancelada
```

## 6. Diagrama de arquitectura del sistema

```mermaid
flowchart TB
    subgraph presentacion[Capa de presentacion]
        vistaCiudadano[Vista ciudadano]
        vistaRescate[Vista rescate y refugio]
        vistaAdopcion[Vista adopciones]
    end

    subgraph control[Capa de control MVC]
        casoController[CasosRescateController]
        animalController[AnimalesController]
        adopcionController[AdopcionesController]
        usuarioController[UsuariosController]
    end

    subgraph negocio[Capa de servicios y reglas de negocio]
        gestorCasos[Gestor de casos]
        gestorAnimales[Gestor de animales]
        gestorVeterinario[Gestor veterinario]
        gestorAdopciones[Gestor de adopciones]
        gestorRefugios[Gestor de refugios]
    end

    subgraph patrones[Patrones de diseno]
        singleton[Singleton\nConexionBD y GestorCasos]
        observer[Observer\nServicioNotificaciones]
        factory[Factory Method\nExpedienteFactory]
    end

    subgraph modelo[Capa de modelo y persistencia]
        entidades[Entidades del dominio\nAnimal, CasoRescate, Adopcion]
        repositorios[Repositorios de datos]
        baseDatos[(Base de datos)]
    end

    vistaCiudadano --> casoController
    vistaRescate --> casoController
    vistaRescate --> animalController
    vistaRescate --> usuarioController
    vistaAdopcion --> adopcionController

    casoController --> gestorCasos
    animalController --> gestorAnimales
    adopcionController --> gestorAdopciones
    usuarioController --> gestorRefugios

    gestorCasos --> gestorAnimales
    gestorCasos --> gestorRefugios
    gestorAnimales --> gestorVeterinario
    gestorAdopciones --> gestorAnimales
    gestorAdopciones --> observer
    gestorCasos --> singleton
    gestorVeterinario --> factory

    singleton --> repositorios
    factory --> entidades
    observer --> entidades
    gestorCasos --> entidades
    gestorAnimales --> entidades
    gestorVeterinario --> entidades
    gestorAdopciones --> entidades
    gestorRefugios --> entidades
    entidades --> repositorios
    repositorios --> baseDatos

    classDef presentacion fill:#dbeafe,stroke:#2563eb,color:#172554
    classDef control fill:#dcfce7,stroke:#16a34a,color:#14532d
    classDef negocio fill:#fef3c7,stroke:#d97706,color:#78350f
    classDef patrones fill:#fce7f3,stroke:#db2777,color:#831843
    classDef modelo fill:#ede9fe,stroke:#7c3aed,color:#4c1d95

    class vistaCiudadano,vistaRescate,vistaAdopcion presentacion
    class casoController,animalController,adopcionController,usuarioController control
    class gestorCasos,gestorAnimales,gestorVeterinario,gestorAdopciones,gestorRefugios negocio
    class singleton,observer,factory patrones
    class entidades,repositorios,baseDatos modelo
```

### Descripcion de la arquitectura

- **Presentacion:** permite reportar casos, gestionar rescates y solicitar adopciones.
- **Control MVC:** recibe las acciones de las vistas y las envia al servicio correspondiente.
- **Servicios:** aplica las reglas de negocio de rescate, veterinaria, refugios y adopciones.
- **Patrones:** Singleton administra la conexion y el gestor central, Observer distribuye notificaciones y Factory Method crea expedientes por especie.
- **Modelo y persistencia:** representa las entidades y almacena la informacion en la base de datos.

