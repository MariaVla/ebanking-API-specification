# ebanking-API-specification

Documento original: https://drive.google.com/drive/folders/11R9irvbkaHkWViHikCM0GdM3rCXBwVgg

Referencia: [https://github.com/Medium/medium-api-docs#2-authentication](https://github.com/Medium/medium-api-docs#2-authentication)

# Login

- En esta pantalla el usuario indica su nombre de `usuario` y `password` y presiona el botón **Login.**
  - Una vez validados los campos desde la interfaz se procede a invocar el EndPoint: DatosUsuario (**INSERT_ENDPOINT_REAL, ejemplo,** `/api/endpoint/ejemplo`) que valida que el usuario exista en el sistema y que tipo de autenticación debe emplearse para el mismo.

## Ejemplo POST

```jsx
POST /api/endpoint/ejemplo HTTP/1.1
Host: api.ebanking.com
Content-Type: application/x-www-form-urlencoded
Accept: application/json
Accept-Charset: utf-8

username={{username}}&password={{password}}
```

Con los siguientes parametros:

| Parameter | Type   | Required? | Description     |
| --------- | ------ | --------- | --------------- |
| username  | string | required  | Client username |
| password  | string | required  | Client password |

### Exito:

```jsx
HTTP/1.1 201 OK
Content-Type: application/json; charset=utf-8

{
  "user": {
      "id_user": {{id_user}},
      "user_login": {{user_login}},
			"username": {{username}},
			"multiple_accounts": {{multiple_accounts}},
			"id_user_parent": {{id_user_parent}},
			"id_access_level": {{id_access_level}},
			"last_access_date": {{last_access_date}},
			"day_messages": {{day_messages}},
			"request_token": {{request_token}},
      "language": {{language}},
   }
}
```

Nota: no recibimos `access_token` o `refresh_token` ?

Con los siguientes parametros:

| Parameter         | Type     | Required? | Description                          |
| ----------------- | -------- | --------- | ------------------------------------ |
| id_user           | integer  | required  | Unique user identifier               |
| user_login        | string   | required  | INSERT_EXPLICATION                   |
| username          | string   | required  | INSERT_EXPLICATION                   |
| multiple_accounts | boolean  | required  | INSERT_EXPLICATION                   |
| id_user_parent    | integer  | required  | INSERT_EXPLICATION                   |
| id_access_level   | integer  |           | INSERT_EXPLICATION                   |
| last_access_date  | dateTime |           | INSERT_EXPLICATION                   |
| day_messages      | boolean  |           | INSERT_EXPLICATION                   |
| request_token     | boolean  |           | INSERT_EXPLICATION                   |
| language          | string   |           | Valores posibles: {"DE", "I1", "I2"} |

Ejemplo:

```jsx
HTTP/1.1 201 OK
Content-Type: application/json; charset=utf-8

{
  "user": {
     "lenguage": "DE",
     "id_user": 12345678,
     "user_login": "mvega",
			"username": "Matias Vega",
			"multiple_accounts": true,
			"id_user_parent": 1234567,
			"id_access_level": 1,
			"last_access_date": "2012-04-23T18:25:43.511Z",
			"day_messages": false,
			"request_token": true
   }
}
```

### Possible errors:

Especificar los posibles errores y sus mensajes que podemos recibir.

| Error code       | Description                                                                       |     |     |
| ---------------- | --------------------------------------------------------------------------------- | --- | --- |
| 401 Unauthorized | The accessToken is invalid, lacks the listPublications scope or has been revoked. |     |     |
| 403 Forbidden    | The request attempts to list publications for another user.                       |     |     |

Despues de esto, si devuelve okay:

- En caso de que se siga adelante con la autenticación del usuario se debe de presentar el botón
  Token Validation e internamente se invoca el EndPoint ValidarAutenticación. Como en el punto
  anterior, desde la interfaz se debe de validar que el Token contenga caracteres válidos de
  acuerdo al tipo de Token requerido.

Nota: No me queda claro del todo este proceso, pero la documentacion es similar al anterior

## Ejemplo Get

`GET https://api.ebanking.com/v1/publications/{{publicationId}}/contributors`

### Exito

```jsx
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

{
  "data": [
    {
      "publicationId": "b45573563f5a",
      "userId": "13a06af8f81849c64dafbce822cbafbfab7ed7cecf82135bca946807ea351290d",
      "role": "editor"
    },
    {
      "publicationId": "b45573563f5a",
      "userId": "1c9c63b15b874d3e354340b7d7458d55e1dda0f6470074df1cc99608a372866ac",
      "role": "editor"
    },
    {
      "publicationId": "b45573563f5a",
      "userId": "1cc07499453463518b77d31650c0b53609dc973ad8ebd33690c7be9236e9384ad",
      "role": "editor"
    },
    {
      "publicationId": "b45573563f5a",
      "userId": "196f70942410555f4b3030debc4f199a0d5a0309a7b9df96c57b8ec6e4b5f11d7",
      "role": "writer"
    },
    {
      "publicationId": "b45573563f5a",
      "userId": "14d4a581f21ff537d245461b8ff2ae9b271b57d9554e25d863e3df6ef03ddd480",
      "role": "writer"
    }
  ]
}
```

Donde un contributos es:

| Field         | Type   | Description                                                                                                |
| ------------- | ------ | ---------------------------------------------------------------------------------------------------------- |
| publicationId | string | An ID for the publication. This can be lifted from response of publications above                          |
| userId        | string | A user ID of the contributor.                                                                              |
| role          | string | Role of the user identified by userId in the publication identified by publicationId. 'editor' or 'writer' |

### Possible errors:

| Error code       | Description                                      |
| ---------------- | ------------------------------------------------ |
| 401 Unauthorized | The accessToken is invalid, or has been revoked. |
