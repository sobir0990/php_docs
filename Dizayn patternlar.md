## Dizayn patternlar

> - Konstruktsion (yaratuvchi) patternlar (`Creational Patterns`) <br>
    **Vazifasi**: Obyektlarni yaratish jarayonini soddalashtirish va boshqarish uchun ishlatiladi. <br>
    **Misol**: Tasavvur qiling, sizning loyihangizda turli xil mashinalar (avtomobillar) mavjud va har safar yangi mashina yaratmoqchi bo'lganingizda, siz turli xil xususiyatlarga ega bo'lgan obyektlarni yaratishingiz kerak. Buning uchun konstruktsion patternlar yordamida obyekt yaratish jarayonini soddalashtirish mumkin.
> - Strukturaviy patternlar (`Structural Patterns`)
> - Xulq-atvor patternlari (`Behavioral Patterns`)

> ### Konstruktsion patternlar (Creational Patterns)
> - `Singleton pattern` - Singleton patterni bir obyektni faqat bitta nusxasini yaratishga imkon beradi va unga butun dastur davomida kirishni ta’minlaydi. <br>
    **Vazifasi**: Biror tizim yoki sinf faqat bitta obyektga ega bo'lishi kerak bo'lgan holatlarda ishlatiladi (
    masalan, ma'lumotlar bazasi bilan bog'lanish). <br>
    Maqsadi: ba’zi hollarda dastur davomida faqat bitta obyekt mavjud bo‘lishi kerak bo‘lgan sinflarni boshqarishdir. Misol uchun, konfiguratsion ma’lumotlar, log faylga yozuvlar yoki ma’lumotlar bazasiga ulanish uchun har safar yangi obyekt yaratishning zaruriyati yo‘q. Bitta yaratilgan obyekt orqali hamma joyda foydalanish yanada optimal hisoblanadi. <br>
    **Misol**:

```
class Singleton {
    private static $instance;

    private function __construct() {}

    public static function getInstance() {
        if (self::$instance === null) {
            self::$instance = new Singleton();
        }
        return self::$instance;
    }
    
     public function doSomething() {
        echo "Bu metod bitta obyekt orqali chaqirilmoqda!\n";
    }
    
}

// Birinchi obyektni yaratish
$obj = Singleton::getInstance(); // faqat bitta obyekt yaratiladi


// Yana bir marta obyekt yaratishga harakat qilish
$singleton2 = Singleton::getInstance(); // Yangi obyekt yaratilmaydi

// Har ikkala obyekt bir xilmi?
var_dump($obj === $singleton2); // bool(true)

Tushuntirish:
self::$instance tekshiriladi: self::$instance === null bo'lsa, yangi obyekt yaratiladi.
Agar obyekt allaqachon mavjud bo'lsa, self::$instance ning qiymati qaytariladi va yangi obyekt yaratishning hojati bo'lmaydi.
```

> - `Factory pattern` - Factory patterni obyektlarni yaratishni soddalashtirish uchun ishlatiladi. Sizga kerakli obyektni sinfni aniq belgilamasdan yaratishga imkon beradi. <br>
> - `Factory pattern` - murakkab obyektlarni yaratishni soddalashtiradi va qaysi sinfni yaratish kerakligini qaror qilishni markazlashtiradi.
    > Bu patternda obyektni yaratish uchun maxsus factory sinfi ishlatiladi

```

// Umumiy interfeys (Car)
interface Car {
    public function getType();
}

// Sedan avtomobil sinfi
class Sedan implements Car {
    public function getType() {
        return "Sedan avtomobili";
    }
}

// SUV avtomobil sinfi
class SUV implements Car {
    public function getType() {
        return "SUV avtomobili";
    }
}

class CarFactory {
    // Avtomobil yaratish fabrikasi
    public static function createCar($type) {
        if ($type == 'sedan') {
            return new Sedan();  // Sedan yaratish
        } elseif ($type == 'suv') {
            return new SUV();    // SUV yaratish
        }
        return null;
    }
}


// Mashina yaratamiz
$car1 = CarFactory::createCar('sedan');
echo $car1->getType(); // "Sedan avtomobili" chiqadi

$car2 = CarFactory::createCar('suv');
echo $car2->getType(); // "SUV avtomobili" chiqadi

```

> **Natija**:
> Ushbu misolda biz konstruktsion pattern – Factory pattern dan foydalanib, avtomobillarni yaratish jarayonini
> soddalashtirdik. Yangi avtomobil turini yaratishda sinflarni to'g'ridan-to'g'ri chaqirmasdan, CarFactory yordamida
> ularni boshqarib olamiz. Bu esa kodni modular va kengaytiriladigan qiladi. Agar kelajakda yangi avtomobil turini
> qo'shish kerak bo'lsa, biz faqat yangi sinfni qo'shamiz va fabrika funksiyasini yangilashimiz kifoya bo'ladi.



> ## Strukturaviy patternlar (Structural Patterns)
> **Vazifasi**: Obyektlar yoki sinflar o'rtasidagi bog'lanishni soddalashtirish va ulardan yaxshiroq foydalanishni
> ta'minlash uchun ishlatiladi. Kod modularligini va ishlashini yaxshilashga qaratilgan.
>  - `Adapter pattern` - Manosi: Adapter patterni turli interfeyslarni moslashtirish va ularni birgalikda ishlashini ta'minlash uchun ishlatiladi. <br>
     **Vazifasi**: Misol uchun, eski va yangi tizimlar orasida integratsiya qilishda moslama vazifasini bajaradi.

```
class OldSystem {
    public function oldRequest() {
        return "Eski tizimdan so'rov";
    }
}

class NewSystemAdapter {
    private $oldSystem;

    public function __construct(OldSystem $oldSystem) {
        $this->oldSystem = $oldSystem;
    }

    public function newRequest() {
        return $this->oldSystem->oldRequest();
    }
}

$adapter = new NewSystemAdapter(new OldSystem());
echo $adapter->newRequest(); // "Eski tizimdan so'rov" qaytaradi

```

> - `Decorator pattern` - Manosi: Decorator patterni obyektning funksionalligini dinamik ravishda kengaytirish (qo'shish) uchun ishlatiladi <br>
    **Vazifasi**: Misol uchun, sizga biror mahsulotga qo‘shimcha xizmatlarni qo‘shish kerak bo‘lsa, asl sinfni o‘zgartirmasdan dekorator orqali bu vazifani bajarishingiz mumkin.

```
interface Coffee {
    public function cost();
}

class BasicCoffee implements Coffee {
    public function cost() {
        return 1000; // oddiy kofe narxi
    }
}

class MilkDecorator implements Coffee {
    protected $coffee;

    public function __construct(Coffee $coffee) {
        $this->coffee = $coffee;
    }

    public function cost() {
        return $this->coffee->cost() + 500; // sut qo'shilganda narx oshadi
    }
}

$coffee = new MilkDecorator(new BasicCoffee());
echo $coffee->cost(); // Narxi 1500 bo'ladi (1000+500)

```

> ## Xulq-atvor patternlari (Behavioral Patterns)
> **Vazifasi** Obyektlar qanday qilib bir-biri bilan muloqot qilishini va ularning harakatlarini qanday
> muvofiqlashtirishni
> osonlashtiradi.
> - `Observer pattern` - Manosi: Observer patterni biror obyektning holati o'zgarganda unga bog'langan boshqa obyektlarni avtomatik ravishda xabardor qilishga imkon beradi. <br>
    **Vazifasi**: Bu pattern yangiliklar yoki o'zgarishlarni kuzatish va shu holatda javob qaytarish kerak bo'lgan hollarda ishlatiladi.
> - `Observer pattern` - bir obyektning holati o'zgarganda boshqa obyektlarga habar berish uchun ishlatiladi. Bu pattern obyektlar o'rtasida bir birini kuzatishni taminlaydi

> #### Observer Pattern nima? <br>
> `Observer pattern` — bu bir obyektni kuzatayotgan boshqa obyektlarni xabardor qilish uchun ishlatiladigan dizayn patterni.
> Tasavvur qiling, sizda bir asosiy obyekt bor va bir nechta kuzatuvchilar (obyektlar) unga bog'langan. Asosiy obyekt
> o'zgarganda, u avtomatik ravishda barcha kuzatuvchilarga bu o'zgarish haqida xabar yuboradi. <br>
> #### Masalan:
> - Bir yangiliklar sayti yangilik chiqaradi.
> - Siz o'sha yangiliklarni kuzatib turasiz.
> - Sayt yangi yangilik chop etganda, sizga avtomatik ravishda yangilik haqida xabar keladi. <br>
> #### Bu holatda:
> - Yangiliklar sayti — bu asosiy obyekt (Subject).
> - Siz — bu kuzatuvchi (Observer).
> - Yangilik kelganda sizga xabar beriladi — bu kuzatuvchilarni xabardor qilish jarayoni. <br>
> #### Patternning maqsadi:
> - Patternning maqsadi shundaki, asosiy obyekt o'zgarganida kuzatuvchilar avtomatik ravishda bu o'zgarishni bilib oladilar. Har safar kuzatuvchilarni qo'lda xabardor qilish shart emas.

```
interface Observer {
    public function update($data);
}

class ConcreteObserver implements Observer {
    public function update($data) {
        echo "Yangilik: $data\n";
    }
}

class Subject {
    private $observers = [];

    public function attach(Observer $observer) {
        $this->observers[] = $observer;
    }

    public function notify($data) {
        foreach ($this->observers as $observer) {
            $observer->update($data);
        }
    }
}

$subject = new Subject();
$observer = new ConcreteObserver();
$subject->attach($observer);
$subject->notify("Yangi ma'lumot keldi!");
```

```
<?php
// 1. Kuzatuvchi interfeysi
interface Observer {
    public function update(string $news);  // Yangilik kelganda chaqiriladigan metod
}

// 2. Subyekt interfeysi (kuzatuvchilarni boshqaradigan asosiy obyekt)
interface Subject {
    public function attach(Observer $observer);  // Kuzatuvchini ro'yxatdan o'tkazish
    public function detach(Observer $observer);  // Kuzatuvchini olib tashlash
    public function notify();                    // Kuzatuvchilarga xabar yuborish
}

// 3. Yangiliklar sayti (Subject ning amalga oshirilishi)
class NewsAgency implements Subject {
    private $observers = [];  // Kuzatuvchilar ro'yxati
    private $latestNews;      // Eng so'nggi yangilik

    // Kuzatuvchini ro'yxatdan o'tkazish
    public function attach(Observer $observer) {
        $this->observers[] = $observer;
    }

    // Kuzatuvchini olib tashlash
    public function detach(Observer $observer) {
        $key = array_search($observer, $this->observers);
        if ($key !== false) {
            unset($this->observers[$key]);
        }
    }

    // Barcha kuzatuvchilarga xabar qilish
    public function notify() {
        foreach ($this->observers as $observer) {
            $observer->update($this->latestNews);
        }
    }

    // Yangi yangilik o'rnatish va kuzatuvchilarga xabar qilish
    public function addNews(string $news) {
        $this->latestNews = $news;
        $this->notify();
    }
}

// 4. Foydalanuvchi (Observer ning amalga oshirilishi)
class User implements Observer {
    private $name;

    public function __construct($name) {
        $this->name = $name;
    }

    // Yangilik kelganda bu metod chaqiriladi
    public function update(string $news) {
        echo $this->name . " yangilik oldi: " . $news . "\n";
    }
}

// 5. Misol
$newsAgency = new NewsAgency();

// Foydalanuvchilarni yaratish
$user1 = new User("Ali");
$user2 = new User("Vali");
$user3 = new User("Sami");

// Foydalanuvchilarni kuzatuvchi sifatida qo'shish
$newsAgency->attach($user1);
$newsAgency->attach($user2);
$newsAgency->attach($user3);

// Yangilik chop etish
$newsAgency->addNews("PHP 8.2 chiqarildi!");

// Foydalanuvchi 1 ni olib tashlash
$newsAgency->detach($user1);

// Yangi yangilik chop etish
$newsAgency->addNews("MySQL 8.0 yangilandi!");

```

> - `Strategy pattern` - Manosi: Strategy patterni turli xil algoritmlar orasidan kerakli birini tanlab qo‘llash imkonini beradi. Har bir algoritm alohida sinf sifatida ajratiladi va ularni oson almashtirish mumkin bo'ladi.<br>
    **Vazifasi**: Masalan, to‘lov tizimlarida (kredit karta, PayPal va boshqalar) har bir to‘lov usulini har xil ko‘rinishda qo‘llash uchun ishlatiladi.

```
interface PaymentStrategy {
    public function pay($amount);
}

class CreditCardPayment implements PaymentStrategy {
    public function pay($amount) {
        echo "Kredit karta bilan $amount to'landi\n";
    }
}

class PayPalPayment implements PaymentStrategy {
    public function pay($amount) {
        echo "PayPal orqali $amount to'landi\n";
    }
}

class PaymentProcessor {
    private $paymentMethod;

    public function __construct(PaymentStrategy $paymentMethod) {
        $this->paymentMethod = $paymentMethod;
    }

    public function process($amount) {
        $this->paymentMethod->pay($amount);
    }
}

$payment = new PaymentProcessor(new PayPalPayment());
$payment->process(100); // PayPal orqali to'lov amalga oshiradi

```

## Dizayn patternlarining asosiy vazifalari va afzalliklari

> - Qayta foydalanish imkoniyati: Patternlar ko'p marta ishlatiladigan kodning oldindan ishlab chiqilgan echimlari bo'
    lib, yangi muammolar uchun ham foydalaniladi.
> - Modularlik: Kodni modullarga ajratish orqali unga keyinchalik o'zgartirishlar kiritish yoki kengaytirish osonlashadi.
> - Ko'rinish va tushunarlilik: Dasturdagi murakkab jarayonlarni patternlar orqali soddalashtirib, kodni yaxshiroq tushunish imkoniyati yaratiladi.
> - Qisqartirish: Patternlar yordamida keraksiz takroriy kodlarni kamaytirish va ma'lum bir muammoni hal qilish uchun optimal yechim yaratish mumkin.