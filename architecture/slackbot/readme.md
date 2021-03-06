# SlackBot
SclackBot выполняет предобработку команд из slack чата. Проверяет, обрабатывает переданные данные из slack чата и формирует правильные запросы в другие микросервисы для успешного выполнения этой команды. Также формирует ответ в slack чат об успешном/неуспешном выполнении команды.
## Описание slack slash command
Для нужд геймификации мы используем возможности slack чата, а именно "slash command". Эти команды отличаются от обычных сообщений тем, что они начинаются со слеша (/). К примеру /away. комманда должна быть одиночным словом и не может быть пустой. Также команды могут содержать параметры. Cтруктура такой команды следующая /command param1 param2. 
Slack чат позволяет создавать собственные команды которые будут дергать нужные нам ендпоинты. Поддерживается только метод POST.
## Пример запроса от slack чата
HTTP POST
Content-type = application/x-www-form-urlencoded
```
token=ly5f23DGgzqHaNlMx48HlnQh
team_id=T0001
team_domain=example
channel_id=C2147483705
channel_name=test
user_id=U2147483697
user_name=Steve
command=/weather
text=params
response_url=https://hooks.slack.com/commands/1234/5678
```
## Валидация запросов
Все запросы от slack чата валидируются SlackBot-ом по токену.

## Правила формирования ответа slack чату.
Пользователь должен получить ответ от SlackBot-а об успешном/неуспешном выполнении команды.
Все ответы должны иметь HTTP 200 "OK" статус код. Если мы хотим сообщить дополнительную информацию то должны ее отправить строкой.

Подробней про slash commands
https://api.slack.com/slash-commands

## Технические характеристики проекта:
* JDK 8
* SpringBOOT
* SpringMVC
* jUnit
* JBot Framework

## Список фичей

### SLB-F15
***SLB-F15 Я как пользователь хочу иметь возможность получить +6 джуджиков за работу в связке. Связка это команда из четырех участников, к которым можно обратиться за помощью, а также она помогает не "отвалится" от своей первоначальной цели.***
* [дока по связкам](https://docs.google.com/document/d/190xabylJ7VW00jbxe5CHI_mns9QrGXL_2KHJeJzO6Mo/edit)

* SLB-F15-D1 необходимо проверить token, если token неверный отправить сообщение об ошибке.
* SLB-F15-D2 от кого запрос (from) в поле user_name.
* SLB-F15-D3 команда содержит один параметр - номер связки.


* SLB-F15-URL-slack
    ```
    url - "/commands/achieve/team"
    method - POST
    ```
* SLB-F15-REQ-slack
    ```
    token=...
    team_id=...
    team_domain=...
    channel_id=...
    channel_name=...
    user_id=...
    user_name=...
    command=/team
    text={teamId}
    response_url=...
    ```
    
* SLB-F15-URL-gamification
    ```
    url - "/achieve/team/{teamId}"
    method - POST
    ```
* SLB-F15-REQ-gamification
    ```
    {
        from : "..."
    }
    ```
    
* SLB-F15-S1-RSP-gamification ["..."] - массив id сохраненных ачивок.
* SLB-F15-S2-RSP-gamification ["..."] - сообщение об ошибке.

* SLB-F16-S3-REQ-slack /team 2
* SLB-F15-S4-RSP-slack "Спасибо, ваша работа в команде учтена".
* SLB-F15-S5-RSP-slack Текст ошибки.

### SLB-F16

***SLB-F16 Я как хранитель хочу иметь возможность создавать связки. 
Связка это команда состоящая из четырех человек, которая за свою работу может получить +6 джуджиков раз в неделю.
подробней о связках в SLB-F16.***

* SLB-F16-D1 в поле text должно прийти четыре slackName.
* SLB-F16-D2 slackName необходимо перевести в uuid.
* SLB-F11-D3 сформировать запрос для геймификации SLB-F16-URL-gamification и SLB-F16-REQ-gamification.
* SLB-F11-D3 отправить ответ в slack чат SLB-F16-S2-RSP-slack.


* SLB-F16-URL-slack
    ```
    url - "/commands/createteam"
    method - POST
    ```
* SLB-F16-REQ-slack
    ```
    token=...
    team_id=...
    team_domain=...
    channel_id=...
    channel_name=...
    user_id=...
    user_name=...
    command=/team
    text={teamId}
    response_url=...
    ```
    
* SLB-F16-URL-gamification
    ```
    url - "/team"
    method - POST
    ```
* SLB-F16-REQ-gamification
    ```
     {
        "from" : "..."
        "user1" : "..."
        "user2" : "..."
        "user3" : "..."
        "user4" : "..."
    }
    ```
* SLB-F16-S1-RSP-gamification ["..."] - один элемент в массиве(id - номер сформированой комманды)
* SLB-F16-S1-RSP-gamification ["..."] - сообщение об ошибке.

* SLB-F16-S1-REQ-slack /create-team @slack1 @slack2 @slack3 @slack4
* SLB-F16-S2-RSP-slack команда с {teamId} создана.
* SLB-F16-S3-RSP-slack текст ошибки.
