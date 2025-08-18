# Используем официальный образ Python
FROM python:3.9-slim

# Устанавливаем рабочую директорию
WORKDIR /app

# Обновляем пакеты и устанавливаем необходимые утилиты (ping, ls, curl и т. д.)
RUN apt-get update && apt-get install -y \
    iputils-ping \
    net-tools \
    curl \
    && rm -rf /var/lib/apt/lists/*

# Копируем зависимости
COPY requirements.txt .

# Устанавливаем зависимости
RUN pip install --no-cache-dir -r requirements.txt

# Копируем остальные файлы
COPY . .

# Открываем порт, на котором работает приложение
EXPOSE 5000

# Запускаем приложение
CMD ["python", "app/app.py"]