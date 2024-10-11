# Php

## OOP haqida

> Ob'ektga yo'naltirilgan dasturlas[middle.md](middle.md)h (OOP) asosan to'rtta asosiy tamoyilga bo'linadi. Ushbu tamoyillar dasturchilarga samarali, o'qilishi oson va qayta ishlatiladigan kod yozish imkonini beradi. Keling, har bir tamoyilni batafsil ko'rib chiqamiz:
> - `Inkapsulyatsiya (Encapsulation)` Bu tamoyil ob'ektning ichki holatini tashqi ta'sirlardan himoya qiladi.
    > `Inkapsulyatsiya` yordamida ob'ekt ichidagi atributlar va metodlarga kirishni boshqarish mumkin. Ob'ektning ma'lumotlarini faqat uning metodlari orqali o'zgartirish va olish mumkin.

```
class BankAccount {
    private $balance; // Maxfiy atribut

    public function __construct($initial_balance) {
        $this->balance = $initial_balance; // Inicializatsiya
    }

    public function deposit($amount) {
        if ($amount > 0) {
            $this->balance += $amount; // Kirim
        }
    }

    public function withdraw($amount) {
        if ($amount > 0 && $this->balance >= $amount) {
            $this->balance -= $amount; // Chiqarish
        }
    }

    public function getBalance() {
        return $this->balance; // Balansni olish
    }
}

$account = new BankAccount(100); // BankAccount ob'ekti
$account->deposit(50); // 50 so'm qo'shish
$account->withdraw(30); // 30 so'm yechish
echo $account->getBalance(); // 120
```

> - `Merosi (Inheritance)` Meros olish orqali klasslar o'zaro bog'lanishi va umumiy xususiyatlarni meros qilib olishi mumkin.
> - Foydalari  Kodni qayta ishlatish, yangilash va yangi klasslar yaratish jarayonini soddalashtiradi.

```
class Animal {
    public function speak() {
        return "Animal sound"; // Umumiy ovoz
    }
}

class Dog extends Animal { // Dog klassi Animal klassidan meros oladi
    public function speak() {
        return "Woof!"; // Itning ovozi
    }
}

class Cat extends Animal { // Cat klassi Animal klassidan meros oladi
    public function speak() {
        return "Meow!"; // Mushukning ovozi
    }
}

$dog = new Dog();
$cat = new Cat();
echo $dog->speak(); // "Woof!"
echo $cat->speak(); // "Meow!"

```


> - `Polimorfizm` asosan meros olish va interfeyslardan foydalanish orqali amalga oshiriladi. Quyidagi misollar orqali polimorfizmni tushuntirib beraman.
> - `Polimorfizm (Polymorphism)` bu bir xil interfeys orqali turli xil ob'ektlarga kirish imkoniyatidir.
> - `Polimorfizm` bir xil interfeys orqali turli xil ob'ektlarga murojaat qilish imkonini beradi.

```
function makeSound(Animal $animal) {
    echo $animal->speak(); // Har qanday Animal uchun speak() metodini chaqirish
}

$dog = new Dog();
$cat = new Cat();

makeSound($dog); // "Woof!"
makeSound($cat); // "Meow!"



__________________
// Umumiy interfeys
interface PaymentMethod {
    public function pay($amount);
}

// PayPal orqali to'lov
class PayPalPayment implements PaymentMethod {
    public function pay($amount) {
        echo "Paid $amount using PayPal.\n";
    }
}

// Stripe orqali to'lov
class StripePayment implements PaymentMethod {
    public function pay($amount) {
        echo "Paid $amount using Stripe.\n";
    }
}

// Credit Card orqali to'lov
class CreditCardPayment implements PaymentMethod {
    public function pay($amount) {
        echo "Paid $amount using Credit Card.\n";
    }
}

// Polimorfizm orqali to'lovni amalga oshirish
function processPayment(PaymentMethod $paymentMethod, $amount) {
    $paymentMethod->pay($amount);
}

// Foydalanuvchi to'lov turini tanlaydi
$paypal = new PayPalPayment();
$stripe = new StripePayment();
$creditCard = new CreditCardPayment();

// Har xil to'lov usullari bilan bir xil interfeys orqali to'lovni amalga oshiramiz
processPayment($paypal, 100);         // Paid 100 using PayPal.
processPayment($stripe, 200);         // Paid 200 using Stripe.
processPayment($creditCard, 300);     // Paid 300 using Credit Card.


```

> - `Abstraksiya (Abstraction)` bu murakkab tizimlarni oddiy shaklda ifodalash imkoniyatidir. Abstraksiya yordamida tizimning muhim xususiyatlarini ajratib ko'rsatish va keraksiz tafsilotlarni yashirish mumkin.
    > `Abstrakt` klasslar yoki interfeyslar yordamida dasturchilar bir xil usullarni ishlatadigan turli klasslar uchun umumiy interfeys yaratadilar.\
    > `Misol`: Bir qator avtomobillar uchun Vehicle interfeysini yaratish va unda start() va stop() metodlarini ta'riflash.
> #### Hulosa
> Abstraksiya dasturlash jarayonini sodda va aniq qilish, xususiyatlarni ajratish, moslashuvchanlikni oshirish va bir xil interfeys orqali turli ob'ektlar bilan ishlashni osonlashtirish uchun juda muhim vositadir. Real loyihalarda abstraksiyaning to'g'ri ishlatilishi dasturiy ta'minotning sifatini va samaradorligini oshiradi.

```
interface Vehicle {
    public function start();
    public function stop();
}

class Car implements Vehicle {
    public function start() {
        echo "Car started";
    }

    public function stop() {
        echo "Car stopped";
    }
}

class Motorcycle implements Vehicle {
    public function start() {
        echo "Motorcycle started";
    }

    public function stop() {
        echo "Motorcycle stopped";
    }
}

```

__________________________________________________

## DRY, KISS, va YAGNI printsiplarini

> - `DRY`: Takrorlanadigan kodni yo'q qilish va funktsiyalarni bir joyda saqlash.

```
# Takrorlanadigan kod
users = ["Ali", "Bob", "Sara"]

for user in users:
print(f"User: {user}")

# Boshqa joyda ham xuddi shunday kod
admins = ["John", "Anna"]

for admin in admins:
print(f"Admin: {admin}")


#DRY printsipi
def print_users(user_list):
    for user in user_list:
        print(f"User: {user}")

users = ["Ali", "Bob", "Sara"]
admins = ["John", "Anna"]

print_users(users)
print_users(admins)

```

> - `KISS`: Kodni oddiy saqlash va keraksiz murakkabliklardan qochish.  Bu printsipning maqsadi murakkabliklardan qochish va tizimni imkon qadar soddalashtirishdir. KISS printsipi ko'pincha "oddiy yechimlar yaxshi yechimlar" degan tushunchani anglatadi.
```
def display_date(year, month, day):
    if month == 1:
        month_name = "January"
    elif month == 2:
        month_name = "February"
    elif month == 3:
        month_name = "March"
    # va hokazo...
    else:
        month_name = "Invalid month"
    
    return f"{month_name} {day}, {year}"

# KISS printsipida 
import calendar

def display_date(year, month, day):
    month_name = calendar.month_name[month]
    return f"{month_name} {day}, {year}"


```
> - `YAGNI`: Faqat hozirda zarur bo'lgan funksiyalarni ishlab chiqish, kelajakda keraksiz narsalarni qo'shmaslik.
```
# Dasturchi dasturiy ta'minotni loyihalash jarayonida kelajakda 
ehtimoliy funktsiyani yaratish uchun kod yozmoqda:

class UserProfile:
    def __init__(self, name, age):
        self.name = name
        self.age = age
        self.profile_picture = None  # Kelajakda kerak bo'lishi mumkin

    def set_profile_picture(self, picture):
        self.profile_picture = picture



#Bu yerda profile_picture xususiyati hozirda zarur emas,
lekin kelajakda ehtimoliy xususiyat sifatida qo'shilgan. 
YAGNI printsipiga amal qilib, biz ushbu xususiyatni qo'shmasligimiz kerak:
class UserProfile:
    def __init__(self, name, age):
        self.name = name
        self.age = age

# Kerak bo'lganda qo'shishimiz mumkin

```

______________________________________________________

## Final class

Agar metod final sifatida e'lon qilinsa, uni subclass (nasl olgan klass) ichida qayta yozish (override) mumkin
bo‘lmaydi. Vazifasi: Final metod orqali dasturchi ma'lum bir metodning subclasslar tomonidan o‘zgartirilishiga yo‘l
qo‘ymasligi mumkin. Misol:

```
public class Vehicle {
    public final void startEngine() {
        System.out.println("Engine started.");
    }
}

public class Car extends Vehicle {
    // Bu kod kompilyatsiya xatosi beradi, chunki startEngine final.
    public void startEngine() {
        System.out.println("Car engine started.");
    }
}

```

## Final o'zgaruvchi

Final o'zgaruvchi qiymati bir marta o'rnatilgandan so'ng qayta o'zgartirilmaydi, ya'ni u konstant (doimiy qiymatli)
bo'lib qoladi.

Vazifasi: Final o'zgaruvchi orqali dasturchi ma'lum bir o'zgaruvchining qiymati bir marta belgilanib, dastur davomida
o'zgarmasligini ta'minlaydi.

Misol:

```
public class Vehicle {
    public final int MAX_SPEED = 120;

    public void setSpeed() {
        MAX_SPEED = 140; // Bu kompilyatsiya xatosi beradi.
    }
}
```

## Xulosa:

> Final klass: Klassni nasl olish uchun yopadi. Final metod: Metodning subclassda qayta yozilishini oldini oladi. Final o'zgaruvchi: O'zgaruvchini konstantga aylantiradi, ya'ni uning qiymati bir marta o'rnatilgandan so'ng o'zgartirib bo'lmaydi. Bu kalit so'zlar dasturchiga kodning izchilligini saqlash, ma'lum elementlarning noto'g'ri ishlatilishini oldini olish va ma'lum funksionallikni muhofaza qilish imkoniyatini beradi.

________________________________________________________

## Abstract class

> bu to'liq amalga oshirilmagan va faqat boshqa klasslar tomonidan nasl olish uchun ishlatiladi. Abstrakt klasslar odatda umumiy kontseptsiyalar yoki asosiy funksionallikni ta'minlash uchun yaratiladi, lekin to'liq bo'lishi uchun uni nasl olgan (subclass) klasslar tomonidan to'ldirilishi kerak.

### Abstrakt klassning xususiyatlari:

> - Nasl olish uchun asos: Abstrakt klass to'g'ridan-to'g'ri obyekt sifatida yaratilishi mumkin emas. U faqat nasl olgan klasslar orqali to'liq bo'lib, obyekt yaratishga imkon beradi.
> - Abstrakt metodlar: Abstrakt klass ichida abstrakt metodlar bo'lishi mumkin, ular faqat deklaratsiya qilinadi, lekin ularning implementatsiyasi subclassda amalga oshirilishi kerak.
> - Qisman implementatsiya: Abstrakt klassda to'liq amalga oshirilgan metodlar ham bo'lishi mumkin. Bu metodlar umumiy funksionallikni nasl olgan barcha klasslar uchun ta'minlaydi.

```
// Abstrakt klass
abstract class Vehicle {
    // Abstrakt metod (implementatsiyasi yo'q)
    public abstract void startEngine();

    // To'liq amalga oshirilgan metod
    public void stopEngine() {
        System.out.println("Engine stopped.");
    }
}

// Nasl oluvchi klass
class Car extends Vehicle {
    // startEngine metodini to'ldirish
    @Override
    public void startEngine() {
        System.out.println("Car engine started.");
    }
}

```

## Asosiy xususiyatlar:

> Obyekt yaratilmasligi: Abstrakt klassdan to'g'ridan-to'g'ri obyekt yaratib bo'lmaydi. Faqat uning nasl olgan klasslari orqali obyekt yaratiladi.

```
Vehicle vehicle = new Vehicle(); // Xato beradi
Vehicle car = new Car(); // To'g'ri
```

## Abstrakt metodlar:

> Agar klassda abstrakt metod bo'lsa, u metod faqat deklaratsiya qilinadi va nasl olgan klassda amalga oshirilishi kerak.

Abstrakt metod:

```
public abstract void startEngine();
```

Bu metodni Car klassida implementatsiya qilish majburiy:

```
@Override
public void startEngine() {
    System.out.println("Car engine started.");
}

```

## Qachon foydalaniladi:

> - Umumiy asos: Agar bir necha klasslar uchun umumiy funksionallik va ma'lum bir strukturani belgilash kerak bo'lsa, abstrakt klasslar ishlatiladi.
> - Shablon berish: Nasl olgan klasslar qanday funksionallikka ega bo'lishi kerakligi aniqlangan bo'lsa, lekin ularni har bir subclass o'ziga xos tarzda amalga oshirishi kerak bo'lsa, abstrakt klasslar ishlatiladi.
> ### Xulosa:
> Abstrakt klasslar OOPda ko'p martalik foydalanish, umumiy kodni meros qilish va klasslararo moslashuvchanlikni oshirish uchun ishlatiladi. Ular muayyan metodlarning qanday bo'lishi kerakligini belgilab beradi, lekin to'liq amalga oshirishni subclasslar ixtiyoriga qoldiradi.

## Abstract bilan interface ni farqi

> - **`Abstract`** `class ko'proq o'xshash classlar uchun asosiy shablon sifatida ishlatiladi.`
> - Abstract classda ba'zi methodlar amalga oshirilishi (implementatsiya qilinishi) mumkin. Shuningdek, abstract methodlar ham bo'lishi mumkin, ular child classda amalga oshirilishi kerak.
> - Abstract classlar o'zlarining propertieslarini (o'zgaruvchilarini) e'lon qilishi mumkin. Bu esa shu classlardan kelib chiqqan classlarga xususiyatlarni meros qilib olish imkonini beradi.
> - **`Interface`** `turli xil classlar uchun umumiy funksiyalarni belgilashda qo'llaniladi, ko'p implementatsiyani qo'llab-quvvatlaydi. `
> - Interfeysda methodlar faqat deklaratsiya qilinadi, ya'ni methodlar faqat nomlanadi va ularning tanasi bo'lmaydi. Ularni implement qiladigan classlar interfeys methodlarini to'liq amalga oshirishi kerak.
> - Xususiyatlar (Properties) Interfeysda faqat methodlar deklaratsiya qilinadi, properties e'lon qilishga ruxsat berilmaydi.

## $This bilan Self ni farqi nimada

> - `$this` kalit so'zi obyektning hozirgi instanceiga murojaat qilib, uning xususiyatlari va methodlari bilan ishlaydi.
> - `self` kalit so'zi esa statik kontekstda ishlatiladi va classning statik method va xususiyatlariga murojaat qiladi.

```
<?php
class Animal {
    public $name; // Obyektga xos xususiyat
    public static $type = "General Animal"; // Statik xususiyat

    public function setName($name) {
        $this->name = $name; // Obyekt darajasida
    }

    public static function getType() {
        return self::$type; // Class darajasida
    }
}

$dog = new Animal();
$dog->setName('Dog'); // $this obyektning xususiyatiga murojaat qiladi
echo $dog->name; // Dog

echo Animal::getType(); // self statik xususiyatni qaytaradi: General Animal
?>

```

## OOP da Access Modefiers haqida

> Object-Oriented Programming (OOP) da access modifiers (kirish modifikatorlari) classning ichidagi o'zgaruvchilar va methodlarga (funksiyalarga) qayerdan kirish mumkinligini boshqaradi. PHPda uchta asosiy kirish modifikatori mavjud:
> - `Public` modifikatori bilan belgilanganda, classdagi method yoki o'zgaruvchi hamma joydan kirish mumkin bo'ladi. Ya'ni, bu methodlar va o'zgaruvchilar classning ichida, tashqarisida va boshqa classlar orqali ham foydalanish mumkin. `Public` kirish modifikatori eng ochiq darajadagi himoya darajasi hisoblanadi.
> - `Protected` modifikatori bilan belgilanganda, o'zgaruvchi yoki method faqat classning o'zida va undan meros olgan (inheritance) classlar ichida foydalanish mumkin bo'ladi. Tashqi koddan (classdan tashqaridan) bevosita foydalanish taqiqlanadi. Bu classlararo bog'liqlikni saqlash va tashqi aralashuvdan himoyalash uchun ishlatiladi.
> - `Private` modifikatori bilan belgilanganda, o'zgaruvchi yoki method faqat o'sha classning o'zida foydalanilishi mumkin. Hatto meros olgan classlar ham ularni ko'ra olmaydi va ulardan foydalana olmaydi. Bu ma'lumotni to'liq himoyalash va kapsulyatsiya qilishni ta'minlaydi, ya'ni tashqi kod classning ichki mantiqiga aralasholmaydi. <br>
> ### Xulosa
> - `Public` — ma'lumotlarni ochiq holda saqlaydi va ularni har joyda foydalanishga ruxsat beradi.
> - `Protected` — faqat class va undan meros olgan classlar ichida foydalanishga ruxsat beradi.
> - `Private` — ma'lumotni to'liq himoyalaydi, faqat classning o'zida foydalaniladi.

## Inkapsulatsiya haqida

> PHPda inkapsulatsiya orqali obyektning ichki ma'lumotlari (o'zgaruvchilar) va ularga kirish usullari (metodlar) tashqi koddan himoyalanadi va faqat kerakli darajada kirish imkoniyati beriladi. <br>
> #### Inkapsulatsiyaning maqsadi:
> - `Ma'lumotlarni yashirish (data hiding)`: Ma'lumotlar tashqi kodga to'g'ridan-to'g'ri ko'rinmasligi va ular bilan ishlash uchun maxsus funksiyalar (metodlar) orqali kirish mumkin bo'lishi kerak.
> - `Kirishni boshqarish`: Classdagi o'zgaruvchilarga yoki metodlarga kirish huquqini belgilash. Masalan, private va protected kabi kirish modifikatorlari orqali ma'lumotlar himoya qilinadi.
> - `Ob'ektni xavfsiz qilish`: Obyektning holatini noto'g'ri yoki kerakmas o'zgarishlardan himoyalash.
> #### Inkapsulatsiyaning asosiy afzalliklari:
> - `Ma'lumotlarni himoya qilish`: Ma'lumotlar class tashqarisidagi noto'g'ri foydalanishdan himoyalanadi. Masalan, foydalanuvchi nomi (username) faqat setUsername() funksiyasi orqali o'zgartiriladi va tashqaridan bevosita o'zgarishi taqiqlanadi.
> - `Yaxlitlik va aniqlik`: Ma'lumotlarga faqat belgilangan metodlar orqali kirish mumkin bo'lgani uchun kod strukturasini tushunish va saqlash osonlashadi.
> - `Modifikatsiya qilishning osonligi`: Agar classdagi biror ma'lumotni saqlash yoki olish mexanizmini o'zgartirish kerak bo'lsa, faqat methodlar orqali o'zgartirish kiritiladi, class tashqarisidagi kodga esa ta'sir qilmaydi.
> - `Ma'lumotlarni validatsiya qilish`: Set funksiyalar orqali kiritilayotgan ma'lumotlarni tekshirish yoki modifikatsiya qilish imkoniyati mavjud.

#### Misol

```
<?php
class BankAccount {
    private $balance = 0; // hisobdagi balans

    // Balansga pul qo'shish (deposit)
    public function deposit($amount) {
        if($amount > 0) {
            $this->balance += $amount;
        }
    }

    // Balansdan pul yechish (withdraw)
    public function withdraw($amount) {
        if($amount > 0 && $amount <= $this->balance) {
            $this->balance -= $amount;
        } else {
            echo "Yetarli mablag' mavjud emas!";
        }
    }

    // Balansni ko'rish (get balance)
    public function getBalance() {
        return $this->balance;
    }
}

$account = new BankAccount();

// Balansni to'g'ridan-to'g'ri o'zgartirib bo'lmaydi:
// $account->balance = 1000; // Xato: Private o'zgaruvchi

// Balans faqat metodlar orqali o'zgartiriladi:
$account->deposit(500);
echo $account->getBalance(); // 500

$account->withdraw(200);
echo $account->getBalance(); // 300

$account->withdraw(500); // "Yetarli mablag' mavjud emas!"
?>

```

## PHPda Dependency Injection turlari:

> `Dependency Injection` — bu classlarning bog'liqliklarini boshqarishning samarali usuli bo'lib, kodni yanada moslashuvchan, testlashga qulay va kengaytirishga oson qiladi, siz sinflarni o'zgartirmasdan ularga yangi imkoniyatlar qo'shishingiz mumkin. Yangi bog'liqliklarni tashqaridan kiritishingiz mumkin.
> - `Constructor Injection` - Eng ko'p qo'llaniladigan usul bu constructor injection bo'lib, sinfning kerakli bog'liqligi class yaratish vaqtida constructorga beriladi.
> - `Setter Injection` - Bog'liqlik setter method (o'rnatuvchi metod) orqali uzatiladi. Bu holda bog'liqlik konstruktor orqali kiritilmaydi, balki qo'shimcha metod orqali kiritiladi.
> - `Interface Injection` - Bu usulda classlar o'zlariga kerak bo'lgan bog'liqlikni olish uchun ma'lum bir interfeysdan foydalanadi. Interfeysga qaratilgan yondashuv orqali, sinflar aniq implementatsiyaga emas, balki interfeysga bog'liq bo'ladi. Bu esa ularni ancha moslashuvchan qiladi.

```
<?php
// Bog'liq class
class Logger {
    public function log($message) {
        echo "Log: " . $message;
    }
}

// Class asosiy logika bilan
class UserController {
    private $logger;

    // Logger classni konstruktor orqali olamiz
    public function __construct(Logger $logger) {
        $this->logger = $logger;
    }

    public function createUser($username) {
        // User yaratish logikasi...
        $this->logger->log("Foydalanuvchi yaratildi: " . $username);
    }
}

// Logger class tashqaridan inject qilinadi
$logger = new Logger();
$controller = new UserController($logger);

$controller->createUser("JohnDoe");
?>

```

_______________________________________________

## PSR

> PSR (PHP Standards Recommendations) — bu PHP dasturchilari va PHP ekotizimidagi barcha ishtirokchilar uchun umumiy standartlarni ishlab chiqish va amalga oshirish jarayoni. PSRlar PHPning ko'plab jihatlarini, jumladan, kod yozish usullarini, kutubxonalar o'rtasidagi interfeyslarni va formatlarni belgilaydi. Ular PHPning sifatini, o'zaro ishlashini va rivojlanishini yaxshilash maqsadida yaratilgan. <br>
PSRlar PHP dasturchilari va PHP ekotizimidagi barcha ishtirokchilar uchun kod yozish, standartlar va metodologiyalarni belgilaydi. Ular kodni yaxshilash, o'zaro ishlashni kuchaytirish va dasturchilar uchun umumiy qoidalarni ta'minlash maqsadida ishlab chiqilgan. PSRlarni amal qilish PHP loyihalarining sifatini oshiradi va ishlab chiqish jarayonini yengillashtiradi. <br>

#### PSRlar haqida ma'lumot

> - Standartlarning maqsadi: PSRlar PHP dasturchilari uchun umumiy qoidalarga muvofiq kod yozishni ta'minlaydi. Bu dasturiy ta'minot va kutubxonalar o'rtasida o'zaro ishlash imkoniyatini oshiradi.
> - O'zaro ishlashni yaxshilash: PSRlar yordamida ishlab chiqilgan kodlar boshqalarga osonroq tushunarli va o'zaro ishlovchi bo'ladi. Bu, ayniqsa, jamoa dasturlashida va ochiq manba loyihalarida muhimdir.
> - Dasturlash standartlarini belgilash: PSRlar dasturlashning turli jihatlarini (masalan, nomlash konventsiya, avtomatik yuklash, loglash va boshqalar) belgilaydi.

> Asosiy `PSR` lar <br>
Quyida PHPda eng ko'p qo'llaniladigan `PSR` lar keltirilgan:<br>
`PSR-1`: Basic Coding Standard Ushbu standart PHP kodining asosiy qoidalarini belgilaydi, masalan, nomlash konventsiyalari va kod formatini.<br>
`PSR-2`: Coding Style Guide Ushbu qo'llanma kod yozish uslubini belgilaydi. Unda format, bo'shliqlar, kirish va chiqarish tartibi va boshqa kodning tashqi ko'rinishi haqida ko'rsatmalar beriladi.<br>
`PSR-3`: Logger Interface Log yozish uchun umumiy interfeysni belgilaydi. Bu logger kutubxonalarini bir-biri bilan bir xil usulda ishlatishga imkon beradi.<br>
`PSR-4`: Autoloading Standard Avtomatik yuklash standartini belgilaydi. Bu PHP fayllarini avtomatik yuklash uchun foydalaniladigan nomlash konventsiyasini o'z ichiga oladi.<br>
`PSR-6`: Caching Interface Keshlash uchun interfeyslarni belgilaydi, bu PHP dasturchilariga keshlash kutubxonalarini bir xil usulda ishlatishga imkon beradi.<br>
`PSR-7`: HTTP Message Interface HTTP so'rov va javoblarini ifodalovchi interfeyslarni belgilaydi. Bu web ilovalarida HTTP protokoli bilan ishlashni standartlashtirishga yordam beradi.<br>
`PSR-12`: Extended Coding Style Guide PSR-2 ning kengaytirilgan versiyasi bo'lib, kod yozish uslubining ko'plab jihatlarini yanada kengaytiradi.<br>

_______________________________________________

## NaN

> PHPda NaN — bu matematik hisob-kitoblar natijasida kutilmagan yoki noaniq qiymatlarni ifodalovchi maxsus konstantadir. NaN bilan ishlashda, uni solishtirish va aniqlashda ehtiyot bo'lish zarur. is_nan() funktsiyasi orqali NaN qiymatini tekshirish imkoniyati mavjud bo'lib, bu sizga hisob-kitoblaringizda xatoliklardan qochish imkonini beradi.

```
$result = sqrt(-1); // NaN

```

_______________________________________________

### PHPda "magic constants" (sehrli konstantalar)

> - echo `__LINE__`; // Bajarilgan qator raqamini chiqaradi
> - echo `__FILE__`; // Hozirgi faylning to'liq yo'li va nomi
> - echo` __DIR__`; // Fayl joylashgan direktoriyaning to'liq yo'li

```
function testFunction() {
    echo __FUNCTION__; // "testFunction"
}
testFunction();

```

### PHPda magic methods (sehrli metodlar)

> - `__construct()`
    Bu sinfning constructor metodidir. Sinfning yangi obyekti yaratilganda avtomatik ravishda chaqiriladi.
> - `__destruct()` Bu destructor metodidir va ob'ekt tugatilganda yoki skriptning ishlash jarayoni yakunlanganda avtomatik ravishda chaqiriladi.
> - __get()
    Mavjud bo'lmagan yoki private/protected sifatida belgilangan property'ga kirishga urinish bo'lganda avtomatik ravishda chaqiriladi.
> - `__set()`
    Mavjud bo'lmagan property'ga qiymat o'rnatishga urinish bo'lganda chaqiriladi.
> - `__isset()`
    ` isset()` yoki `empty()` funksiyalari yordamida mavjud bo'lmagan property ni tekshirganda chaqiriladi.
> - `__unset()`
    `unset()` funksiyasi yordamida property'ni o'chirishga urinish bo'lganda chaqiriladi.
> - `__toString()`
    Ob'ektni string sifatida chop etishga yoki ishlatishga urinish bo'lganda chaqiriladi.

_______________________________________________

## Override nima ?

> `PHP`da `override` (qayta belgilash) — bu klassda ota (parent) klassning metodini bolalar (child) klassda qayta aniqlash jarayonidir.
> Bu usul, bolalar klasslari ota klassning funksional imkoniyatlarini o'zgartirish yoki kengaytirish imkoniyatini beradi. <br>
> #### Override qanday ishlaydi?
> - Ota klass: Ota klassda (parent class) bir metod aniqlanadi.
> - Bolalar klassi: Bolalar klassida (child class) ota klassdagi metod qayta aniqlanadi, bu esa bolalar klassiga o'ziga xos xatti-harakat yoki logikani ta'minlaydi.
> - Polimorfizm: Override yordamida polimorfizmdan foydalanish mumkin, ya'ni bir xil metod nomi bilan turli klasslar uchun turli xil xatti-harakatlarni amalga oshirish.

```
class ParentClass {
    public function display() {
        echo "Bu ota klass metodidir.";
    }
}

class ChildClass extends ParentClass {
    public function display() {
        echo "Bu bolalar klass metodidir.";
    }
}

// Foydalanish
$parent = new ParentClass();
$parent->display(); // "Bu ota klass metodidir."

$child = new ChildClass();
$child->display(); // "Bu bolalar klass metodidir."

```

> #### Xulosa
> PHPda override — bu ota klassning metodini bolalar klassda qayta aniqlash jarayonidir.
> Bu o'zgaruvchan xatti-harakatlarni yaratish va polimorfizmga erishish imkonini beradi.
> Ota klassdagi metodni chaqirish uchun parent kalit so'zidan foydalanish mumkin. Override orqali dasturlashda yaxshiroq tuzilishga erishish va kodning qayta foydalanilishini ta'minlash mumkin.


_______________________________________________

# Constructor

> PHPda constructor (konstruktor) — bu klass ob'ekti yaratilganda avtomatik ravishda chaqiriladigan maxsus metoddir. Konstruktor metodining asosiy vazifasi ob'ektning dastlabki holatini sozlash va ob'ektni yaratishda kerakli boshlang'ich qiymatlarni berishdir.
> #### Konstruktorning asosiy vazifalari
> - Ob'ektning boshlang'ich holatini sozlash: Konstruktor yordamida ob'ekt yaratilganda uning xususiyatlariga dastlabki qiymatlarni berish mumkin.
> - Resurslarni tayyorlash: Ba'zan konstruktor yordamida ob'ekt yaratish jarayonida kerakli resurslar (masalan, ma'lumotlar bazasiga ulanish) tayyorlanadi.
> - Validatsiya va qoidalar: Konstruktor ichida foydalanuvchi tomonidan kiritilgan yoki tashqi manbalardan olingan ma'lumotlarni tekshirish va validatsiya qilish amalga oshirilishi mumkin.

```
class Car {
    // Klass xususiyatlari
    private $brand;
    private $model;
    private $year;

    // Konstruktor
    public function __construct($brand, $model, $year) {
        $this->brand = $brand; // Avtomobil brendi
        $this->model = $model; // Avtomobil modeli
        $this->year = $year;   // Ishlab chiqarilgan yil
    }

    // Avtomobil ma'lumotlarini ko'rsatish
    public function getInfo() {
        return "Avtomobil: $this->brand $this->model, Yil: $this->year";
    }
}

// Ob'ektlarni yaratish
$car1 = new Car("Toyota", "Corolla", 2020);
$car2 = new Car("Honda", "Civic", 2019);

// Avtomobil ma'lumotlarini olish
echo $car1->getInfo(); // "Avtomobil: Toyota Corolla, Yil: 2020"
echo "\n"; // Yangi qator
echo $car2->getInfo(); // "Avtomobil: Honda Civic, Yil: 2019"

```

> Ushbu misol yordamida konstruktor orqali ob'ektning boshlang'ich holatini qanday sozlash mumkinligini ko'rdik. Konstruktor metodlari ob'ekt yaratishda boshlang'ich qiymatlarni berish va ob'ektlar bilan ishlashda muhim rol o'ynaydi.


_______________________________________________

# Array turlari

Oddiy massivlar (Indexed Arrays)

```
$fruits = array("Apple", "Banana", "Cherry");
echo $fruits[1]; // "Banana"
```

Assotsiativ massivlar (Associative Arrays)

```
$person = array(
    "name" => "John",
    "age" => 30,
    "city" => "New York"
);
echo $person["name"]; // "John"

```

Ko'p qirrali massivlar (Multidimensional Arrays)

```
$matrix = array(
    array(1, 2, 3),
    array(4, 5, 6),
    array(7, 8, 9)
);
echo $matrix[1][2]; // 6

```

Dinamika massivlar (Dynamic Arrays)
PHPda massivlar dinamik tarzda o'zgarishi mumkin, ya'ni yangi elementlar qo'shish yoki mavjud elementlarni olib tashlash
mumkin. Bu oddiy yoki assotsiativ massivlar bo'lishi mumkin.

```
$colors = array("Red", "Green");
$colors[] = "Blue"; // Yangi element qo'shish
print_r($colors); // ["Red", "Green", "Blue"]

```

Stack (LIFO) va Queue (FIFO) strukturasi <br>

```
array_push($stack, "item1"); // Qo'shish
$lastItem = array_pop($stack); // Olish

```

Stack (LIFO): Oxirgi kirgan birinchi chiqadi.

```
array_unshift($queue, "item1"); // Qo'shish
$firstItem = array_pop($queue); // Olish

```

## Do while bilan While farqi

> - `Do while` - oldin bajaradi keyin shartni tekshiradi
> - `While` oldin shartni tekshiradi keyin bajaradi

```
#do while

$i = 8;
do {
  echo $i;
  $i++;
} while ($i < 6);

#while
$i = 1;
while ($i < 6) {
  echo $i;
  $i++;
}
```

## Ternary operator

> "Ternary operator" PHP tilida va boshqa ko'plab dasturlash tillarida mavjud bo'lib, if-else bayonotining qisqartirilgan shaklidir. U shartni tekshirish va shartga qarab qiymatni qaytarish uchun ishlatiladi. Ternary operator uch qismdan iborat bo'lib, shu sababli uning nomi "ternary" deb ataladi (ternary — uchlikni anglatadi).<br>
> PHP'dagi ternary operatorining sintaksisi quyidagicha:

```
$variable = (shart) ? true_value : false_value;

#Misol
$age = 18;

// Ternary operator ishlatildi
$status = ($age >= 18) ? "Katta" : "Kichik";

echo $status;  // Natija: "Katta"

```

## Singelton

> Asosiy vazifasi proyektni har qanday joyida bitta classdan bir dona obyekt olish uchun moljallangan


## DTO
> DTOlar dasturlash jarayonini soddalashtiradi va kodni tartibga solishga yordam beradi. Ular ma'lumotlarni uzatish uchun qulay va yengil ob'ektlar bo'lib, ko'pincha web ilovalarda va mikroservis arxitekturalarida ishlatiladi. DTOlar yordamida ma'lumotlar qatlamini va biznes logikasini ajratish mumkin, bu esa kodni osonroq tushunishga va kengaytirishga yordam beradi.
```
// UserDTO - ma'lumotlarni uzatish ob'ekti
class UserDTO {
    public string $name;
    public string $email;
    public int $age;

    public function __construct(string $name, string $email, int $age) {
        $this->name = $name;
        $this->email = $email;
        $this->age = $age;
    }
}

// UserService - biznes logikasi
class UserService {
    public function createUser(UserDTO $userDTO): void {
        // Ma'lumotlar bazasiga saqlash logikasi
        echo "User created: " . $userDTO->name . ", Email: " . $userDTO->email;
    }
}

// DTOni yaratish
$userDTO = new UserDTO("John Doe", "john@example.com", 25);

// UserService orqali foydalanuvchini yaratish
$userService = new UserService();
$userService->createUser($userDTO);

```

_______________________________________________

## Git

> - `git fetch` — bu sizga masofaviy repozitoriydan o'zgarishlarni olish imkonini beradi, lekin lokal branchni o'zgartirmaydi. Bu o'zgarishlarni ko'rib chiqish va qo'shish imkonini beradi.
> - `git pull` — bu masofaviy o'zgarishlarni olib, ularni avtomatik ravishda lokal branchga qo'shadi.
> - `git merge` — bu Gitda bir branchdagi o'zgarishlarni boshqa branchga qo'shish uchun ishlatiladigan buyruq. U asosan rivojlanayotgan kodni birlashtirish uchun foydalaniladi. Masalan, siz yangi xususiyatlar ustida ishlayotgan branchni (feature branch) asosiy branchga (masalan, main yoki develop) birlashtirish uchun git mergedan foydalanasiz.
> - `git stash` - Agar siz barcha o'zgarishlaringizni saqlamoqchi bo'lsangiz va ularni keyinchalik qayta tiklamoqchi bo'lsangiz, `git stash` buyrug'idan foydalanishingiz mumkin:
> Bu buyruq sizning o'zgarishlaringizni "stash" (saqlash) joyiga joylashtiradi va ishchi katalogingizni toza holatga qaytaradi. 
> Keyinchalik o'zgarishlaringizni qaytarish uchun quyidagi buyruqni bajarishingiz mumkin: `git stash pop`


_______________________________________________

## Mysqlda Left Join bilan Inner Join ni farqi

> #### INNER JOIN
> Faqat ikkala jadvalda ham mos keladigan ma'lumotlar bo'lgan qatorlar qaytariladi.

```
SELECT students.name, courses.course_name
FROM students
INNER JOIN courses
ON students.student_id = courses.student_id;
```

> #### LEFT JOIN
> Chap jadvaldagi barcha qatorlar, ular ikkinchi jadvalda mos keladigan qator bo'lsa ham, bo'lmasa ham natijaga kiritiladi.

```
SELECT students.name, courses.course_name
FROM students
LEFT JOIN courses
ON students.student_id = courses.student_id;

```

> #### Xulosa:
> - `INNER JOIN` — bu ikkala jadvalda mos keladigan qatorlarni olish uchun ishlatiladi.
> - `LEFT JOIN` — bu chap jadvaldagi barcha qatorlarni olish uchun ishlatiladi, hatto ularning mos keladigan qatorlari bo'lmasa ham.

## Mysql bilan Sql ni farqi nimada

> - `SQL` — bu til, ya'ni u MySQL kabi ma'lumotlar bazasi boshqarish tizimlariga buyruqlar berish uchun ishlatiladi.
> - `MySQL` — bu SQL so'rovlarini ishlatadigan ma'lumotlar bazasi boshqarish tizimi.
    > <br><br>
    > Qisqacha aytganda, `SQL` — bu ma'lumotlar bilan ishlash uchun til, `MySQL` esa ma'lumotlarni boshqarish uchun dasturiy tizim bo'lib, SQLdan foydalanadi.

## Mysql triggerlar

> `MySQL`da triggers (trekkers yoki tetiklovchilar) — bu ma'lum bir voqea sodir bo'lganda avtomatik ravishda bajariladigan SQL buyruqlari to'plamidir. Triggerslar odatda ma'lumotlar jadvali ustida `INSERT`, `UPDATE`, yoki `DELETE` amallari bajarilganda ishlatiladi. <br>
Triggerslar nima uchun ishlatiladi? <br>
Triggerslar MySQLda quyidagi holatlarda ishlatilishi mumkin: <br>
> - Ma'lumotlarni kiritish, yangilash yoki o'chirish vaqtida avtomatik validatsiya yoki ma'lumotlarni tekshirish.
> - Audit yoki log maqsadlari uchun, ya'ni kim, qachon, qanday o'zgarishlar kiritganini kuzatish.
> - Bir jadvalda ma'lumotlar o'zgarganda avtomatik ravishda boshqa jadvallarga o'zgartirish kiritish.
> - Qoidalarva cheklovlarni avtomatlashtirish, masalan, ma'lumotni o'zgartirishdan oldin shartlarni tekshirish.

> MySQLda triggerslarni yaratish: <br>
Triggerslar odatda quyidagi uchta asosiy voqealarga bog'langan holda ishlatiladi: <br>
> - `INSERT`: Yangi yozuv qo'shilganda.
> - `UPDATE`: Yozuv yangilanganda.
> - `DELETE`: Yozuv o'chirilganda. <br>
    Triggerni yaratishda uni oldin (`BEFORE`) yoki keyin (`AFTER`) bajarishni tanlashingiz mumkin. Bu shuni anglatadiki, ma'lum bir amaldan oldin yoki keyin trigger faollashadi. Trigger yaratish sintaksisi:
    Quyida `CREATE TRIGGER` yordamida trigger yaratishning umumiy sintaksisini ko'rishingiz mumkin:  <br>
    misol: <br>

```
CREATE TRIGGER trigger_name
{BEFORE | AFTER} {INSERT | UPDATE | DELETE}
ON table_name
FOR EACH ROW
BEGIN
    -- Trigger body (SQL buyruqlar)
END;
```

Misol: UPDATE uchun trigger

```
CREATE TRIGGER after_employee_update
AFTER UPDATE ON employees
FOR EACH ROW
BEGIN
    INSERT INTO employee_log (emp_id, action, log_date)
    VALUES (NEW.id, 'UPDATE', NOW());
END;

```

## Use

> `MySQL`da `USE` termini, ma'lum bir ma'lumotlar bazasini tanlash uchun ishlatiladi.
> Bu buyruq bajarilgandan so'ng, barcha keyingi SQL buyruqlari tanlangan ma'lumotlar bazasiga nisbatan bajariladi.
> `USE database_name`;

## Mysql da table ni tahlil qilish uchun
```
#bazani yangilavoradi
ANALYZE merchant.promotion
#so'rovni tekshiradi
explain SELECT * from merchant.promotion
```


## Mysql da cascade actionlar

MySQLda CASCADE ikkita asosiy holatda ishlatiladi:
> - `ON DELETE CASCADE`: Asosiy jadvaldagi yozuv o'chirilganda, chet kalit orqali bog'langan boshqa jadvaldagi yozuvlar ham avtomatik ravishda o'chiriladi.
> - `ON UPDATE CASCADE`: Asosiy jadvaldagi yozuv yangilanganda, bog'liq jadvaldagi chet kalit qiymatlari ham avtomatik ravishda yangilanadi.<br>
    > `CASCADE` amallarni qanday ishlatish kerak?
    > `Foreign Key` yaratishda siz `CASCADE` qoidasini ko'rsatishingiz mumkin. Misol uchun, agar asosiy jadvaldagi yozuv o'chirilsa yoki yangilansa, bog'liq yozuvlar qanday o'zgartirilishini belgilaysiz.

```
CREATE TABLE departments (
    id INT PRIMARY KEY,
    name VARCHAR(100)
);

CREATE TABLE employees (
    id INT PRIMARY KEY,
    name VARCHAR(100),
    department_id INT,
    FOREIGN KEY (department_id) REFERENCES departments(id) ON DELETE CASCADE
);
```[dizayn patternlar.md](dizayn%20patternlar.md)


## Deadlock
> `Deadlock` (o‘lik bog‘lanish) - bu kompyuter tizimlarida, ayniqsa, ma'lumotlar bazalari, 
> masalan, MySQL'da yuzaga keladigan muammo bo‘lib, 
> u bir necha jarayon yoki tranzaksiyalar bir-birini kutganda va hech biri o‘z ishini tugatishiga imkon bermaganda sodir bo‘ladi. 
> Bu vaziyatda jarayonlar bir-birini "tutib" qoladi va natijada har biri o‘z vazifasini bajarish uchun kerakli resurslarga kira olmaydi.
> - Misol uchun:
> - Jarayon A resurs X ni egallab olgan va resurs Y ni kutmoqda.
> - Jarayon B resurs Y ni egallab olgan va resurs X ni kutmoqda.





__________________________________________
> - `MyISAM`
> - Agar siz asosan o'qish operatsiyalari bilan ishlayotgan bo'lsangiz va ma'lumotlarni tezda o'qish kerak bo'lsa.
> - Kichik va oddiy ma'lumotlar bazalari uchun.
> - Tranzaksiyalar yoki foreign keylar talab qilinmaydigan hollarda.
> - `InnoDB`:
> - Agar sizning ilovangizda ma'lumotlarni o'qish va yozish operatsiyalari bir xil darajada muhim bo'lsa.
> - Agar sizga tranzaksiyalarni qo'llab-quvvatlash, ma'lumotlar yaxlitligini ta'minlash yoki foreign keylar bilan ishlash kerak bo'lsa.
> - Katta va murakkab ma'lumotlar bazalari uchun.

__________________________________________

## RabbitMQ Gearman

> `RabbitMQ` va `Gearman` o'zaro farq qiladi. RabbitMQ xabarlarni almashish uchun broker sifatida ishlaydi, 
> `Gearman` esa vazifalarni asinxron ravishda bajarish uchun mo'ljallangan. Agar sizga xabarlar almashishi va saqlanishi kerak bo'lsa,
> `RabbitMQ` yaxshi tanlov. Agar siz ko'p vazifalarni asinxron ravishda bajarish zarurati bo'lsa, 
> `Gearman` sizga yordam beradi. Har ikkala tizim ham zamonaviy dasturlashda juda foydali vositalar hisoblanadi.

__________________________________________

## Apache Solr Elasticsearch

> `Apache Solr` va `Elasticsearch`, o'zaro farqlarga ega bo'lsa-da, ikkalasi ham kuchli qidiruv va indekslash imkoniyatlarini taqdim etadi. 
> Qanday platformani tanlash sizning loyihangizning maxsus talablariga bog'liq. 
> Agar sizga kengaytirilgan qidiruv imkoniyatlari kerak bo'lsa, `Solr` yaxshi tanlov bo'lishi mumkin, 
> lekin agar siz real vaqtli qidirish va tahlil qilishga e'tibor berayotgan bo'lsangiz, `Elasticsearch` juda yaxshi variant.






