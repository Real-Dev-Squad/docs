# Profile Service API Specifications and Contracts

Profile Service is a custom backend set up by everyone in the RDS community. It includes 3 endpoints:

1. `/health`
2. `/profile`
3. `/verify`

## Endpoint: /health

This is a `GET` endpoint that returns a message "Profile Service - Health Verified".

- **Request**

GET `/health`

- **Response**

HTTP/1.1 200 OK
Content-Type: application/json

{
"message": "Profile Service - Health Verified"
}

## Endpoint: /profile

This is a `GET` endpoint that returns a JSON object in the following format. It is a verified route, you need to verify the token sent in the header when a request is made to this route. It will be verified with the chain code provided when linked to RDS.

- **Request**
GET /profile
Authorization: Bearer <JWT_TOKEN>

- **Response**

- Success (HTTP Status 200 OK):

```json
HTTP/1.1 200 OK
Content-Type: application/json

{
    "first_name": "YOUR_FIRST_NAME",
    "last_name": "YOUR_LAST_NAME",
    "email": "YOUR_EMAIL_ID",
    "phone": "YOUR_PHONE_NUMBER",
    "yoe": YOUR_YEARS_OF_EXPERIENCE_AS_INTEGER,
    "company": "YOUR_CURRENT_COMPANY/COLLEGE",
    "designation": "YOUR_DESIGNATION",
    "github_id": "YOUR_GITHUB_ID",
    "linkedin_id": "YOUR_LINKEDIN_ID",
    "twitter_id": "YOUR_TWITTER_ID",
    "instagram_id": "YOUR_INSTAGRAM_ID",
    "website": "YOUR_WEBSITE",
    "dob": "YOUR_DATE_OF_BIRTH"
}
```

- Unauthorized (HTTP Status 401 Unauthorized):

    If the token is not provided or is invalid, the response will be:

```json
HTTP/1.1 401 Unauthorized
Content-Type: application/json

{
    "message": "Invalid Token"
}
```

- Internal Server Error (HTTP Status 500 Internal Server Error):
  - If there is any internal server error, the response will be:

    ```json
    HTTP/1.1 500 Internal Server Error
    Content-Type: application/json

    {
        "message": "Internal Server Error"
    }
    ```

## Endpoint: /verify

This is a `POST` endpoint that returns a hash. The request body will contain a key named `salt`.

- **Request**

```json
POST /verify
Content-Type: application/json

{
"salt": "SOME_SALT_VALUE"
}
```

- **Response**

- Success (HTTP Status 200 OK):

    ```json
    HTTP/1.1 200 OK
    Content-Type: application/json

    {
        "hash": "<Generated Hash>"
    }
    ```

- Internal Server Error (HTTP Status 500 Internal Server Error):
- If there is an error in generating the hash, the response will be:

    ```json
    HTTP/1.1 500 Internal Server Error
    Content-Type: application/json

    {
        "message": "Error while encryption"
    }
    ```
