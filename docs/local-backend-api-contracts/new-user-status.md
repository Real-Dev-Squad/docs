## Get User Status

- **Endpoint:** `/v1/users/status/:userId`
- **Method:** GET
- **Description:** Retrieves the status of a specific user identified by their user ID.
- **Parameters:**
  - `userId` (path parameter): The unique identifier of the user whose status is to be retrieved.
- **Response:**
  - **Success (200 OK):** Returns the user's status.
    - Content-Type: application/json
    ```json
    {
        "id": "string",
        "userId": "string",
        "data": "object",
        "message": "string"
    }
    ```
  - **Error Responses:**
    - 404 Not Found: If the specified user ID does not exist.
    - 401 Unauthorized: If the request lacks valid authentication credentials.
    - 403 Forbidden: If the authenticated user does not have sufficient permissions to access the user status.

---

## Update User Status

- **Endpoint:** `/v1/users/status/:userId`
- **Method:** PATCH
- **Description:** Updates the status of a specific user identified by their user ID.
- **Parameters:**
  - `userId` (path parameter): The unique identifier of the user whose status is to be updated.
- **Request Body Validator:**
  ```javascript
  Joi.object({
      status: Joi.string()
        .trim()
        .valid(...validUserStatuses)
        .required()
        .error(new Error(`Invalid Status. the acceptable statuses are ${validUserStatuses}`)),
      appliedOn: Joi.number()
        .min(todaysTime)
        .required()
        .error(new Error(`The 'appliedOn' field must have a value that is either today or a date that follows today.`)),
      endsOn: Joi.any().when("status", {
        is: userState.OOO,
        then: Joi.number()
          .min(Joi.ref("appliedOn"))
          .required()
          .error(
            new Error(
              `The 'endsOn' field must have a value that is either 'appliedOn' date or a date that comes after 'appliedOn' day.`
            )
          ),
        otherwise: Joi.optional(),
      }),
      message: Joi.when(Joi.ref("status"), {
        is: userState.OOO,
        then: Joi.when(Joi.ref("endsOn"), {
          is: Joi.number().greater(
            Joi.ref("appliedOn", {
              adjust: (value) => value + threeDaysInMilliseconds,
            })
          ),
          then: Joi.string()
            .required()
            .error(
              new Error(`The value for the 'message' field is mandatory when State is OOO for more than three days.`)
            ),
          otherwise: Joi.optional(),
        }),
        otherwise: Joi.optional(),
      }),
    })
  ```
- **Response:**
  - **Success (200 OK):** Returns the updated user's status.
    - Content-Type: application/json
    ```json
    {
        "id": "string",
        "data": "object",
        "message": "string"
    }
    ```
  - **Error Responses:**
    - 400 Bad Request: If the request body is missing or malformed.
    - 404 Not Found: If the specified user ID does not exist.
    - 401 Unauthorized: If the request lacks valid authentication credentials.
    - 403 Forbidden: If the authenticated user does not have sufficient permissions to update the user status.