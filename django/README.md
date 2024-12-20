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
        ├── list_filters.py # django admin custom SimpleListFilter
        ├── metrics.py # django prometheus metrics
        ├── mixins.py # custom mixins
        ├── models.py
        ├── schemas.py # django-ninja serializers
        ├── security.py # django-ninja authentication classes
        ├── tasks.py # background tasks (celery, pydantic)
        ├── urls.py
        └── utils.py # utility functions that is not a task or a view
```

## Choices

Use `models.TextChoices` or `models.IntegerChoices`.

```py
from django.db import models


class RegionChoices(models.TextChoices):
    BR = "BR", "BR"
    EUNE = "EUNE", "EUNE"
    EUW = "EUW", "EUW"
    JP = "JP", "JP"
    KR = "KR", "KR"
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

### Foreign Key in Views

```py
@games.post("/", response={201: GameSchema})
def create_game(request: HttpRequest, data: CreateGameSchema):
    payload = data.dict()
    # Cannot assign "205506": "Game.account" must be a "Account" instance.
    # To fix this error, we convert integer to object
    payload["account"] = get_object_or_404(
        Account, id=payload.pop("account")
    )
    return 201, Game.objects.create(**payload)
```

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

#### Create Schema

```py
class CreatUserSchema(ModelSchema):
    class Meta:
        model = User
        exclude = ["id", "date_created", "date_modified"]
```

#### Create View

```py
@users.post("/", response={201: UserSchema})
def create_user(request: HttpRequest, data: CreateUserSchema):
    return 201, User.objects.create(**data)
```

### Common Exception Handlers

```py
from django.core.exceptions import ValidationError
from django.db import IntegrityError
from django.http import HttpRequest
from ninja import NinjaAPI

api = NinjaAPI(docs_url="/docs/")


@api.exception_handler(IntegrityError)
def integrity_error_handler(request: HttpRequest, exc: IntegrityError):
    return api.create_response(request, {"detail": str(exc)}, status=400)


@api.exception_handler(ValueError)
def value_error_handler(request: HttpRequest, exc: ValueError):
    return api.create_response(request, {"detail": str(exc)}, status=400)


@api.exception_handler(ValidationError)
def validation_error_handler(request: HttpRequest, exc: ValidationError):
    return api.create_response(
        request,
        {"detail": " ".join([str(i) for i in exc])},
        status=400,
    )
```
