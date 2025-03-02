# Особенности каскадности: какие стили имеют преимущество
CSS предлагает метод определения приоритетов.
Он основан на присвоении значений в условных единицах каждому типу селекторов:
селекторам тегов, классам, идентификаторам и т. д. Система работает так.
* Селектор тегов имеет специфичность, равную 1 условной единице.
* Класс — 10 условных единиц.
* Идентификатор — 100 условных единиц.
* Строчный стиль — 1000 условных единиц.  
>ПРИМЕЧАНИЕ
Математические расчеты, используемые для определения приоритетов, на самом деле немного сложнее. Но эта формула работает во всех случаях, кроме самых странных и запутанных.

[страницу](w3.org/TR/css3-selectors/#specificity).
Чтобы узнать о том, как браузеры рассчитывают приоритеты, посетите 
Чем больше числовое значение, тем выше специфичность (значимость) данного типа селектора.
Предположим, вы создали три стиля:
* стиль тега для элемента img (специфичность = 1);
* класс с именем .highlight (специфичность = 10);
* идентификатор с именем #logo (специфичность = 100).
Веб-страница содержит следующий HTML-код: `<img id="logo" class="highlight" src="logo.gif" />`.
Если определить одинаковый атрибут во всех трех стилях (например, свойство border), то будет применен стиль идентификатора (#logo), как
наиболее специфичного.
>ПРИМЕЧАНИЕ
Псевдоэлемент (например, ::first-child) обрабатывается браузером как селектор тегов и 
имеет специфичность 1 пункт. Псевдокласс (например, :link) рассматривается как класс и имеет 
специфичность 10 пунктов.
Поскольку селекторы потомков состоят из нескольких простых, например
content p или h2 strong, определить их специфичность сложнее: необходимо вычислить 
суммарное значение их приоритетов (таблица ниже).

| Селектор                          	| Идентификатор 	| Класс 	| Тег 	| Итого 	|
|-----------------------------------	|---------------	|-------	|-----	|-------	|
| p                                 	| 0             	| 0     	| 1   	| 1     	|
| .byline                           	| 0             	| 10    	| 0   	| 10    	|
| p.byline                          	| 0             	| 10    	| 1   	| 11    	|
| #banner                           	| 100           	| 0     	| 0   	| 100   	|
| #banner                           	| p             	| 100   	| 0   	| 1     	|
| #banner                           	| .byline       	| 100   	| 10  	| 0     	|
| a:link                            	| 0             	| 10    	| 1   	| 11    	|
| p:first-line                      	| 0             	| 0     	| 2   	| 2     	|
| h2                                	| strong        	| 0     	| 0   	| 2     	|
| #wrapper #content .byline a:hover 	| 200           	| 20    	| 1   	| 221   	|

# С чистого листа
Браузеры применяют к элементам свои собственные стили: например, шрифт заголовков h1 крупнее h2, они оба выделены полужирным начертанием, в то время как шрифт
текста абзацев меньше и не выделен полужирным шрифтом; ссылки подчеркнутые
и имеют синий цвет, а у маркированных списков есть отступ. 

В стандарте HTML нет ничего, что бы определяло все это форматирование: браузеры просто добавляют его
для того, чтобы простой HTML-код при визуализации был более читабельным. Разные браузеры обрабатывают элементы очень похоже, но все же неодинаково.
Так, например, браузеры Chrome и Firefox используют свойство padding для
создания отступа в маркированных списках, а Internet Explorer применяет свойство
margin. Кроме того, вы сможете найти небольшие различия в размерах элементов
в разных браузерах и обнаружить вовсе вводящее в заблуждение использование
отступов самыми распространенными на сегодняшний день браузерами. Из-за этих
несоответствий вы столкнетесь с проблемами, когда, например, Firefox добавит
отступ от верхнего края, а Internet Explorer этого не сделает. Такого рода проблемы
не ваша вина — они вытекают из различий стилей, встроенных в браузер.
Для предотвращения несоответствия стилей между браузерами лучше всего
начинать таблицу стилей с чистого листа. Другими словами, удалить встроенное
в браузер форматирование и добавить собственное. Концепция устранения стилей
браузера называется сбросом стилей (CSS Reset).
В частности, есть базовый набор стилей, который вы должны включить в верхнюю
часть своей таблицы стилей. Они устанавливают базовые значения для свойств, которые обычно по-разному обрабатываются во всех браузерах.

# Сброс стандартных стилей и создание стилей с чистого листа
Для начала взгляните на страницу, с которой будете работать.
1. В браузере откройте страницу cascade.html из папки 05.
Страница выглядит очень просто: две колонки (одна из них на синем фоне)
и много однотипного текста. К этому файлу уже применялись некоторые стили,
поэтому откройте CSS-код в текстовом редакторе и просмотрите его.
2. Откройте файл reset.css из папки 05 в текстовом либо в HTML-редакторе.
Это и есть концепция устранения стилей браузера - сброс стилей (CSS Reset) посредством
подключения внешних стелей reset.css. 
Это достаточно популярная практика в веб-разработке, в Интернете можно найти
множество вариаций данного reset.css. 
3. Теперь откройте файл styles.css, в нем уже есть несколько стилей. 
Два стиля — классы .main и .sidebar — создают две колонки.
HTML-код разделен на два элемента div, каждый из которых имеет собственный класс. Стили классов здесь, по существу, размещают
два элемента div так, чтобы они отображались друг рядом с другом в виде
колонок.
Сначала вы добавите пару стилей, чтобы улучшить общий вид страницы и ее
верхний заголовок.
4. В файле styles.css добавьте два этих стиля в нижней части таблицы стилей
после строки с последней скобкой } класса .sidebar:
Сброс стандартных стилей на этой странице устраняет небольшие различия в том,
как разные браузеры отображают основные HTML-элементы. Устраняются и любые различия
в отображении элементов. Ваша задача — взять пустой холст и отформатировать
элементы нужным образом
    ```
    body {
        color: #B1967C;
        font-family: "Palatino Linotype", Baskerville, serif;
        padding-top: 115px;
        background: #CDE6FF url(images/bg_body.png) repeat-x;
        max-width: 800px;
        margin: 0 auto;
    }
    h1 {
        font-size: 3em;
        font-family: "Arial Black", Arial, sans-serif;
        margin-bottom: 15px;
    }
    ```
    Первый стиль добавляет фоновое изображение и цвет для страницы, а также
устанавливает для нее фиксированную ширину. Сохранив этот файл и просмотрев
страницу cascade.html в браузере, вы заметите, что эти атрибуты
не наследуются другими элементами — то же изображение, например, не повторяется за элементами заголовков или абзацев.
Свойства font-family и color, с другой стороны, наследуются, так что другие
элементы на странице теперь используют шрифт Arial и имеют коричневый
цвет. Тем не менее вы увидите, что, хоть верхний заголовок такого же цвета, как
и остальной текст на странице, у него другой шрифт — вот наглядный пример каскадности в действии. Для форматирования элемента h1 не было назначено никакого цвета, так что заголовок наследует коричневый цвет, который был применен к элементу body. Но, 
поскольку стиль элемента h1 определяет шрифт, он замещает унаследованный шрифт от стиля body.

Наследование и каскадность в действии: элемент h1 в верхней части получившейся страницы
наследует цвет шрифта из форматирования элемента body, но получает размер и шрифт
из стиля элемента h1.
# Комбинирование стилей
В этом примере мы создадим два стиля. Один стиль будет форматировать все заголовки второго уровня веб-страницы, а другой — более специфичный стиль — будет повторно форматировать те же заголовки, но в крупной, главной колонке веб-страницы.
1. В файле styles.css добавьте следующий стиль в конец таблицы стилей:
    ```
    h2 {
        font-size: 2.2em;
        color: #AFC3D6;
        margin-bottom: 5px;
    }
    ```
    Этот стиль меняет цвет и увеличивает размер шрифта элемента h2, а также добавляет немного пространства под ним. Если вы просмотрите страницу в браузере,
    то увидите, что заголовки h2 из основной колонки и те же заголовки из правой
    колонки похожи друг на друга.
    Далее вы создадите стиль для форматирования только тех заголовков второго
    уровня, которые находятся в главной колонке.
2. Вернитесь в HTML-редактор к файлу styles.css, щелкните кнопкой мыши сразу после стиля элемента h2 и нажмите клавишу Enter, чтобы перейти на новую строку. Добавьте следующий стиль:
    ```
    .main h2 {
        color: #E8A064;
        border-bottom: 2px white solid;
        background: url(images/bullet_flower.png) no-repeat;
        padding: 0 0 2px 80px;
    }
    ```
    Вы только что создали селектор потомков, форматирующий все элементы h2, расположенные внутри элемента с классом main. Две колонки
    текста на этой странице заключаются в элементы div с разными именами классов.
    У большей, расположенной слева колонки класс носит имя main, поэтому такой
    особый стиль будет применяться только к элементам h2 внутри
    этого раздела div.
    Рассматриваемый стиль добавляет подчеркивание и значок с изображением цветка
    к заголовку. Этот стиль также определяет оранжевый цвет шрифта.
3. Сохраните таблицу стилей и снова просмотрите страницу в браузере.
    Вы заметите, что у всех заголовков второго уровня (у двух в основной колонке
    и у одного в боковой) одинаковый размер шрифта, но у тех двух, которые расположены в основной колонке, также есть подчеркивающая линия и изображение цветка.
    Поскольку стиль .main h2 специфичнее простого стиля h2, то при возникновении
    каких-либо конфликтов между двумя стилями (в данном случае в значении
    свойства color) свойства стиля .main h2 приоритетнее. Таким образом, хотя
    шрифту заголовков второго уровня в основной колонке передается синий цвет
    стиля h2, оранжевый цвет более специфичного стиля .main h2 приоритетнее.
    Однако, поскольку для стиля .main h2 не указан размер шрифта или нижнего
    поля, заголовки в основной колонке получают эти свойства от стиля h2.
# Преодоление конфликтов
Поскольку свойства каскадных таблиц стилей конфликтуют, когда несколько стилей применяются к одному и тому же элементу, 
иногда вы можете обнаружить, что ваши страницы выглядят не так, как вы этого ожидали.
Когда это происходит, вам нужно установить, почему это произошло, и внести изменения в селекторы таблиц стилей, чтобы каскадность приводила к нужному результату.
1. Вернитесь к HTML-редактору и открытому файлу styles.css.
Сейчас вы создадите новый стиль для форматирования исключительно абзацев
в основной колонке.
2. Добавьте следующий стиль в конец таблицы стилей:
    ```
    .main p {
        color: #616161;
        font-family: "Palatino Linotype", Baskerville, serif;
        font-size: 1.1em;
        line-height: 150%;
        margin-bottom: 10px;
        margin-left: 80px;
    }
    ```
    Стили h2 и .main h2 применяются к заголовкам второго уровня
    в левой колонке этой страницы, причем стиль .main h2 — только к ним
    Просмотрев страницу в браузере, вы увидите, что изменился цвет, размер и шрифт
    текста абзацев, строки растянулись (свойство line-height) и изменились отступы
    абзацев слева и снизу.
    Далее вы отформатируете первый абзац более крупным шрифтом с полужирным
    начертанием, чтобы привлечь к нему внимание. Самый простой способ отформатировать один-единственный абзац — создать класс и применить его к этому абзацу.
3. Добавьте последний стиль в конце таблицы стилей:
    ```
    .intro {
        color: #6A94CC;
        font-family: Arial, Helvetica, sans-serif;
        font-size: 1.2em;
        margin-left: 0;
        margin-bottom: 15px;
    }
    ```
    Этот стиль изменяет цвет, шрифт и размер, а также незначительно регулирует
    отступы. Все, что вы должны сделать, — применить класс к HTML-коду.
4. Откройте файл cascade.html в HTML-редакторе. Найдите элемент p, расположенный после загловка `<h1> CSS практика</h1>` и строки `<div class="main">`,
и добавьте следующий атрибут класса:
`<p class="intro">`
5. Просмотрите страницу в браузере.
И… абзац никак не изменился. Согласно правилам каскадности, .intro — простой класс, а .main p — селектор потомков и состоит из имен класса и тега. Они
добавлены для создания более специфичного стиля, поэтому его свойства решают любой конфликт между ним и стилем .intro.
Для того чтобы заработал стиль .intro, необходимо немного «укрепить» его,
наделив этот селектор большей специфичностью.
6. Вернемся к файлу styles.css в HTML-редакторе и изменим имя стиля с .intro
на p.intro.
Убедитесь в том, что между p и .intro нет пробела. По сути, вы создали связь:
.main p — один класс и один селектор тега и p.intro — один тег и один класс. Оба
имеют уровень специфичности 11, но поскольку p.intro появляется в таблице
стилей после .main p, именно этот селектор побеждает и его свойства применяются к абзацу. (Чтобы преодолеть конфликт, можно создать и еще более специфичный стиль — .main .intro.)
7. Просмотрите страницу в браузере.
Теперь в абзаце цвет шрифта сменился на голубой, шрифт изменился и стал
крупнее, а также отсутствует отступ слева. Если бы у вас не было четкого понимания принципа каскадности, вы бы ломали голову над тем, почему этот класс
не работал в первый раз.

На рисунке ниже представлен окончательный вид страницы.

![result](https://github.com/julia9961/css-lessons/blob/master/05/result.png)