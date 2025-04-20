# Пишем скрипт
```
#!/bin/bash

#Настройки
LOGFILE="/var/log/access-4560-644067.log"
STATEFILE="/var/tmp/log_script.offset"
EMAIL="admin@otus.ru"
LOCKFILE="/var/lock/log_report.lock"
SUBJECT="Log Report: $(date)"


# === Блокировка (предотвращаем повторный запуск) ===
exec 200>"$LOCKFILE"
flock -n 200 || {
    echo "Скрипт уже выполняется. Выход."
    exit 1
}

#Временные файлы
TMPDIR=$(mktemp -d)
REPORT="$TMPDIR/report.txt"
trap 'rm -rf "$TMPDIR"' EXIT

#Подготовка логов с момента последнего запуска
START=0
if [ -f "$STATEFILE" ]; then
    START=$(cat "$STATEFILE")
fi
END=$(wc -l < "$LOGFILE")
echo "$END" > "$STATEFILE"

# Извлекаем новые строки с момента последнего запуска
NEW_LOG="$TMPDIR/new_access.log"
tail -n +"$((START + 1))" "$LOGFILE" > "$NEW_LOG"

# Генерация отчета
echo "Отчет логов с $(date)" > "$REPORT"
echo >> "$REPORT"

# 1. Топ IP адресов
echo "Топ IP адресов:" >> "$REPORT"
awk '{print $1}' "$NEW_LOG" | sort | uniq -c | sort -nr | head -10 >> "$REPORT"
echo >> "$REPORT"

# 2. Топ URLs
echo "Топ URL:" >> "$REPORT"
awk '$6 == "\"GET" {print $7}' "$NEW_LOG" | sort | uniq -c | sort -nr | head -10 >> "$REPORT"
echo >> "$REPORT"

# 3. Ошибки веб-сервера/приложения (HTTP коды 4xx, 5xx)
echo "Ошибки веб-сервера/приложения (4xx/5xx):" >> "$REPORT"
awk '$9 ~ /^[45][0-9]{2}$/ {print $9}' "$NEW_LOG" | sort | uniq -c | sort -nr >> "$REPORT"
echo >> "$REPORT"

# 4. Все коды HTTP-ответов
echo "Все коды HTTP-ответов:" >> "$REPORT"
awk '$9 ~ /^[0-9]{3}$/ {print $9}' "$NEW_LOG" | sort | uniq -c | sort -nr >> "$REPORT"
echo >> "$REPORT"



# Отправка по почте
mail -s "$SUBJECT" "$EMAIL" < "$REPORT"
```
