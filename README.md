# 🔎 Lost & Found Portal

A full-stack web application for managing **lost and found items within a community or campus environment**. The platform allows users to report lost or found belongings, upload images, search approved reports, receive email notifications, and claim items. An administrator can review, approve, manage, and resolve submitted reports.

## 🚀 Features

### User

* Report a **Lost** or **Found** item
* Add item title, description, category, location, date, and contact information
* Upload up to **5 images** for an item
* Browse approved lost and found reports
* Search reports by title, description, location, email, or submitter
* Filter reports by type, location, and category
* Receive email confirmation after submitting a report
* Receive email notifications for potential matches
* Claim a found item
* Mark submitted items as resolved

### Admin

* Secure admin login using JWT authentication
* View all submitted reports
* Review and moderate submitted items
* Approve or reject/manage item reports
* Delete item reports
* Mark items as resolved
* Automatically identify potential lost/found matches
* Send match notifications to relevant users

---

## 🛠️ Tech Stack

### Frontend

* **React 19**
* **React Router**
* **Axios**
* **Tailwind CSS**
* **Framer Motion**
* **Firebase**
* **Lucide React**
* **React Toastify**
* **Three.js / React Three Fiber**

### Backend

* **Node.js**
* **Express.js**
* **MongoDB**
* **Mongoose**
* **JWT**
* **bcryptjs**
* **Multer**
* **Cloudinary**
* **Nodemailer**
* **dotenv**
* **CORS**

The frontend dependencies and scripts are defined in the project configuration. The backend uses Express, MongoDB/Mongoose, JWT, bcrypt, Cloudinary, Multer, Nodemailer, and dotenv.

---

## 🏗️ Architecture

```text
                    ┌─────────────────────┐
                    │        User         │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │   React Frontend    │
                    │                     │
                    │ • Reports           │
                    │ • Search            │
                    │ • Filters           │
                    │ • Claims            │
                    └──────────┬──────────┘
                               │
                         REST API
                               │
                               ▼
                    ┌─────────────────────┐
                    │  Node.js + Express  │
                    └──────────┬──────────┘
                               │
             ┌─────────────────┼─────────────────┐
             ▼                 ▼                 ▼
      ┌─────────────┐   ┌─────────────┐   ┌─────────────┐
      │   MongoDB   │   │  Cloudinary │   │  Nodemailer │
      │   Database  │   │    Images   │   │    Emails   │
      └─────────────┘   └─────────────┘   └─────────────┘
                              
                    ┌─────────────────────┐
                    │   Admin Dashboard   │
                    │ Review • Approve    │
                    │ Delete • Resolve    │
                    └─────────────────────┘
```

---

## 🔄 Application Flow

### 1. Item Submission

A user submits a lost or found item with its details and optional images.

```text
User
 ↓
Submit Lost/Found Item
 ↓
Upload Images
 ↓
Save Report
 ↓
Status = Pending
 ↓
Submission Confirmation Email
```

Submitted items are initially stored with a `pending` status.

### 2. Admin Moderation

The administrator reviews submitted reports and updates their status.

```text
Pending Report
      ↓
Admin Review
      ↓
Approved / Rejected
```

Only approved reports are returned through the public item listing API.

### 3. Potential Matching

When an item is approved, the backend searches for an approved unresolved report of the opposite type using the reported **title and location**.

```text
        New Approved Item
                ↓
       Determine Opposite Type
                ↓
      Search Approved Reports
                ↓
       Compare Title + Location
                ↓
         Potential Match
                ↓
        Email Notification
```

The current implementation searches for the opposite `lost`/`found` type while excluding resolved reports and matching title and location case-insensitively.

### 4. Claim & Resolution

A user can claim a found item by submitting their name, email, and roll number.

```text
Found Item
    ↓
Claim Request
    ↓
Name + Email + Roll No.
    ↓
Item Marked as Resolved
```

The claim information and resolution details are stored with the item.

---

# 📂 Project Structure

```text
lost-and-found/
│
├── frontend/
│   ├── public/
│   ├── src/
│   │   ├── components/
│   │   ├── pages/
│   │   └── ...
│   ├── package.json
│   ├── package-lock.json
│   ├── tailwind.config.js
│   └── postcss.config.js
│
├── backend/
│   ├── middlewares/
│   │   └── verifyToken.js
│   ├── models/
│   │   └── Item.js
│   ├── utils/
│   │   ├── cloudinary.js
│   │   └── mailer.js
│   ├── index.js
│   ├── package.json
│   └── package-lock.json
│
├── .gitignore
└── README.md
```

---

# 🔌 API

## Public

| Method | Endpoint          | Description              |
| ------ | ----------------- | ------------------------ |
| `GET`  | `/`               | API health/status check  |
| `GET`  | `/api/items`      | Get approved items       |
| `POST` | `/api/items`      | Submit lost/found item   |
| `GET`  | `/api/categories` | Get available categories |

## Admin

| Method   | Endpoint                        | Description             |
| -------- | ------------------------------- | ----------------------- |
| `POST`   | `/api/admin/login`              | Admin authentication    |
| `GET`    | `/api/admin/items`              | Get all submitted items |
| `PUT`    | `/api/admin/items/:id/moderate` | Moderate an item        |
| `DELETE` | `/api/admin/items/:id`          | Delete an item          |
| `PUT`    | `/api/admin/items/:id/resolve`  | Resolve an item         |

## User Actions

| Method | Endpoint                 | Description                   |
| ------ | ------------------------ | ----------------------------- |
| `PUT`  | `/api/items/:id/resolve` | Resolve user's submitted item |
| `PUT`  | `/api/items/:id/claim`   | Claim a found item            |

---

# 🔐 Authentication

Administrative access uses **JWT-based authentication**.

```text
Admin Login
    ↓
Username + Password
    ↓
Credential Verification
    ↓
JWT Token
    ↓
Protected Admin Operations
```

The backend generates a JWT for successful admin authentication, with the token configured to expire after two hours.

---

# ☁️ Image Storage

Item images are uploaded using **Multer** and stored through **Cloudinary**.

The application supports multiple images per report and stores the resulting image URLs with the item record.

---

# 📧 Email Notifications

The system uses **Nodemailer** to send automated emails.

### Submission Confirmation

Users receive confirmation after successfully submitting a lost/found report.

### Potential Match

When the system identifies a possible match, the relevant user receives an email containing the item title and location so they can verify the report.

---

# ⚙️ Installation

## Prerequisites

* Node.js
* npm
* MongoDB
* Cloudinary account
* Email account with SMTP/App Password support

---

## 1. Clone Repository

```bash
git clone https://github.com/YOUR-USERNAME/YOUR-REPOSITORY.git
cd YOUR-REPOSITORY
```

---

## 2. Install Backend

```bash
cd backend
npm install
```

Start the backend:

```bash
npm start
```

For development:

```bash
npm run dev
```

The backend configuration defines `node index.js` for production/startup and `nodemon index.js` for development.

---

## 3. Configure Backend Environment

Create:

```text
backend/.env
```

Add:

```env
MONGO_URI=your_mongodb_connection_string

PORT=5000

CLOUD_NAME=your_cloudinary_cloud_name
CLOUD_API_KEY=your_cloudinary_api_key
CLOUD_API_SECRET=your_cloudinary_api_secret

EMAIL_USER=your_email
EMAIL_PASS=your_email_app_password

ADMIN_USERNAME=your_admin_username
ADMIN_PASSWORD=your_admin_password

JWT_SECRET=your_secure_jwt_secret
```

---

## 4. Install Frontend

Open another terminal:

```bash
cd frontend
npm install
```

Start the application:

```bash
npm start
```

The React development server runs on:

```text
http://localhost:3000
```

---

# 🔑 Environment Variables

| Variable           | Purpose                    |
| ------------------ | -------------------------- |
| `MONGO_URI`        | MongoDB connection         |
| `PORT`             | Backend server port        |
| `CLOUD_NAME`       | Cloudinary cloud name      |
| `CLOUD_API_KEY`    | Cloudinary API key         |
| `CLOUD_API_SECRET` | Cloudinary API secret      |
| `EMAIL_USER`       | Email account              |
| `EMAIL_PASS`       | Email application password |
| `ADMIN_USERNAME`   | Admin username             |
| `ADMIN_PASSWORD`   | Admin password             |
| `JWT_SECRET`       | JWT signing secret         |

**Never commit `.env` to GitHub.**

---

# 🖥️ Running Locally

Start the backend:

```bash
cd backend
npm run dev
```

Start the frontend in another terminal:

```bash
cd frontend
npm start
```

Then open:

```text
http://localhost:3000
```

---

# 📸 Screenshots

Add screenshots of your application here:

```text
docs/
├── home.png
├── report-item.png
├── search.png
├── item-details.png
└── admin-dashboard.png
```

Example:

```markdown
![Home Page](docs/home.png)
```

Recommended screenshots:

* Home page
* Lost items page
* Found items page
* Report item page
* Item details page
* Search/filter interface
* Admin dashboard
* Claim/resolution interface

---

# 🎯 Objectives

* Provide a centralized platform for lost and found reports
* Simplify reporting of lost and found belongings
* Enable quick searching and filtering
* Facilitate identification of potential matches
* Automate user notifications
* Provide administrative moderation
* Provide a structured claiming and resolution process

---

# 🔮 Future Improvements

* AI-powered image matching
* Semantic text-based matching
* Map-based location search
* Real-time notifications
* User accounts and profiles
* In-app messaging
* Advanced admin analytics
* Mobile application
* Improved role-based access control

---

# 👨‍💻 Author

**Praveen Kumar**

B.Tech — Metallurgical Engineering & Materials Science
Indian Institute of Technology Indore

---

# 📄 License

This project currently uses the **ISC License** for the backend package.

---

## ⭐ Acknowledgement

Built as a full-stack web application to provide a practical and centralized solution for managing lost and found items.

> **Lost something? Found something? Help it find its way home. 🔎**
