# JWT как документ

JWT состоит из трех блоков.
Каждый блок в рамках JWT-RTC может быть закодирован произвольным образом.
Кроме того, разметка данных в блоках может быть реализована произвольным образом.
Далее представлены общие правила формирования документа в рамказ протокола:

1. Разделитель между блоками документа должен быть однозначное отличим от контента блока.
2. Каждый блок может быть закодирован произвольным образом.
3. Каждый блок может содержать произвольную рзаметку данных.

## Подпись

Как и в случае JWT, мы вычисляем подпись документа путем вычисления хеша от сложения заголовка и тела документа через разделитель.
Основное отличие что мы не учитываем кодирование и разметку этих блоков, а берем их содеримое как есть.
А так же, поскольку разделитель может быть любым символом, мы его не учитываем.
Пример ниже содержит валидную подпись по алгоритму: `HMACSHA256(headers + payload)`, где:

* headers - список всех заголовков пакета.
* payload - контент бервого блока, включая его заголовки.

Подпись кодирована в Base64.

## Пример

```JWT
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
```

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

обратное преобразование в JWT добавит поля пакета HTTP

```JWT
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCIsIkNvbnRlbnQtVHlwZSI6WyJtdWx0aXBhcnQvZm9ybS1kYXRhIiwiYm91bmRhcnk9Qm91bmRhcnkiXX0.eyJDb250ZW50LURpc3Bvc2l0aW9uIjoiZm9ybS1kYXRhIiwiQ29udGVudC1UeXBlIjoiYXBwbGljYXRpb24vanNvbiIsIiI6eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfX0.X4mGw_oFsprylhmi9B7r72nL7SI-nyK-F1bRHOHvf1o
```

## Примецания

1. Любой разделитель тут может быть реализован как последовательность символов, основное условие - однозначное отличие от любых других последовательностей в результирующем документе.