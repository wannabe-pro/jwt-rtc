# JWT-RTC поверх HTTP

Для внедрения JWT в HTTP в рамках протокола JWT-RTC используются следующие соглашения:

1. Блок заголовков из [JSON кодируется как простой список](json-as-plain.md), обратное преобразование должно включать все заголовки из пакета HTTP.
2. Пакет HTTP представляет собой [документ состоящий из трех частей](jwt-as-document.md), загловками (как принято в HTTP) и двумя блоками в теле запроса.
3. Блок тела может быть частью тела запроса, папример полем. Если используется multipart тело, то телом JWT-RTC считается весь блок.
4. При расчете подписи документа, заголовки берутся с пустой строкой в конце если она не является разделителем блоков, а multipart тело берется вместе с его загловками.
5. Блок подписи в multipart теле может содержать дополнительные заголовки, для проверки подписи используется только тело этого блока.
6. После блока подписи могут быть еще блоки или произвольные данные для выравнивания длины пакета.

### Прмер

в виде пакета HTTP c multipart телом

```HTTP
GET / HTTP/1.1
alg: HS256
typ: JWT
Content-Type: multipart/form-data; boundary=Boundary

--Boundary--
Content-Disposition: form-data
Content-Type: application/json

{
  "sub": "1234567890",
  "name": "John Doe",
  "iat": 1516239022
}
--Boundary--
Content-Disposition: form-data

VM0afv1kz7zHpjoIRCJXiIXZx3kDLBoOJKXg3vFWW64
--Boundary--
```

в виде пакета HTTP c простым телом, тут разделитель - пустая строка

```HTTP
GET / HTTP/1.1
alg: HS256
typ: JWT

{
  "sub": "1234567890",
  "name": "John Doe",
  "iat": 1516239022
}

VYVq89xXHRSA1OjG6evaNjvJxujiDPdFtMq1AglzFyk
--Boundary--
```
