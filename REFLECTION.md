# REFLECTION — Бие даалт 14

**F.CSM311 Программ хангамжийн бүтээлт**

---

## 1. Аль assertion хамгийн их үнэ цэнэтэй санагдсан вэ? Яагаад?

Хамгийн үнэ цэнэтэй assertion нь **schema / property шалгалт** байсан. Тухайлбал:

```javascript
pm.expect(json).to.have.property("id");
pm.expect(json).to.have.property("title");
pm.expect(json.email).to.be.a("string").and.match(/@/);
```

Энэ төрлийн тест нь зөвхөн "хариу ирсэн үү" гэдгийг биш, харин **хариуны бүтэц зөв эсэх**-ийг шалгадаг. API-ийн response өөрчлөгдөх үед — жишээлбэл `id` талбарыг `postId` болгон нэрлэвэл — status code нь хэвийн 200 буцааж байхад schema assertion нь алдааг шууд илрүүлнэ. Энэ нь "амжилттай харагдах боловч бодитоор эвдэрсэн" нөхцлийг таслан зогсооно.

---

## 2. Negative test-ийн жишээ — яг ямар алдааг олох вэ?

`GET /posts/999999` руу хүсэлт явуулж **404** буцааж байгааг шалгасан тест:

```javascript
pm.test("Status 404", () => {
    pm.response.to.have.status(404);
});
pm.test("Response is empty object", () => {
    const json = pm.response.json();
    pm.expect(json).to.eql({});
});
```

Энэ тест дараах алдаануудыг илрүүлнэ:

- API байхгүй resource-д **200 буцаавал** — буруу зан төлөв
- Алдааны response-д **өөр хэн нэгний мэдээлэл** буцаавал — аюулгүй байдлын асуудал
- **500 буцаавал** — серверийн дотоод алдаа далдлагдаж байгааг илрүүлнэ

Хэвийн ажиллагааг шалгах нь хангалтгүй — алдааны зам ч бас contract тул negative test заавал байх ёстой.

---

## 3. Postman дотор pass болсон тест Newman-д fail болсон уу? Яагаад?

Тийм, болсон. Newman-д ажиллуулахад бүх тест **Invalid URI "http:///posts"** алдаатай fail болсон. Шалтгаан нь `env.dev.json` болон `env.ci.json` файлуудад `baseUrl`-ийн **Initial value** хоосон байсан.

Postman desktop дотор **Current value**-г ашигладаг тул тэнд ажиллаж байсан. Харин Newman export хийхэд зөвхөн **Initial value**-г авдаг тул `{{baseUrl}}` хоосон болж, `http:///posts` гэсэн буруу URL үүсэв.

Сургамж: Environment export хийхээсээ өмнө **Initial value** болон **Current value** хоёулаа бөглөгдсөн байгааг шалгах хэрэгтэй.

---

## 4. Token болон secret-ыг хэрхэн зохицуулсан вэ?

JSONPlaceholder auth шаардахгүй тул энэ лабораторид token байгаагүй. Гэхдээ environment-ийн соёлыг дагаж дараах аргаар зохицуулсан:

- `env.dev.json` — бодит `baseUrl` утгатай, GitHub-т оруулахдаа аюулгүй
- `env.ci.json` — CI-д зориулсан тусдаа файл, placeholder ашигласан

---

## 5. API өөрчлөгдвөл collection-ийн аль хэсэг хамгийн их эвдрэх вэ?

Хамгийн эмзэг хэсэг нь **chain хийсэн request** — `Create Post`-ын response-оос `id`, `title` авч дараагийн request-д ашигладаг хэсэг:

```javascript
pm.environment.set("createdPostId", json.id);
pm.environment.set("createdPostTitle", json.title);
```

Хэрэв API-ийн response-д `id` талбарын нэр `postId` болж өөрчлөгдвөл chain бүхэлдээ эвдэрч, түүнээс хамааралтай бүх дараагийн request ажиллахаа болино.

**Эмзэг байдлыг бууруулах арга:**

- Schema assertion-ийг **бүх request-д** нэмэх — өөрчлөлтийг хурдан илрүүлнэ
- Chain-д ашиглах талбаруудад **тусдаа тест** бичих
- API-ийн **versioning** (v1, v2) ашигладаг бол `baseUrl`-д version оруулах: `{{baseUrl}}/v1`
- Collection-ийг **тогтмол шинэчлэх** — API документац өөрчлөгдөх бүрт тест шинэчлэх