# 🏥 Hello Doctor — AI-Powered Medical Diagnostic Platform

> An intelligent health platform that helps patients understand their medical reports — including MRI brain scans, ECG, and X-rays — using cutting-edge AI models. Designed to make diagnostics accessible, especially for underserved communities.

---

## 📋 Table of Contents

- [Overview](#-overview)
- [Features](#-features)
- [Tech Stack](#-tech-stack)
- [Architecture](#-architecture)
- [Project Structure](#-project-structure)
- [Getting Started](#-getting-started)
- [API Reference](#-api-reference)
- [AI Model](#-ai-model)
- [Team](#-team)

---

## 🌟 Overview

**Hello Doctor** is a full-stack web application combining AI/ML capabilities with a modern frontend interface to assist patients in understanding complex medical reports. The platform supports:

- 🧠 **MRI Brain Tumor Segmentation** — Upload NIfTI MRI files and get AI-powered tumor analysis
- 📄 **Text Report Chat** — Upload PDF medical reports and chat with an AI assistant (Gemini-powered) for plain-language explanations
- 👤 **User Authentication** — Secure sign-up, login, and profile management
- 📊 **PDF Report Generation** — Automatically generates detailed MRI analysis reports with tumor visualizations

---

## ✨ Features

### 🧠 MRI Brain Tumor Analysis
- Upload 4 MRI sequences: **FLAIR**, **T1CE**, **T2**, and **Mask** (NIfTI format: `.nii` / `.nii.gz`)
- Deep Learning model (Keras/TensorFlow) performs **3D tumor segmentation**
- Identifies tumor regions:
  - **NCR/NET** — Necrotic and Non-Enhancing Tumor Core
  - **ED** — Peritumoral Edema
  - **ET** — Enhancing Tumor
- Calculates **tumor volumes** in mm³ with percentage breakdown
- Generates a **downloadable PDF report** with patient info and slice-by-slice visualizations

### 💬 AI-Powered Medical Report Chat
- Upload any medical PDF report (blood test, ECG, X-ray, etc.)
- AI extracts text from the PDF automatically
- Chat with **Google Gemini 1.5 Flash** for simple, clear medical explanations
- Full conversation history saved per report
- Multi-report dashboard for managing all reports

### 🔐 User Authentication & Security
- Secure registration with profile photo upload
- JWT-based authentication stored in HTTP-only cookies
- Protected routes for authenticated users only
- Password hashing with bcrypt

---

## 🛠 Tech Stack

### Frontend
| Technology | Purpose |
|---|---|
| **React 19** | UI Framework |
| **Vite 6** | Build Tool |
| **Tailwind CSS 4** | Styling |
| **React Router v7** | Client-side routing |
| **Axios** | HTTP client |
| **Framer Motion / Motion** | Animations |
| **Radix UI** | Accessible UI primitives |
| **Lucide React** | Icons |
| **React Type Animation** | Typewriter effects |
| **Swiper** | Carousel/slider |

### Backend — Express (Node.js)
| Technology | Purpose |
|---|---|
| **Express.js** | REST API server |
| **MongoDB + Mongoose** | Database & ODM |
| **JWT** | Authentication tokens |
| **bcrypt** | Password hashing |
| **Multer** | File uploads |
| **pdf-parse** | PDF text extraction |
| **@google/generative-ai** | Gemini AI integration |
| **cookie-parser** | Cookie management |

### Backend — Django (Python)
| Technology | Purpose |
|---|---|
| **Django** | AI/ML API server |
| **TensorFlow / Keras** | Deep learning model inference |
| **NiBabel** | NIfTI MRI file processing |
| **NumPy / SciPy** | Numerical computing & image processing |
| **scikit-learn** | Data preprocessing (MinMaxScaler) |
| **ReportLab** | PDF generation |
| **Matplotlib** | MRI slice visualization |

---

## 🏗 Architecture

```
Hello Doctor Platform
│
├── Frontend (React + Vite)         → Port 5173
│   └── Communicates with both backends via REST APIs
│
├── Backend: Express (Node.js)      → Port 3000
│   ├── User auth (signup/login/logout)
│   ├── PDF upload & text extraction
│   ├── Chat history management (MongoDB)
│   └── Gemini AI integration for chat responses
│
└── Backend: Django (Python)        → Port 8000
    ├── MRI NIfTI file processing
    ├── AI tumor segmentation (Keras model)
    ├── Volume calculation & analysis
    └── PDF report generation with visualizations
```

### Data Flow — MRI Analysis
```
User uploads 4 NIfTI files
        ↓
React frontend (MRIProcessing.jsx)
        ↓
Django REST API (/api/mri-process/)
        ↓
Preprocessing: Normalize → Stack → Resize to 128×128×128
        ↓
Keras Model (model-V2-diceLoss_focal.keras)
        ↓
Argmax → Tumor segmentation mask (4 classes)
        ↓
Volume calculation per region
        ↓
ReportLab PDF generation with slice overlays
        ↓
JSON response with base64 visualization + PDF download URL
```

### Data Flow — Report Chat
```
User uploads PDF report
        ↓
Express API (/api/v1/chat/create-report)
        ↓
pdf-parse extracts text content
        ↓
Stored in MongoDB (Report model)
        ↓
User asks questions via chat
        ↓
Gemini 1.5 Flash answers with report context
        ↓
Chat history saved per report
```

---

## 📁 Project Structure

```
Hello-doctor/
├── frontend/                        # React + Vite application
│   ├── src/
│   │   ├── pages/                   # Route-level page components
│   │   │   ├── Home.jsx             # Landing page
│   │   │   ├── Login.jsx            # Sign in page
│   │   │   ├── SignUp.jsx           # Registration page
│   │   │   ├── Dashboard.jsx        # User dashboard (reports list)
│   │   │   ├── MRIProcessing.jsx    # MRI upload & AI analysis
│   │   │   ├── ModelProcessing.jsx  # AI model selection page
│   │   │   ├── ChatPage.jsx         # Report chat interface
│   │   │   ├── TextReportLanding.jsx# Upload report page
│   │   │   ├── Profile.jsx          # User profile
│   │   │   ├── Services.jsx         # Services listing page
│   │   │   └── ForgotPass.jsx       # Password recovery
│   │   ├── components/              # Shared UI components
│   │   │   ├── Navbar.jsx
│   │   │   ├── Herosection.jsx
│   │   │   ├── About.jsx
│   │   │   ├── Services.jsx
│   │   │   ├── Teams.jsx
│   │   │   ├── Footer.jsx
│   │   │   └── ChatCard.jsx
│   │   ├── context/
│   │   │   ├── AuthContext.jsx      # Global auth state
│   │   │   └── ProtectedRoute.jsx   # Route guard component
│   │   └── App.jsx                  # Router setup
│   └── package.json
│
├── backend/
│   ├── express/                     # Node.js REST API
│   │   ├── server.js                # Entry point (port 3000)
│   │   ├── config/
│   │   │   └── db.js                # MongoDB connection
│   │   ├── controllers/
│   │   │   ├── auth.controller.js   # Signup/login/logout logic
│   │   │   └── chat.controller.js   # Report & chat logic + Gemini AI
│   │   ├── middleware/
│   │   │   └── auth.middleware.js   # JWT verify + Multer upload
│   │   ├── models/
│   │   │   ├── user.models.js       # User schema (Mongoose)
│   │   │   ├── report.models.js     # Medical report schema
│   │   │   └── chat.models.js       # Chat message schema
│   │   └── routes/
│   │       ├── auth.routes.js       # /api/v1/auth/*
│   │       └── chat.routes.js       # /api/v1/chat/*
│   │
│   └── django/                      # Python AI/ML API
│       ├── manage.py
│       ├── requirements.txt
│       ├── core/                    # Django project settings
│       │   └── settings.py
│       ├── api/
│       │   ├── views.py             # MRI processing endpoints
│       │   ├── models.py            # Profile model
│       │   └── urls.py              # URL routing
│       └── model/
│           └── model-V2-diceLoss_focal.keras   # Trained AI model
│
└── README.md
```

---

## 🚀 Getting Started

### Prerequisites
- Node.js ≥ 18
- Python ≥ 3.9
- MongoDB (local or Atlas)
- Google Gemini API key

---

### 1. Clone the Repository
```bash
git clone <repo-url>
cd Hello-doctor
```

---

### 2. Frontend Setup
```bash
cd frontend
npm install
npm run dev
# Runs on http://localhost:5173
```

---

### 3. Express Backend Setup
```bash
cd backend/express
npm install
```

Create a `.env` file:
```env
MONGODB_URI=mongodb://localhost:27017/hello-doctor
JWT_SECRET=your_jwt_secret_key
GEMINI_API_KEY=your_google_gemini_api_key
NODE_ENV=development
```

```bash
npm run dev
# Runs on http://localhost:3000
```

---

### 4. Django Backend Setup
```bash
cd backend/django
python -m venv venv
venv\Scripts\activate        # Windows
# source venv/bin/activate   # Linux/Mac

pip install -r requirements.txt
python manage.py migrate
python manage.py runserver
# Runs on http://localhost:8000
```

> [!IMPORTANT]
> The Django backend requires the trained Keras model file at:
> `backend/django/model/model-V2-diceLoss_focal.keras`
> Make sure this file is present before running MRI analysis.

---

## 🔌 API Reference

### Express API (Port 3000)

#### Auth Endpoints
| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| `POST` | `/api/v1/auth/signup` | Register new user (with profile image) | ❌ |
| `POST` | `/api/v1/auth/signin` | Login user | ❌ |
| `POST` | `/api/v1/auth/logout` | Logout user | ❌ |
| `GET` | `/api/v1/auth/me` | Get current user info | ✅ |

#### Chat/Report Endpoints
| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| `POST` | `/api/v1/chat/create-report` | Upload PDF & create report | ✅ |
| `POST` | `/api/v1/chat/send-message` | Send chat message, get AI reply | ✅ |
| `GET` | `/api/v1/chat/history/:reportId` | Get chat history for a report | ✅ |
| `GET` | `/api/v1/chat/reports` | Get all user reports | ✅ |

---

### Django API (Port 8000)

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/api/mri-process/` | Process 4 MRI NIfTI files, returns analysis + PDF |
| `GET` | `/api/download_pdf/<filename>` | Download generated PDF report |

#### MRI Process Request Format
```
Content-Type: multipart/form-data
files: [flair.nii, t1ce.nii, t2.nii, mask.nii]   (in order)
patient_data: {"first_name": "John", "last_name": "Doe", "age": 35}
```

---

## 🤖 AI Model

### Brain Tumor Segmentation Model
- **Architecture:** 3D U-Net (custom)
- **Loss Function:** Combined Dice Loss + Focal Loss
- **Input:** 128 × 128 × 128 voxels with 3 channels (FLAIR, T1CE, T2)
- **Output:** 4-class segmentation mask
  - `0` — Background (no tumor)
  - `1` — NCR/NET (Necrotic / Non-enhancing Tumor Core)
  - `2` — ED (Peritumoral Edema)
  - `3` — ET (Enhancing Tumor)
- **Training Dataset:** BraTS 2020 (Brain Tumor Segmentation Challenge)
- **Framework:** TensorFlow / Keras

### Preprocessing Pipeline
1. Load NIfTI files via NiBabel
2. Apply **MinMax normalization** per channel
3. Remap label `4 → 3` (BraTS convention)
4. Crop to region of interest: `[40:210, 40:210, :]`
5. Resize to `128×128×128` (cubic interpolation for image, nearest-neighbor for mask)
6. Stack channels: `[FLAIR, T1CE, T2]`

---

## 📐 Frontend Routes

| Path | Component | Auth Required | Description |
|------|-----------|---------------|-------------|
| `/` | `Home` | ❌ | Landing page |
| `/services` | `Services` | ❌ | Services overview |
| `/signin` | `Login` | ❌ | Sign in |
| `/signup` | `SignUp` | ❌ | Register |
| `/forgot-pass` | `ForgotPass` | ❌ | Password reset |
| `/profile` | `Profile` | ❌ | User profile |
| `/user/dashboard` | `Dashboard` | ✅ | Reports dashboard |
| `/user/mri-processing` | `MRIProcessing` | ✅ | MRI upload & analysis |
| `/user/model-processing` | `ModelProcessing` | ✅ | Model selection |
| `/user/testchat` | `TextReportLanding` | ✅ | Upload text report |
| `/user/chat/:reportId` | `ChatPage` | ✅ | AI chat per report |


---

> [!NOTE]
> This platform is for **educational purposes only**. All medical analysis results should be reviewed and validated by a qualified healthcare professional before making any medical decisions.

