# Flowchart: Алгоритм проведения платежа за ЖКУ

```mermaid
flowchart TD
    Start([Начало: запрос на оплату]) --> InputAccount[/Ввод номера лицевого счета/]
    InputAccount --> ValidateAccount{Счет найден в ЕРИП?}
    
    ValidateAccount -- Нет --> ShowAccountError[Вывод ошибки: счет не найден]
    ShowAccountError --> EndFail([Завершение операции])
    
    ValidateAccount -- Да --> ShowDebt[/Отображение начисленной суммы/]
    ShowDebt --> ChooseCard[/Выбор карты плательщика/]
    ChooseCard --> CheckBalance{Баланс >= Сумме?}
    
    CheckBalance -- Нет --> ShowBalanceError[Ошибка: Недостаточно средств]
    ShowBalanceError --> SuggestAnotherCard{Выбрать другую карту?}
    SuggestAnotherCard -- Да --> ChooseCard
    SuggestAnotherCard -- Нет --> EndFail
    
    CheckBalance -- Да --> HoldFunds[Блокировка суммы на карте]
    HoldFunds --> SendSMS[/Отправка SMS с кодом подтверждения/]
    SendSMS --> Confirm[Подтверждение 2FA / биометрия]
    Confirm --> ProcessPayment[Списание средств]
    ProcessPayment --> SendNotification[Передача реестра в ЖКХ]
    SendNotification --> GenerateReceipt[Формирование фискального чека]
    GenerateReceipt --> EndSuccess([Успешное завершение])
