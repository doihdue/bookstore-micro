# Bookstore Microservices Project

Một dự án microservices được xây dựng bằng Django và FastAPI cho một hiệu sách trực tuyến với nhiều dịch vụ độc lập.

## Required Services
1. **customer-service**: Quản lý đăng ký và thông tin khách hàng (Django)
2. **cart-service**: Quản lý giỏ hàng (Django)
3. **book-service**: Quản lý sách và tồn kho (Django)
4. **staff-service**: Quản lý nhân viên (Django)
5. **order-service**: Quản lý đơn hàng (FastAPI)
6. **api-gateway**: Cổng giao tiếp và giao diện người dùng (FastAPI)

## Functional Requirements
- Customer registration automatically creates a cart
- Staff manages books
- Customer adds books to cart, view cart, update cart
- Order triggers payment and shipping
- Customer can rate books

## Technical Stack
- Django REST Framework
- FastAPI
- Docker & Docker Compose
- MySQL (single instance with multiple databases)
- REST inter-service calls

## Project Structure
```
bookstore-micro05/
├── customer-service/
│   ├── app/
│   │   ├── models.py # Django app
│   │   ├── serializers.py
│   │   ├── urls.py
│   │   └── tests.py
│   ├── customer_service/
│   │   ├── settings.py
│   │   ├── urls.py
│   │   ├── wsgi.py
│   │   └── asgi.py
│   ├── manage.py
│   ├── Dockerfile
│   └── requirements.txt
├── cart-service/
├── order-service/ # FastAPI app
├── api-gateway/ # FastAPI app
├── book-service/
├── staff-service/
├── docker-compose.yml
└── .gitignore
```

## Prerequisites
- Docker Desktop installed and running
- Windows 10/11 or Linux/macOS

## Installation & Running

### 1. Navigate to project directory
```bash
cd d:\bookstore-micro05
```

### 2. Build and start all services
```bash
docker-compose up --build
```

This will:
- Create 4 PostgreSQL databases (one per service)
- Build Docker images for each service
- Run migrations
- Start all services

### 3. Access the APIs

Once running, you can access:
- **Customer Service**: http://localhost:8001/api/customers/
- **Cart Service**: http://localhost:8002/api/carts/
- **Book Service**: http://localhost:8003/api/books/
- **Staff Service**: http://localhost:8004/api/staff/
- **API Gateway UI**: http://localhost:8080/

### 4. Example API Calls

**Create a customer** (automatically creates a cart):
```bash
curl -X POST http://localhost:8001/api/customers/ \
  -H "Content-Type: application/json" \
  -d '{"name": "John Doe", "email": "john@example.com"}'
```

**Create a book** (staff-service):
```bash
curl -X POST http://localhost:8003/api/books/ \
  -H "Content-Type: application/json" \
  -d '{"title": "Django Book", "author": "Expert", "price": 49.99, "stock": 100}'
```

**Add item to cart**:
```bash
curl -X POST http://localhost:8002/api/carts/1/ \
  -H "Content-Type: application/json" \
  -d '{"book_id": 1, "quantity": 2}'
```

### 5. Stopping Services
```bash
docker-compose down
```

## Environment Variables
Each service uses the following environment variables (configured in docker-compose.yml):
- `DEBUG`: Set to 'True' for development
- `ALLOWED_HOSTS`: Comma-separated list of allowed hosts
- `DB_ENGINE`: Database engine (django.db.backends.postgresql)
- `DB_NAME`: Database name
- `DB_USER`: Database user (postgres)
- `DB_PASSWORD`: Database password
- `DB_HOST`: Database hostname
- `DB_PORT`: Database port

## Database Schema

### Customer Service
- **Customer**: id, name, email

### Cart Service
- **Cart**: id, customer_id
- **CartItem**: id, cart_id, book_id, quantity

### Book Service
- **Book**: id, title, author, price, description, stock

### Staff Service
- **Staff**: id, name, email, role, is_active


