# Django Guidelines

## Strict Typing

Strict typing using pyright is compulsory. Add pyright section in `pyproject.toml`

```toml
[tool.pyright]
typeCheckingMode = "strict"
exclude = ["**/venv"]
```

Extra packages are necessary for typing to work properly. Use `django-stubs`.

## Django Ninja

Django Ninja is used for creating API in Django. OpenAPI and SwaggerDocs are generated automatically.

The convention for swagger docs url is `/docs/`.

```py
# core/api.py

from ninja import NinjaAPI


api = NinjaAPI(docs_url="/docs/")
```

### Schemas (Serializers)

Schemas are stored in `schemas.py`.

#### Naming Convention

- `UserDTO` for data transfer and model schemas.
- `CreateUser` for create (POST) request body schema.
- `UpdateUser` for update (PUT) request body schema.
- `PartialUpdateUser` for partial update (PATCH) request body schema.
- `DisableUser`, `EnableUser`, `BanUser` etc for non CRUD endpoints.

### CRUD

| Function Name       | Endpoint            | Request           | Response         | Status Code |
| ------------------- | ------------------- | ----------------- | ---------------- | ----------- |
| retrive_user        | `GET /users/ID/`    |                   | UserDTO          | 200         |
| list_users          | `GET /users/`       |                   | PaginatedUserDTO | 200         |
| create_user         | `POST /users/`      | CreateUser        | UserDTO          | 201         |
| partial_update_user | `PATCH /users/ID/`  | PartialUpdateUser | UserDTO          | 200         |
| update_user         | `PUT /users/ID/`    | UpdateUser        | UserDTO          | 200         |
| destroy_user        | `DELETE /users/ID/` |                   |                  | 204         |
