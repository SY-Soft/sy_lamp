## Xdebug и PhpStorm

### Xdebug настроен на запуск отладки по триггеру:

```ini
xdebug.mode=debug
xdebug.start_with_request=trigger
xdebug.client_host=host.docker.internal
xdebug.client_port=9003
```

### Настраиваем PhpStorm

Далее предполагается, что репозиторий sy_lamp находится в D:\sy_lamp, а новый проект будет называться laravel_my. Если у Вас другой путь или имя проекта, замените их в командах на свои.

#### 1. Запустите контейнеры и соберите PHP-образ (если еще не запускали):

```powershell
cd D:\sy_lamp\stack
docker compose up -d --build
```

#### 2. Настройка PHP
**Меню: File → Settings → PHP**

Жмем "..." напротив CLI Interpreter → "*+*" → "From Docker, Vagrant, VM, WSL, Remote..."
→ Dcoker → Image name:sy_lamp_free-php:latest → ok → apply → ok

**PHP language level:** 8.3

**CLI Interpreter:** Image name:sy_lamp_free-php:latest

#### 3. Настройка сервера

В этом же окне выбираем PHP → Servers → "*+*"

**Name:** ~^(?\<project>[a-zA-Z0-9_-]+)\.localhost$

**Host:** ~^(?\<project>[a-zA-Z0-9_-]+)\.localhost$


Включаем "Use path mappings (select if the server is remote or symlinks are used)"

Добавляем одно соответствие:

**File/Directory:** D:/sy_lamp/projects/laravel_my

**Absolute path on the server:** /var/www/projects/laravel_my

Жмем Apply → ok

### Проверяем

**1. В Вашем коде PHP ставим breakpoint - Ctrl+F8 или мышкой по № строки**

**2. Включаем "прослушивание" - Меню: Run → Start Listening for PHP Debug Connections или значек на тулбаре**

**3. В браузере открываем Ваш сайт с триггером `XDEBUG_TRIGGER=1` - 
`http://laravel_my.localhost/?XDEBUG_TRIGGER=1`**

Загрузка страницы остановится на первом breakpoint'е и PhpStorm перехватить управление и откроет окно отладки.

### Для комфортной работы избавляемся от GET-параметра `XDEBUG_TRIGGER=1`

**1. Для Вашего браузера устанавливаем плагин:**


| Chrome | Firefox | Internet Explorer | Microsoft Edge |
|---|---|---|---|
| [Xdebug Helper by JetBrains](https://chromewebstore.google.com/detail/aoelhdemabeimdhedkidlnbkfhnhgnhm) | [Xdebug Helper by JetBrains](https://addons.mozilla.org/en-GB/firefox/addon/xdebug-helper-by-jetbrains/) | [Xdebug helper](https://microsoftedge.microsoft.com/addons/detail/xdebug-helper/ggnngifabofaddiejjeagbaebkejomen) | [Xdebug Extension](https://microsoftedge.microsoft.com/addons/detail/xdebug-extension/kddfioocjgjmmambaijldfbpiaecmoco) |

**2. Включаем "прослушивание" - Меню: Run → Start Listening for PHP Debug Connections или значек на тулбаре**

**3. В браузере открываем Ваш сайт - `http://laravel_my.localhost/`**

**4. Жмем на появившиюся иконку Xdebug Helper → Отладка и обновляем страницу**

Загрузка страницы остановится на первом breakpoint'е и PhpStorm перехватить управление и откроет окно отладки.