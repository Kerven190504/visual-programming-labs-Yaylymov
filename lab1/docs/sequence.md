# Sequence Diagram: Оплата ЖКУ

```mermaid
sequenceDiagram
    autonumber
    actor User as Пользователь
    participant App as Мобильный банк
    participant ERIP as Расчетная система (ЕРИП)
    participant Billing as Биллинг ЖКХ

    User->>App: Ввод лицевого счета
    App->>ERIP: Запрос суммы задолженности (номер счета)
    ERIP->>Billing: Валидация счета
    Billing-->>ERIP: Данные счета и начисления
    
    alt Счет не найден
        ERIP-->>App: Ошибка: лицевой счет отсутствует
        App-->>User: Сообщение об ошибке
    else Счет корректен
        ERIP-->>App: Сумма к оплате и период
        App-->>User: Отображение суммы и запрос оплаты
        User->>App: Выбор карты и подтверждение
        App->>App: Списание средств с карты
        App-->>User: Push-уведомление о списании
        App->>ERIP: Фиксация оплаты
        ERIP-->>Billing: Закрытие задолженности
        App-->>User: Электронный чек об оплате
    end
