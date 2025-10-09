# 22685821-TrinhVanVu-EProject
---

## 📌 Mô tả dự án
**EProject** là một hệ thống **microservices** bao gồm các service nhỏ, liên kết lỏng lẻo, phục vụ các chức năng riêng biệt:  
- **API Gateway**: Điểm đầu vào tập trung, định tuyến request, xác thực người dùng.  
- **Auth Service**: Quản lý người dùng, JWT authentication.  
- **Product Service**: Quản lý sản phẩm, lưu trữ trên MongoDB.  
- **Order Service**: Quản lý đơn hàng, xử lý bất đồng bộ qua RabbitMQ.  
- **MongoDB**: Lưu trữ dữ liệu của tất cả service.  
- **RabbitMQ**: Hỗ trợ pub/sub giữa các service (event-driven architecture).  

Mỗi service có thể sử dụng **ngôn ngữ, framework, database riêng** theo nhu cầu. Toàn bộ hệ thống được triển khai bằng **Docker** để đảm bảo tính độc lập và dễ quản lý.

---

## 🗂️ Cấu trúc thư mục
```
EProject/
├─ api-gateway/
├─ auth/
├─ product/
├─ order/
├─ docker-compose.yml
├─ README.md
```
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

## 🚀 Hướng dẫn chạy project

1️⃣ Clone project
```bash
git clone https://github.com/VanVu13/22685821-TrinhVanVu-EProject.git
cd 22685821-TrinhVanVu-EProject

2️⃣ Cấu hình biến môi trường .env

Mỗi service có file .env riêng:
# auth/.env
PORT=3000
MONGODB_URI=mongodb://mongo:27017/authdb
JWT_SECRET=mysecretkey

# product/.env
PORT=3001
MONGODB_URI=mongodb://mongo:27017/productdb
RABBITMQ_URI=amqp://rabbitmq:5672
JWT_SECRET=mysecretkey

# order/.env
PORT=3002
MONGODB_URI=mongodb://mongo:27017/orderdb
RABBITMQ_URI=amqp://rabbitmq:5672
JWT_SECRET=mysecretkey

3️⃣ Chạy toàn bộ hệ thống bằng Docker
docker-compose up --build
- Mỗi service, MongoDB và RabbitMQ chạy trên container riêng.
- API Gateway chạy trên http://localhost:3003, định tuyến đến các service.


4️⃣Hướng dẫn test API
1.Đăng ký tài khoản
    POST http://localhost:3000/register
    Body (JSON):
    {
      "username": "admin",
      "password": "123456"
    }
2.Đăng nhập để lấy Token
  POST http://localhost:3000/login
  Body:
  {
    "username": "admin",
    "password": "123456"
  }
  👉 Lưu token trả về để sử dụng ở các request sau (Authorization → Bearer Token)
3.Thêm sản phẩm
  POST http://localhost:3001/products
  Headers:
  Authorization: Bearer <token>
  Body:
  {
    "name": "Laptop Dell XPS 13",
    "price": 32000000,
    "description": ok
  }

4.Xem danh sách sản phẩm
  GET http://localhost:3001/products
5. Mua sản phẩm
  POST http://localhost:3003/orders
  Headers:
  Authorization: Bearer <token>
  Body:
  {
    "ids": "ID của sản phẩm",
  }
  



