# Django Guidelines

## Strict Typing

Strict typing using pyright is compulsory. Add pyright section in `pyproject.toml`

```toml
[tool.pyright]
typeCheckingMode = "strict"
exclude = ["**/migrations"]
```

Extra packages are necessary for typing to work properly. Use `django-stubs`.

## Cookiecutter

If you are starting a new project, start the project using Sandbox's [django-cookiecutter](https://github.com/sandbox-pokhara/django-cookiecutter).

## Folder Structure

```
.
└── project-name/
    ├── .gitignore
    ├── Dockerfile
    ├── manage.py
    ├── requirements.txt
    ├── taskfile.yml # alias for frequently used commands (https://taskfile.dev/)
    ├── project_name/
    │   ├── __init__.py
    │   ├── asgi.py
    │   ├── env.py # typed .env parser using pydantic_settings
    │   ├── settings.py
    │   ├── urls.py
    │   └── wsgi.py
    └── app_name/
        ├── __init__.py
        ├── actions.py # django admin actions
        ├── admin.py
        ├── api.py # django-ninja views
        ├── apps.py
        ├── buttons.py # django admin extra buttons using django-form-button
        ├── choices.py # django model choices
        ├── exceptions.py
        ├── forms.py
        ├── mixins.py # custom mixins
        ├── models.py
        ├── schemas.py # django-ninja serializers
        ├── security.py # django-ninja authentication classes
        ├── tasks.py # background tasks (celery, pydantic)
        ├── urls.py
        └── utils.py # utility functions that is not a task or a view
```

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

#### Partial Update Schema

In partial update schema, `fields_optional` to `__all__` and exclude `id`.

```py
class PartialUpdateUserSchema(ModelSchema):

    class Meta:
        model = User
        exclude = ["id"]
        fields_optional = "__all__"
```

#### Partial Update View

To allow the user to make partial updates, use `payload.dict(exclude_unset=True).items()`. This ensures that only the specified fields get updated.

```py
@users.patch("/{id}/", response={200: GenericSchema, 404: GenericSchema})
def partial_update_user(
    request: HttpRequest, id: int, payload: PatchDict[PartialUpdateUserSchema]
):
    user = get_object_or_404(User, id=id)
    for attr, value in payload.dict(exclude_unset=True).items():
        setattr(user, attr, value)
    user.save()
    return 200, GenericSchema(detail="User updated Successfully.")
```
