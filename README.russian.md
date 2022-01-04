![Шаблоны проектирования для людей](https://cloud.githubusercontent.com/assets/11269635/23065273/1b7e5938-f515-11e6-8dd3-d0d58de6bb9a.png)

<sub>Перевод выполнен [Виленой Воробьевой](https://github.com/vylenne) на безвозмездной основе. Ссылка на [оригинальный репозиторий и автора](https://github.com/kamranahmedse/design-patterns-for-humans)</sub>
***

<p align="center">
🎉 Максимально упрощенное объяснение шаблонов проектирования! 🎉
</p>
<p align="center">
  Тема, которая легко может заставить кого угодно пошатнуться. <br>
  Здесь я пытаюсь заставить их отпечататься в вашем сознании (и, возможно, в моем), объясняя их как можно проще.
</p>

***

<sub>Загляните на [блог автора](http://kamranahmed.info) и скажите "Привет!" на [Twitter](https://twitter.com/kamranahmedse).</sub>

Введение
=================

Шаблоны проектирования - это решения повторяющихся проблем; **рекомендации о том, как решать определенные проблемы**. Это не классы, пакеты или библиотеки, которые можно подключить к своему приложению и ждать магии. Это, скорее, рекомендации о том, как решать определенные проблемы в тех или иных ситуациях.


> Шаблоны проектирования - это решения повторяющихся проблем; рекомендации о том, как решать определенные проблемы.

Википедия описывает их так:
> В разработке программного обеспечения шаблон проектирования программного обеспечения - это общее многоразовое решение часто встречающейся проблемы в данном контексте при разработке программного обеспечения. Это не законченный дизайн, который может быть преобразован непосредственно в исходный или машинный код. Это описание или шаблон решения проблемы, который можно использовать в самых разных ситуациях.

⚠️ С осторожностью!
-----------------
- Шаблоны проектирования это не серебряная пуля от всех ваших проблем.
- Не пытайтесь применить их там, где не надо; плохие вещи произойдут, если это сделать. 
- Имейте в виду, что шаблоны проектирования - это решения **самих проблем**, а не решения **поиска** проблем; так что не переусердствуйте.
- Если их использовать правильно и в нужном месте, они могут быть спасителями; в противном случае они могут привести к ужасному беспорядку в коде.

> На заметку: приведенные ниже примеры кода написаны на ```PHP-7```, однако это не должно вас останавливать, потому что концепции в любом случае одинаковы.

Типы шаблонов проектирования
-----------------

* [Порождающие](#Порождающие-шаблоны-проектирования)
* [Структурные](#Структурные-паттерны-проектирования)
* [Поведенческие](#Поведенческие-паттерны-проектирования)

Порождающие шаблоны проектирования
==========================

Простыми словами:
> Шаблоны создания, сосредоточенные на том, как создать экземпляр объекта или группы связанных объектов.

Википедия говорит:
> В разработке программного обеспечения шаблоны творческого проектирования - это шаблоны проектирования, которые связаны с механизмами создания объектов, пытаясь создавать объекты способом, соответствующим ситуации. Базовая форма создания объекта может привести к проблемам с проектированием или усложнить дизайн. Шаблоны порождающего проектирования решают эту проблему, управляя созданием этого объекта.

 * [Простая фабрика](#-Простая-фабрика)
 * [Фабричный метод](#-Фабричный-метод)
 * [Абстрактная фабрика](#-Абстрактная-фабрика)
 * [Строитель](#-Строитель)
 * [Прототип](#-Прототип)
 * [Одиночка](#-Одиночка)

🏠 Простая фабрика
--------------
Пример из реальной жизни:
> Представьте, вы строите дом и вам нужны двери. Вы можете либо надеть одежду плотника, принести немного дерева, клей, гвозди и все инструменты, необходимые для изготовления двери, и начать строить ее в своем доме, либо вы можете просто позвонить на фабрику и заказать доставку готовой двери, чтобы вам не нужно было ничего узнавать об изготовлении двери или разбираться с беспорядком при ее изготовлении.

Простыми словами:
> Простая фабрика просто создает экземпляр для пользователя, не предоставляя ему никакой логики создания экземпляра.

Словами Википедии:
> В объектно-ориентированном программировании (ООП), фабрика это объект для создания других объектов – формально фабрика - это функция или метод, который возвращает объекты какого-то прототипа или класса из некоторого вызванного метода, которые считается "новыми".

**Программный пример**

Прежде всего, у нас есть дверной интерфейс и его реализация:
```php
interface Door
{
    public function getWidth(): float;
    public function getHeight(): float;
}

class WoodenDoor implements Door
{
    protected $width;
    protected $height;

    public function __construct(float $width, float $height)
    {
        $this->width = $width;
        $this->height = $height;
    }

    public function getWidth(): float
    {
        return $this->width;
    }

    public function getHeight(): float
    {
        return $this->height;
    }
}
```

Теперь у нас есть наша фабрика дверей, которая изготавливает дверь и возвращает ее:
```php
class DoorFactory
{
    public static function makeDoor($width, $height): Door
    {
        return new WoodenDoor($width, $height);
    }
}
```

И теперь она может быть использована так:
```php
// Создай мне дверь с размерами 100x200
$door = DoorFactory::makeDoor(100, 200);

echo 'Ширина: ' . $door->getWidth();
echo 'Высота: ' . $door->getHeight();

// Сделай мне дверь с размерами 50x100
$door2 = DoorFactory::makeDoor(50, 100);
```

**Где применяется?**

Когда создание объекта это не просто назначения и включение некоторой логики, есть смысл вынести его на выделенную фабрику, чтобы избежать повсеместного дублирования кода.

🏭 Фабричный метод
--------------
Пример из реальной жизни:
> Рассмотрим случай с менеджером по найму. Невозможно, чтобы один человек проходил собеседование на каждую из должностей. В зависимости от открывшейся вакансии он должен принять решение и делегировать этапы собеседования разным людям.

Простыми словами:
> Это способ делегирования(если еще проще – передачи) логики создания экземпляров дочерним классам.

Википедия говорит:
> В программировании на основе классов шаблон фабричного метода представляет собой шаблон создания, использующий фабричные методы для решения проблемы создания объектов без необходимости указывать точный класс объекта, который будет создан. Это делается путем создания объектов путем вызова фабричного метода — либо указанного в интерфейсе и реализованного дочерними классами, либо реализованного в базовом классе и, возможно, переопределенного наследуемыми классами — вместо вызова конструктора.

**Программный пример**

Возьмем пример нашего менеджера по найму выше. Прежде всего, у нас есть интерфейс интервьюера и некоторые реализации для него:
```php
interface Interviewer
{
    public function askQuestions();
}

class Developer implements Interviewer
{
    public function askQuestions()
    {
        echo 'Asking about design patterns!';
    }
}

class CommunityExecutive implements Interviewer
{
    public function askQuestions()
    {
        echo 'Asking about community building';
    }
}
```

Теперь давайте создадим наш `HiringManager`:
```php
abstract class HiringManager
{
    // Фабричный метод
    abstract protected function makeInterviewer(): Interviewer;

    public function takeInterview()
    {
        $interviewer = $this->makeInterviewer();
        $interviewer->askQuestions();
    }
}
```

Теперь любой ребенок может расширить его и предоставить необходимого интервьюера:
```php
class DevelopmentManager extends HiringManager
{
    protected function makeInterviewer(): Interviewer
    {
        return new Developer();
    }
}

class MarketingManager extends HiringManager
{
    protected function makeInterviewer(): Interviewer
    {
        return new CommunityExecutive();
    }
}
```
и тогда он может быть использован так:

```php
$devManager = new DevelopmentManager();
$devManager->takeInterview(); // Результат: Вопрос о шаблонах проектирования

$marketingManager = new MarketingManager();
$marketingManager->takeInterview(); // Результат: Вопрос о создании сообщества
```

**Где применяется?**

Полезно, когда в классе есть некоторая общая обработка, но требуемый подкласс динамически определяется во время выполнения. Или когда клиент не знает, какой именно подкласс ему может понадобиться.

🔨 Абстрактная фабрика
----------------
Пример из реального мира:
> Расширение нашего примера дверей с простой фабрики. В зависимости от ваших потребностей вы можете приобрести деревянную дверь в магазине деревянных дверей, железную дверь в магазине железных дверей или дверь из ПВХ в соответствующем магазине. Кроме того, вам может быть нужен парень с разными специальностями, чтобы установить дверь, например, плотник для деревянной двери, сварщик для железной двери и т.д. Как вы можете видеть, есть зависимость между дверями: деревянная дверь нуждается в плотнике, железная дверь нуждается в сварщике и т.д.

Простыми словами:
> Фабрика фабрик; фабрика, которая группирует отдельные, но связанные/зависимые фабрики вместе без указания их конкретных классов.

Википедия говорит:
> Шаблон абстрактной фабрики предоставляет способ инкапсуляции(объединения в одно целое) группы отдельных фабрик, имеющих общую тему, без указания их конкретных классов.

**Программный пример**

Перевод приведенного выше примера двери. Для начала, у нас есть наш интерфейс `Door`("Дверь") и некоторая реализация для него:
```php
interface Door
{
    public function getDescription();
}

class WoodenDoor implements Door
{
    public function getDescription()
    {
        echo 'Я современная дверь';
    }
}

class IronDoor implements Door
{
    public function getDescription()
    {
        echo 'Я есть железная дверь';
    }
}
```

Теперь у нас есть несколько экспертов по установке для каждого типа дверей:
```php
interface DoorFittingExpert
{
    public function getDescription();
}

class Welder implements DoorFittingExpert
{
    public function getDescription()
    {
        echo 'Я могу установить только железные двери';
    }
}

class Carpenter implements DoorFittingExpert
{
    public function getDescription()
    {
        echo 'Я могу поставить только деревянную дверь';
    }
}
```

Теперь у нас есть наша абстрактная фабрика, которая дась нам создать семейство связанных объектов, т.е. фабрика деревянных дверей создаст деревянную дверь и специалиста по их установке, точно так же как фабрика железных дверей создаст по аналогии железные двери иэксперта по установке железных дверей:
```php
interface DoorFactory
{
    public function makeDoor(): Door;
    public function makeFittingExpert(): DoorFittingExpert;
}

// Деревянная фабрика возвращает плотника и деревянную дверь
class WoodenDoorFactory implements DoorFactory
{
    public function makeDoor(): Door
    {
        return new WoodenDoor();
    }

    public function makeFittingExpert(): DoorFittingExpert
    {
        return new Carpenter();
    }
}

// Железная дверная фабрика получит железную дверь и релевантного специалиста
class IronDoorFactory implements DoorFactory
{
    public function makeDoor(): Door
    {
        return new IronDoor();
    }

    public function makeFittingExpert(): DoorFittingExpert
    {
        return new Welder();
    }
}
```

И затем его можно использовать как:
```php
$woodenFactory = new WoodenDoorFactory();

$door = $woodenFactory->makeDoor();
$expert = $woodenFactory->makeFittingExpert();

$door->getDescription();  // Результат: Я - деревянная дверь
$expert->getDescription(); // Результат: Я могу установить только деревянные двери

// Аналогично для железной фабрики
$ironFactory = new IronDoorFactory();

$door = $ironFactory->makeDoor();
$expert = $ironFactory->makeFittingExpert();

$door->getDescription();  // Результат: Я - железная дверь
$expert->getDescription(); // Результат: Я могу установить только железные двери
```

Как вы видите, деревянная фабрика инкапсулировала `carpenter`(плотника) и `wooden door`(деревянная дверь) так же, как железная фабрика инкапсулировала `welder`(сварщика) и `iron door`(железная дверь). Таким образом, это помогает нам быть уверенными, что для каждой созданной двери мы не получим неправильного специалиста по установке.

**Где использовать?**

Когда существуют взаимосвязанные зависимости с задействованной сложной логикой создания.

👷 Строитель
--------------------------------------------
Пример из реального мира:
> Представьте, что вы находитесь в кафе "Hardee" и заказываете конкретную сделку, скажем, "Big Hardee" (большой-пребольшой бургер), и они передают ее вам без *каких либо вопросов*; это пример простой фабрики. Но бывают случаи, когда логика создания может включать в себя больше шагов. Например, вы хотите индивидуально собранный бургер, у вас есть несколько вариантов приготовления вашего бургера, например: Какой хлеб вы хотите? Какие виды соусов вы бы хотели? Какой сыр вы хотите добавить? и т.д. В таких случаях на помощь приходит шаблон строителя.

Простыми словами:
> Позволяет создавать различные варианты объекта, избегая загрязнения конструктора. Полезно, когда у объекта может быть несколько разновидностей, возможность кастомизации. Или когда в создании объекта задействовано много шагов.

Википедия говорит
> Шаблон "Строитель" - это шаблон проектирования программного обеспечения для создания объектов, предназначенный для поиска решения проблемы анти-шаблона телескопического конструктора.

Сказав это, позвольте мне добавить немного ясности в то, что есть анти-шаблон телескопического конструктора. В тот или иной момент мы все видели конструктор, подобный приведенному ниже:
```php
public function __construct($size, $cheese = true, $pepperoni = true, $tomato = false, $lettuce = true)
{
}
```

Как вы видите; количество параметров конструктора может быстро выйти из-под контроля, и может быть сложно понять расположение параметров/их смысл. Кроме того, этот список параметров может еще вырасти, если вы захотите добавить больше опций в будущем. Это называется анти-шаблоном телескопического конструктора.

**Пример из программирования**

Разумной альтернативой является использование шаблона "Строитель". Итак, у нас есть наш бургер, который мы хотим приготовить:
```php
class Burger
{
    protected $size;

    protected $cheese = false;
    protected $pepperoni = false;
    protected $lettuce = false;
    protected $tomato = false;

    public function __construct(BurgerBuilder $builder)
    {
        $this->size = $builder->size;
        $this->cheese = $builder->cheese;
        $this->pepperoni = $builder->pepperoni;
        $this->lettuce = $builder->lettuce;
        $this->tomato = $builder->tomato;
    }
}
```

И теперь у нас есть строитель с необходимыми параметрами:
```php
class BurgerBuilder
{
    public $size;

    public $cheese = false;
    public $pepperoni = false;
    public $lettuce = false;
    public $tomato = false;

    public function __construct(int $size)
    {
        $this->size = $size;
    }

    public function addPepperoni()
    {
        $this->pepperoni = true;
        return $this;
    }

    public function addLettuce()
    {
        $this->lettuce = true;
        return $this;
    }

    public function addCheese()
    {
        $this->cheese = true;
        return $this;
    }

    public function addTomato()
    {
        $this->tomato = true;
        return $this;
    }

    public function build(): Burger
    {
        return new Burger($this);
    }
}
```

И теперь мы используем так:
```php
$burger = (new BurgerBuilder(14))
                    ->addPepperoni()
                    ->addLettuce()
                    ->addTomato()
                    ->build();
```

**Где он может быть использован?**

Когда может быть несколько разновидностей объекта, чтобы избежать телескопирования конструктора. Ключевое отличие от **фабричного шаблона** заключается в том, что фабричный шаблон следует использовать, когда создание представляет собой **одноэтапный процесс**, в то время как шаблон строителя следует использовать, когда создание представляет собой **многоступенчатый процесс**.

🐑 Прототип
------------
Пример из реального мира:
> Вы же все помните Долли? Овца, которую клонировали! Давайте обойдемся без подробностей, но ключевый принцип здесь – клонирование

Простыми словами:
> Создание объекта на основе существующего путем клонирования.

Википедия говорит:
> Шаблон прототипа - это шаблон дизайн-проектирования при разработке программного обеспечения, который используется, когда тип создаваемых объектов определяется прототипом экземпляра, клонируемого для создания новых объектов.

Короче говоря, это позволяет вам создать копию существующего объекта и изменить его в соответствии с вашими потребностями, не тратя время на создание объекта с нуля и его настройку.

**Пример из программирования**

В PHP, это легко делается через `clone`(клонирование):
```php
class Sheep
{
    protected $name;
    protected $category;

    public function __construct(string $name, string $category = 'Овечья гора')
    {
        $this->name = $name;
        $this->category = $category;
    }

    public function setName(string $name)
    {
        $this->name = $name;
    }

    public function getName()
    {
        return $this->name;
    }

    public function setCategory(string $category)
    {
        $this->category = $category;
    }

    public function getCategory()
    {
        return $this->category;
    }
}
```
Затем его можно клонировать, как показано ниже:
```php
$original = new Sheep('Долли');
echo $original->getName(); // Долли
echo $original->getCategory(); // Овечья гора

// Клонируем и изменяем то, что требуется
$cloned = clone $original;
$cloned->setName('Долли');
echo $cloned->getName(); // Долли
echo $cloned->getCategory(); // Овечья гора
```

Также можно заюзать волшебный метод `__clone` для модификации поведения при клонировании.

**Где используем?**

Когда требуется объект, похожий на существующий объект, или когда создание будет дорогостоящим/долговременным по сравнению с клонированием.


💍 Одиночка
------------
Пример из реальной жизни:
> Одновременно в стране может быть только один президент. Один и тот же президент должен быть в работе, когда того требует долг. Президентом здесь является единственным и неповторимым.

Простыми словами:
> Гарантируется, что когда-либо будет создан только один объект определенного класса.

Википедия говорит:
> В разработке программного обеспечения шаблон "Одиночка" - это шаблон проектирования программного обеспечения, который ограничивает создание экземпляра класса одним объектом. Это полезно, когда для координации действий в системе требуется только один объект.

Шаблон одиночки на самом деле считается анти-шаблоном, и следует избегать его чрезмерного использования. Это не обязательно плохо и может иметь некоторые допустимые варианты использования, но их следует использовать с осторожностью, потому что он вводит глобальное состояние в ваше приложение, и изменение его в одном месте может повлиять на другие области, что может усложнить отладку в дальнейшем. Другая плохая вещь в этом шаблоне заключается в том, что они делают ваш код тесно связанным, плюс издевательство над шаблоном может вызвать сложности.

**Пример из программирования**

Создаем шаблон одиночки, делаем конструктор приватным, убираем возможность клонирования, расширения и создаем статическую переменную для размещения экземпляра:
```php
final class President
{
    private static $instance;

    private function __construct()
    {
        // Скрываем конструктор
    }

    public static function getInstance(): President
    {
        if (!self::$instance) {
            self::$instance = new self();
        }

        return self::$instance;
    }

    private function __clone()
    {
        // Отключаем клонирование
    }

    private function __wakeup()
    {
        // Отключаем несериализацию
    }
}
```
Затем используем:
```php
$president1 = President::getInstance();
$president2 = President::getInstance();

var_dump($president1 === $president2); // true
```

Структурные паттерны проектирования
==========================
Простыми словами:
> Структурные шаблоны в основном связаны с составом объектов или с тем, как сущности могут использовать друг друга. Или еще одним объяснением было бы то, что они помогают ответить на вопрос "Как создать программный компонент?".

Википедия говорит:
> В разработке программного обеспечения структурные шаблоны проектирования - это шаблоны проектирования, которые упрощают проектирование, определяя простой способ реализации взаимосвязей между сущностями.

 * [Адаптер](#-Адаптер)
 * [Мост](#-Мост)
 * [Компоновщик](#-Компоновщик)
 * [Декоратор](#-Декоратор)
 * [Фасад](#-Фасад)
 * [Легковес](#-Легковес)
 * [Заместитель](#-Заместитель)

🔌 Адаптер
-------
Пример из реального мира:
> Допустим, что у вас есть несколько фотографий на карте памяти, и вам нужно перенести их на свой компьютер. Для их передачи вам понадобится какой-нибудь адаптер, совместимый с портами вашего компьютера, чтобы вы могли подключить карту памяти к компьютеру. В данном случае кардридер является адаптером.
> Другим примером может служить знаменитый адаптер питания; трехногий штекер не может быть подключен к двухконтурной розетке, для этого надо использовать адаптер питания, который делает его совместимым с двухконтурной розеткой.
> Еще одним примером может быть переводчик, переводящий слова, произносимые одним человеком другому.

Простыми словами:
> Шаблон адаптера позволяет обернуть несовместимый объект в адаптер, чтобы сделать его совместимым с другим классом.

Википедия говорит:
> В разработке программного обеспечения шаблон адаптера - это шаблон проектирования программного обеспечения, который позволяет использовать интерфейс существующего класса в качестве другого интерфейса. Он часто используется для того, чтобы заставить существующие классы работать с другими без изменения их исходного кода.

**Пример из программирования**

Представим игру, в которой есть охотник, и он охотится на львов. Во-первых, у нас есть интерфейс `Lion` ("Лев"), который должны реализовать все типы львов:
```php
interface Lion
{
    public function roar();
}

class AfricanLion implements Lion
{
    public function roar()
    {
    }
}

class AsianLion implements Lion
{
    public function roar()
    {
    }
}
```

И охотник ожидает, что любая реализация интерфейса `Lion` ("Лев") будет охотиться:
```php
class Hunter
{
    public function hunt(Lion $lion)
    {
        $lion->roar();
    }
}
```

Теперь давайте предположим, что мы должны добавить `WildDog` ("Дикая собака") в нашей игре, чтобы охотник мог охотиться и на это. Но мы не можем сделать это напрямую, потому что у собаки другой интерфейс. Чтобы сделать его совместимым для нашего охотника, нам придется создать совместимый адаптер:
```php
// Его нужно добавить в игру
class WildDog
{
    public function bark()
    {
    }
}

// Адаптер для дикой собаки, чтобы сделать его совместимым с нашей игрой
class WildDogAdapter implements Lion
{
    protected $dog;

    public function __construct(WildDog $dog)
    {
        $this->dog = $dog;
    }

    public function roar()
    {
        $this->dog->bark();
    }
}
```

И сейчас `WildDog`("Дикая собака") может быть использован в игре с адаптером `WildDogAdapter`:
```php
$wildDog = new WildDog();
$wildDogAdapter = new WildDogAdapter($wildDog);

$hunter = new Hunter();
$hunter->hunt($wildDogAdapter);
```

🚡 Мост
------
Пример из реального мира
> Представьте, что у вас есть веб-сайт с разными страницами, и вы должны разрешить пользователю менять тему. Что бы вы сделали? Создайте несколько копий каждой страницы для каждой из тем или просто создайте отдельную тему и загрузите их в соответствии с предпочтениями пользователя? Шаблон моста позволяет вам выполнить второе, т.е.

![С И без паттерна моста](https://cloud.githubusercontent.com/assets/11269635/23065293/33b7aea0-f515-11e6-983f-98823c9845ee.png)

Простыми словами:
> Шаблон моста - это выбор/предпочтение композиции перед наследованием. Детали реализации переносятся из иерархии в другой объект с отдельной иерархией.

Википедия говорит:
> Шаблон моста - это шаблон проектирования, используемый в разработке программного обеспечения, который предназначен для "отделения абстракции от ее реализации, чтобы они могли изменяться независимо".

**Программный пример**

Перевод нашего примера веб-страницы сверху. Здесь у нас есть иерархия `WebPage`("Веб-страницы"):
```php
interface WebPage
{
    public function __construct(Theme $theme);
    public function getContent();
}

class About implements WebPage
{
    protected $theme;

    public function __construct(Theme $theme)
    {
        $this->theme = $theme;
    }

    public function getContent()
    {
        return "About page in " . $this->theme->getColor();
    }
}

class Careers implements WebPage
{
    protected $theme;

    public function __construct(Theme $theme)
    {
        $this->theme = $theme;
    }

    public function getContent()
    {
        return "Careers page in " . $this->theme->getColor();
    }
}
```

И отдельная иерархия тем:
```php

interface Theme
{
    public function getColor();
}

class DarkTheme implements Theme
{
    public function getColor()
    {
        return 'Dark Black';
    }
}
class LightTheme implements Theme
{
    public function getColor()
    {
        return 'Off white';
    }
}
class AquaTheme implements Theme
{
    public function getColor()
    {
        return 'Light blue';
    }
}
```

И обе иерархии:
```php
$darkTheme = new DarkTheme();

$about = new About($darkTheme);
$careers = new Careers($darkTheme);

echo $about->getContent(); // "About page in Dark Black";
echo $careers->getContent(); // "Careers page in Dark Black";
```

🌿 Компоновщик
-----------------
Пример из реального мира:
> Каждая организация состоит из сотрудников. Каждый из сотрудников обладает одинаковыми характеристиками, т.е. имеет зарплату, имеет некоторые обязанности, может отчитываться перед кем-то или не отчитываться, может иметь или не иметь некоторых подчиненных и т.д.

Простыми словами:
> Составной шаблон позволяет клиентам единообразно обрабатывать отдельные объекты.

Википедия говорит:
> В программной инженерии шаблон компоновщика - это сегментирующий шаблон проектирования. Компонирующий шаблон описывает, что с группой объектов следует обращаться так же, как с одним экземпляром объекта. Цель компоновщика состоит в том, чтобы "компоновать" объекты в древовидные структуры для представления иерархий частей и целых. Реализация шаблона компоновщика позволяет клиентам единообразно обрабатывать отдельные объекты и композиции.

**Программный пример**
Берем пример с наших сотрудников сверху. Здесь у нас есть разные типы сотрудников
```php
interface Employee
{
    public function __construct(string $name, float $salary);
    public function getName(): string;
    public function setSalary(float $salary);
    public function getSalary(): float;
    public function getRoles(): array;
}

class Developer implements Employee
{
    protected $salary;
    protected $name;
    protected $roles;
    
    public function __construct(string $name, float $salary)
    {
        $this->name = $name;
        $this->salary = $salary;
    }

    public function getName(): string
    {
        return $this->name;
    }

    public function setSalary(float $salary)
    {
        $this->salary = $salary;
    }

    public function getSalary(): float
    {
        return $this->salary;
    }

    public function getRoles(): array
    {
        return $this->roles;
    }
}

class Designer implements Employee
{
    protected $salary;
    protected $name;
    protected $roles;

    public function __construct(string $name, float $salary)
    {
        $this->name = $name;
        $this->salary = $salary;
    }

    public function getName(): string
    {
        return $this->name;
    }

    public function setSalary(float $salary)
    {
        $this->salary = $salary;
    }

    public function getSalary(): float
    {
        return $this->salary;
    }

    public function getRoles(): array
    {
        return $this->roles;
    }
}
```

Теперь у нас есть организация, состоящая из нескольких различных типов сотрудников:
```php
class Organization
{
    protected $employees;

    public function addEmployee(Employee $employee)
    {
        $this->employees[] = $employee;
    }

    public function getNetSalaries(): float
    {
        $netSalary = 0;

        foreach ($this->employees as $employee) {
            $netSalary += $employee->getSalary();
        }

        return $netSalary;
    }
}
```

Итак, используем:
```php
// Подготовим сотрудников
$john = new Developer('John Doe', 12000);
$jane = new Designer('Jane Doe', 15000);

// Добавим их в организацию
$organization = new Organization();
$organization->addEmployee($john);
$organization->addEmployee($jane);

echo "Чистая заработная плата: " . $organization->getNetSalaries(); // Чистая заработная плата: 15000
```

☕ Декоратор
-------------

Пример из реального мира:
> Представьте, что вы управляете автомастерской, предлагающей множество услуг. Теперь, как вы рассчитываете счет на оплату? Вы выбираете одну услугу и динамически продолжаете добавлять к ней цены на предоставляемые услуги, пока не получите окончательную стоимость. Здесь каждый вид услуг - это декоратор.

Простыми словами:
> Шаблон декоратора позволяет динамически изменять поведение объекта во время выполнения, заключая его в объект класса декоратора.

Википедия говорит
> В ООП шаблон декоратора - это шаблон проектирования, который позволяет добавлять поведение к отдельному объекту статически или динамически, не влияя на поведение других объектов из того же класса. Шаблон декоратора часто полезен для соблюдения принципа единой ответственности, поскольку он позволяет разделить функциональность между классами с уникальными проблемными областями.

**Пример из программирования**

Давайте возьмем кофе, как пример. Прежде всего, нам нужен простой кофе, имплементирующий интерфейс кофе:
```php
interface Coffee
{
    public function getCost();
    public function getDescription();
}

class SimpleCoffee implements Coffee
{
    public function getCost()
    {
        return 10;
    }

    public function getDescription()
    {
        return 'Simple coffee';
    }
}
```

Мы хотим сделать код масштабируемым, чтобы при необходимости его можно было изменять. Давайте сделаем несколько дополнений (декораторов):
```php
class MilkCoffee implements Coffee
{
    protected $coffee;

    public function __construct(Coffee $coffee)
    {
        $this->coffee = $coffee;
    }

    public function getCost()
    {
        return $this->coffee->getCost() + 2;
    }

    public function getDescription()
    {
        return $this->coffee->getDescription() . ', milk';
    }
}

class WhipCoffee implements Coffee
{
    protected $coffee;

    public function __construct(Coffee $coffee)
    {
        $this->coffee = $coffee;
    }

    public function getCost()
    {
        return $this->coffee->getCost() + 5;
    }

    public function getDescription()
    {
        return $this->coffee->getDescription() . ', whip';
    }
}

class VanillaCoffee implements Coffee
{
    protected $coffee;

    public function __construct(Coffee $coffee)
    {
        $this->coffee = $coffee;
    }

    public function getCost()
    {
        return $this->coffee->getCost() + 3;
    }

    public function getDescription()
    {
        return $this->coffee->getDescription() . ', vanilla';
    }
}
```

Давайте теперь сделаем кофе:
```php
$someCoffee = new SimpleCoffee();
echo $someCoffee->getCost(); // 10
echo $someCoffee->getDescription(); // Simple Coffee

$someCoffee = new MilkCoffee($someCoffee);
echo $someCoffee->getCost(); // 12
echo $someCoffee->getDescription(); // Simple Coffee, milk

$someCoffee = new WhipCoffee($someCoffee);
echo $someCoffee->getCost(); // 17
echo $someCoffee->getDescription(); // Simple Coffee, milk, whip

$someCoffee = new VanillaCoffee($someCoffee);
echo $someCoffee->getCost(); // 20
echo $someCoffee->getDescription(); // Simple Coffee, milk, whip, vanilla
```

📦 Фасад
----------------

Пример из реального мира:
> Как вы включаете компьютер? "Нажми кнопку включения", - говоришь ты! Это то, во что вы верите, потому что вы используете простой интерфейс, который компьютер предоставляет снаружи, а внутри должно быть сделано много вещей, чтобы это произошло. Этот простой интерфейс в сложной подсистеме является фасадом.

Простыми словами:
> Шаблон фасада обеспечивает упрощенный интерфейс для сложной подсистемы.

Википедия говорит:
> Фасад - это объект, который предоставляет упрощенный интерфейс для большего объема кода, такого как библиотека классов.

**Пример из программирования**

Возьмем наш компьютерный пример сверху. Здесь у нас есть компьютерный класс:
```php
class Computer
{
    public function getElectricShock()
    {
        echo "Ouch!";
    }

    public function makeSound()
    {
        echo "Beep beep!";
    }

    public function showLoadingScreen()
    {
        echo "Loading..";
    }

    public function bam()
    {
        echo "Ready to be used!";
    }

    public function closeEverything()
    {
        echo "Bup bup bup buzzzz!";
    }

    public function sooth()
    {
        echo "Zzzzz";
    }

    public function pullCurrent()
    {
        echo "Haaah!";
    }
}
```

Теперь у нас есть фасад:
```php
class ComputerFacade
{
    protected $computer;

    public function __construct(Computer $computer)
    {
        $this->computer = $computer;
    }

    public function turnOn()
    {
        $this->computer->getElectricShock();
        $this->computer->makeSound();
        $this->computer->showLoadingScreen();
        $this->computer->bam();
    }

    public function turnOff()
    {
        $this->computer->closeEverything();
        $this->computer->pullCurrent();
        $this->computer->sooth();
    }
}
```

Сейчас применим шаблон фасада:
```php
$computer = new ComputerFacade(new Computer());
$computer->turnOn(); // Ouch! Beep beep! Loading.. Ready to be used!
$computer->turnOff(); // Bup bup buzzz! Haah! Zzzzz
```

🍃 Легковес
---------
Пример из реального мира:
> Вы когда-нибудь пили свежий чай из какого-нибудь ларька? Они часто делают больше одной чашки, которую вы потребовали, и оставляют остальное для любого другого клиента, чтобы сэкономить ресурсы, например, газ и т.д. Шаблон Flyweight - это все об этом, то есть о совместном использовании.

Простыми словами:
> Он используется для минимизации использования памяти или вычислительных затрат за счет максимально возможного совместного использования с аналогичными объектами.

Википедия говорит:
> В компьютерном программировании легковес - это шаблон проектирования программного обеспечения. Легковес - это объект, который минимизирует использование памяти за счет обмена как можно большим количеством данных с другими подобными объектами; это способ использования объектов в больших количествах, когда простое повторное представление потребовало бы неприемлемого объема памяти.


**Программный пример**

Переводим наш пример с чаем сверху. Прежде всего, у нас есть виды чая и кофеварка:
```php
// Anything that will be cached is flyweight.
// Types of tea here will be flyweights.
class KarakTea
{
}

// Acts as a factory and saves the tea
class TeaMaker
{
    protected $availableTea = [];

    public function make($preference)
    {
        if (empty($this->availableTea[$preference])) {
            $this->availableTea[$preference] = new KarakTea();
        }

        return $this->availableTea[$preference];
    }
}
```

Теперь есть `TeaShop`("Магазин чая"), который принимает заказы и обслуживает их:
```php
class TeaShop
{
    protected $orders;
    protected $teaMaker;

    public function __construct(TeaMaker $teaMaker)
    {
        $this->teaMaker = $teaMaker;
    }

    public function takeOrder(string $teaType, int $table)
    {
        $this->orders[$table] = $this->teaMaker->make($teaType);
    }

    public function serve()
    {
        foreach ($this->orders as $table => $tea) {
            echo "Serving tea to table# " . $table;
        }
    }
}
```

И он может применяться как показано ниже:
```php
$teaMaker = new TeaMaker();
$shop = new TeaShop($teaMaker);

$shop->takeOrder('less sugar', 1);
$shop->takeOrder('more milk', 2);
$shop->takeOrder('without sugar', 5);

$shop->serve();
// Serving tea to table# 1
// Serving tea to table# 2
// Serving tea to table# 5
```

🎱 Заместитель
-------------------

Пример из реального мира:
> Вы когда-нибудь использовали карту доступа, чтобы пройти через дверь? Существует несколько вариантов открытия этой двери, т.е. ее можно открыть либо с помощью карты доступа, либо нажатием кнопки, которая обходит систему безопасности. Основная функция двери - открываться, но поверх нее добавлен прокси-сервер для добавления некоторых функций. Позвольте мне лучше объяснить это, используя приведенный ниже пример кода.

Простыми словами:
> Используя шаблон прокси, класс представляет функциональность другого класса.

Википедия говорит:
> Прокси-сервер в его наиболее общей форме - это класс, функционирующий как интерфейс по отношению к чему-то другому. Прокси - это объект-оболочка или агент, который вызывается клиентом для доступа к реальному обслуживающему объекту за кулисами. Использование прокси-сервера может быть просто перенаправлением на реальный объект или может обеспечить дополнительную логику. В прокси могут быть предоставлены дополнительные функции, например кэширование, когда операции с реальным объектом требуют больших ресурсов, или проверка предварительных условий перед вызовом операций с реальным объектом.

**Программный пример**

Возьмем наш пример с защитной дверью сверху. Во-первых, у нас есть интерфейс `Door` и реализация `Door`:
```php
interface Door
{
    public function open();
    public function close();
}

class LabDoor implements Door
{
    public function open()
    {
        echo "Opening lab door";
    }

    public function close()
    {
        echo "Closing the lab door";
    }
}
```

Теперь у нас есть прокси для защиты двери, какой пожелаем:
```php
class SecuredDoor
{
    protected $door;

    public function __construct(Door $door)
    {
        $this->door = $door;
    }

    public function open($password)
    {
        if ($this->authenticate($password)) {
            $this->door->open();
        } else {
            echo "Big no! It ain't possible.";
        }
    }

    public function authenticate($password)
    {
        return $password === '$ecr@t';
    }

    public function close()
    {
        $this->door->close();
    }
}
```

И он может применяться как показано ниже:
```php
$door = new SecuredDoor(new LabDoor());
$door->open('invalid'); // Big no! It ain't possible.

$door->open('$ecr@t'); // Opening lab door
$door->close(); // Closing lab door
```
Yet another example would be some sort of data-mapper implementation. For example, I recently made an ODM (Object Data Mapper) for MongoDB using this pattern where I wrote a proxy around mongo classes while utilizing the magic method `__call()`. All the method calls were proxied to the original mongo class and result retrieved was returned as it is but in case of `find` or `findOne` data was mapped to the required class objects and the object was returned instead of `Cursor`.

Еще одним примером может быть своего рода реализация картографа данных. Например, недавно я создал ODM (средство сопоставления объектных данных) для MongoDB, используя этот шаблон, в котором я написал прокси для классов Mongo, используя волшебный метод `__call()`. Все вызовы методов были перенаправлены в исходный класс Mongo, и полученный результат был возвращен как есть, но в случае `find` или `findOne` данные были сопоставлены с требуемыми объектами класса, и объект был возвращен вместо `Cursor`.


Поведенческие паттерны проектирования
==========================

In plain words
> It is concerned with assignment of responsibilities between the objects. What makes them different from structural patterns is they don't just specify the structure but also outline the patterns for message passing/communication between them. Or in other words, they assist in answering "How to run a behavior in software component?"

Wikipedia says
> In software engineering, behavioral design patterns are design patterns that identify common communication patterns between objects and realize these patterns. By doing so, these patterns increase flexibility in carrying out this communication.

* [Chain of Responsibility](#-chain-of-responsibility)
* [Command](#-command)
* [Iterator](#-iterator)
* [Mediator](#-mediator)
* [Memento](#-memento)
* [Observer](#-observer)
* [Visitor](#-visitor)
* [Strategy](#-strategy)
* [State](#-state)
* [Template Method](#-template-method)

🔗 Chain of Responsibility
-----------------------

Real world example
> For example, you have three payment methods (`A`, `B` and `C`) setup in your account; each having a different amount in it. `A` has 100 USD, `B` has 300 USD and `C` having 1000 USD and the preference for payments is chosen as `A` then `B` then `C`. You try to purchase something that is worth 210 USD. Using Chain of Responsibility, first of all account `A` will be checked if it can make the purchase, if yes purchase will be made and the chain will be broken. If not, request will move forward to account `B` checking for amount if yes chain will be broken otherwise the request will keep forwarding till it finds the suitable handler. Here `A`, `B` and `C` are links of the chain and the whole phenomenon is Chain of Responsibility.

In plain words
> It helps building a chain of objects. Request enters from one end and keeps going from object to object till it finds the suitable handler.

Wikipedia says
> In object-oriented design, the chain-of-responsibility pattern is a design pattern consisting of a source of command objects and a series of processing objects. Each processing object contains logic that defines the types of command objects that it can handle; the rest are passed to the next processing object in the chain.

**Programmatic Example**

Translating our account example above. First of all we have a base account having the logic for chaining the accounts together and some accounts

```php
abstract class Account
{
    protected $successor;
    protected $balance;

    public function setNext(Account $account)
    {
        $this->successor = $account;
    }

    public function pay(float $amountToPay)
    {
        if ($this->canPay($amountToPay)) {
            echo sprintf('Paid %s using %s' . PHP_EOL, $amountToPay, get_called_class());
        } elseif ($this->successor) {
            echo sprintf('Cannot pay using %s. Proceeding ..' . PHP_EOL, get_called_class());
            $this->successor->pay($amountToPay);
        } else {
            throw new Exception('None of the accounts have enough balance');
        }
    }

    public function canPay($amount): bool
    {
        return $this->balance >= $amount;
    }
}

class Bank extends Account
{
    protected $balance;

    public function __construct(float $balance)
    {
        $this->balance = $balance;
    }
}

class Paypal extends Account
{
    protected $balance;

    public function __construct(float $balance)
    {
        $this->balance = $balance;
    }
}

class Bitcoin extends Account
{
    protected $balance;

    public function __construct(float $balance)
    {
        $this->balance = $balance;
    }
}
```

Now let's prepare the chain using the links defined above (i.e. Bank, Paypal, Bitcoin)

```php
// Let's prepare a chain like below
//      $bank->$paypal->$bitcoin
//
// First priority bank
//      If bank can't pay then paypal
//      If paypal can't pay then bit coin

$bank = new Bank(100);          // Bank with balance 100
$paypal = new Paypal(200);      // Paypal with balance 200
$bitcoin = new Bitcoin(300);    // Bitcoin with balance 300

$bank->setNext($paypal);
$paypal->setNext($bitcoin);

// Let's try to pay using the first priority i.e. bank
$bank->pay(259);

// Output will be
// ==============
// Cannot pay using bank. Proceeding ..
// Cannot pay using paypal. Proceeding ..:
// Paid 259 using Bitcoin!
```

👮 Command
-------

Real world example
> A generic example would be you ordering food at a restaurant. You (i.e. `Client`) ask the waiter (i.e. `Invoker`) to bring some food (i.e. `Command`) and waiter simply forwards the request to Chef (i.e. `Receiver`) who has the knowledge of what and how to cook.
> Another example would be you (i.e. `Client`) switching on (i.e. `Command`) the television (i.e. `Receiver`) using a remote control (`Invoker`).

In plain words
> Allows you to encapsulate actions in objects. The key idea behind this pattern is to provide the means to decouple client from receiver.

Wikipedia says
> In object-oriented programming, the command pattern is a behavioral design pattern in which an object is used to encapsulate all information needed to perform an action or trigger an event at a later time. This information includes the method name, the object that owns the method and values for the method parameters.

**Programmatic Example**

First of all we have the receiver that has the implementation of every action that could be performed
```php
// Receiver
class Bulb
{
    public function turnOn()
    {
        echo "Bulb has been lit";
    }

    public function turnOff()
    {
        echo "Darkness!";
    }
}
```
then we have an interface that each of the commands are going to implement and then we have a set of commands
```php
interface Command
{
    public function execute();
    public function undo();
    public function redo();
}

// Command
class TurnOn implements Command
{
    protected $bulb;

    public function __construct(Bulb $bulb)
    {
        $this->bulb = $bulb;
    }

    public function execute()
    {
        $this->bulb->turnOn();
    }

    public function undo()
    {
        $this->bulb->turnOff();
    }

    public function redo()
    {
        $this->execute();
    }
}

class TurnOff implements Command
{
    protected $bulb;

    public function __construct(Bulb $bulb)
    {
        $this->bulb = $bulb;
    }

    public function execute()
    {
        $this->bulb->turnOff();
    }

    public function undo()
    {
        $this->bulb->turnOn();
    }

    public function redo()
    {
        $this->execute();
    }
}
```
Then we have an `Invoker` with whom the client will interact to process any commands
```php
// Invoker
class RemoteControl
{
    public function submit(Command $command)
    {
        $command->execute();
    }
}
```
Finally let's see how we can use it in our client
```php
$bulb = new Bulb();

$turnOn = new TurnOn($bulb);
$turnOff = new TurnOff($bulb);

$remote = new RemoteControl();
$remote->submit($turnOn); // Bulb has been lit!
$remote->submit($turnOff); // Darkness!
```

Command pattern can also be used to implement a transaction based system. Where you keep maintaining the history of commands as soon as you execute them. If the final command is successfully executed, all good otherwise just iterate through the history and keep executing the `undo` on all the executed commands.

➿ Iterator
--------

Real world example
> An old radio set will be a good example of iterator, where user could start at some channel and then use next or previous buttons to go through the respective channels. Or take an example of MP3 player or a TV set where you could press the next and previous buttons to go through the consecutive channels or in other words they all provide an interface to iterate through the respective channels, songs or radio stations.  

In plain words
> It presents a way to access the elements of an object without exposing the underlying presentation.

Wikipedia says
> In object-oriented programming, the iterator pattern is a design pattern in which an iterator is used to traverse a container and access the container's elements. The iterator pattern decouples algorithms from containers; in some cases, algorithms are necessarily container-specific and thus cannot be decoupled.

**Programmatic example**

In PHP it is quite easy to implement using SPL (Standard PHP Library). Translating our radio stations example from above. First of all we have `RadioStation`

```php
class RadioStation
{
    protected $frequency;

    public function __construct(float $frequency)
    {
        $this->frequency = $frequency;
    }

    public function getFrequency(): float
    {
        return $this->frequency;
    }
}
```
Then we have our iterator

```php
use Countable;
use Iterator;

class StationList implements Countable, Iterator
{
    /** @var RadioStation[] $stations */
    protected $stations = [];

    /** @var int $counter */
    protected $counter;

    public function addStation(RadioStation $station)
    {
        $this->stations[] = $station;
    }

    public function removeStation(RadioStation $toRemove)
    {
        $toRemoveFrequency = $toRemove->getFrequency();
        $this->stations = array_filter($this->stations, function (RadioStation $station) use ($toRemoveFrequency) {
            return $station->getFrequency() !== $toRemoveFrequency;
        });
    }

    public function count(): int
    {
        return count($this->stations);
    }

    public function current(): RadioStation
    {
        return $this->stations[$this->counter];
    }

    public function key()
    {
        return $this->counter;
    }

    public function next()
    {
        $this->counter++;
    }

    public function rewind()
    {
        $this->counter = 0;
    }

    public function valid(): bool
    {
        return isset($this->stations[$this->counter]);
    }
}
```
And then it can be used as
```php
$stationList = new StationList();

$stationList->addStation(new RadioStation(89));
$stationList->addStation(new RadioStation(101));
$stationList->addStation(new RadioStation(102));
$stationList->addStation(new RadioStation(103.2));

foreach($stationList as $station) {
    echo $station->getFrequency() . PHP_EOL;
}

$stationList->removeStation(new RadioStation(89)); // Will remove station 89
```

👽 Mediator
========

Real world example
> A general example would be when you talk to someone on your mobile phone, there is a network provider sitting between you and them and your conversation goes through it instead of being directly sent. In this case network provider is mediator.

In plain words
> Mediator pattern adds a third party object (called mediator) to control the interaction between two objects (called colleagues). It helps reduce the coupling between the classes communicating with each other. Because now they don't need to have the knowledge of each other's implementation.

Wikipedia says
> In software engineering, the mediator pattern defines an object that encapsulates how a set of objects interact. This pattern is considered to be a behavioral pattern due to the way it can alter the program's running behavior.

**Programmatic Example**

Here is the simplest example of a chat room (i.e. mediator) with users (i.e. colleagues) sending messages to each other.

First of all, we have the mediator i.e. the chat room

```php
interface ChatRoomMediator 
{
    public function showMessage(User $user, string $message);
}

// Mediator
class ChatRoom implements ChatRoomMediator
{
    public function showMessage(User $user, string $message)
    {
        $time = date('M d, y H:i');
        $sender = $user->getName();

        echo $time . '[' . $sender . ']:' . $message;
    }
}
```

Then we have our users i.e. colleagues
```php
class User {
    protected $name;
    protected $chatMediator;

    public function __construct(string $name, ChatRoomMediator $chatMediator) {
        $this->name = $name;
        $this->chatMediator = $chatMediator;
    }

    public function getName() {
        return $this->name;
    }

    public function send($message) {
        $this->chatMediator->showMessage($this, $message);
    }
}
```
And the usage
```php
$mediator = new ChatRoom();

$john = new User('John Doe', $mediator);
$jane = new User('Jane Doe', $mediator);

$john->send('Hi there!');
$jane->send('Hey!');

// Output will be
// Feb 14, 10:58 [John]: Hi there!
// Feb 14, 10:58 [Jane]: Hey!
```

💾 Memento
-------
Real world example
> Take the example of calculator (i.e. originator), where whenever you perform some calculation the last calculation is saved in memory (i.e. memento) so that you can get back to it and maybe get it restored using some action buttons (i.e. caretaker).

In plain words
> Memento pattern is about capturing and storing the current state of an object in a manner that it can be restored later on in a smooth manner.

Wikipedia says
> The memento pattern is a software design pattern that provides the ability to restore an object to its previous state (undo via rollback).

Usually useful when you need to provide some sort of undo functionality.

**Programmatic Example**

Lets take an example of text editor which keeps saving the state from time to time and that you can restore if you want.

First of all we have our memento object that will be able to hold the editor state

```php
class EditorMemento
{
    protected $content;

    public function __construct(string $content)
    {
        $this->content = $content;
    }

    public function getContent()
    {
        return $this->content;
    }
}
```

Then we have our editor i.e. originator that is going to use memento object

```php
class Editor
{
    protected $content = '';

    public function type(string $words)
    {
        $this->content = $this->content . ' ' . $words;
    }

    public function getContent()
    {
        return $this->content;
    }

    public function save()
    {
        return new EditorMemento($this->content);
    }

    public function restore(EditorMemento $memento)
    {
        $this->content = $memento->getContent();
    }
}
```

And then it can be used as

```php
$editor = new Editor();

// Type some stuff
$editor->type('This is the first sentence.');
$editor->type('This is second.');

// Save the state to restore to : This is the first sentence. This is second.
$saved = $editor->save();

// Type some more
$editor->type('And this is third.');

// Output: Content before Saving
echo $editor->getContent(); // This is the first sentence. This is second. And this is third.

// Restoring to last saved state
$editor->restore($saved);

$editor->getContent(); // This is the first sentence. This is second.
```

😎 Observer
--------
Real world example
> A good example would be the job seekers where they subscribe to some job posting site and they are notified whenever there is a matching job opportunity.   

In plain words
> Defines a dependency between objects so that whenever an object changes its state, all its dependents are notified.

Wikipedia says
> The observer pattern is a software design pattern in which an object, called the subject, maintains a list of its dependents, called observers, and notifies them automatically of any state changes, usually by calling one of their methods.

**Programmatic example**

Translating our example from above. First of all we have job seekers that need to be notified for a job posting
```php
class JobPost
{
    protected $title;

    public function __construct(string $title)
    {
        $this->title = $title;
    }

    public function getTitle()
    {
        return $this->title;
    }
}

class JobSeeker implements Observer
{
    protected $name;

    public function __construct(string $name)
    {
        $this->name = $name;
    }

    public function onJobPosted(JobPost $job)
    {
        // Do something with the job posting
        echo 'Hi ' . $this->name . '! New job posted: '. $job->getTitle();
    }
}
```
Then we have our job postings to which the job seekers will subscribe
```php
class EmploymentAgency implements Observable
{
    protected $observers = [];

    protected function notify(JobPost $jobPosting)
    {
        foreach ($this->observers as $observer) {
            $observer->onJobPosted($jobPosting);
        }
    }

    public function attach(Observer $observer)
    {
        $this->observers[] = $observer;
    }

    public function addJob(JobPost $jobPosting)
    {
        $this->notify($jobPosting);
    }
}
```
Then it can be used as
```php
// Create subscribers
$johnDoe = new JobSeeker('John Doe');
$janeDoe = new JobSeeker('Jane Doe');

// Create publisher and attach subscribers
$jobPostings = new EmploymentAgency();
$jobPostings->attach($johnDoe);
$jobPostings->attach($janeDoe);

// Add a new job and see if subscribers get notified
$jobPostings->addJob(new JobPost('Software Engineer'));

// Output
// Hi John Doe! New job posted: Software Engineer
// Hi Jane Doe! New job posted: Software Engineer
```

🏃 Visitor
-------
Real world example
> Consider someone visiting Dubai. They just need a way (i.e. visa) to enter Dubai. After arrival, they can come and visit any place in Dubai on their own without having to ask for permission or to do some leg work in order to visit any place here; just let them know of a place and they can visit it. Visitor pattern lets you do just that, it helps you add places to visit so that they can visit as much as they can without having to do any legwork.

In plain words
> Visitor pattern lets you add further operations to objects without having to modify them.

Wikipedia says
> In object-oriented programming and software engineering, the visitor design pattern is a way of separating an algorithm from an object structure on which it operates. A practical result of this separation is the ability to add new operations to existing object structures without modifying those structures. It is one way to follow the open/closed principle.

**Programmatic example**

Let's take an example of a zoo simulation where we have several different kinds of animals and we have to make them Sound. Let's translate this using visitor pattern

```php
// Visitee
interface Animal
{
    public function accept(AnimalOperation $operation);
}

// Visitor
interface AnimalOperation
{
    public function visitMonkey(Monkey $monkey);
    public function visitLion(Lion $lion);
    public function visitDolphin(Dolphin $dolphin);
}
```
Then we have our implementations for the animals
```php
class Monkey implements Animal
{
    public function shout()
    {
        echo 'Ooh oo aa aa!';
    }

    public function accept(AnimalOperation $operation)
    {
        $operation->visitMonkey($this);
    }
}

class Lion implements Animal
{
    public function roar()
    {
        echo 'Roaaar!';
    }

    public function accept(AnimalOperation $operation)
    {
        $operation->visitLion($this);
    }
}

class Dolphin implements Animal
{
    public function speak()
    {
        echo 'Tuut tuttu tuutt!';
    }

    public function accept(AnimalOperation $operation)
    {
        $operation->visitDolphin($this);
    }
}
```
Let's implement our visitor
```php
class Speak implements AnimalOperation
{
    public function visitMonkey(Monkey $monkey)
    {
        $monkey->shout();
    }

    public function visitLion(Lion $lion)
    {
        $lion->roar();
    }

    public function visitDolphin(Dolphin $dolphin)
    {
        $dolphin->speak();
    }
}
```

And then it can be used as
```php
$monkey = new Monkey();
$lion = new Lion();
$dolphin = new Dolphin();

$speak = new Speak();

$monkey->accept($speak);    // Ooh oo aa aa!    
$lion->accept($speak);      // Roaaar!
$dolphin->accept($speak);   // Tuut tutt tuutt!
```
We could have done this simply by having an inheritance hierarchy for the animals but then we would have to modify the animals whenever we would have to add new actions to animals. But now we will not have to change them. For example, let's say we are asked to add the jump behavior to the animals, we can simply add that by creating a new visitor i.e.

```php
class Jump implements AnimalOperation
{
    public function visitMonkey(Monkey $monkey)
    {
        echo 'Jumped 20 feet high! on to the tree!';
    }

    public function visitLion(Lion $lion)
    {
        echo 'Jumped 7 feet! Back on the ground!';
    }

    public function visitDolphin(Dolphin $dolphin)
    {
        echo 'Walked on water a little and disappeared';
    }
}
```
And for the usage
```php
$jump = new Jump();

$monkey->accept($speak);   // Ooh oo aa aa!
$monkey->accept($jump);    // Jumped 20 feet high! on to the tree!

$lion->accept($speak);     // Roaaar!
$lion->accept($jump);      // Jumped 7 feet! Back on the ground!

$dolphin->accept($speak);  // Tuut tutt tuutt!
$dolphin->accept($jump);   // Walked on water a little and disappeared
```

💡 Strategy
--------

Real world example
> Consider the example of sorting, we implemented bubble sort but the data started to grow and bubble sort started getting very slow. In order to tackle this we implemented Quick sort. But now although the quick sort algorithm was doing better for large datasets, it was very slow for smaller datasets. In order to handle this we implemented a strategy where for small datasets, bubble sort will be used and for larger, quick sort.

In plain words
> Strategy pattern allows you to switch the algorithm or strategy based upon the situation.

Wikipedia says
> In computer programming, the strategy pattern (also known as the policy pattern) is a behavioural software design pattern that enables an algorithm's behavior to be selected at runtime.

**Programmatic example**

Translating our example from above. First of all we have our strategy interface and different strategy implementations

```php
interface SortStrategy
{
    public function sort(array $dataset): array;
}

class BubbleSortStrategy implements SortStrategy
{
    public function sort(array $dataset): array
    {
        echo "Sorting using bubble sort";

        // Do sorting
        return $dataset;
    }
}

class QuickSortStrategy implements SortStrategy
{
    public function sort(array $dataset): array
    {
        echo "Sorting using quick sort";

        // Do sorting
        return $dataset;
    }
}
```

And then we have our client that is going to use any strategy
```php
class Sorter
{
    protected $sorter;

    public function __construct(SortStrategy $sorter)
    {
        $this->sorter = $sorter;
    }

    public function sort(array $dataset): array
    {
        return $this->sorter->sort($dataset);
    }
}
```
And it can be used as
```php
$dataset = [1, 5, 4, 3, 2, 8];

$sorter = new Sorter(new BubbleSortStrategy());
$sorter->sort($dataset); // Output : Sorting using bubble sort

$sorter = new Sorter(new QuickSortStrategy());
$sorter->sort($dataset); // Output : Sorting using quick sort
```

💢 State
-----
Real world example
> Imagine you are using some drawing application, you choose the paint brush to draw. Now the brush changes its behavior based on the selected color i.e. if you have chosen red color it will draw in red, if blue then it will be in blue etc.  

In plain words
> It lets you change the behavior of a class when the state changes.

Wikipedia says
> The state pattern is a behavioral software design pattern that implements a state machine in an object-oriented way. With the state pattern, a state machine is implemented by implementing each individual state as a derived class of the state pattern interface, and implementing state transitions by invoking methods defined by the pattern's superclass.
> The state pattern can be interpreted as a strategy pattern which is able to switch the current strategy through invocations of methods defined in the pattern's interface.

**Programmatic example**

Let's take an example of text editor, it lets you change the state of text that is typed i.e. if you have selected bold, it starts writing in bold, if italic then in italics etc.

First of all we have our state interface and some state implementations

```php
interface WritingState
{
    public function write(string $words);
}

class UpperCase implements WritingState
{
    public function write(string $words)
    {
        echo strtoupper($words);
    }
}

class LowerCase implements WritingState
{
    public function write(string $words)
    {
        echo strtolower($words);
    }
}

class DefaultText implements WritingState
{
    public function write(string $words)
    {
        echo $words;
    }
}
```
Then we have our editor
```php
class TextEditor
{
    protected $state;

    public function __construct(WritingState $state)
    {
        $this->state = $state;
    }

    public function setState(WritingState $state)
    {
        $this->state = $state;
    }

    public function type(string $words)
    {
        $this->state->write($words);
    }
}
```
And then it can be used as
```php
$editor = new TextEditor(new DefaultText());

$editor->type('First line');

$editor->setState(new UpperCase());

$editor->type('Second line');
$editor->type('Third line');

$editor->setState(new LowerCase());

$editor->type('Fourth line');
$editor->type('Fifth line');

// Output:
// First line
// SECOND LINE
// THIRD LINE
// fourth line
// fifth line
```

📒 Template Method
---------------

Real world example
> Suppose we are getting some house built. The steps for building might look like
> - Prepare the base of house
> - Build the walls
> - Add roof
> - Add other floors

> The order of these steps could never be changed i.e. you can't build the roof before building the walls etc but each of the steps could be modified for example walls can be made of wood or polyester or stone.

In plain words
> Template method defines the skeleton of how a certain algorithm could be performed, but defers the implementation of those steps to the children classes.

Wikipedia says
> In software engineering, the template method pattern is a behavioral design pattern that defines the program skeleton of an algorithm in an operation, deferring some steps to subclasses. It lets one redefine certain steps of an algorithm without changing the algorithm's structure.

**Programmatic Example**

Imagine we have a build tool that helps us test, lint, build, generate build reports (i.e. code coverage reports, linting report etc) and deploy our app on the test server.

First of all we have our base class that specifies the skeleton for the build algorithm
```php
abstract class Builder
{

    // Template method
    final public function build()
    {
        $this->test();
        $this->lint();
        $this->assemble();
        $this->deploy();
    }

    abstract public function test();
    abstract public function lint();
    abstract public function assemble();
    abstract public function deploy();
}
```

Then we can have our implementations

```php
class AndroidBuilder extends Builder
{
    public function test()
    {
        echo 'Running android tests';
    }

    public function lint()
    {
        echo 'Linting the android code';
    }

    public function assemble()
    {
        echo 'Assembling the android build';
    }

    public function deploy()
    {
        echo 'Deploying android build to server';
    }
}

class IosBuilder extends Builder
{
    public function test()
    {
        echo 'Running ios tests';
    }

    public function lint()
    {
        echo 'Linting the ios code';
    }

    public function assemble()
    {
        echo 'Assembling the ios build';
    }

    public function deploy()
    {
        echo 'Deploying ios build to server';
    }
}
```
And then it can be used as

```php
$androidBuilder = new AndroidBuilder();
$androidBuilder->build();

// Output:
// Running android tests
// Linting the android code
// Assembling the android build
// Deploying android build to server

$iosBuilder = new IosBuilder();
$iosBuilder->build();

// Output:
// Running ios tests
// Linting the ios code
// Assembling the ios build
// Deploying ios build to server
```

## 🚦 В заключение

И на этом статья заканчивается. Я буду продолжать улучшать ее, так что вы, возможно, захотите посмотреть/отметить звездой этот репозиторий, чтобы вернуться. Кроме того, у меня есть планы написать подобное об архитектурных шаблонах, следите за обновлениями.

## 👬 Содействие

- Сообщать о проблемах в issues
- Открытые запросы на PR
- Распространение информации
- Напишите отзыв в твиттере (на английском, разумеется) [![Twitter URL](https://img.shields.io/twitter/url/https/twitter.com/kamranahmedse.svg?style=social&label=Follow%20%40kamranahmedse)](https://twitter.com/kamranahmedse)

## Лицензия

[![Лицензия: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)
