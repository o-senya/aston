#!/bin/bash

if [ -z "$1" ]; then
    echo "Usage: $0 <url>"
    exit 1
fi

URL=$1


curl -k -f -s --max-time 5 --connect-timeout 2 "$URL"

CURL_STATUS=$?

if [ $CURL_STATUS -eq 0 ]; then
    echo "Ресурс $URL доступен."
    exit 0 # Успешное завершение
else
    echo "Ресурс $URL недоступен. Код ошибки CURL: $CURL_STATUS"
    exit 1 # Завершение с ошибкой
fi