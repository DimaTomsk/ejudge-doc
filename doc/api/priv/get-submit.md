Навигация: [Главная страница](../../Main_Page.md)/[Система ejudge](../../Система_ejudge.md)/[Использование](../../Использование.md)/[API](../API.md)/[get-submit (привилегированный)](get-submit.md)

Запрос возвращает информацию о посылке, отправленной с помощью
submit-run-input

Уровень доступа: администратор в контесте (аутентификация с помощью API
key или EJSID/session_id).

Method: `GET`

Параметры:

- `contest_id` (int) - идентификатор турнира
- `action` (string) - строка get-submit
- `submit_id` (int64) - ID посылки

Response content type: `application/json`

В случае ошибки возвращается JSON:

```jsonc
{
  "ok": false,
  "server_time": 123,          // UNIX-TIMESTAMP
  "action": "get-submit",
  "error": {
    "num": 123,                // ERROR-CODE,
    "symbol": "ERROR-SYMBOL",
    "message": "ERROR-MESSAGE"
  }
}
```

В случае успеха возвращается JSON:

```jsonc
{  
  "ok": true,
  "result": {
    "submit_id": 1,                                 // SUBMIT ID
    "user_id": 1,                                   // USER-ID
    "prob_id": 1,                                   // PROB-ID
    "lang_id": 1,                                   // LANG-ID
    "ext_user_kind": "EXT-USER-KIND",
    "ext_user": "EXT-USER",
    "notify_driver": "NOTIFY-DRIVER",
    "notify_kind" : "NOTIFY-KIND",
    "notify_queue" : "NOTIFY-QUEUE",
    "status": 0,                                    // STATUS
    "status_str": "STATUS-SHORT-STR",
    "compiler_output": "COMPILER-MESSAGES",
    "test_checker_output": "TEST-CHECKER-MESSAGES",
    "time": 1,                                      // CPU-TIME (millis)
    "real_time": 1,                                 // REAL-TIME (millis)
    "exit_code": 0,                                 // PROCESS-EXIT-CODE
    "term_signal": 0,                               // TERMINATION-SIGNAL
    "max_memory_used": 1024,                        // VIRT-MEM-USE (bytes)
    "max_rss": 1024,                                // MEMORY-USE (bytes)
    "input": "INPUT",
    "output": "OUTPUT-TEXT",
    "error": "ERROR-TEXT"
  }  
}
```

Поля `ext_user_kind`, `ext_user` содержат [внешний идентификатор пользователя](../../Внешние_идентификаторы_пользователей.md) ([3.11.0](../../Изменения_в_версии_3.11.0.md)), если он был установлен при отправке посылки.

Поля `notify_driver`, `notify_kind`, `notify_queue` содержат [идентификатор очереди сообщений](../../Нотификации_во_внешние_системы.md) ([3.11.0](../../Изменения_в_версии_3.11.0.md)), если он был установлен при отправке посылки.

Пример кода на python:

```python
import requests
import os
import urllib.parse
  
URL = 'http://HOST/cgi-bin/master'
TOKEN = 'TOKEN'
CONTEST_ID = 1
SUBMIT_ID = 50
  
headers = {
    "Authorization": "Bearer AQAA" + TOKEN
}
  
data = {
    "contest_id" : CONTEST_ID,
    "json" : 1,
    "action" : "get-submit",
    "submit_id" : SUBMIT_ID,
}
response = requests.get(URL+"?"+urllib.parse.urlencode(data), headers=headers)
print(response.json())
```
