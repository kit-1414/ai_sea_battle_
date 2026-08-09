---
description: Печатает новый вариант строки приветствия с текущим 12-часовым временем и долями секунды.
mode: subagent
permission:
  edit: deny
  task: deny
  bash:
    "*": deny
    "Get-Date *": allow
---
# Агент базового приветствия

загрузи инструкцию print_hello_mod.md.   

Выполни запрос `print_hello_line` согласно имеющимся инструкциям.