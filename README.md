# 🧩 22685821 - Trịnh Văn Vũ | EProject: Microservices E-Commerce System
> 🔧 **Môn học:** Lập trình hướng dịch vụ
> 👨‍💻 **Sinh viên thực hiện:** *Trịnh Văn Vũ*  
> 🆔 **MSSV:** 22685821  
> 🏫 **Đề tài:** Xây dựng hệ thống thương mại điện tử sử dụng kiến trúc Microservices  
> 🕓 **Thời gian thực hiện:** 2025
---

## 📌 Mô tả dự án
**EProject** là một hệ thống **microservices** bao gồm các service nhỏ, liên kết lỏng lẻo, phục vụ các chức năng riêng biệt:  
- **API Gateway**: Điểm đầu vào tập trung, định tuyến request, xác thực người dùng.  
- **Auth Service**: Quản lý người dùng, JWT authentication.  
- **Product Service**: Quản lý sản phẩm, lưu trữ trên MongoDB.  
- **Order Service**: Quản lý đơn hàng, xử lý bất đồng bộ qua RabbitMQ.  
- **MongoDB**: Lưu trữ dữ liệu của tất cả service.  
- **RabbitMQ**: Hỗ trợ pub/sub giữa các service (event-driven architecture).  

Tất cả được **đóng gói & triển khai bằng Docker**, đảm bảo dễ dàng mở rộng, triển khai và bảo trì.

---

## 🗂️ Cấu trúc thư mục
```
22685821-TrinhVanVu-EProject-main/
│
├── api-gateway/         # Entry point for external clients
├── auth/                # Authentication service (JWT, login/register)
├── product/             # Product service (CRUD, message broker)
├── order/               # Order service (consumes product/auth data)
├── utils/               # Shared helper modules
│
├── docker-compose.yml   # Docker orchestration file
├── package.json         # Root-level dependencies
└── README.md            # Project documentation
```
⚙️ Kiến trúc hệ thống
```
                       ┌────────────┐
                       │   Client   │
                       └─────┬──────┘
                             │
                      ┌──────▼──────┐
                      │ API Gateway │
                      └──────┬──────┘
       ┌────────────┬───────────────┬──────────────┐
       │            │               │              │
       ▼            ▼               ▼              ▼
      AuthSvc   ProductSvc      OrderSvc     RabbitMQ Broker
       │            │               │              │
       ▼            ▼               ▼              ▼
      authdb    productdb       orderdb       (message queue)

```
---

## ⚙️ Yêu cầu hệ thống

- Node.js >= 18  
- Docker & Docker Compose  
- Postman hoặc bất kỳ HTTP client nào để test API  

---
## ⚙️ Services Description

| Service         | Port         | Description                                  |
| --------------- | ------------ | -------------------------------------------- |
| **api-gateway** | 3003         | Central gateway routing requests to services |
| **auth**        | 3000         | Handles user authentication and JWT          |
| **product**     | 3001         | Manages product catalog and inventory        |
| **order**       | 3002         | Handles order creation and status tracking   |
| **mongo**       | 27017        | NoSQL database for persistence               |
| **rabbitmq**    | 5672 / 15672 | Message broker for service communication     |

## 🧠 Công nghệ sử dụng
---
| Thành phần           | Công nghệ               |
| -------------------- | ----------------------- |
| **Ngôn ngữ chính**   | Node.js (Express.js)    |
| **Database**         | MongoDB                 |
| **Message Broker**   | RabbitMQ                |
| **Authentication**   | JWT                     |
| **Containerization** | Docker & Docker Compose |
| **Orchestration**    | Docker Compose          |
| **Version Control**  | Git + GitHub            |
| **CI/CD**            | GitHub Actions          |
---
## 🐳 Triển khai hệ thống bằng Docker
1️⃣ Clone project
- git clone https://github.com/VanVu13/22685821-TrinhVanVu-EProject.git
- cd 22685821-TrinhVanVu-EProject-main
2️⃣ Cấu hình môi trường .env
📁 auth/.env
-PORT=
-MONGO_URI=
-JWT_SECRET=
📁 product/.env
-PORT=
-MONGO_URI=
-RABBITMQ_URI=amqp://rabbitmq:5672
-JWT_SECRET=
📁 order/.env
-PORT=
-MONGO_URI=
-RABBITMQ_URI=amqp://rabbitmq:5672
-JWT_SECRET=
3️⃣ Chạy toàn bộ hệ thống
-docker-compose up --build
## 📬 Hướng dẫn test API
-🔹 1. Đăng ký tài khoản
- POST http://localhost:3003/register
>{
  "username": "admin",
  "password": "123456"
>}
- 🔹 2. Đăng nhập để lấy token
- POST http://localhost:3003/login
>{
  "username": "admin",
  "password": "123456"
>}
- 🔹 3. Thêm sản phẩm
- POST http://localhost:3001/products
> Authorization: Bearer <token>
> {
  "name": "Laptop Dell XPS 13",
  "price": 32000000,
  "description": "UltraBook cao cấp"
> }
## 🔁 CI/CD với GitHub Actions
>File: .github/workflows/ci-cd.yml


