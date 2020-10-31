# oscript-actions-template

Шаблон нового проекта для библиотеки на OneScript

## Пример использования

Команда SilverBulleters на стриме [Инженерные практики в OpenSource на GitHub. Часть I - Библиотека на OneScript](https://www.youtube.com/watch?v=oAi8F8WFZL0) разобрала как использовать этот шаблон для библиотек OneScript.

## Автоматизация

Для автоматизации рутинных и скучных действий используется [GitHub Actions](https://github.com/features/actions).
Автоматизировано следующее:
* Запуск тестов
* Запуск анализа в SonarQube и сбор покрытия кода
* Загрузка артефактов в релиз GitHub и публикация пакета в хабе hub.oscript.io

### Секреты

Перед началом запуска workflow нужно настроить секреты в проекте (`Settings` -> `Secrets`). 
Нужны следующие переменные:
* `SONARQUBE_HOST` - ссылка на сервис SonarQube. Например: `https://open.checkbsl.org/`.
* `SONARQUBE_TOKEN` - токен доступа на сервис SonarQube. Генерируется на странице профиля в SonarQube.
* `OSHUB_TOKEN` - токен авторизации на GitHub, который подтверждает право пушить в организацию `oscript-library`.

### Блок CI

В блок входит:
* Тестирование [testing.yml](/.github/workflows/testing.yml)
* Анализ качества кода [qa.yml](/.github/workflows/qa.yml)

#### testing.yml

В текущем workflow изначально ничего менять не нужно. По умолчанию:
* Сборка на Ubuntu
* Версия OneScript 1.4.0

#### qa.yml

В текущем workflow нужно поменять следующее:
* Фильтр по названию основного проекта. В строке `if: github.repository == 'owner/project'` заменить `owner/project` 
на адрес основного проекта. Например: `oscript-library/gitsync`.

Остальное по умолчанию как в `testing.yml`. Не стоит забывать что нужно предварительно заполнить секреты `SONARQUBE_HOST` и `SONARQUBE_TOKEN`.

И последнее. В файле `sonar-project.properties` нужно сменить ключ и название проекта для SonarQube.

### Блок CD

В блок входит [release.yml](/.github/workflows/release.yml):
* Обновление `Assets` в новом / измененном релизе.
* Публикация пакета в хабе [hub.oscript.io](http://hub.oscript.io).

#### release.yml

В текущем workflow нужно поменять следующее:
* В строке `PACKAGE_MASK: project-*.ospx` заменить `project-*.ospx` на маску нового пакета. Например: `gitsync-*.ospx`.

Остальное по умолчанию как в `testing.yml`. Не стоит забывать что нужно предварительно заполнить секреты `OSHUB_TOKEN`.
