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


users = Router()

api = NinjaAPI(docs_url="/docs/")
api.add_router("/users/", users, tags=["users"])
```

### Schemas (Serializers)

Schemas are stored in `schemas.py`.

#### Naming Convention

- `UserSchema` for data transfer and model schemas.
- `CreateUserSchema` for create (POST) request body schema.
- `UpdateUserSchema` for update (PUT) request body schema.
- `PartialUpdateUserSchema` for partial update (PATCH) request body schema.
- `DisableUserSchema`, `EnableUserSchema`, `BanUserSchema` etc for non CRUD endpoints.

### CRUD

| Function Name       | Endpoint            | Request                 | Response            | Status Code |
| ------------------- | ------------------- | ----------------------- | ------------------- | ----------- |
| retrive_user        | `GET /users/ID/`    |                         | UserSchema          | 200         |
| list_users          | `GET /users/`       |                         | PaginatedUserSchema | 200         |
| create_user         | `POST /users/`      | CreateUserSchema        | UserSchema          | 201         |
| partial_update_user | `PATCH /users/ID/`  | PartialUpdateUserSchema | UserSchema          | 200         |
| update_user         | `PUT /users/ID/`    | UpdateUserSchema        | UserSchema          | 200         |
| delete_user         | `DELETE /users/ID/` |                         |                     | 204         |
