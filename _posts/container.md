---
layout: post
title: Гайд по grid
description: Полный список свойств для создания раскладки на гридах и управления ею.
contributors:
  - corocoto
  - filimonovalexey
  - starhamster
  - skorobaeus
editors:
  - tachisis
---

## Grid-ᲙᲝᲜᲢᲔᲘᲜᲔᲠᲘᲡ ᲗᲕᲘᲡᲔᲑᲔᲑᲘ

### `display`

```css
.container {
  display: grid;
}
```

თუ ელემენტის თვისებას `display`-ს მივანიჭებთ მნიშვნელობა `grid`-ს, ელემენტი გახდება Grid კონტეინერი. ამ კონტეინერის შვილობილი ელემენტები იწყებენ Grid-მოწყობის წესების დამორჩილებას. გარედან Grid კონტეინერი იქცევა როგორც ბლოკი.

```css
.container {
  display: inline-grid;
}
```

პრაქტიკულად ანალოიურია წინა მნიშვნელობისა, იმ განსხვავებით, რომ გარედან Grid კონტეინერი, რომელიც შეიქმნა  `inline-grid`-ის დახმარებით, მოიქცევა, როგორც სტრიქონული ელემენტი.

### სვეტის და სტრიქონის წარმოქმნა 

- `grid-template-columns` სვეტების ზომა და რაოდენობა
- `grid-template-rows` სტრიქონების ზომა და რაოდენობა


```css
.container {
  display: grid;
  /* შეიქმნება 3 სვეტი */
  grid-template-columns: 150px auto 40%;
  /* შეიქმნება 3 სტრიქონი */
  grid-template-rows: 250px 10vw 15rem;
}
```

![grid-template-columns, grid-template-rows თვისებების მაგალითი.]({{ site.url }}/assets/images/5.png)

კვადრატული ფრჩილების გამოყენებით Grid ხაზებს შეიძლება დაერქვას სახელები:

```css
.container {
  display: grid;
  grid-template-columns: [start] 250px [line2] 400px [line3] 600px [end];
  grid-template-rows: [row1-start] 15rem [row1-end] 30vh [last];
}
```

![grid-template-columns, grid-template-rows თვისების გამოყენების მაგალითი.]({{ site.url }}/assets/images/6.png)

თითოეულ ხაზს შესაძლებელია ჰქონდეს ერთზე მეტი სახელი. ეს რაღაცით გავს HTML კლასებს — ელემენტს შეგიძლიათ დაუწესოთ ერთზე მეტი კლასი. აქაც ანალოგიური სიტუაციაა. მაგალითად, შემდეგი კოდი არის ვალიდური, მეორე და მესამე Grid ხაზებს აქვს ორ-ორი სახელი:

```css
.container {
  display: grid;
  grid-template-columns: [start] 140px [line2 col2-start] 250px [col2-end end];
  grid-template-rows: [row1-start] 25rem [row1-end] 10vh [last];
}
```

თუ საჭიროა ერთნაირი სვეტები ან სტრიქონები, შესაძლებელია ისარგებლოთ ფუნქციით `repeat()`.

შეიქმნება 250 პიქსელიანი 3 სვეტი:

```css
.container {
  display: grid;
  grid-template-columns: repeat(3, 250px);
}
```

Grid გამოჩენის მერე გამოჩნდა ახალი ზომის ერთეული: `fr` 🦊

`fr` ( ტერმინიდან _fraction_ — ნაწილი) განაგებს თავისუფალ ადგილს Grid კონტეინერის შიგნით.

მაგალითისთვის, შემდეგი კოდი შექმნის სამ სვეტს, სადაც თითოეული იკავებს წინაპარი კონტეინერის სიგანის 1/3-ს:

```css
.container {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
}
```

რაც შემდეგი ჩანაწერის ანალოგიურია:

```css
.container {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
}
```

თავისუფალი სივრცის გამოთვლა ხდება იმის მერე, რაც ადგილი მიეცემა ყველა ფიქსირებულ ზომებს. მაგალითისთვის, ქვემოთ მოყვანილი კოდის მიხედვით, პირველ რიგში შეიქმნება 200 პიქსელიანი სვეტი, შემდეგ კი, თავისუფალი სივრცე - წინაპარი ელემენტის სიგანეს გამოკლებული 200 პიქსელი, გადანაწილებული იქნება დანარჩენ ორ სვეტს შორის. თითოეული მათგანი დაიკავებს  (100% - 200px) / 2 სიგანეს:

```css
.container {
  display: grid;
  grid-template-columns: 1fr 200px 1fr;
}
```

### `auto-fill` და `auto-fit`

იმ შემთხვევაში, თუ საჭირო სვეტების ან სტრიქონების რაოდენობა წინასწარ არაა ცნობილი, შესაძლებელია `grid-template-columns` და `grid-template-rows` თვისებებში გამოყნებული იქნას მნიშვნელობები `auto-fill` ან `auto-fit` შემდეგ ფომაში:

```css
.container {
  grid-template-columns: repeat(/* auto-fill ან auto-fit */, /* სვეტის ზომა */);
}
```

ორივე პარამეტრი (`auto-fill` და `auto-fit`) Grid ბადეში ავტომატურად ამატებს სვეტებს. მაგრამ ამ სვეტებს აქვთ განსახვავებული ხასიათი:

- `auto-fill` ცდილობს შეავსოს მთლიანი ხელმისაწვდომი სივრცე, ხოლო როცა ელემენტები გამოილევა, ქმნის ცარიელ სვეტებს, თანაბრად ანაწილებს ხელმისაწვდომ სივრცეს არსებული და ვირტუალურ სვეტებს შორის.
- `auto-fit` მოქმედებს მსგავსად, ოღონდ იწოვს ცარიელ სვეტებს და აძლევს ადგილს შესავსებად.

ანალოგიურად შეიძლება გამოყენებული იქნას სტრიქონების შესაქმნელად.

### `grid-auto-columns`, `grid-auto-rows`

Если элементов внутри грид-контейнера больше, чем может поместиться в заданные явно ряды и колонки, то для них создаются автоматические, неявные ряды и колонки. При помощи свойств `grid-auto-columns` и `grid-auto-rows` можно управлять размерами этих автоматических рядов и колонок.

```css
.container {
  display: grid;
  grid-template-rows: 50px 140px;
  grid-auto-rows: 40px;
  gap: 20px;
}
```

![Пример реализации свойств grid-auto-columns, grid-auto-rows.]({{ site.url }}/assets/images/7.png)

В этом примере создаются два явных ряда размером 50 и 140 пикселей. Элементы, начиная с третьего, в эти два ряда не помещаются, и для них создаются автоматические ряды. При помощи свойства `grid-auto-rows` мы указываем, что автоматические ряды должны иметь размер 40 пикселей.

Можно задавать больше одного значения для автоматических колонок или рядов. Тогда паттерн размера будет повторяться до тех пор, пока не кончатся грид-элементы.

```css
.container {
  display: grid;
  grid-template-columns: 200px 500px;
  grid-auto-columns: 75px 100px 50px;
  grid-auto-flow: column;
  gap: 20px;
}
```

Важно указать при помощи `grid-auto-flow: column`, что элементы должны вставать в колонки, чтобы элементы без контента были видны.

![Пример реализации свойств grid-auto-columns, grid-auto-rows с разными размерами колонок.]({{ site.url }}/assets/images/8.png)

Как видите, автоматически создаются колонки размером 75, 100 и затем 50 пикселей. И так до тех пор, пока элементы не закончатся.

### `grid-auto-flow`

Если грид-элементов больше, чем явно объявленных колонок или рядов, то они автоматически размещаются внутри родителя. А вот каким образом — в ряд или в колонку — можно указать при помощи свойства `grid-auto-flow`. По умолчанию значение у этого свойства `row`, _лишние_ элементы будут выстраиваться в ряды в рамках явно заданных колонок.

Возможные значения:

- `row` (значение по умолчанию) — автоматически размещаемые элементы выстраиваются в ряды.
- `column` — автоматически размещаемые элементы выстраиваются в колонки.
- `dense` — браузер старается заполнить _дырки_ (пустые ячейки) в разметке, если размеры элементов позволяют. Можно сочетать с остальными значениями.

Принципы работы этого свойства удобнее всего изучать на примере, когда есть большой блок, который не помещается в одну грид-ячейку.

```css
.container {
  display: grid;
  /* Три колонки */
  grid-template-columns: auto auto auto;
  /* Два ряда */
  grid-template-rows: auto auto;
  /* Автоматическое размещение в ряд */
  grid-auto-flow: row;
  /* Отступы между ячейками */
  gap: 20px;
}

.item3 {
  /* Занимает один ряд и
  растягивается на две колонки */
  grid-column: span 2;
}
```

![Пример реализации свойства grid-auto-flow со значением row]({{ site.url }}/assets/images/9.png)

Как видите, третий элемент не поместился в последнюю ячейку первого ряда и был перенесён в следующий ряд. Следующий за ним четвёртый элемент встал в ближайшую доступную ячейку во втором ряду.

Добавим к значению свойства `grid-auto-flow` слово `dense`:

```css
.container {
  /* Автоматическое размещение в ряд */
  grid-auto-flow: row dense;
}
```

![Пример реализации свойства grid-auto-flow со значением row dense.]({{ site.url }}/assets/images/10.png)

Теперь четвёртый элемент встал в ряд, но занял при этом пустую ячейку в первом ряду. Браузер старается закрыть все _дырки_ в сетке, переставляя подходящие элементы на свободные места.

Посмотрим пример со значением `column`:

```css
.container {
  grid-template-columns: auto auto;
  grid-template-rows: auto auto auto;
  /* Автоматическое размещение в колонку */
  grid-auto-flow: column;
}

.item3 {
  grid-row: span 2;
}
```

![Пример реализации свойства grid-auto-flow со значением column.]({{ site.url }}/assets/images/11.png)

Видим аналогичную картину: третий элемент не поместился в последнюю ячейку первой колонки и встал во вторую колонку. Следующий за ним четвёртый элемент встал ниже во второй колонке.

Добавим `dense` к текущему значению:

```css
.container {
  /* Автоматическое размещение в ряд */
  grid-auto-flow: column dense;
}
```

![Пример реализации свойства grid-auto-flow со значением column dense.]({{ site.url }}/assets/images/12.png)

В результате четвёртый элемент занял пустую ячейку в первой колонке.

### `grid-template-areas`

Позволяет задать шаблон сетки расположения элементов внутри грид-контейнера. Имена областей задаются при помощи свойства `grid-area`. Текущее свойство `grid-template-areas` просто указывает, где должны располагаться эти грид-области.

Возможные значения:

- `none` (значение по умолчанию) — области сетки не задано имя.
- `.` — означает пустую ячейку.
- название — название области, может быть абсолютно любым словом или даже эмодзи! 🤯

Обратите внимание, что нужно называть каждую из ячеек. Например, если шапка или подвал нашего сайта будут занимать все три существующие колонки, то нужно будет трижды написать названия этих областей. Удобнее всего будет подписывать области в виде некой таблицы. Подобный способ записи чем-то похож на [таблицы в Markdown](/tools/markdown/#tablicy):

```css
.container {
  display: grid;
  grid-template-columns: repeat(3, 500px);
  grid-template-rows: repeat(4, 1fr);
  grid-template-areas:
    "header header header"
    "content content 👾"
    "content content ."
    "footer footer footer";
}

.item1 {
  grid-area: header;
}

.item2 {
  grid-area: content;
}

.item3 {
  grid-area: 👾;
}

.item4 {
  grid-area: footer;
}
```

Обратите внимание, что между строками не ставятся запятые или какие-то другие символы, имена разделяются пробелами.

Получится такая раскладка:

![Пример реализации свойства grid-template-areas.]({{ site.url }}/assets/images/13.png)

Имена областей должны разделяться пробелами. Это важно, особенно в том случае, если вы хотите расположить две пустых ячейки рядом. Разделите точки пробелами, иначе браузер подумает, что это одна пустая ячейка.

### `grid-template`

Свойство-шорткат для свойств `grid-template-rows`, `grid-template-columns`. Позволяет записать все значения в одну строку. Главное после этого не запутаться при чтении 😅

Можно прописать все колонки и ряды сразу, разделяя их слэшем `/`. Сперва идут ряды, а затем колонки, не перепутайте!

```css
.container {
  display: grid;
  grid-template: repeat(4, 1fr) / repeat(3, 500px);
}
```

В этом же свойстве можно задавать значение и для свойства `grid-template-areas`, но тогда код превращается в кашу и становится совершенно нечитабельным. Лучше всё же использовать это свойство отдельно:

Лучше так?

```css
.container {
  display: grid;
  grid-template:
    [row1-start] "header header header" 25px [row1-end]
    [row2-start] "footer footer footer" 25px [row2-end]
    / auto 50px auto;
}
```

Или так?

```css
.container {
  display: grid;
  grid-template:
    [row1-start] 25px [row1-end]
    [row2-start] 25px [row2-end]
    / auto 50px auto;
  grid-template-areas:
    "header header header"
    "footer footer footer";
}
```

Но техническая возможность есть, выбирать вам! 😉

### `row-gap`, `column-gap`

Задают отступы между рядами или колонками.

```css
.container {
  display: grid;
  grid-template-columns: 1fr 350px 1fr;
  grid-template-rows: repeat(3, 150px);
  /* Отступы между рядами */
  row-gap: 50px;
  /* Отступы между колонками */
  column-gap: 20px;
}
```

![Пример реализации свойств column-gap и row-gap.]({{ site.url }}/assets/images/14.png)

В инспекторе отступы заштриховываются, так их можно отличить от грид-элементов. В этом примере между рядами отступы по 50 пикселей, а между колонками — по 20 пикселей.

### `gap`

Шорткат для записи значений свойств `row-gap` и `column-gap`. Значения разделяются пробелом:

```css
.container {
  display: grid;
  grid-template-columns: 1fr 350px 1fr;
  grid-template-rows: repeat(3, 150px);
  gap: 50px 20px;
}
```

### `justify-content`

Свойство, с помощью которого можно выровнять элементы вдоль оси строки. Данное свойство работает, только если общая ширина столбцов меньше ширины контейнера сетки. Другими словами, вам нужно свободное пространство вдоль оси строки контейнера, чтобы выровнять его столбцы слева или справа.

Возможные значения:

`start` — выравнивает сетку по левой стороне грид-контейнера.

```css
.container {
  display: grid;
  grid-template-columns: repeat(3, 300px);
  gap: 20px;

  justify-content: start;
}
```

![Пример реализации свойства justify-content со значением start.]({{ site.url }}/assets/images/15.png)

`end` — выравнивает сетку по правой стороне грид-контейнера.

```css
.container {
  display: grid;
  grid-template-columns: repeat(3, 300px);
  gap: 20px;

  justify-content: end;
}
```

![Пример реализации свойства justify-content со значением end.]({{ site.url }}/assets/images/16.png)

`center` — выравнивает сетку по центру грид-контейнера.

```css
.container {
  display: grid;
  grid-template-columns: repeat(3, 300px);
  gap: 20px;

  justify-content: center;
}
```

![Пример реализации свойства justify-content со значением center.]({{ site.url }}/assets/images/17.png)

`stretch` — масштабирует элементы, чтобы сетка могла заполнить всю ширину грид-контейнера.

```css
.container {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  gap: 20px;

  justify-content: stretch;
}
```

![Пример реализации свойства justify-content со значением stretch.]({{ site.url }}/assets/images/18.png)

`space-around` — одинаковое пространство между элементами и полуразмерные отступы по краям.

```css
.container {
  display: grid;
  grid-template-columns: repeat(3, 300px);
  gap: 20px;

  justify-content: space-around;
}
```

![Пример реализации свойства justify-content со значением space-around.]({{ site.url }}/assets/images/19.png)

`space-evenly` — одинаковое пространство между элементами и полноразмерные отступы по краям.

```css
.container {
  display: grid;
  grid-template-columns: repeat(3, 300px);
  gap: 20px;

  justify-content: space-evenly;
}
```

![Пример реализации свойства justify-content со значением space-evenly.]({{ site.url }}/assets/images/20.png)

`space-between` — одинаковое пространство между элементами без отступов по краям.

```css
.container {
  display: grid;
  grid-template-columns: repeat(3, 300px);
  gap: 20px;

  justify-content: space-between;
}
```

![Пример реализации свойства justify-content со значением space-between.]({{ site.url }}/assets/images/21.png)

### `justify-items`

Свойство, с помощью которого задаётся выравнивание грид-элементов по горизонтальной оси. Применяется ко всем элементам внутри грид-родителя.

Возможные значения:

`start` — выравнивает элемент по начальной (левой для LTR) линии.

```css
.container {
  display: grid;
  grid-template-columns: 1fr 400px 1fr;
  grid-template-rows: repeat(3, 170px);
  gap: 20px;

  justify-items: start;
}

.item {
  min-width: 300px;
}
```

![Пример реализации свойства justify-items со значением start.]({{ site.url }}/assets/images/22.png)

`end` — выравнивает элемент по конечной (правой для LTR) линии.

```css
.container {
  justify-items: end;
}
```

![Пример реализации свойства justify-items со значением end.]({{ site.url }}/assets/images/23.png)

`center` — выравнивает элемент по центру грид-ячейки.

```css
.container {
  justify-items: center;
}
```

![Пример реализации свойства justify-items со значением center.]({{ site.url }}/assets/images/24.png)

`stretch` — растягивает элемент на всю ширину грид-ячейки, при условии, что у него не задана фиксированная ширина.

```css
.container {
  justify-items: stretch;
}
```

![Пример реализации свойства justify-items со значением stretch.]({{ site.url }}/assets/images/25.png)

Можно управлять выравниванием отдельных грид-элементов при помощи свойства `justify-self`.

### `align-items`

Свойство, с помощью которого можно выровнять элементы по вертикальной оси внутри грид-контейнера.

Возможные значения:

`start` — выравнивает элемент по начальной (верхней) линии.

```css
.container {
  display: grid;
  grid-template-columns: 1fr 400px 1fr;
  grid-template-rows: repeat(3, 170px);
  gap: 20px;

  align-items: start;
}

.item {
  min-height: 100px;
}
```

![Пример реализации свойства align-items со значением start.]({{ site.url }}/assets/images/26.png)

`end` — выравнивает элемент по конечной (нижней) линии.

```css
.container {
  align-items: end;
}
```

![Пример реализации свойства align-items со значением end.]({{ site.url }}/assets/images/27.png)

`center` — выравнивает элемент по центру грид-ячейки.

```css
.container {
  align-items: center;
}
```

![Пример реализации свойства align-items со значением center.]({{ site.url }}/assets/images/28.png)

`stretch` — растягивает элемент на всю высоту грид-ячейки.

```css
.container {
  align-items: stretch;
}
```

![Пример реализации свойства align-items со значением stretch.]({{ site.url }}/assets/images/29.png)

Можно также управлять выравниванием отдельных грид-элементов при помощи свойства `align-self`.

### `place-items`

Шорткат для указания значений сразу и для `align-items` и для `justify-items`. Указывать нужно именно в таком порядке.

```css
.container {
  display: grid;
  place-items: stretch end;
}
```

### `grid`

Мегашорткат, позволяющий задать значения всему и сразу. А конкретно с его помощью можно указать значения для следующих свойств:

- `grid-template-rows`;
- `grid-template-columns`;
- `grid-template-areas`;
- `grid-auto-rows`;
- `grid-auto-columns`;
- `grid-auto-flow`.

Перед тем как соблазниться возможностью расписать всё в одном свойстве, дважды (а то и трижды) подумайте о читабельности кода. Учтите и то, что гриды относительно новая и не такая уж простая технология. Не каждый коллега сможет прочесть этот шорткат.

Возможные значения:

`none` — значение по умолчанию. Это ключевое слово сбрасывает значения для всех свойств, входящих в этот шорткат.

```css
.container {
  display: grid;
  grid: none;
}
```

Можно указать допустимые значения для шортката [`grid-template`](/css/grid-template/):

```css
.container {
  display: grid;
  grid: repeat(4, 150px) / 1fr 200px 1fr;
}
```

В том числе можно указать имена линий:

```css
.container {
  display: grid;
  grid:
    [row1-start] 25px [row1-end row2-start] 25px [row2-end]
    / auto 50px auto;
}
```

Можно задать размеры колонок и рядов. Создадим два ряда и две колонки:

```css
.container {
  display: grid;
  grid: 200px 100px / 30% 30%;
}
```

`auto-flow` — это ключевое слово даёт браузеру понять, что создавать при необходимости: колонки или ряды.

Если `auto-flow` стоит справа от слэша, то будут создаваться автоматические колонки:

```css
.container {
  display: grid;
  grid: 200px 100px / auto-flow 30%;
}
```

Если `auto-flow` стоит слева от слэша, то будут создаваться автоматические ряды:

```css
.container {
  display: grid;
  grid: auto-flow 30% / 200px 100px;
}
```

`dense` — к ключевому слову `auto-flow` можно добавить `dense`. Это укажет браузеру, что элементы должны стараться заполнить свободные ячейки. Подробнее про работу этого ключевого слова можно почитать в статье про [`grid-auto-flow`](/css/grid-auto-flow/).

Важно ставить это слово сразу после `auto-flow`:

```css
.container {
  display: grid;
  grid:  auto-flow dense 30% / 200px 100px;
}
```

Ниже будет несколько примеров с блоками кода, которые делают одно и то же, в первом случае через шорткат, во втором — как обычно.

Задаёт размеры и количество колонок и рядов:

```css
.container {
  grid: 50px 150px / 2fr 1fr;
}

.container {
  grid-template-rows: 50px 150px;
  grid-template-columns: 2fr 1fr;
}
```

Задаёт размеры и количество рядов, а также автоматические колонки:

```css
.container {
  grid: 200px 1fr / auto-flow 200px;
}

.container {
  grid-auto-flow: column;
  grid-template-rows: 200px 1fr;
  grid-auto-columns: 200px;
}
```

Задаёт размеры и количество рядов, автоматические колонки и распределение `dense`:

```css
.container {
  grid: auto-flow dense 300px / 2fr 3fr;
}

.container {
  grid-auto-flow: row dense;
  grid-auto-rows: 300px;
  grid-template-columns: 2fr 3fr;
}
```

Возможны и более сложные комбинации значений этого свойства. Например, можно сразу задать имена областям:

```css
.container {
  grid:
    [row1-start] "header header header" 1fr [row1-end]
    [row2-start] "footer footer footer" 25px [row2-end]
    / auto 50px auto;
}
```

Что соответствует такому коду:

```css
.container {
  grid-template-areas:
    "header header header"
    "footer footer footer";
  grid-template-rows: [row1-start] 1fr [row1-end row2-start] 25px [row2-end];
  grid-template-columns: auto 50px auto;
}
```