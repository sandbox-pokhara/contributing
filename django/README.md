# Django Guidelines

## Strict Typing

Strict typing using pyright is compulsory. Add pyright section in `pyproject.toml`

```toml
[tool.pyright]
typeCheckingMode = "strict"
exclude = ["**/venv"]
```

Extra packages are necessary for typing to work properly. Use `djangorestframework-stubs` and `django-stubs`.

## OpenAPI Docs

Use `drf-spectacular` to generate openapi docs.

## Routing/Views

DRF Viewsets are favored instead of APIView.

Example `urls.py`

```python
router = DefaultRouter()
router.register("app-settings", AppSettingsViewSet)
router.register("accounts", AccountViewSet)
router.register("workers", WorkerViewSet)
router.register("gold", GoldViewSet)
router.register("crystals", CrystalViewSet)
router.register("trophies", TrophyViewSet)
router.register("magic-dust", MagicDustViewSet)
router.register("games", GameViewSet)
router.register("cards", CardViewSet)
router.register("crit", CritViewSet)
router.register("analytics", AnalyticsViewSet, basename="analytics")
urlpatterns = router.urls
```

Example `views.py`

````python
class GoldViewSet(ModelViewSet[Gold]):
    queryset = Gold.objects.all().order_by("id")
    serializer_class = GoldSerializer
    permission_classes = [permissions.IsAuthenticated]


class WorkerViewSet(ModelViewSet[Worker]):
    @extend_schema(responses=GenericOutSerializer(), request=None)
    @action(detail=True, methods=["post"], url_path="assign-accounts")
    def assign_accounts(self, request: Request, pk: int | None = None):
        ...```
````

## Serializers

The default naming convention for DRF serializers is too long. Use `In`/`Out` for request/response specific serializers.

Example:

```
AssignWorkerRequestSerializer -> AssignWorkerIn
AssignWorkerResponseSerializer -> AssignWorkerOut
```
