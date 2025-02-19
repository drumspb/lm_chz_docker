# Ansible Role: Установка ЛМ ЧЗ (Локальный модуль «Честный знак») в Docker

Этот репозиторий содержит Ansible-роль для автоматизации установки и настройки ЛМ ЧЗ (Локальный модуль «Честный знак») в Docker. Роль выполняет следующие задачи:
- Устанавливает Docker (если не установлен).
- Создает каталоги для конфигурации, данных и логов.
- Загружает Docker-образ ЛМ ЧЗ.
- Создает и заполняет файл конфигурации `ext.ini`.
- Запускает контейнер с ЛМ ЧЗ.

---

## Требования

- Ansible 2.9 или выше.
- Целевые хосты с ОС Linux (поддерживаются Debian, Ubuntu, CentOS, RHEL).
- Docker-образ ЛМ ЧЗ (`regime_1.1.0-243-ubuntu22_amd64.tgz`), который должен быть размещен в папке `roles/lm_chz_docker/files/`.

---

## Переменные роли

Основные переменные, которые можно настроить:

| Переменная                  | Описание                                                                 | Значение по умолчанию                     |
|-----------------------------|-------------------------------------------------------------------------|-------------------------------------------|
| `docker_image_path`         | Путь к Docker-образу на локальной машине.                               | `{{ playbook_dir }}/roles/lm_chz_docker/files/regime_1.1.0-243-ubuntu22_amd64.tgz` |
| `config_dir`                | Каталог для хранения файла конфигурации `ext.ini`.                      | `/opt/lm_chz/config`                      |
| `data_dir`                  | Каталог для хранения файлов БД ЛМ ЧЗ.                                   | `/opt/lm_chz/data`                        |
| `log_dir`                   | Каталог для хранения логов ЛМ ЧЗ.                                       | `/opt/lm_chz/log`                         |
| `lm_chz_config.api.login`   | Логин для доступа к API ЛМ ЧЗ.                                          | `admin`                                   |
| `lm_chz_config.api.password`| Пароль для доступа к API ЛМ ЧЗ.                                         | `securepassword`                          |
| `lm_chz_config.remote.service_url` | Адрес центрального сервера конфигурации.                     | `https://suzrsapi.sandbox.crptech.ru`     |
| `lm_chz_config.remote.proxy_host`  | Адрес прокси-сервера (если требуется).                          | `""`                                      |
| `lm_chz_config.remote.proxy_port`  | Порт прокси-сервера (если требуется).                          | `""`                                      |
| `lm_chz_config.remote.proxy_user`  | Имя пользователя для прокси-сервера (если требуется).           | `""`                                      |
| `lm_chz_config.remote.proxy_password` | Пароль для прокси-сервера (если требуется).                | `""`                                      |
| `lm_chz_port`               | Порт, на котором будет доступен ЛМ ЧЗ.                                 | `5995`                                    |
| `container_name`            | Имя контейнера ЛМ ЧЗ.                                                  | `lm_chz_container`                        |

---

## Использование

### 1. Установка роли

Скопируйте роль в директорию `roles/` вашего Ansible-проекта:

```bash
git clone https://github.com/drumspb/lm_chz_docker.git roles/lm_chz_docker
```

### 2. Настройка переменных

Создайте плейбук и определите переменные:

```yaml
---
- hosts: all
  become: yes
  roles:
    - role: lm_chz_docker
      vars:
        lm_chz_config:
          api:
            login: "myuser"
            password: "mypassword"
          remote:
            service_url: "https://suzrsapi.sandbox.crptech.ru"
            proxy_host: "proxy.example.com"
            proxy_port: "3128"
            proxy_user: "proxyuser"
            proxy_password: "proxypassword"
```

### 3. Запуск плейбука

Запустите плейбук на целевых хостах:

```bash
ansible-playbook -i inventory playbook.yml
```

---

## Пример структуры проекта

```plaintext
project/
  roles/
    lm_chz_docker/
      tasks/
        main.yml
      templates/
        ext.ini.j2
      defaults/
        main.yml
      files/
        regime_1.1.0-243-ubuntu22_amd64.tgz
  inventory
  playbook.yml
```

---

## Логирование и проверка

После успешного выполнения роли:
- Проверьте, что контейнер запущен:
  ```bash
  docker ps
  ```
- Проверьте логи контейнера:
  ```bash
  docker logs lm_chz_container
  ```
- Убедитесь, что ЛМ ЧЗ доступен по указанному порту (по умолчанию `5995`).

---

## Лицензия

Этот проект распространяется под лицензией MIT. Подробности см. в файле [LICENSE](LICENSE).

---

## Авторы

- [Дромашко Кирилл](https://github.com/drumspb)

---

## Обратная связь

Если у вас есть вопросы или предложения, создайте [issue](https://github.com/your-repository/lm_chz_docker/issues) в репозитории.