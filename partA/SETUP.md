# Part A — Setup

## Сонгосон API

**JSONPlaceholder** — `https://jsonplaceholder.typicode.com`

Энэхүү API нь frontend болон тестийн зориулалттай үнэгүй, нээлттэй REST API бөгөөд хуурамч (fake) өгөгдөл буцаадаг тул бодит өгөгдлийн сан шаардахгүй.

---

## Brief

JSONPlaceholder нь posts, users, todos, comments, albums, photos гэсэн 6 төрлийн resource-тай. CRUD үйлдлийг бүгдийг дэмждэг (GET, POST, PUT, PATCH, DELETE). Бодит өгөгдөл өөрчлөгддөггүй — POST/PUT/DELETE хийсэн ч зөвхөн симуляц хийдэг.

---

## Auth

**Auth шаардахгүй.**

---

## Base URL

```
https://jsonplaceholder.typicode.com
```

Postman Environment-д `baseUrl` хувьсагчаар тохируулсан:

| Environment | baseUrl                                    |
|-------------|--------------------------------------------|
| dev         | https://jsonplaceholder.typicode.com       |
| staging     | (хоосон)                                   |
| prod        | (хоосон)                                   |