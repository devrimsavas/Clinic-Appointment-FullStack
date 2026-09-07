# NOVAMED Clinic Appointment Booking System

A full-stack clinic appointment booking application for an imaginary clinic group called **NOVAMED**, supporting patients, doctors, desk staff, and admin roles. Built with an ASP.NET Core 8 backend and a Next.js 14 frontend.

## 🏗️ Application Structure

The project is split into two parts:

- **`BACK-END/`** — ASP.NET Core 8 + Entity Framework Core + MySQL. Handles all business logic, REST API routes, and database operations.
- **`front_end/`** — Next.js 14 + Tailwind CSS. The user-facing interface, including an admin panel.

## 🚀 Features

### Public pages
- **Book Appointment** (`/`) — create a new appointment, restricted to working hours and future dates only, with booking rules displayed on the page
- **Search Doctor** (`/search`) — search doctors by name or view all doctors
- **Appointment Calendar** (`/calendar`) — monthly and daily calendar view via [DayPilot](https://code.daypilot.org/62886/next-js-calendar-day-week-month-open-source)
- **About Us, Contact, Privacy Policy** — supporting pages, including a dummy contact form

### Admin panel (`/admin`)
- Full CRUD management for Clinics, Doctors, Patients, Appointments, and Specialities
- Business rule enforced: **a patient with an existing appointment cannot be deleted**
- ⚠️ No authentication/authorization is implemented yet — this is a known limitation, with role-based access control planned to comply with personal data protection requirements

## 🛠 Tech Stack

**Backend:** ASP.NET Core 8, Entity Framework Core, MySQL, Swagger, LINQ, CORS
**Frontend:** Next.js 14, React 18, TypeScript, Tailwind CSS, `@daypilot/daypilot-lite-react` (calendar), `lucide-react` (icons), `animate.css`

## 📂 Project Structure

```
Clinic-Appointment-FullStack/
├── BACK-END/
│   ├── Controllers/
│   │   ├── AppointmentsController.cs
│   │   ├── ClinicController.cs
│   │   ├── DoctorsController.cs
│   │   ├── PatientsController.cs
│   │   ├── SearchDoctorController.cs
│   │   └── SpecialitiesController.cs
│   ├── DTOs/            # Response shaping, avoids entity leakage
│   ├── Models/
│   └── Migrations/
└── front_end/
    └── app/
        ├── admin/
        │   ├── clinics/ doctors/ patients/ appointments/ specialities/
        ├── search/
        ├── calendar/
        ├── aboutus/ contact/ privacy/
```

## 📖 API Endpoints Overview

| Resource | Endpoints |
|---|---|
| **Appointments** | `GET/POST /api/appointments`, `GET/PUT/DELETE /api/appointments/{id}` |
| **Patients** | `GET/POST /api/patients`, `GET/PUT/DELETE /api/patients/{id}` |
| **Doctors** | `GET/POST /api/doctors`, `GET/PUT/DELETE /api/doctors/{id}` |
| **Clinics** | `GET/POST /api/clinics`, `GET/PUT/DELETE /api/clinics/{id}` |
| **Specialities** | `GET/POST /api/specialities`, `GET/PUT/DELETE /api/specialities/{id}` |
| **Doctor Search** | `POST /api/search/doctors` — search by first/last name, no auth required |

All endpoints are documented and testable via Swagger UI.

Sample search request body:
```json
{
  "firstName": "John",
  "lastName": "Doe"
}
```

## ▶️ Getting Started

### Option A — Automatic startup (recommended)

The root `package.json` uses `concurrently` to run both backend and frontend together:

```json
{
  "scripts": {
    "dev": "concurrently -k -n FRONT,BACK -c green,blue \"npm run front\" \"npm run back\"",
    "front": "npm --prefix front_end run dev",
    "back": "dotnet watch run --project BACK-END"
  }
}
```

From the root folder:
```bash
npm install
npm run dev
```
Frontend logs appear in green, backend logs in blue.

### Option B — Manual startup

**Backend:**
```bash
cd BACK-END
dotnet build
```
Create `appsettings.json` (see `appsettings.json.sample.txt`):
```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost;Database=medicalclinics;user=root;password=yourpassword"
  },
  "AllowedHosts": "*"
}
```
Then:
```bash
dotnet ef database update
dotnet run
```
Swagger UI: `https://localhost:<port>/swagger` (default port `5196` — verify in `launchSettings.json`)

**Frontend:**
```bash
cd front_end
npm install
```
Create `.env.local` with the backend API URL, then:
```bash
npm run dev
```
> ⚠️ Make sure the backend is running before starting the frontend.

## 🔐 CORS Configuration

All origins, headers, and methods are currently allowed (development setting):
```csharp
builder.Services.AddCors(options => {
    options.AddPolicy("AllowAll", builder => {
        builder.AllowAnyOrigin().AllowAnyHeader().AllowAnyMethod();
    });
});
```

## 📝 Notes

- Only non-sensitive personally identifiable information is stored
- DTOs shape all API responses to avoid leaking EF Core entities directly
- XML comments are included throughout for Swagger documentation
- Application name, placeholder text, and images were generated with ChatGPT and have no relation to any real clinic

## 📚 References

1. Noroff Learning Resources — Front-End Technologies (Module 4), Back-End Technologies
2. ChatGPT (OpenAI) — dummy text, company name, and placeholder visuals
3. The author's own [Movie Theater project](https://github.com/devrimsavas/Movie_Theater_New) — reused for Next.js component structure
4. [DayPilot](https://code.daypilot.org) — appointment calendar component
