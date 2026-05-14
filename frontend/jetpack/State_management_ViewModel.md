# Управление состоянием (ViewModel)

Класс `BrokerViewModel` координирует работу приложения и хранит его текущее состояние.

## Состояния (StateFlow)
- `isLoggedIn`: Флаг авторизации.
- `stocks`: Список акций на рынке.
- `balance`: Доступные денежные средства.
- `portfolio`: Список купленных активов.
- `stockHistory`: Словарь `Map<String, List<PriceHistoryCandle>>` для кэширования графиков.

## Логика обновления цен
При получении обновления через WebSocket, ViewModel находит нужную акцию в списке и обновляет только её цену, используя метод `.copy()`:
```kotlin
val index = currentStocks.indexOfFirst { it.symbol == update.symbol }
if (index != -1) {
    currentStocks[index] = currentStocks[index].copy(price = update.price)
    _stocks.value = currentStocks
}
```

## Инициализация данных
После входа запускаются два параллельных процесса:
1. `loadData()`: Разовая загрузка всех списков через REST.
2. `startWebSocket()`: Подписка на живые котировки.
