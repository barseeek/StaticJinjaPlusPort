# StaticJinjaPlusPort

Порт для [StaticJinjaPlus](https://github.com/MrDave/StaticJinjaPlus), учебный проект для работы с Docker

## Шаги для запуска контейнера и сборки сайта:
[Ссылка на образы](https://hub.docker.com/repository/docker/barseeek/static-jinja-plus/general)
1. Убедитесь, что Docker установлен:
   - Если у вас еще не установлен Docker, загрузите и установите его с официального сайта Docker (https://www.docker.com/).

2. Загрузите образ из Docker Hub (если это еще не сделано):
    ```bash
   docker pull barseeek/static-jinja-plus:tagname
   ```  
    
3. Запустите контейнер:
    ```bash
   cd /path/to/project_folder/
   docker run --rm -v "$(pwd)/templates:/WORKDIR/templates" -v "$(pwd)/output:/WORKDIR/build" barseeek/static-jinja-plus:tagname
   ```
   - `WORKDIR` для образов ubuntu `/opt/StaticJinjaPlus/`, для slim `/StaticJinjaPlus/`
   - `--rm`: удалить контейнер после завершения работы.
   - `-v "$(pwd)/templates:/StaticJinjaPlus/templates"`: смонтируйте папку templates с вашего локального компьютера в контейнер, чтобы docker мог получить доступ к вашим шаблонам и ассетам.
   - `-v "$(pwd)/build:/StaticJinjaPlus/build"`: смонтируйте папку `build` с вашего локального компьютера в контейнер, чтобы вы могли получить результаты работы статического генератора сайтов.
   - `tagname` замените на один из [тэгов](https://hub.docker.com/repository/docker/barseeek/static-jinja-plus/tags) из Docker Hub.
4. Проверка выходных данных:

    После запуска контейнера сгенерированные статические файлы будут доступны в папке `templates/build` (если StaticJinjaPlus использует стандартные настройки).
    Пример успешного вывода в консоль:
    ```
    Rendering about2.html...
    Rendering app.js...
    Rendering assets/style.css...
    Rendering assets/style2.css...
    Rendering faq.html...
    Rendering index.html...
    Watching '/opt/StaticJinjaPlus/templates' for changes...
    Press Ctrl+C to stop.
    ```
## Аргументы для создания образа
В Dockerfilах проекта аргументы объявляются следующим образом (ниже представлены значения по умолчанию):
```bash
ARG SJP_TAG=latest
ARG STAGE=main
ARG TEMPLATE_FOLDER=templates
```
`STAGE` задает какую версию будем скачивать, `main` или с тэгом, `SJP_TAG` задает тэг(необходимо точки менять на подчеркивания, для автоматической проверки контрольной суммы), а `TEMPLATE_FOLDER` задает папку в образе, в которой будут храниться шаблоны.
## Пример создания образа
Образ `0.1.0-slim`:
```bash
docker build -t barseeek/static-jinja-plus:0.1.0-slim -f Dockerfiles/Dockerfile_slim . --build-arg SJP_TAG=0_1_0 --build-arg STAGE=tag --build-arg TEMPLATE_FOLDER=new_templates
docker run -v "$(pwd)/build:/StaticJinjaPlus/build" -v "$(pwd)/new_templates:/StaticJinjaPlus/new_templates" -it barseeek/static-jinja-plus:0.1.0-slim
```
Образ `latest`:
```bash
docker build -t barseeek/static-jinja-plus -f Dockerfiles/Dockerfile_ubuntu . 
docker run -v "$(pwd)/build:/opt/StaticJinjaPlus/build" -v "$(pwd)/templates:/opt/StaticJinjaPlus/templates" -it barseeek/static-jinja-plus
```