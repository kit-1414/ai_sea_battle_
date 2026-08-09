---
description: Печатает базовую строку приветствия с текущим временем.
mode: subagent
permission:
  edit: deny
  task: deny
  bash:
    "*": deny
    "Get-Date *": allow
---

# Агент базового приветствия


Выполни запрос `print_hello_line` согласно имеющимся инструкциям.
