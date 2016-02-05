# SimpleCI

SimpleCI - Система непрерывной интеграции, основанная на Docker. Основные особенности системы:
- Установка на собственный сервер
- Полная изоляция тестов - Docker-контейнеры
- Возможность добавлять неограниченное число Worker-ов
- Кэш данных тестов 
- Вывод лога билда в реальном времени
- Использование собственных образов для тестирования
- Конвейер задач. Каждый билд разделяется на несколько задач, объединенных в блоки (stage). Каждый следующий болк выполняется только, если предыдущий завершилс успешно. Это можно использовать, например, для развертывания - если тестирование прошло успешно, то выполнится задача развертывания.
- Наличие Tty - важно для некоторых проектов
- Создание билдов автоматически (при push-е в репозиторий) и вручную из интерфейса
    
# Установка
    
Установка при помощи докера проста -  скачайте файлы examples/compose/docker-compose.yml, examples/compose/parameters.env,
измените параметры конфигурации и запустите: docker-compose up.
При вервом запуске создается пользователь admin с паролем admin. Вы можете изменить логин и пароль на странице редактировании профиля.

Параметры конфигурации:
- HOST - название домена, на котором запущен инстанс SImpleCI. Используетс при генерации ключей ssh
- SECRET - секретный ключ инстанса SimpleCI.
- USE_GITHUB, GITHUB_TOKEN - интеграция с Github. Для интеграции с Github укажите ваш access token, или сгеенируйте новый токен в [профиле](https://github.com/settings/tokens)
- USE_GITLAB, GITLAB_URL, GITLAB_TOKEN - интеграция с Gitlab

# Использование

SimpleCI использует конфиграционный файл .simpleci.yml, расположенный в корне вашего проекта. Примеры конфигурационных файлов:
[Sylius](https://github.com/lewbor/Sylius/blob/master/.simpleci.yml), [Magento2](https://github.com/lewbor/magento2/blob/develop/.simpleci.yml)
TODO: Описание формата конфигурационного файла

# Архитектура

SimpleCI состоит из нескольких независимых частей:
- [frontend](https://github.com/simpleci/frontend) - Web-интерфейс приложения
- [dispatcher](https://github.com/simpleci/dispatcher) - Компонент, отвечающий за координацию между веб-интерфейсом и worker - создание задач, обработке билд-логов и т.д.
- [worker](https://github.com/simpleci/worker) - Запускает задачи

