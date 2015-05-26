# Шаблон Front-End проектов

Репозиторий, служащий шаблоном для начала работ над Front-End проектами

Компиляция и сборка осуществляется с помощью Gulp

## Требования

Для корректной работы шаблона в системе должны быть глобально установлены `bower` и `nodejs` (вместе с 'npm')

- [Установка Nodejs](https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager "Installing Node.js via package manager")
- [Установка Bower](http://bower.io/#install-bower "Install Bower")

## Установка шаблона

``` sh
$ git clone https://github.com/Enkil/template-frontend.git app-name
$ cd app-name
$ npm i -d
$ bower install
```

По окончании выполнения будут установлены все необходимые пакеты и их зависимости.
По умолчанию в шаблоне устанавливается и подключается Bootstrap 3. Однако, шаблон не накладывает никаких ограничений и не навязывает использование определенных фреймворков - с помощью `bower` иожно установить/удвлить нужные пакеты.

## Компиляция с помощью Gulp

После установки шаблона можно приступать к работе над проектом.

### Задачи Gulp
 
 - `$ gulp html` - сборка HTML проекта с использованием простейшией шаблонизации.
 - `$ gulp js` - сборка JavaScript проекта
 - `$ gulp less` - компилиция Less
 - `$ gulp images` - оптимизация изображений
 - `$ gulp svg` - оптимизация SVG
 - `$ gulp png-sprite` - создание PNG-спрайта
 - `$ gulp svg-sprite` - создание SVG-спрайта
 - `$ gulp fonts` - копирование файлов шрифтов
 - `$ gulp clean` - очистка каталога `build/`
 - `$ gulp webserver` - запуск локального веб-сервера для livereload и синхронного тестирования в разных браузерах
 - `$ gulp gh-pages`- размещение скомпилированного проекта на Github Pages
 - `$ gulp build` - полная сборка проекта
 - `$ gulp watch` - запуск задачи `webserver` и отслеживания изменений
 - `$ gulp default` - запуск задачи `watch`
 
## Общая концепция

- `src/` - каталог для размещения рабочих файлов (html-шаблоны, less-файлы, файлы и библиотки js, изображения для сборки в спрайты и т.д.)
- `build/` - каталог для размещения скомпилированной верстки
- `design/` - каталог для локального хранения файлов макета. Содержимое не отслеживается в Git и не будет в последствии залито в репозиторий

Вся работа осуществляется в каталоге `src/`. В каталоге `build/` никакие изменения не вносятся в файлы напрямую.
Back-End разработчикам и/или Заказчиками передается содержимое каталога `build/` или предоставляется доступ к репозиторию проекта.

## Концепции работы

### HTML-разметка

Задача `$ gulp html`

Страницы, которые необходимо сверстать размещаются в корне каталога `src/` (пример index.html)  
Файлы html повторяющихся блоков (например, общий header, footer, модальные окна и т.д.) размещаются в каталоге `src/html-blocks/` :
- `src/html-blocks/common` - файлы html-блоков, использующиеся на всех страницах сайта
- `src/html-blocks/common/modals` - файлы html модальных окон, использующихся на всех страницах сайта
- `src/html-blocks/pages--external` - файлы html-блоков, внешних страниц сайта (страниц, доступных любому пользователю без авторизации на сайте)
- `src/html-blocks/common--internal` - файлы html-блоков, внутренних страниц сайта (страниц, доступных залогиненному пользователю - например, страницы раздела "Личный кабинет")

При необходимости внутри данных каталогов можно вводить дополнительную структуру, при этом сборка html пройдет корректно.  

Для указания содержимое какого файла необходимо вставить используется конструкция вида  
 `//= html-blocks/folder-name/file-name.html` - содержимое файла `file-name.html` будет вставлено на место этой строки
 
Допускается использование вложенности, то есть в одном файле, содержащим код html-разметки блока, можно вызвать содержимое другого файла с блоком разметки.  
При этом необходимо учитывать правильное написание пути к файлу.  

Например,  
В файле `src/html-blocks/pages-external/header--external.html` мы хотим вызвать содержимое файла `src/html-blocks/common/modals/login.html`  
В таком случае вызов пишеться так `//= ../common/modals/login.html`
 
### Компиляция Less

Задача `$ gulp less`

