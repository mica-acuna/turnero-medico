# Turnero Médico

Sistema web para gestionar los turnos de un centro médico: pacientes, profesionales, agendas, consultas y recetas, con un panel para administración y otro para médicos.

Proyecto académico de la materia **Desarrollo de Aplicaciones con Objetos (DAO)**, Ingeniería en Sistemas de Información, UTN Facultad Regional Córdoba (2025). Desarrollado en equipo de 3 personas.

## Funcionalidades

**Panel de administración**
- Alta y gestión de pacientes, profesionales, especialidades y obras sociales.
- Registro de turnos según la agenda de cada profesional.
- Gestión de alergias, antecedentes, medicamentos y laboratorios.
- Reportes (PDF y gráficos).

**Panel del médico**
- Agenda diaria con los turnos asignados.
- Atención del turno y cierre de la consulta, con emisión de recetas.

**Ciclo de vida del turno:** Disponible → Agendado → En proceso → Finalizado, o Cancelado / Ausente.

## Tecnologías

| Capa | Tecnologías |
| --- | --- |
| Backend | Python, FastAPI, SQLModel / SQLAlchemy, Pydantic Settings |
| Frontend | React 19, TypeScript, Vite, React Router, Axios, CSS Modules |
| Base de datos | MySQL 8 en Docker |
| Reportes | ReportLab, Matplotlib |

## Arquitectura

El backend está organizado en capas:

```
backend/app/
├── api/routes/        # Endpoints REST (pacientes, profesionales, turnos, reportes...)
├── services/          # Lógica de negocio
├── repositories/      # Acceso a datos
├── domain/
│   ├── models/        # Entidades
│   └── schemas/       # Validación de entrada y salida
└── core/              # Configuración, conexión a la base y manejo de errores
```

Patrones de diseño aplicados:
- **State:** cada estado del turno es una clase que define qué transiciones se permiten (`domain/models/estados_turno`).
- **Observer:** al cambiar un turno se notifica a los interesados por email o SMS (`services/interfaces`, `notificador_email.py`, `notificador_sms.py`).
- **Singleton:** un único administrador de sesiones de base de datos (`core/database.py`).

## Cómo ejecutarlo

Requisitos: Docker, Python 3.11 o superior y Node.js 20 o superior.

**1. Configurar las credenciales**

```bash
cd backend
cp .env.example .env
# Editá .env y poné tus propias contraseñas
```

**2. Levantar la base de datos**

```bash
docker compose up -d
```

La primera vez crea las tablas y carga datos de ejemplo (`db/init`).

**3. Levantar el backend**

```bash
python -m venv .venv
source .venv/bin/activate      # En Windows: .venv\Scripts\activate
pip install -r requirements.txt
fastapi dev main.py
```

La API queda en http://localhost:8000 y la documentación interactiva en http://localhost:8000/docs.

**4. Levantar el frontend**

```bash
cd ../frontend/turnero-medico
npm install
npm run dev
```

Abrí http://localhost:5173. Usuarios de demostración: `admin` / `1234` (administración) y `medico` / `1234` (médico).

## Limitaciones conocidas

- El inicio de sesión es de demostración: los usuarios están fijos en el frontend y la API no tiene autenticación. Para un entorno real habría que agregar autenticación en el backend (contraseñas con hash, tokens y control de acceso por rol).
- Las credenciales de la base de datos se leen de un archivo `.env` que no se versiona.

## Equipo

| Integrante | Aportes principales |
| --- | --- |
| [Micaela Acuña](https://github.com/mica-acuna) | Frontend del panel médico (agenda, atención de turnos, cierre de consulta y recetas), registro de turnos, inicio de sesión y menús por rol |
| [matipe1](https://github.com/matipe1) | A completar |
| [Taritto](https://github.com/Taritto) | A completar |
