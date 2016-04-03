# 8 Tips to help you get the Best out of Sass (Sass Good Practices)

## Предисловие

- Автор: Cathy Dutton
- Оригинал: http://www.sitepoint.com/8-tips-help-get-best-sass/
- Перевод: Sasha Zmiivets

Перевод вольный, а это значит что автор перевода вносит небольшие корректировки в текст материала для более удобного восприятия читателем.

## Начало

Sass предлагает использовать очень удобные(ну или почти) таблицы стилей.

Используя Sass эффективно вы сможете стоить масштабируемый и DRY(Don't Repeat Yoursefl) CSS код. Но используя его не корректно вы рискуете раздуть размер своих стилей, добавить ненужный и дублирующийся код.

Как лучше всего организовывать и использовать Sass читайте ниже...

### 1. Структурируйте ваш Sass-код

Изначально правильно заложенная структура это жизненно необходимая вещь для любого проекта. Использование фрагментов ([Фрагменты](http://sass-scss.ru/documentation/pravila_i_direktivi/fragmenti.html), [Partials](http://sass-lang.com/documentation/file.SASS_REFERENCE.html#partials)) позволит вам разбить CSS на более компактные и более управляемые блоки кода которые проще сопровождать и разрабатывать в последствии.


Для создания фрагмента используется знак нижнего подчеркивания в начале файла и они(фрагменты) в последствии не будут включены в итоговый CSS файл. Каждый фрагмент необходимо импортировать в __мастер__ файл(аля некий global.scss). И то что будет включено в мастер будет скомпилировано в основной __CSS__ файл.

Для демонстрации, вот пример структуры вымышленного проекта:

```
vendor/
base/
|
|-- _variables.scss
|-- _mixins.scss
|-- _placeholders.scss

framework/
modules/
global.scss
```

Такая структура позволяет с легкостью обходится с нашим сайтом. Например для добавления нового модуля, необходимо создать новый фрагмент в папке __base__ и импортировать его в global.scss с помощью  директивы __@import__.

Посмотрим на наш ключевой global.scss файл:

```
/* VENDOR - Default fall-backs and external files.
========================================================================== */

@import 'vendor/_normalize.scss';


/* BASE - Base Variable file along with starting point Mixins and Placeholders.
========================================================================== */

@import 'base/_variables.scss';
@import 'base/_mixins.scss';
@import 'base/_placeholders.scss';


/* FRAMEWORK - Structure and layout files.
========================================================================== */

@import 'framework/_grid.scss';
@import 'framework/_breakpoints.scss';
@import 'framework/_layout.scss';


/* MODULES - Re-usable site elements.
========================================================================== */

@import 'modules/_buttons.scss';
@import 'modules/_lists.scss';
@import 'modules/_tabs.scss';
```

Дополнительно как можно структурировать файлы можно почитать тут >> http://www.sitepoint.com/architecture-sass-project/

### 2. Используйте переменные в Sass более эффективно

Переменные это одно из наиболее простых возможностей, но все же часто не правильно используемых. Объявление общепринятой конвенции именования переменных имеет важное значение, без которой работа стала бы более запутанной и сложной.

Несколько правил для создания полезных переменных:

- Не будьте слишком расплывчатым, когда именуете переменные
- Придерживайтесь какой-то конвенции при именовании (BEM, SMACSS, OOCSS)
- Убедитесь что переменная используется действительно оправданно

Вот несколько правильных примеров:

```
$orange: #ffa600;
$grey: #f3f3f3;
$blue: #82d2e5;

$link-primary: $orange;
$link-secondary: $blue;
$link-tertiary: $grey;

$radius-button: 5px;
$radius-tab: 5px;
```

И плохих >>

```
$link: #ffa600;
$listStyle: none;
$radius: 5px;
```

### 3. Меньше используйте Миксины

Миксины это прекрасное средство для повторного использования кода. Однако чрезмерное использование миксинов чревато появлением кучи дубликатов что может раздуть ваш итоговый CSS файл.

Правильный пример:

```
@mixin rounded-corner($arc) {
    -moz-border-radius: $arc;
    -webkit-border-radius: $arc;
    border-radius: $arc;  
}
```

Этот миксин может быть использован в любой ситуации простым изменением значения переменной __$arc__
Пример использования:

```
.tab-button {
     @include rounded-corner(5px);
}

.cta-button {
     @include rounded-corner(8px);
}
```

Плохой пример миксина:

```
@mixin cta-button {
    padding: 10px;
    color: #fff;
    background-color: red;
    font-size: 14px;
    width: 150px;
    margin: 5px 0;
    text-align: center;
    display: block;
}
```

Этот миксин без аргументов и было лучше если бы его написали как __placeholder__, что переносит на к 4 пункту.

### 4. Используйте Плейсхолдеры(placeholders)

В отличии от миксинов, плейсхолдеры можно многократно использовать без страха дублирования кода. Эта опция делает их более удобными в итоговом CSS файле:

```
%bg-image { // объявляем сам плейсхолдер
    width: 100%;
    background-position: center center;
    background-size: cover;
    background-repeat: no-repeat;
}

.image-one {
    @extend %bg-image; // расширяем .image-one с помощью %bg-image
    background-image:url(/img/image-one.jpg");
}

.image-two {
    @extend %bg-image; // расширяем .image-two с помощью %bg-image
    background-image:url(/img/image-two.jpg");
}
```

Что компилируется в >>

```
.image-one, .image-two {
    width: 100%;
    background-position: center center;
    background-size: cover;
    background-repeat: no-repeat;
}

.image-one {
    background-image:url(/img/image-one.jpg") ;
}

.image-two {
    background-image:url(/img/image-two.jpg") ;
}
```

Повторение кода при использовании плейсхолдера будет происходить только в случае если уникальные стили будут применятся к селекторам индивидуально. Если же стили не будут применятся то на выходе они вовсе будут отсутствовать.

Учитывая пункт №3, плейсхолдеры можно использовать вместе с миксинами дабы уменьшить повторение кода оставив при этом всю пластичность миксинов...

```
/* PLACEHOLDER
============================================= */

%btn {
    padding: 10px;
    color:#fff;
    curser: pointer;
    border: none;
    shadow: none;
    font-size: 14px;
    width: 150px;
    margin: 5px 0;
    text-align: center;
    display: block;
}

/* BUTTON MIXIN
============================================= */

@mixin  btn-background($btn-background) {
    @extend %btn;
    background-color: $btn-background;
    &:hover {
        background-color: lighten($btn-background,10%);
    }
}

/* BUTTONS
============================================= */

.cta-btn {
    @include btn-background(green);
}

.main-btn {
    @include btn-background(orange);
}

.info-btn {
    @include btn-background(blue);
}
```

### 5. Используйте математические ф-ции

[Функции](http://sass-scss.ru/documentation/sassscript/funktsii.html)([Functions](http://sass-lang.com/documentation/file.SASS_REFERENCE.html#functions)) используются для выполнения математических операций. На выходе Sass ф-ций возвращается только __значение__ свойства.

Для примера, ф-ции пригодятся для калькуляции процентажа ширины некого элемента:

```
@function calculate-width ($col-span) {
    @return 100% / $col-span
}

.span-two {
    width: calculate-width(2); // spans 2 columns, width = 50%
}

.span-three {
    width: calculate-width(3); // spans 3 columns, width = 33.3%
}
```

### 6. Храните ваш код в порядке

Помещайте все миксины, плейсхолдеры и переменные в соответствующие части файла. Хранение блоков кода вместе поможет в будущем избежать проблем с поддержкой и пере использованием кода.

Все элементы(фрагменты) сайта должны хранится вместе в основной папке(base folder). Эта папка должна содержать файлы с глобальными переменными шрифтов и цветовых схем вашего сайта.

```
$font-primary: 'Roboto', sans-serif;
$font-secondary: Arial, Helvetica, sans-serif;

$color-primary: $orange;
$color-secondary: $blue;
$color-tertiary: $grey;
```

Все миксины, ф-ции и переменные определенные для конкретного модуля должны соответствовать имени __Фрагмента__

```
\\ filename: _tab.scss

$tab-radius: 5px;
$tab-color: $grey;
```

### 7. Ограничивайте вложенность

Злоупотребление вложенностью может принести значительные проблемы, начиная от усложнения CSS кода до чрезмерной каскадности. Такая ситуация может в будущем привести к необходимости использования !important директив (что не очень приветствуется).

Вот несколько золотых правил при написании вложенных стилей:

- Не использовать вложенность глубиной более 3х уровней
- Убедитесь что на выходе код чистый и предрасположен к дальнейшей поддержке
- Используйте вложенность только тогда когда это имеет действительно смысл, а не от балды

### 8. KISS([Keep It Simple, Stupid](https://ru.wikipedia.org/wiki/KISS_(%D0%BF%D1%80%D0%B8%D0%BD%D1%86%D0%B8%D0%BF)))

Ключевая мысль этой статьи - "Делайте вещи так просто как только это возможно". Цель Saas - написание чистого и все более управляемого CSS кода. Прежде объявлять какой либо миксин, переменную или ф-цию, убедитесь что это расширит ваши возможности в разработке, а не усложнит вашу работу.

Написание бесконечно длинного списка переменных без осмысленного использования или набора ф-ций которые трудно понять кому либо кроме автора, не есть хорошая идея и не принесет никакой помощи в разработке и написании DRY кода.

## p.s.
- [fb.com/zandr.dewhite](https://fb.com/zandr.dewhite)
- [github.com/zmts](https://github.com/zmts)
- [gitter.im/dev-garage/main](https://gitter.im/dev-garage/main)

Good Luck !!!
