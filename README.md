# Многослойный CSS
## Введение
Многослойная система организации CSS основана на принципах [OOCSS](http://oocss.org/) и [БЭМ](http://bem.info/). Методология родилась и развивается в команде разработчиков [Одноклассники.ру](http://odnoklassniki.ru) и рекомендуется к использованию как основа для собственного свода правил и документаций.

Несмотря на то что **MCSS** изначально был создан для организации большого количества стилей, данная методология подходит и порталам среднего размера.

Документация будет постоянно обновляться и развиваться, в том числе и в сопровождении вспомогательных инструментов, которые будут анонсированы в ближайшее время.

*С вопросами можно обращаться в раздел [Issues на Github](https://github.com/operatino/MCSS/issues) или по почте <r@rhr.me>.*

### Общие принципы
Методология **MCSS** максимально абстрагирована и не акцентирует внимание на конкретном code style, организации файлов и обязательных инструментах для работы с ней.

Блоки вёрстки делятся на модули, каждый из которых сам по себе независим, максимально абстрагирован от фиксированных размеров и имеет значение по умолчанию.

В свою очередь модули распределяются по слоям, каждый из которых имеет свой свод правил использования и взаимодействия с другими модулями.
### Расположение стилей
Стили каждого модуля должны храниться рядом друг с другом, можно их выделять как в закомментированные секции, так и в отдельные файлы:

    /* Module name
    -------------------------------------------------- */

    .module { … }

    .module_list { … }
      .module_list__modificator { … }

    /* /Module name */

Селекторы, модифицируемые каскадом, должны так же храниться рядом с родительским классом:

	/* Module name
	-------------------------------------------------- */

	.module { … }

	.module_list { … }
		.module_list .other-module { … }

	/* /Module name */

Исключением могут быть только классы из слоя контекста:

	/* Module name
	-------------------------------------------------- */

	.module { … }

	.module_list { … }
		.touch .module_list { … }
		.ie9 .module_list { … }

	/* /Module name */

Такая организация стилей упрощает дальнейший рефакторинг кода и позволяет непосвященному разработчику быстро разобраться во всех стилях модуля и быстро приступить к работе с ним.

При чистке кода, просто избавляться от старых модулей и от всех его зависимостей.
### Наименование классов
Названия классов формируются из ключевого слова / названия модуля, таким образом модуль под названием **.module_*** порождает другие связанные с ним классы и модификаторы.

Части модуля разделяются одинарным подчёркиванием: **"_"**, а модификаторы двойным: **"__"**:

	/* Module name
	-------------------------------------------------- */

	.module { … }

	.module_list { … }
		.module_list__modificator { … }

	/* /Module name */

Модификаторы могут использоваться в HTML только рядом с модифицируемым классом:

	<div class="module_list module_list__modificator"></div>

Для определения названия из нескольких слов используется дефис **"-"**:

	.big-module-name { … }
	.big-module-name_block { … }

## Фундамент
Фундамент включает в себя ресеты и мало изменяемые стили, которые закладываются в основу вёрстки и применяются на все страницы.

Стили фундамента, как и все ресеты, подключаются с самого начала, либо в отдельном файле, либо в начале единого CSS файла.
## Первый слой — базовый
Первый слой — это портальный фреймворк, ядро интерфейса, которое состоит из наиболее переиспользуемых и абстрактных конструкций, например:

* формы
* кнопки
* блоки навигации и пр.

Разметка модулей первого слоя тесно завязана на общем стиле портала и на дизайнерских гайдлайнах. При работе над базовыми конструкциями рекомендуется тесная связь с дизайнерами, особенно в решении создания общепортальных стандартов.

Внедряя методологию **MCSS** в рабочий проект, первое с чего стоит начинать — создать базу переиспользуемых стандартов.

Как часть первого слоя можно смело использовать такие популярные фреймворки, как [Bootstrap](http://twitter.github.com/bootstrap/), [960gs](http://960.gs/), [inuit.css](https://github.com/csswizardry/inuit.css) и пр.

### Правила
Основное правило первого слоя — абстрактность, как в названиях, так и в разметке:

* Названия классов этого слоя не должны выглядеть чужеродно в любом месте на портале.
* Блоки модуля должны иметь стиль по умолчанию, но должны оставаться простыми и легко модифицируемыми под задачи различных проектных модулей.

Стили первого слоя могут быть модифицированы каскадом от 1-го и 2-го слоя. Это связано с правилом расположения смежных стилей в одном месте (см. пункт "Расположение стилей").

Ядро (1-й слой) должно быть абстрагировано от модулей 2ого слоя, пример:

Cтандарт форм:

	.input-field { … }
		.input-field_text { … }

Взаимодействия стандарта форм, со стандартом кнопок - модифицирование кастадом от 1-ого слоя:

	.input-field { … }
		.input-field .button { … }

Взаимодействие проектного модуля со стандартом во 2-м слое:

	.project-module { … }
		.project-module .input-field { … }

**Модификация 2-го слоя, от 1-ого запрещена!** В такой ситуации слои смешиваются, вызывая беспорядок:

		.input-field .project-module { … }

Стили подключаются сразу после фундамента, перед 2-м слоем, для поддержания низкого уровня приоритета по весу селекторов.

### Преимущества
Переиспользуемые, продуманные модули первого слоя позволяют сэкономить время на поддержке популярных конструкций, избавляя от необходимости поддерживать сразу несколько похожих модулей. Переиспользуемость также хорошо сказывается на итоговом размере CSS-файлов.

Имея разработанную базу, легко на её основе создавать новые интерфейсы, большая часть которых состоит из стандартных элементов.

Наработки стандартных конструкций, можно переиспользовать из проекта в проект, для ускорения разработки вёрстки и завязанного на ней бэкэнд функционала.

## Второй слой — проектный
Второй слой включает в себя изолированные, проектные модули, из которых в дальнейшем собирается страница:

* форма регистрации
* блок логина
* корзина товаров

### Правила

В конструкциях второго слоя рекомендуется использовать как можно больше уникальных классов, даже если в конкретном дизайне блок не нуждается в стилизации, лучше заранее присвоить ему уникальный класс. Такой подход улучшает доступность каждой отдельной части конструкции, что в дальнейшем позволит легко изменять стили, не трогая HTML-конструкцию.

Каждый модуль должен быть максимально изолирован и существовать как отдельная, независимая единица портала, которая общается только с ядром / базовым слоем.

Чтобы в проектном модуле использовать конструкцию из первого слоя, нужно просто добавить рядом CSS-класс:

	<header class="toolbar">
		<a href="/" class="toolbar_logo"></a>
		<menu class="toolbar_dropdown_ul umenu">
			<li class="umenu_li"></li>
			<li class="umenu_li"></li>
		</menu>
	</header>

В примере описан модуль второго слоя **.toolbar**, который использует стандарт первого слоя **.umenu**. Чтобы модифицировать стандарт под проектные нужны, используется каскад в CSS:

	/* Toolbar
	-------------------------------------------------- */

	.toolbar { … }

	.toolbar_dropdown_ul { … }
		.toolbar_dropdown_ul .umenu_li { … }

	/* /Toolbar */

**Стили второго слоя могут быть модифицированы каскадом только от других модулей второго слоя.**

Можно:

	.project-module .other-project-module { … }

**Нельзя:**

	.standard-module .project-module { … }

### Преимущества
Изолированность модулей позволяет легко работать с их стилями, не боясь задеть отдельные части портала. Работая в команде, каждый может взяться за разработку отдельного модуля, не мешая другим разработчикам.

Стили каждого модуля можно подключать по отдельности только на те страницы, где они необходимы.

Если какой-то функционал исчезнет с портала, избавиться от стилей будет очень просто: достаточно лишь избавиться от файла или от секции с его стилями.

## Третий слой — косметический
Третий слой состоит из простых, мало влияющих стилей:

* цвета ссылок
* низкоуровневый OOCSS — пара свойств на класс (.float-left, .mt-10)
* глобальные модификаторы

Слой может не существовать в некоторых вариациях использования методологии, но на больших проектах стили этого слоя позволяют решить проблему с дубликацией стилей и позволяют описывать редкие, непроектные ситуации, не плодя лишнюю разметку.

### Правила
Стили третьего слоя должны быть построены так, чтобы при их удалении вёрстка не ломалась. Максимум могут теряться какие-то незначительные стили — цвета, отступы и пр.

Позволяется использовать простые OOCSS-классы, не больше трёх на блок, для редких, не проектных ситуаций:

	<div class="float-left mt-10 c-red"></div>

**Стили косметического слоя нельзя модифицировать каскадом от других слоёв, кроме контекста.**

Косметические стили подключаются в самом конце, так же допускается использовать **!important**.

### Преимущества
Стиль мало влияет на общую вёрстку портала, при этом решая проблемы дублированного кода и избавляя от необходимости плодить маленькие проектные и базовые модули.

Простые селекторы позволяют быстро справляться с редкими случаями, когда нужно повесить всего пару свойств на не проектный модуль.

## Контекст
Слой включает в себя стили высшего контекста и @media-правила, которые могут использоваться для изменения стандартных стилей под особенности различного контекста:

* .ie8, .ie9 — браузеры
* .touch — особенности устройства
* .logined — высший контекст в рамках портала
* Media queries

Слой контекста является исключением в правиле "Расположение стилей". Стили этого слоя располагаются по всем слоям, рядом с селекторами, которые каскадом модифицируются от контекста.

	/* Module name
	-------------------------------------------------- */

	.module { … }

	.module_list { … }
		.touch .module_list { … }
		.ie9 .module_list { … }

	@media screen and (max-width: 1000px) {
		.module { … }
		}

	/* /Module name */

## Рекомендации

### Code style
Вместе с методологией советуем использовать следующие полезные практики для улучшения своего кода:

* [Google HTML/CSS style guide](http://google-styleguide.googlecode.com/svn/trunk/htmlcssguide.xml) - стайл гайд по оформлению HTML/CSS кода
* [CSScomb](http://csscomb.ru/) - инструмент для сортировки CSS-свойств в рамках селектора
* [CSSDoc](http://cssdoc.net/) - правила оформления комментариев в CSS ([описание](http://habrahabr.ru/post/87406/))

### Словарь сокращений
Что бы не раздувать названия классов, используется словарь популярных сокращений:

	i — item
	tx — text
	it — input text
	ico — icon
	t — title
	f — footer
	h — header
	col — column
	cnt — content
	img — image
	a — link
	ac — action
	usr — user
	e — error

В будущем будет доступен в более развёрнутом виде.

### Best practices
* Как можно больше комментируйте CSS, любые нестандартные свойства, конструкции, магические числа — это пригодится не только членам вашей команды, но и вам, когда вы через пару месяцев вернётесь к коду.

###  Ссылки
* [Презентация MCSS](http://rhr.me/pres/mcss/) c Web Standards Days
* [Модель строгости в MCSS](http://tohtml.it/post/34285787881/mcss-unstrict-mode)