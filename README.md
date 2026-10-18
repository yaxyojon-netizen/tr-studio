# TR Studio — full stack

Indoor cycling studiyasi uchun trener ilovasi (Figma dizayni asosida): kirish (telefon + SMS), jadval (hafta/ro'yxat),
mashg'ulotlar kutubxonasi + interval konstruktori, mijozlar, statistika, jonli mashg'ulot ekrani.

## Stack
- **Backend:** Node.js 22.13+ (`node:http`, `node:sqlite`) — **hech qanday npm paket kerak emas**
- **DB:** SQLite (`data.db`, birinchi ishga tushganda avtomatik yaratiladi va demo ma'lumot bilan to'ldiriladi)
- **Frontend:** vanilla JS SPA (`public/`), hash-router, SVG grafiklar

## Ishga tushirish
```bash
node server.js          # http://localhost:3000
node server.js --reseed # demo ma'lumotni qayta yaratish
PORT=8080 node server.js
```
Dev rejimida SMS kodi doim **1234** (`NODE_ENV=production` bo'lsa tasodifiy kod yaratiladi —
`server.js` ichidagi `TODO` joyiga SMS provayder (Eskiz, Twilio…) ulang).

## API (hammasi `Authorization: Bearer <token>`, `/api/auth/request|verify` dan tashqari)
| Method | Yo'l | Izoh |
|---|---|---|
| POST | `/api/auth/request` `{phone}` | SMS kod yuborish |
| POST | `/api/auth/verify` `{phone,code}` | token + `needs_registration` |
| POST | `/api/auth/register` | profilni to'ldirish/yangilash |
| GET | `/api/me`, POST `/api/auth/logout` | |
| GET/POST | `/api/workouts?q&type&has_video…` | kutubxona |
| GET/PUT/DELETE | `/api/workouts/:id` | |
| GET | `/api/sessions?from&to&zone&trainer&workout_id` | jadval |
| POST/PUT/DELETE | `/api/sessions[/:id]` | seans CRUD |
| POST/DELETE | `/api/sessions/:id/bookings[/:clientId]` | mijozni yozish |
| POST | `/api/sessions/:id/finish` `{results[]}` | jonli mashg'ulot natijalarini saqlash |
| GET/POST | `/api/clients?q&sort&dir` | mijozlar |
| GET/PUT/DELETE | `/api/clients/:id`, GET `/api/clients/:id/stats?from&to` | profil va statistika |
| GET | `/api/stats/overview` | umumiy statistika |

## Fayllar
```
server.js   — HTTP server, router, auth, barcha endpointlar
db.js       — sxema + seed
public/     — index.html, style.css, app.js (barcha ekranlar)
```

## Dizaynda bor, lekin hali qilinmagan
- Haqiqiy SMS va PayPay to'lov integratsiyasi (hozir faqat banner)
- Jonli ekranda real datchiklar (hozir quvvat/kadens simulyatsiya qilinadi), video/musiqa/relyef pleeri
- Musobaqa (Соревнование) rejimi, raqamli reytinglar
