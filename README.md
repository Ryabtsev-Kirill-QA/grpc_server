# gRPC Photo Catalog Service

gRPC-сервер на Python для загрузки и получения фотографий с поддержкой стриминга.

## Окружение

- Python
- Poetry
- Pydantic
- grpcio / grpcio-tools / grpcio-reflection

## Установка и запуск сервера

```
git clone https://github.com/Ryabtsev-Kirill-QA/grpc_server
cd grpc_server

poetry install
poetry env activate
```

```
poetry run python app/main.py
```

Сервер запустится на `0.0.0.0:50051` с поддержкой reflection API для автоматического обнаружения сервисов.

## Структура проекта

```
grpc_server/
├── app/
│   ├── __init__.py
│   ├── main.py                    # Запуск сервера
│   └── service/
│       └── photocatalog/
│           ├── __init__.py
│           ├── protocol.py         # Интерфейс репозитория
│           ├── mock_repository.py  # Mock репозиторий (эмуляция БД)
│           └── service.py          # Бизнес-логика сервиса
├── internal/
│   └── pb/                         # Сгенерированные protobuf файлы
│       ├── __init__.py
│       ├── photocatalog_pb2.py
│       ├── photocatalog_pb2.pyi
│       └── photocatalog_pb2_grpc.py
├── protos/
│   └── photocatalog.proto          # Proto спецификация
```

## Генерация кода из proto

При изменении `protos/photocatalog.proto` выполните:

```bash
python -m grpc_tools.protoc \
  -I./protos \
  --python_out=./internal/pb \
  --pyi_out=./internal/pb \
  --grpc_python_out=./internal/pb \
  ./protos/photocatalog.proto
```

## Тестирование в Postman

1. **Запустите сервер**

2. **Подключитесь к серверу**:
    - URL: `0.0.0.0:50051`
    - Благодаря reflection API методы будут доступны автоматически

3. **Доступные методы**:
    - `PhotoCatalogService.Photo` — получение фото по ID
    - `PhotoCatalogService.AddPhoto` — добавление одного фото
    - `PhotoCatalogService.RandomPhotos` — получение случайных фото (stream)
    - `PhotoCatalogService.UploadPhotos` — загрузка нескольких фото (stream)

## Документация

- [gRPC Python Documentation](https://grpc.io/docs/languages/python/)
- [Protocol Buffers](https://protobuf.dev/)
- [Презентация](https://speakerdeck.com/ozontech/valierii-mienshikov-avtotiesty-i-kodoghienieratsiia-python-kliientov-dlia-grpc-i-rest-siervisov)
