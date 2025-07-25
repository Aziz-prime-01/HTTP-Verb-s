# 🌐 HTTP VERBS – KIRISH VA ASOSIY MA’LUMOTLAR

HTTP (HyperText Transfer Protocol) – bu vebdagi muloqot tilidir. U **klient (foydalanuvchi)** va **server (xizmat ko‘rsatuvchi)** o‘rtasidagi so‘rov va javob mexanizmini boshqaradi. Bu protokol orqali brauzer, mobil ilova yoki boshqa xizmatlar web server bilan gaplashadi.

Shu muloqotni boshqaruvchi asosiy qurollar bu – **HTTP VERB’lari** deb ataladi.

HTTP verb'lar – bu **HTTP so‘rovining niyatini** bildiruvchi komandalar. Ular RESTful API arxitekturasi uchun **asosiysiy tuzilma** hisoblanadi. Har bir verb biror amalni anglatadi: ma’lumotni olish, yuborish, yangilash yoki o‘chirish.

---



## 🎯 **Bu nima uchun kerak?**  
HTTP verb’lar **veb ilovalarning asosiy amallarini boshqarish** uchun kerak. Ular orqali RESTful API’lar orqali **ma’lumotlar bilan ishlash (CRUD)** bajariladi:

- Foydalanuvchini ko‘rish (`GET /users/1`)
- Yangi foydalanuvchi qo‘shish (`POST /users`)
- Foydalanuvchini yangilash (`PUT /users/1`)
- O‘chirish (`DELETE /users/1`)

Ya’ni ular serverga sizning niyatingizni aytadi: “Men faqat ko‘rayapman” yoki “Men o‘zgartirmoqchiman”.

---

## 🧠 **Nega bizga kerak?**  
HTTP verb’lar **tartibli va aniq API loyihalash uchun** kerak. Ular:

- REST arxitekturani **standartlashtiradi**  
- API'ni **o‘qilishi oson**, **foydalanilishi aniq** qiladi  
- Kesh, xavfsizlik, autentifikatsiya tizimlari bilan **to‘g‘ri ishlashga imkon beradi**  
- API’ni **test qilish va dokumentatsiya qilishni osonlashtiradi**

Agar verb’lar to‘g‘ri ishlatilmasa, API chalkash va ishonchsiz bo‘lib qoladi.

---

## 🧩 Asosiy HTTP verb’lar:

| Verb    | Maqsadi                              | Xavfsizmi | Idempotentmi | Tavsiya qilingan foydalanish     |
|---------|--------------------------------------|-----------|---------------|----------------------------------|
| `GET`   | Ma’lumotni olish                      | ✅         | ✅             | Profilni ko‘rish, ro‘yxatni o‘qish |
| `POST`  | Yangi resurs yaratish                | ❌         | ❌             | Yangi foydalanuvchi qo‘shish      |
| `PUT`   | Mavjud resursni to‘liq yangilash     | ❌         | ✅             | Profilni to‘liq yangilash         |
| `PATCH` | Resursning faqat bir qismini yangilash | ❌       | ✅             | Status, sozlama, maydonlarni o‘zgartirish |
| `DELETE`| Resursni o‘chirish                   | ❌         | ✅             | Foydalanuvchini o‘chirish         |
| `HEAD`  | Faqat header’larni olish (body yo‘q) | ✅         | ✅             | Fayl bor yoki yo‘qligini tekshirish |
| `OPTIONS`| Endpoint qanday metodlarni qo‘llaydi | ✅       | ✅             | API introspektsiya, CORS uchun    |

---

## 🚀 Hayotiy misol;

Tasavvur qiling: sizda mahsulotlar bazasi bor va siz REST API orqali uni boshqarasiz:

- `GET /products` → barcha mahsulotlarni ko‘rsatadi  
- `POST /products` → yangi mahsulot qo‘shadi  
- `GET /products/10` → ID-si 10 bo‘lgan mahsulotni ko‘rsatadi  
- `PUT /products/10` → ID-si 10 bo‘lgan mahsulotni to‘liq yangilaydi  
- `PATCH /products/10` → mahsulotning narxini o‘zgartiradi  
- `DELETE /products/10` → mahsulotni o‘chiradi

---

## 🧠 Muhim tushunchalar:

- **Safe (xavfsiz)** – Server holatini o‘zgartirmaydi (`GET`, `HEAD`, `OPTIONS`)  
- **Idempotent** – Bir necha marta bajarsangiz ham, natija bir xil bo‘ladi (`GET`, `PUT`, `DELETE`, `HEAD`)  
- **Non-idempotent** – Har gal bajarishda yangi natija chiqaradi (`POST`)

---

> **Xulosa –** HTTP verb’lar bu web API’larning yuragidir. Ular yordamida muloqot aniq, izchil va standartga mos bo‘ladi. Har bir verb – bu REST falsafasining bir bo‘lagi. Ularni to‘g‘ri ishlatish – mustahkam va kengaytiriladigan ilovalarning asosi.

---

# 📘 GET

## 🧠 Bu nima?
GET – bu **ma’lumotni olish** uchun ishlatiladigan HTTP verb. Serverdan resurs so‘raladi, ammo hech qanday o‘zgarish qilinmaydi.

## 🎯 Nimaga ishlatiladi?
- Profilni ko‘rish
- Mahsulotlar ro‘yxatini yuklash
- Yangiliklar maqolasini o‘qish

## 🌍 Hayotiy misol:
Siz do‘kondagi biror mahsulotni ko‘rayapsiz. Brauzer `GET /products/5` yuboradi va server sizga rasm, narx va tafsilotlarini qaytaradi.

## 💻 C# misol:
```csharp
using var client = new HttpClient();
var response = await client.GetAsync("https://api.example.com/products/5");
var content = await response.Content.ReadAsStringAsync();
Console.WriteLine(content);
```
---

# 🟢 POST

## 🧠 Bu nima?
POST – bu **yangi resurs yaratish** uchun ishlatiladi. Client tomonidan yuborilgan ma’lumot asosida serverda yangi obyekt hosil bo‘ladi.

## 🎯 Nimaga ishlatiladi?
- Foydalanuvchini ro‘yxatdan o‘tkazish
- Buyurtma berish
- Komment qo‘shish

## 🌍 Hayotiy misol:
Siz ro‘yxatdan o‘tyapsiz. Ilova `POST /users` yuboradi va server siz uchun yangi akkaunt yaratadi.

## 💻 C# misol:
```csharp
var newUser = new { name = "Ali", email = "ali@example.com" };
var content = new StringContent(JsonSerializer.Serialize(newUser), Encoding.UTF8, "application/json");
var response = await client.PostAsync("https://api.example.com/users", content);
Console.WriteLine($"Status: {response.StatusCode}");
```

---

# 🟡 PUT

## 🧠 Bu nima?
PUT – bu **mavjud resursni to‘liq yangilash** uchun ishlatiladi. Serverdagi eski obyekt to‘liq yangilanadi.

## 🎯 Nimaga ishlatiladi?
- Foydalanuvchi ma’lumotlarini to‘liq yangilash
- Sozlamalarni to‘liq qayta yozish

## 🌍 Hayotiy misol:
Siz profilingizdagi barcha ma’lumotlarni yangilaysiz. Ilova `PUT /users/10` yuboradi va to‘liq yangilangan obyektni serverga jo‘natadi.

## 💻 C# misol:
```csharp
var updatedUser = new { name = "Ali Updated", email = "ali@newmail.com" };
var content = new StringContent(JsonSerializer.Serialize(updatedUser), Encoding.UTF8, "application/json");
var response = await client.PutAsync("https://api.example.com/users/10", content);
Console.WriteLine($"Status: {response.StatusCode}");

```
---


# 🟠 PATCH

## 🧠 Bu nima?
PATCH – bu **resursning faqat kerakli qismini yangilash** uchun ishlatiladi. PUT'dan farqli ravishda, to‘liq obyekt emas, faqat o‘zgarayotgan maydonlar yuboriladi.

## 🎯 Nimaga ishlatiladi?
- Email manzilini o‘zgartirish
- Holatni yangilash (masalan: status = active)

## 🌍 Hayotiy misol:
Siz faqat email manzilingizni o‘zgartirmoqchisiz. Ilova `PATCH /users/10` yuboradi va faqat email maydonini yangilaydi.

## 💻 C# misol:
```csharp
var patchContent = new StringContent(
    "[{ \"op\": \"replace\", \"path\": \"/email\", \"value\": \"new@example.com\" }]",
    Encoding.UTF8,
    "application/json-patch+json"
);

var request = new HttpRequestMessage(new HttpMethod("PATCH"), "https://api.example.com/users/10")
{
    Content = patchContent
};

var response = await client.SendAsync(request);
Console.WriteLine($"Status: {response.StatusCode}");

```
---


# 🔴 DELETE

## 🧠 Bu nima?
DELETE – bu **resursni serverdan o‘chirib tashlash** uchun ishlatiladi. Resurs keyinchalik mavjud bo‘lmaydi.

## 🎯 Nimaga ishlatiladi?
- Akkountni o‘chirish
- Kommentni olib tashlash
- Mahsulotni bazadan olib tashlash

## 🌍 Hayotiy misol:
Siz akkauntingizni o‘chirmoqchisiz. Ilova `DELETE /users/10` yuboradi va server foydalanuvchini yo‘q qiladi.

## 💻 C# misol:
```csharp
var response = await client.DeleteAsync("https://api.example.com/users/10");
Console.WriteLine($"Status: {response.StatusCode}");

```
---

# ⚪ HEAD

## 🧠 Bu nima?
HEAD – bu `GET` ga o‘xshash, lekin **body’ni yubormaydi**. Faqat **header ma’lumotlari** olinadi.

## 🎯 Nimaga ishlatiladi?
- Fayl mavjudligini tekshirish
- Kontent hajmini aniqlash
- Kesh holatini ko‘rish

## 🌍 Hayotiy misol:
Ilova PDF fayl bor yoki yo‘qligini tekshiradi, lekin uni yuklab olmaydi.

## 💻 C# misol:
```csharp
var request = new HttpRequestMessage(HttpMethod.Head, "https://api.example.com/manual.pdf");
var response = await client.SendAsync(request);
Console.WriteLine($"Content length: {response.Content.Headers.ContentLength}");

```

---


# ⚫ OPTIONS

## 🧠 Bu nima?
OPTIONS – bu endpoint qaysi HTTP metodlarni qo‘llab-quvvatlayotganini aniqlash uchun ishlatiladi. Asosan **CORS** preflight so‘rovlarida ishlatiladi.

## 🎯 Nimaga ishlatiladi?
- Qanday metodlarga ruxsat borligini aniqlash
- Brauzerlar orasida xavfsiz almashish imkonini berish

## 🌍 Hayotiy misol:
Ilova tekshiradi: “`POST` metodi bu endpointda ruxsat etilganmi?” Shunda `OPTIONS /users` yuboriladi.
## 💻 C# misol:
```csharp
var request = new HttpRequestMessage(HttpMethod.Options, "https://api.example.com/users");
var response = await client.SendAsync(request);
Console.WriteLine($"Ruxsat etilgan metodlar: {string.Join(", ", response.Content.Headers.Allow)}");







