# API – Broken Access Control (IDOR)

## 🧩 Challenge Information

- Platform: Root-Me  
- Category: Web / API  
- Challenge Name: API – Broken Access  
- Vulnerability Type: Broken Access Control / IDOR  
- Interface: Swagger-based API  

---

## 🎯 Goal

The goal of this challenge is to identify access control weaknesses in an API-based application that allow an authenticated user to access resources belonging to other users.

---

## 🧠 Application Overview

The application exposes a Swagger interface that allows interaction with several API endpoints.

Available endpoints:

- `POST /api/signup` – Register a new user  
- `POST /api/login` – Authenticate a user  
- `POST /api/note` – Create a note  
- `GET /api/user` – Retrieve user information  

Authentication is required to access user-related functionality, but authorization mechanisms are not clearly defined.

---

## 🔍 Initial Analysis

A new user account named `dalli` was created using the `/api/signup` endpoint and authenticated via `/api/login`.

After authentication, the `/api/user` endpoint became accessible.  
This endpoint includes a `user_id` input field in the Swagger interface.

However, changing the value of `user_id` did not affect the response.
The API always returned information related to the currently authenticated user.

Example response:

```json
{
  "note": "",
  "userid": 2,
  "username": "dalli"
}```


🧪 Testing Attempts
Attempt 1: Manipulating HTTP Method

The HTTP method was changed from GET to POST, and a different user ID was sent in the request body.

Result:

Server responded with 405 Method Not Allowed

Indicates strict HTTP method enforcement

No change in returned data

This confirmed that the endpoint does not accept alternative methods for accessing user data.
