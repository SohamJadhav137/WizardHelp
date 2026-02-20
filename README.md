# 🧙‍♂️ WizardHelp — Freelance Marketplace

WizardHelp is a freelance marketplace platform built with the **MERN stack**, enabling buyers to discover services, place and view orders, and communicate with sellers in real time. Sellers can create and manage gigs, handle orders, and build their reputation through reviews and ratings.

### 🌍 Experience the platform live: [wizardhelp.vercel.app](https://wizardhelp.vercel.app)

> [!WARNING]
> This website is compatible for desktop screens only. Mobile and tablet views are not supported.
---

## 📌 Table of Contents

- [Features](#-features)
- [Tech Stack](#-tech-stack)
- [System Overview](#-system-overview)
- [Getting Started](#-getting-started)
- [Environment Variables](#-environment-variables)
- [Order Flow](#-order-flow)
- [Real-Time Updates](#-real-time-updates)
- [File Uploads](#-file-uploads)
- [Security](#-security)
- [Deployment](#-deployment)
- [Screenshots](#-screenshots)

---

## ✨ Features

### Buyers
- Browse and search gigs with filters (Price/Rating/Best Selling)
- View detailed gig pages with seller info and pricing
- Place orders and provide requirements
- Chat directly with sellers (even before placing the order)
- Leave reviews after order completion

### Sellers
- Create, edit, and manage gigs
- Upload images/files to showcase work
- Set pricing, delivery time, and descriptions (Markdown supported)
- Rate the buyer after order completion

### Platform
- JWT-based authentication with role-based access control (Buyer / Seller)
- Real-time messaging powered by Socket.io
- Cloud file storage via AWS S3
- Track order status in real time
- Dynamic gig ratings with aggregated reviews

---

## 🛠 Tech Stack

### Frontend
| Tool | Purpose |
|---|---|
| React (Vite) | UI framework |
| Font Awesome | Icons |
| Lucide React | Form icons |
| Lottie Files | Animations |
| SweetAlert2 | Alerts, modals, toasts, confirmations |
| React Slick | Carousels and sliders |
| Framer Motion | Gig highlights section animations |

### Backend
| Tool | Purpose |
|---|---|
| Node.js + Express | REST API server |
| MongoDB + Mongoose | Database and ODM |
| Socket.io | Real-time bidirectional communication and live order updates |
| JWT | Authentication and session management |
| AWS S3 | Cloud file storage |

---

## 🏗 System Overview

<p align="center">
  <img src="./Screenshots/flowchart.png" width="500"/>
</p>

**Collections:** `users`, `gigs`, `orders`, `reviews`, `conversations`, `messages`

---

## 🚀 Getting Started

### Prerequisites

- Node.js v22+
- MongoDB (Atlas or local)
- AWS S3 bucket
- npm or yarn

### Clone the Repositories

```bash
# Backend
git clone https://github.com/SohamJadhav137/freelancehub-backend.git
cd freelancehub-backend
npm install

# Frontend
git clone https://github.com/SohamJadhav137/freelancehub-frontend.git
cd freelancehub-frontend
npm install
```

### Run Locally

```bash
# Backend
npm run dev      # Starts Express server (default: http://localhost:5000)

# Frontend
npm run dev      # Starts Vite dev server (default: http://localhost:5173)
```

---

## 🔐 Environment Variables

### Backend `.env`

```env
PORT=5000
MONGO_URI=your_mongodb_connection_string

JWT_SECRET=your_jwt_secret
JWT_EXPIRES_IN=7d

AWS_ACCESS_KEY_ID=your_aws_access_key
AWS_SECRET_ACCESS_KEY=your_aws_secret_key
AWS_REGION=your_aws_region
AWS_BUCKET_NAME=your_s3_bucket_name

CLIENT_URL=http://localhost:5173
```

### Frontend `.env`

```env
VITE_API_URL=http://localhost:5000/api
VITE_SOCKET_URL=http://localhost:5000
```

---

## 📦 Order Flow

Orders move through well-defined states to reflect the full transaction process.

```
Standard Flow:
Requested → Active → Delivered → Completed

With Revision:
Requested → Active → Delivered → Revision → Delivered → Completed

Declined by Seller (before activation):
Requested → Declined

Cancellation (only when order is live/Active):
Active → [Cancellation Request Sent] → Cancelled
```

| State | Description |
|---|---|
| `Requested` | Buyer placed the order; awaiting seller's response |
| `Active` | Seller accepted the order; work is in progress |
| `Delivered` | Seller submitted the deliverable |
| `Revision` | Buyer requested changes; seller revises |
| `Completed` | Buyer accepted the delivery |
| `Declined` | Seller declined before the order was initiated |
| `Cancellation Request` | Either party has requested cancellation, pending the other party's approval |
| `Cancelled` | Cancellation request was accepted; order is closed |

> [!NOTE]
> Cancellation requests can only be sent while an order is in the Active or later live states. A cancellation request must be accepted by the other party to reach the Cancelled state; otherwise, the order falls back to its previous state.

---

## 💬 Real-Time Updates

The platform uses **Socket.io** for seamless, low-latency chat between buyers and sellers.

**Key capabilities:**
- Real-time message delivery
- Real-time order status updates

---

## ☁️ File Uploads

Files (gig images, deliverables) are uploaded directly to **AWS S3**.

- The backend generates a pre-signed or direct upload URL
- File keys/URLs are stored in MongoDB
- Orphaned files are cleaned up on gig edits and deletions
- Only authorized users can upload or overwrite their own files

---

## 🔒 Security

- **JWT Authentication** — All protected routes require a valid token
- **Role-Based Access Control** — Buyers and sellers have distinct permissions enforced server-side
- **Input Validation** — Requests are validated before reaching business logic
- **Order Validation** — Buyers cannot order unpublished gigs; authorization checks run on every order action

---

## 🌐 Deployment

The frontend and backend are deployed as separate services.

| Service | Platform |
|---|---|
| Frontend | Vercel |
| Backend | Render |
| Database | MongoDB Atlas |

Each service reads from its own `.env` configuration. Secrets are never committed to version control.

---

## 📸 Screenshots

### Home Page:

<p align="center">
  <img src="./Screenshots/homepage.gif" width="650"/>
</p>


### Gig Detail Page:

<p align="center">
  <img src="./Screenshots/gig_page.gif" width="650"/>
</p>


### 📦 Order Flow:

| Stage | Buyer View | Seller View |
|-------|------------|-------------|
| Place Order | <img src="./Screenshots/gig-checkout.png" width="500"/> |  |
| Requested State | <img src="./Screenshots/order_b1.png" width="500"/> | <img src="./Screenshots/order_s1.png" width="500"/> |
| Active State | <img src="./Screenshots/order_b2.png" width="500"/> | <img src="./Screenshots/order_s2.png" width="500"/> |
| Delivered State | <img src="./Screenshots/order_b3.png" width="500"/> | <img src="./Screenshots/order_s3.png" width="500"/> |
| Revision State | <img src="./Screenshots/order_b4.png" width="500"/> | <img src="./Screenshots/order_s4.png" width="500"/> |
| Delivered (After Revision) | <img src="./Screenshots/order_b5.png" width="500"/> | <img src="./Screenshots/order_s5.png" width="500"/> |
| Completed State | <img src="./Screenshots/order_b6.png" width="500"/> | <img src="./Screenshots/order_s6.png" width="500"/> |
| Review Page | <img src="./Screenshots/review_by_buyer.png" width="500"/> | <img src="./Screenshots/review_by_seller.png" width="500"/> |


### My Orders Page:

<p align="center">
  <img src="./Screenshots/my_orders.png" width="650"/>
</p>


### Chats Page:

<p align="center">
  <img src="./Screenshots/chats.png" width="650"/>
</p>


### My Gigs Page (Seller Exclusive):

<p align="center">
  <img src="./Screenshots/my-gigs.png" width="650"/>
</p>


---

## 📄 License

Copyright © 2026 Soham Jadhav.  
This project is licensed under the [MIT License](LICENSE).

---
