# 🏨 BookYourStay

> Plataforma de reservas de alojamientos desarrollada en Java con JavaFX, orientada a la gestión completa de hospedajes turísticos en Colombia.

---

## 📋 Tabla de Contenidos

- [Descripción](#descripción)
- [Características](#características)
- [Tecnologías](#tecnologías)
- [Arquitectura](#arquitectura)
- [Patrones de Diseño](#patrones-de-diseño)
- [Estructura del Proyecto](#estructura-del-proyecto)
- [Instalación y Ejecución](#instalación-y-ejecución)
- [Funcionalidades por Rol](#funcionalidades-por-rol)
- [Modelo de Datos](#modelo-de-datos)
- [Pruebas](#pruebas)
- [Autores](#autores)

---

## 📖 Descripción

**BookYourStay** es una aplicación de escritorio construida con JavaFX que simula una plataforma de reservas de alojamientos turísticos. Permite a los usuarios buscar, reservar y reseñar hoteles, casas y apartamentos en distintas ciudades de Colombia, mientras que los administradores gestionan el inventario, las ofertas y las estadísticas de la plataforma.

---

## ✨ Características

### Para Clientes
- 🔐 Registro con verificación de cuenta por correo electrónico
- 🔑 Inicio de sesión con validación de estado de cuenta
- 🔍 Búsqueda de alojamientos por ciudad, tipo y precio máximo
- 📅 Reserva de alojamientos con selección de fechas y número de huéspedes
- 💳 Billetera virtual con recarga de saldo
- ❌ Cancelación de reservas con reintegro automático del saldo
- ⭐ Sistema de reseñas con comentario y valoración (1–5 estrellas)
- 🗑️ Eliminación de reseñas propias
- 🔒 Recuperación de contraseña por código enviado al correo

### Para Administradores
- 🏠 CRUD completo de alojamientos (Hotel, Casa, Apartamento)
- 🛏️ Registro y eliminación de habitaciones en hoteles
- 🎁 Gestión de ofertas especiales (temporada y estadía prolongada)
- 👥 Gestión de usuarios (bloqueo de cuentas)
- 📊 Estadísticas:
  - Porcentaje de reservas por tipo de alojamiento
  - Alojamientos más populares por ciudad
  - Tipos de alojamiento más rentables
  - Ocupación porcentual por alojamiento
- 🔑 Cambio de contraseña mediante código de verificación

### General
- 📧 Notificaciones por correo electrónico (registro, recuperación, facturas)
- 🧾 Generación de factura con código QR por cada reserva
- 💰 Aplicación automática de descuentos según ofertas vigentes
- 💾 Persistencia de datos mediante serialización de objetos Java

---

## 🛠️ Tecnologías

| Tecnología | Versión | Uso |
|---|---|---|
| **Java** | 17+ | Lenguaje principal |
| **JavaFX** | 23 | Interfaz gráfica de usuario |
| **Lombok** | Latest | Reducción de boilerplate |
| **Simple Java Mail** | Latest | Envío de notificaciones por correo |
| **ZXing** | Latest | Generación de códigos QR |
| **JUnit 5** | Latest | Pruebas unitarias |
| **Jakarta Mail** | Latest | Soporte para envío de correos con imagen |
| **Maven** | 3.x | Gestión de dependencias y build |

---

## 🏛️ Arquitectura

El proyecto sigue una arquitectura en capas con separación clara de responsabilidades:

```
┌─────────────────────────────────────────────┐
│              CAPA DE PRESENTACIÓN            │
│         Controladores FXML (JavaFX)          │
│  ControladoresAdm / ControladoresCliente     │
└───────────────────┬─────────────────────────┘
                    │
┌───────────────────▼─────────────────────────┐
│              CAPA DE SERVICIOS               │
│  ServicioAdm / ServicioCliente /             │
│  ServicioReserva / ServicioAlojamiento /     │
│  ServicioOferta / ServicioBilleteraVirtual   │
└───────────────────┬─────────────────────────┘
                    │
┌───────────────────▼─────────────────────────┐
│             CAPA DE REPOSITORIOS             │
│  AlojamientoRepository / ClienteRepository  │
│  ReservaRepository / OfertaRepository        │
└───────────────────┬─────────────────────────┘
                    │
┌───────────────────▼─────────────────────────┐
│              CAPA DE PERSISTENCIA            │
│         Serialización de objetos (.data)     │
└─────────────────────────────────────────────┘
```

---

## 🧩 Patrones de Diseño

| Patrón | Clase / Contexto |
|---|---|
| **Singleton** | `ControladorPrincipal`, `SesionUsuario` |
| **Factory Method** | `AlojamientoFactory`, `OfertaFactory` |
| **Builder** | `Reserva`, `Factura`, `Review`, todos los servicios con Lombok |
| **Abstract Class / Polimorfismo** | `Alojamiento` → `Hotel`, `Casa`, `Apartamento` |
| **Abstract Class / Polimorfismo** | `Oferta` → `OfertaTemporada`, `OfertaEstadiaProlongada` |
| **Observer (parcial)** | `ObservableList` de JavaFX en reservas |

---

## 📁 Estructura del Proyecto

```
src/
├── main/
│   ├── java/
│   │   └── co/edu/uniquindio/proyectofinalhotelfx/
│   │       ├── App.java                        # Punto de entrada
│   │       ├── Constantes/                     # Constantes del sistema
│   │       ├── Controladores/                  # Controladores JavaFX
│   │       │   ├── ControladoresAdm/           # Vistas del administrador
│   │       │   └── ControladoresCliente/       # Vistas del cliente
│   │       ├── Factory/                        # Fábricas de entidades
│   │       │   ├── AlojamientoFactory/
│   │       │   └── OfertaFactory/
│   │       ├── Modelo/
│   │       │   ├── Entidades/                  # Clases de dominio
│   │       │   └── Enums/                      # Enumeraciones
│   │       ├── Notificacion/                   # Servicio de correo
│   │       ├── Persistencia/                   # Serialización y rutas
│   │       ├── Repo/                           # Repositorios de datos
│   │       ├── Servicios/                      # Lógica de negocio
│   │       ├── Singleton/                      # Sesión de usuario
│   │       ├── utils/                          # Utilidades (QR)
│   │       └── vo/                             # Value Objects
│   └── resources/
│       └── co/edu/uniquindio/proyectofinalhotelfx/
│           ├── FXMLDW(ADMIN)/                  # Vistas FXML del admin
│           ├── *.fxml                          # Vistas FXML del cliente
│           └── *.css                          # Estilos
├── test/
│   └── java/
│       └── PlataformaTest.java                 # Pruebas unitarias
data/                                           # Archivos de persistencia
Img/                                            # Imágenes de la aplicación
```

---

## 🚀 Instalación y Ejecución

### Prerrequisitos

- **Java JDK 17** o superior
- **Maven 3.x**
- **JavaFX SDK 23** (si no está incluido en el JDK)

### Pasos

1. **Clonar el repositorio**
   ```bash
   git clone https://github.com/tu-usuario/bookyourstay.git
   cd bookyourstay
   ```

2. **Compilar el proyecto**
   ```bash
   mvn clean compile
   ```

3. **Ejecutar la aplicación**
   ```bash
   mvn javafx:run
   ```

4. **Ejecutar las pruebas**
   ```bash
   mvn test
   ```

### Datos iniciales

- El administrador se carga desde el archivo serializado `data/adm.data`.
- Si no existe, se debe crear manualmente o ajustar la inicialización en `ServicioAdm`.
- Las carpetas `data/` e `Img/` deben existir en el directorio raíz del proyecto antes de ejecutar.

```bash
mkdir -p data
mkdir -p Img/ImagenesAlojamientos
mkdir -p Img/ImagenesOfertas
mkdir -p Img/ImagenesApp
```

---

## 👥 Funcionalidades por Rol

### 🧑‍💼 Administrador

| Módulo | Acciones disponibles |
|---|---|
| Alojamientos | Crear, ver, actualizar, eliminar |
| Habitaciones | Registrar y eliminar habitaciones en hoteles |
| Ofertas | Crear, visualizar, eliminar ofertas de temporada y estadía prolongada |
| Usuarios | Visualizar y bloquear clientes |
| Estadísticas | Ver gráficos de ocupación, popularidad y rentabilidad |
| Perfil | Cambiar contraseña con verificación por correo |

### 🧑‍🦱 Cliente

| Módulo | Acciones disponibles |
|---|---|
| Registro | Registro con verificación de correo |
| Búsqueda | Filtrar alojamientos por ciudad, tipo y precio |
| Reservas | Crear y cancelar reservas con descuentos automáticos |
| Billetera | Consultar saldo y recargar |
| Reseñas | Agregar y eliminar reseñas con valoración |
| Contraseña | Recuperar contraseña por correo |

---

## 📊 Modelo de Datos

### Entidades principales

```
Usuario (abstracta)
  ├── Administrador
  └── Cliente
        └── BilleteraVirtual
              └── Transaccion

Alojamiento (abstracta)
  ├── Hotel
  │     └── Habitacion
  ├── Casa
  └── Apartamento

Oferta (abstracta)
  ├── OfertaTemporada
  └── OfertaEstadiaProlongada

Reserva
  ├── Cliente
  ├── Alojamiento
  ├── Factura
  └── Review
```

### Enumeraciones

- `Ciudad` — 21 ciudades colombianas
- `TipoAlojamiento` — CASA, HOTEL, APARTAMENTO
- `ServiciosIncluidos` — WIFI, DESAYUNO, PISCINA, SPA, etc.
- `TipoHabitacionHotel` — SIMPLE, DOBLE, SUITE, FAMILIAR, DELUXE
- `OfertaTipo` — TEMPORADA, ESTADIAPROLONGADA
- `TipoTransaccion` — RETIRO, DEPOSITO

---

## 🧪 Pruebas

Las pruebas están ubicadas en `src/test/java/.../PlataformaTest.java` y cubren:

| Método Probado | Casos cubiertos |
|---|---|
| `registrarAlojamiento` | Campos nulos, vacíos, precio inválido, tipo nulo |
| `consultarSaldo` | Cédula vacía, cliente inexistente, caso exitoso |
| `recuperarContrasena` | Correo vacío, correo inexistente, caso exitoso |
| `agregarReserva` | Todos los parámetros nulos/inválidos individualmente |
| `bloquearCliente` | ID nulo, ID vacío |
| `loginCliente` | Correo nulo, vacío, contraseña vacía, usuario inexistente |
| `registrarOferta` | Nombre nulo, fechas inválidas, descuento negativo |
| `actualizarAlojamiento` | ID nulo, precio negativo, tipo nulo |
| `listarReservas` | Ejecución sin errores |
| `calcularPorcentajeReservasPorTipo` | Ejecución sin errores |

```bash
# Ejecutar pruebas
mvn test

# Ver reporte de pruebas
open target/surefire-reports/
```

---

## ⚙️ Configuración de Correo

El sistema utiliza Gmail con SMTP para el envío de notificaciones. Las credenciales están configuradas en `Notificacion.java`. Para usar tu propia cuenta:

1. Activa la autenticación en dos pasos en tu cuenta de Gmail.
2. Genera una **contraseña de aplicación** en la configuración de seguridad.
3. Actualiza las credenciales en `Notificacion.java`:

```java
.withSMTPServer("smtp.gmail.com", 587, "tu-correo@gmail.com", "tu-contrasena-app")
```

> ⚠️ **No subas credenciales reales al repositorio.** Considera usar variables de entorno o un archivo de configuración externo.

---

## 👨‍💻 Autores

Desarrollado como proyecto final de la asignatura **Programación** en la **Universidad del Quindío**.

| Nombre | Rol |
|---|---|
| Equipo de desarrollo | Desarrolladores |

---

## 📄 Licencia

Este proyecto fue desarrollado con fines académicos en el marco del programa de Ingeniería de Sistemas de la Universidad del Quindío.

---

<p align="center">
  Hecho con ☕ y JavaFX · Universidad del Quindío
</p>
