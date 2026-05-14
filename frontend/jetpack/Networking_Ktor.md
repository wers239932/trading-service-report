# Сетевой слой (Networking)

Вся сетевая логика инкапсулирована в классе `ApiClient`.

## Технологии
- **Ktor Client:** Используется движок `CIO`.
- **ContentNegotiation:** Автоматический парсинг JSON.
- **WebSockets:** Для получения котировок в реальном времени.

## Ключевые особенности
### Авторизация
Токен подставляется автоматически во все запросы через функцию-расширение `authHeader()`:
```kotlin
private fun HttpRequestBuilder.authHeader() {
    authToken?.let { header(HttpHeaders.Authorization, "Bearer $it") }
}
```

### Реальное время (WebSockets)
Метод `observeLivePrices` слушает поток сообщений, десериализует их в `WsMessage` и, если тип сообщения `price_update`, передает данные в UI-слой.

### Эндпоинты
- `POST /auth/login`: Вход в систему.
- `GET /market/stocks`: Получение списка акций.
- `GET /portfolio`: Получение активов пользователя.
- `POST /trades`: Выполнение сделок.
- `GET /market/stocks/{symbol}/history`: Загрузка свечных данных.
