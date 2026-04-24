# CGR LMS - Learning Management System
## Sistema de Gestión de Aprendizaje para CGR Segur@

### 🏗️ Arquitectura del Sistema

#### **Stack Tecnológico**
- **Frontend**: React 19 + Vite + TailwindCSS + Framer Motion
- **Backend**: Node.js + Express + MariaDB
- **Cache**: Redis 7.2 (Sesiones y Rate Limiting)
- **Base de Datos**: MariaDB 11.2 (Optimizado para 700+ usuarios)
- **Autenticación**: Google OAuth 2.0 (Dominio @cgr.go.cr)
- **Containerización**: Docker + Docker Compose
- **Reverse Proxy**: Nginx (Configuración de seguridad y caché)

### 📊 Capacidad y Rendimiento
- ✅ Soporta **700 funcionarios** concurrentes.
- ✅ Pool de conexiones optimizado y monitoreo de salud del backend.
- ✅ Cache Redis para persistencia de sesiones y reducción de carga en DB.
- ✅ Sistema de migraciones automáticas para integridad del schema (ej. `response_data`).

### 🔐 Seguridad Implementada

#### **Autenticación y Acceso**
- Login exclusivo con Google OAuth institucional.
- Tokens JWT con rotación y expiración configurada.
- Sesiones centralizadas en Redis para alta disponibilidad.
- Validación de MFA con soporte para interacción rápida (Enter key).

#### **Protección de Infraestructura**
- Helmet.js para endurecimiento de headers.
- CORS restrictivo por entorno.
- Rate limiting inteligente (100 requests/15min).
- Prevención de SQL Injection y XSS en todas las entradas.
- Control de integridad en la reproducción de videos (bloqueo de adelantado).

### 🎮 Gamificación y Juegos Interactivos
El LMS incluye experiencias inmersivas diseñadas para aumentar el compromiso:

1. **Data Tetris** 🕹️
   - Juego de lógica cyberpunk donde el usuario limpia "sectores de datos corruptos".
   - Sistema de vidas representado por corazones y puntuación basada en velocidad.
   - Leaderboard global dedicado para los mejores "limpiadores de datos".

2. **Hackea al Vecino** 🔍
   - Simulador de toma de decisiones en entornos de red.
   - Sistema de pistas desbloqueables secuencialmente para guiar el aprendizaje.

3. **Guardianes de la CGR** 🛡️
   - Sistema de rangos: Novato → Defensor → Guardián → CISO Honorario.
   - Insignias dinámicas por completar hitos de seguridad.

### 📚 Funcionalidades Principales

#### **Para el Usuario**
- **Dashboard Dinámico**: Visualización de progreso, puntos y próximos módulos.
- **Ranking Institucional**: Visualización clara del puesto global y por área.
- **Lecciones Interactivas**: Videos, documentos y juegos inmersivos.
- **Evaluaciones**: Quizzes con retroalimentación inmediata y límites de intentos.
- **Certificación Automática**: Generación de certificados PDF con código de verificación.

#### **Para el Administrador**
- **Panel de Control**: Gestión integral de usuarios, roles y contenidos.
- **Reportes Avanzados**:
  - Exportación masiva a **CSV** de cumplimiento y completación.
  - Analíticas de progreso global y por departamento con tooltips dinámicos.
  - Conteo exacto de funcionarios que finalizaron el 100% de la capacitación.
- **Simulacros de Phishing**: Programación, ejecución y seguimiento de campañas.
- **Monitoreo de Logs**: Auditoría completa de acciones críticas y errores (Cybercat 500).

### 📈 Plan de Módulos 2026
1. **Módulo 1** (Febrero): Fundamentos de Seguridad
2. **Módulo 2** (Marzo): Protección de Datos
3. **Módulo 3** (Abril): IA y Ciberseguridad
4. **Módulo 4** (Mayo): Malware y Amenazas
5. **Módulo 5** (Julio): Redes y Comunicaciones
6. **Módulo 6** (Agosto): Teletrabajo Seguro
7. **Módulo 7** (Octubre): Gestión de Incidentes
8. **Módulo 8** (Noviembre): Aspectos Avanzados

### 📁 Estructura del Proyecto

```text
cgr-lms/
├── backend/                # API REST (Node.js/Express)
│   ├── config/             # Conexión DB, Redis y Logger
│   ├── controllers/        # Lógica de negocio por entidad
│   ├── middleware/         # Auth, Validación, Error Handling
│   ├── routes/             # Definición de end-points
│   ├── services/           # Lógica compleja y migraciones DB
│   └── server.js           # Punto de entrada principal
├── frontend/               # Aplicación SPA (React/Vite)
│   ├── src/
│   │   ├── components/     # UI Reutilizable, Juegos, Layouts
│   │   ├── pages/          # Vistas principales (Dashboard, Admin, etc.)
│   │   ├── store/          # Estado global (Zustand/Context)
│   │   └── assets/         # Imágenes, Estilos y Recursos
│   └── vite.config.js      # Configuración de compilación
├── database/               # Scripts SQL de inicialización (init.sql)
├── nginx/                  # Configuración del proxy inverso
├── uploads/                # Directorio persistente para archivos (certificados, recursos)
└── docker-compose.yml      # Orquestación de contenedores
```

### 🚀 Cómo Ejecutar el Sistema

#### **Requisitos**
- Docker & Docker Compose
- Credenciales Google OAuth (Client ID & Secret)

#### **Pasos Rápidos**

1. **Configurar Entorno**:
   ```bash
   cp backend/.env.example backend/.env
   # Editar backend/.env con GOOGLE_CLIENT_ID y GOOGLE_CLIENT_SECRET
   ```

2. **Despliegue con Docker**:
   ```bash
   # Desarrollo
   docker-compose up -d
   
   # Producción
   docker-compose -f docker-compose.prod.yml up -d
   ```

3. **Accesos**:
   - **Frontend**: `http://localhost:3000`
   - **API / Health**: `http://localhost:5002/health`
   - **Usuarios Test**: `admin@cgr.go.cr` / `funcionario@cgr.go.cr`

### 💾 Mantenimiento y Backups

#### **Backup de Base de Datos (MariaDB)**
```powershell
docker exec cgr-lms-mariadb mariadb-dump -u cgr_user -p"cgr_password" cgr_lms > backup_$(Get-Date -Format "yyyyMMdd").sql
```

#### **Restauración**
```powershell
cat backup_archivo.sql | docker exec -i cgr-lms-mariadb mariadb -u cgr_user -p"cgr_password" cgr_lms
```

---

**CGR Segur@ 2026** - *Fortaleciendo la cultura de ciberseguridad en la Contraloría General de la República.*
**Basado en**: ISO/IEC 27001:2022
