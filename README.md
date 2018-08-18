jQuery YoutubeVideo Plugin <sup>1.0.2</sup>
-------
_Позволяет встраивать на сайт видеоролики с YouTube более простым и оптимальным способом с точки зрения скорости загрузки сайта и адаптивности._

* Формирование превью в стиле YouTube + возможность кастомизации элементов (обложка, продолжительность, оригинальные заголовок и описание, кнопка воспроизведения).
* Загрузка содержимого плеера по событию (экономия трафика при загрузке страницы).
* Адаптивный(возможность указывать соотношение сторон для области видео)/фиксированный режимы.
* Возможность задавать новые/переопределять опции через data-атрибуты индивидуально для каждого видео.

## Пакетные менеджеры:
```sh
# Bower
bower install --save youtubevideo-js
```

## Подключение:

1. Подключить jQuery, jquery.youtubevideo.js и css jquery.youtubevideo.css
```html
<!-- jquery.youtubevideo.css -->
<link rel="stylesheet" href="jquery.youtubevideo.css"/>

<!-- jQuery -->
<script src="libs/jquery/dist/jquery.min.js"></script>

<!-- jQuery ajaxTransport XDomainRequest (для некотор. опций, если нужна поддержка IE8 и IE9) -->
<script src="libs/jQuery-ajaxTransport-XDomainRequest/jquery.xdomainrequest.min.js"></script>

<!-- jquery.youtubevideo.js -->
<script src="jquery.youtubevideo.js"></script>
```
2. Задать HTML-элемент, в который будет помещен видеоролик.<br>
В атрибуте "data-ytb-vedeo" указать [ID ролика](http://bit.ly/2ANIMl2).
```html
<div class="example-video" data-ytb-video="9VDFTEoMSis"></div>
```
3. Инициализировать плагин на элементе(группе элементов):
```javascript
$('.example-video').youtubeVideo();
```

## Примеры использования:
https://wahawaher.github.io/youtubeVideo/

## Параметры:

Опция | Тип | Поум. | Описание
------ | ---- | ------- | -----------
`type` | string | 'video' | Тип встраиваемого видео:<br><br>**'video'** - обычный видеоролик и **'playlist'**.
`cover` | string \| array \| boolean(false) | 'mqdefault' | Обложка видео.<br><br>**false** - не загружать обложку<br><br>**'default'** - 120x90<br>**'mqdefault'** - 320x180<br>**'hqdefault'** - 480x360 (не у всех видеороликов)<br>**'sddefault'** - 640x480 (не у всех видеороликов)<br>**'maxresdefault'** - 1280x720 (не у всех видеороликов)<br>**'http://...'** - путь к собственному изображению<br><br>Можно задать массив значений(строки: стд. значения и ссылки). В данном случае будет загружена обложка по первому доступному ключу(т.е. итоговая ссылка которого будет рабочей). Для работы с этой опцией требуется указать ключ API.
`aspectRatio` | number \| boolean(false) | 56.25 | Адаптивный режим. Подстраивает высоту ролика под ширину в определенном соотношении. Значение **false** отменяет адаптивный режим (размеры уст. вручную через css).<br><br>**56.25** - 16:9<br>**75.25** - 4:3<br>**80.25** - 5:4<br>**100** - 1:1
`playEvent` | string \| boolean(false) | 'click' | Событие, при которм будет подгружен iFrame с роликом. Поддерж. стандартные js-события. При false, iFrame будет загружаться автоматически.
`parametrs` | string | 'autoplay=1&autohide=1' | Параметры проигрывателя. Каждый новый параметр через "&"
`layout` | object | [см.<sup>1</sup>](#option-tip-layout) | Основные элементы разметки.
`api` | string | '' | Ключ YouTube Data API v3. Необходимо указывать для работы с параметрами: title, description, duration, cover. Получить ключ [здесь](https://console.developers.google.com/apis/credentials)
`title` | boolean | false | Отображение оригинального заголовка видео. HTML-элемент должен иметь атрибут "data-ytb-title-id" со значением ID видео. Может быть помещен в любое место документа. Для работы с этой опцией требуется указать ключ API.
`description` | boolean | false | Отображение оригинального описания видео. HTML-элемент должен иметь атрибут "data-ytb-descr-id" со значением ID видео. Может быть помещен в любое место документа. Для работы с этой опцией требуется указать ключ API.
`duration` | boolean | false | Отображение продолжительности видео. Для работы с этой опцией требуется указать ключ API.

_<div id="option-tip-layout">1. Основные элементы разметки:</div>_

```javascript
{
   wrap: $('<div />', { class: 'ytb-video-wrap' }),
   container: $('<div />', { class: 'ytb-video-container' }),
   iframe: $('<iframe />', { class: 'ytb-video-iframe' }),
   button: $('<div />', { class: 'ytb-video-play-button'})
      .append('<svg>...</svg>'),
}
```

## Функции обратного вызова:

Callback | Аргументы | Поум. | Описание
------ | ---- | ------- | -----------
`beforeInit` | \[sets:object\] | n/a | Перед началом инициализации.
`afterInit` | \[sets:object\] | n/a | После инициализации.
`beforeLoadIframe` | \[sets:object\] | n/a | Перед загрузкой iFrame.
`afterLoadIframe` | \[sets:object\] | n/a | По окончанию загрузки iFrame.
`afterLoadCover` | \[sets:object\] | n/a | По окончанию загрузки обложки.

```javascript
$('.example-video').youtubeVideo({
   beforeInit:       function(sets) {},
   afterInit:        function(sets) {},
   beforeLoadIframe: function(sets) {},
   afterLoadIframe:  function(sets) {},
   afterLoadCover:   function(sets) {},
});
```
## Публичные методы:
Метод | Описание
----------- | -----------
`init` | Инициализация
`reinit` | Реинициализация
`destroy` | Вернуть состояние до инита
`play` | Воспроизведение

```javascript
// Инициализация
var options = {};
$('.example-video').youtubeVideo('init', optuins);

// Реинициализация
$('.example-video').youtubeVideo('reinit'); // Реинит с текущими параметрами

var newOptions = {}; // Объект с новыми параметрами
$('.example-video').youtubeVideo('reinit', newOptions); // Реинит с новыми параметрами

// Вернуть состояние элементa/ов до инита
$('.example-video').youtubeVideo('destroy');

// Воспроизведение первого ранее инициализированного видео из выборки
$('.example-video').eq(0).youtubeVideo('play');


// Переопределение настроек по умолчанию:
$.fn.youtubeVideo.defaults = {};
```

## Зависимости:
- [jQuery](http://jquery.com/download/)
- [jQuery ajaxTransport XDomainRequest](https://github.com/MoonScript/jQuery-ajaxTransport-XDomainRequest)

## Требования
- jQuery версия 1.9.1 или выше
- jQuery ajaxTransport XDomainRequest (Cross-Domain AJAX для IE8 и IE9)

## История изменений:
### 1.0.2 - 18.08.2018
- Добавлена поддержка цепочек вызовов
- Внесены изменения в наименования некоторых переменных
- Некоторые правки в readme.md

## Лицензия (MIT)
Copyright (c) 2018 Sergey Kravchenko

Данная лицензия разрешает лицам, получившим копию данного программного обеспечения и сопутствующей документации (в дальнейшем именуемыми «Программное Обеспечение»), безвозмездно использовать Программное Обеспечение без ограничений, включая неограниченное право на использование, копирование, изменение, слияние, публикацию, распространение, сублицензирование и/или продажу копий Программного Обеспечения, а также лицам, которым предоставляется данное Программное Обеспечение, при соблюдении следующих условий:

Указанное выше уведомление об авторском праве и данные условия должны быть включены во все копии или значимые части данного Программного Обеспечения.

ДАННОЕ ПРОГРАММНОЕ ОБЕСПЕЧЕНИЕ ПРЕДОСТАВЛЯЕТСЯ «КАК ЕСТЬ», БЕЗ КАКИХ-ЛИБО ГАРАНТИЙ, ЯВНО ВЫРАЖЕННЫХ ИЛИ ПОДРАЗУМЕВАЕМЫХ, ВКЛЮЧАЯ ГАРАНТИИ ТОВАРНОЙ ПРИГОДНОСТИ, СООТВЕТСТВИЯ ПО ЕГО КОНКРЕТНОМУ НАЗНАЧЕНИЮ И ОТСУТСТВИЯ НАРУШЕНИЙ, НО НЕ ОГРАНИЧИВАЯСЬ ИМИ. НИ В КАКОМ СЛУЧАЕ АВТОРЫ ИЛИ ПРАВООБЛАДАТЕЛИ НЕ НЕСУТ ОТВЕТСТВЕННОСТИ ПО КАКИМ-ЛИБО ИСКАМ, ЗА УЩЕРБ ИЛИ ПО ИНЫМ ТРЕБОВАНИЯМ, В ТОМ ЧИСЛЕ, ПРИ ДЕЙСТВИИ КОНТРАКТА, ДЕЛИКТЕ ИЛИ ИНОЙ СИТУАЦИИ, ВОЗНИКШИМ ИЗ-ЗА ИСПОЛЬЗОВАНИЯ ПРОГРАММНОГО ОБЕСПЕЧЕНИЯ ИЛИ ИНЫХ ДЕЙСТВИЙ С ПРОГРАММНЫМ ОБЕСПЕЧЕНИЕМ.
