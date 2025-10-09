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

4️⃣ Test API
Dùng Postman:
| Service | Endpoint              | Mô tả                     |
| ------- | --------------------- | ------------------------- |
| Auth    | `POST /register`      | Tạo tài khoản             |
| Auth    | `POST /login`         | Đăng nhập, nhận JWT       |
| Product | `GET /api/products`   | Lấy danh sách sản phẩm    |
| Product | `POST /api/products`  | Thêm sản phẩm             |
| Product | `POST /api/products`  | Mua sản phẩm              |



