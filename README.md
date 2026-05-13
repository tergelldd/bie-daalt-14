# Бие даалт 14 — API Testing with Postman + Newman

**F.CSM311 Программ хангамжийн бүтээлт**  
**API:** JSONPlaceholder (`https://jsonplaceholder.typicode.com`)

---

## Агуулга

- Postman collection — 8 request (Posts, Users, Errors)
- Newman CLI — локал болон CI дээр ажиллуулах
- GitHub Actions — автомат CI pipeline
- HTML тайлан — `reports/api.html`

---

## Шаардлага

- [Node.js](https://nodejs.org) v20+
- [Newman](https://www.npmjs.com/package/newman)

---

## Суулгах

```bash
# Repository клонлох
git clone https://github.com/<таны_нэр>/bie-daalt-14.git
cd bie-daalt-14

# Newman суулгах
npm install -g newman newman-reporter-htmlextra
```

---

## Ажиллуулах

### Энгийн ажиллуулах
```bash
newman run postman/collection.json -e postman/env.dev.json
```

### HTML тайлантай ажиллуулах
```bash
newman run postman/collection.json \
  -e postman/env.dev.json \
  --reporters cli,htmlextra \
  --reporter-htmlextra-export reports/api.html
```

Тайланг үзэх: `reports/api.html` файлыг браузерт нээнэ.

---

## Environment тохиргоо

`postman/env.dev.json` дотор `baseUrl` утгыг шалгана:

| Хувьсагч | Утга |
|---|---|
| `baseUrl` | `https://jsonplaceholder.typicode.com` |

> ⚠️ Token шаардах API сонгосон бол `env.dev.json` дотор `REPLACE_THIS` гэж бичээд README-д заана.

---

## Файлын бүтэц

```
bie-daalt-14/
├── .github/
│   └── workflows/
│       └── api-tests.yml       # GitHub Actions CI
├── partA/
│   ├── SETUP.md                # API сонголт, тайлбар
│   └── screenshot.png          # Эхний амжилттай request
├── postman/
│   ├── collection.json         # 8 request
│   ├── env.dev.json            # Dev environment
│   └── env.ci.json             # CI environment
├── reports/
│   └── api.html                # Newman HTML тайлан
├── README.md
└── REFLECTION.md
```

---

## CI/CD

GitHub Actions дээр `push` болон `pull_request` үед автоматаар Newman ажиллана.  
Actions tab → `API tests` workflow-д үр дүнг харна.  
HTML тайлан `api-test-report` artifact болж хадгалагдана.