**Bootstrap** — это самый популярный  HTML, CSS и JS фреймворк для разработки адаптивных (responsive) и mobile-first веб-сайтов.


### Главные компоненты

Bootstrap работает за счет добавления **классов** к HTML-тегам. Вам практически не нужно писать CSS, вы просто "навешиваете" готовые классы.

#### А. Сеточная система (Grid System)

Это основа верстки в Bootstrap. Экран делится на **12 колонок**.

- **Контейнер (`.container`):** Обертка, которая центрирует контент и ограничивает его ширину.
    
- **Ряд (`.row`):** Группа колонок, выстроенная горизонтально.
    
- **Колонка (`.col-*`):** Ячейка сетки.
    

**Пример логики:** Если вы хотите 3 блока в ряд на больших экранах, вы даете каждому блоку ширину 4 (потому что 4+4+4=12).

#### Б. Брейкпоинты (Breakpoints)

Классы имеют приставки для разных размеров экрана:

- `col-` (для телефонов, <576px)
    
- `col-sm-` (маленькие планшеты, ≥576px)
    
- `col-md-` (средние планшеты, ≥768px)
    
- `col-lg-` (ноутбуки, ≥992px)
    
- `col-xl-` (мониторы, ≥1200px)
    

Пример: `<div class="col-12 col-md-6">` — на телефоне блок занимает всю ширину (12), а на планшете и выше — половину (6).

#### В. Готовые компоненты

Bootstrap предоставляет стилизованные элементы:

- **Navbar:** Адаптивное меню навигации (сворачивается в "бургер" на телефоне).
    
- **Card:** Карточки товаров или статей с картинкой и текстом.
    
- **Modal:** Всплывающие (модальные) окна.
    
- **Carousel:** Слайдер изображений.
    
- **Buttons:** Красивые кнопки (`btn btn-primary`, `btn btn-success` и т.д.).
    

#### Г. Утилиты (Utility Classes)

Классы-помощники для быстрой правки стилей без CSS-файла:

- **Отступы:** `m-3` (margin), `p-2` (padding), `mt-5` (margin-top).
    
- **Цвета:** `text-danger` (красный текст), `bg-dark` (черный фон).
    
- **Текст:** `text-center`, `fw-bold` (жирный), `fs-1` (размер шрифта).
    
- **Дисплей:** `d-flex`, `d-none` (скрыть), `d-block`.


```html
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Bootstrap Пример</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
</head>
<body>

    <nav class="navbar navbar-dark bg-primary mb-4">
        <div class="container">
            <span class="navbar-brand mb-0 h1">Мой Сайт</span>
        </div>
    </nav>

    <div class="container">
        <div class="row">
            <div class="col-12 text-center">
                <h1 class="display-4">Привет, Bootstrap!</h1>
                <p class="lead">Это пример использования сетки и кнопок.</p>
                <button class="btn btn-success btn-lg">Нажми меня</button>
            </div>
        </div>

        <div class="row mt-5">
            <div class="col-md-4">
                <div class="p-3 border bg-light">Колонка 1</div>
            </div>
            <div class="col-md-4">
                <div class="p-3 border bg-light">Колонка 2</div>
            </div>
            <div class="col-md-4">
                <div class="p-3 border bg-light">Колонка 3</div>
            </div>
        </div>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>
</body>
</html>
```
### Как подключить?

1. **CDN (Самый быстрый способ):** Просто скопируйте `<link>` и `<script>` с официального сайта `getbootstrap.com` в свой `index.html`.
    
2. **Через пакетный менеджер (для проектов на Node.js/React/Vue):** `npm install bootstrap` Затем импортируете в JS файл: `import 'bootstrap/dist/css/bootstrap.min.css'`.
    
3. **Скачать файлы:** Скачать архив, распаковать и положить файлы `.css` и `.js` в папку проекта.
___
### Плюсы и минусы

|**Плюсы**|**Минусы**|
|---|---|
|Очень быстрый старт проекта.|Сайты могут выглядеть "шаблонно", если не переопределять стили.|
|Отличная документация.|Лишний вес файлов (много CSS, который вы можете не использовать).|
|Огромное сообщество (легко найти ответ на вопрос).|
