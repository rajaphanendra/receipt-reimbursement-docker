# Receipt Reimbursement System

## Overview
This is a full-stack application to manage receipt reimbursement requests. It allows users to upload receipt details and view them, with a backend API storing data in SQL Server and a frontend UI built with Angular.

Docker Compose is used to orchestrate the services: frontend, backend, and database.

---

## Tech Stack
- **Frontend:** Angular 19
- **Backend:** ASP.NET Web API (.NET 8)
- **Database:** Microsoft SQL Server 2022
- **Containerization & Orchestration:** Docker & Docker Compose

---

## Repository Structure
```
receipt-reimbursement-docker/
├── .env.example                  # Environment variables template
├── docker-compose.yaml          # Orchestration file for all services
├── ReceiptReimbursementAPI/     # Submodule for .NET backend
├── receipt-reimbursement-ui/    # Submodule for Angular frontend
└── README.md                    # This file
```

---

## Prerequisites
Make sure the following tools are installed:

- [Docker](https://www.docker.com/products/docker-desktop)
- [Git](https://git-scm.com/downloads)

---

## Environment Setup

1. Create a `.env` file in the project root:

```bash
cp .env.example .env
```

2. Open `.env` and set your values:

```env
SA_PASSWORD=YourStrong@Password123
```

- `SA_PASSWORD`: The SQL Server `sa` user password (required by SQL Server container and backend connection string).

---

## Running the Application

### 1. Clone the Repositories

```bash
git clone https://github.com/rajaphanendra/receipt-reimbursement-docker.git
cd receipt-reimbursement-docker

# Initialize and pull submodules
git submodule update --init --recursive
```

### 2. Start the Application

```bash
docker compose up --build
```

This will:
- Build and start the backend API (`http://localhost:5267`)
- Serve the Angular frontend (`http://localhost:4200`)
- Start SQL Server database on port `1433`


---

## Postman API Testing

You can use Postman to test the backend API:

### Submit a Receipt

**POST** `http://localhost:5267/api/Receipt`

**Body (form-data):**
| Key         | Value             |
|-------------|-------------------|
| amount      | 123.45            |
| date        | 2025-04-14        |
| description | Sample receipt    |
| file        | (Upload PDF/Image)|

### Get All Receipts
**GET** `http://localhost:5267/api/Receipt`

### View Uploaded File
Click the "View" link in the frontend table. This triggers download/open of the uploaded receipt file.

---

## Repository Links

- **Frontend Repo:** [receipt-reimbursement-ui](https://github.com/rajaphanendra/receipt-reimbursement-ui)
- **Backend Repo:** [ReceiptReimbursementAPI](https://github.com/rajaphanendra/ReceiptReimbursementAPI)

---

## Troubleshooting & Tips

- Ensure your `.env` file is created with a valid SQL Server password.
- If ports are already in use, change the values in `docker-compose.yaml` accordingly.
- Make sure the Angular build outputs to `dist/receipt-reimbursement-ui` before running Docker Compose.

```bash
cd receipt-reimbursement-ui
npm install
npm run build -- --configuration production
```

- To rebuild everything from scratch:
```bash
docker compose down -v
docker compose up --build
```

- If you encounter container name conflicts or want a fresh start:

```bash
docker compose down --volumes --remove-orphans
```
This will:
- Stop all services
- Remove containers and volumes
- Clean up any orphaned containers

If that doesn't fully clean up (e.g., some containers still exist independently), run:
```bash
docker rm -f sqlserver2022 receipt-backend receipt-frontend
```

Then rebuild everything with:
```bash
docker compose up --build
```
