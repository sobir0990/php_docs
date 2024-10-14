## Solid Prinsiplari

> `SOLID` tamoyillari — bu obyektga yo‘naltirilgan dasturlashda kodni yaxshi tuzish va loyihalash uchun asosiy
> qo'llaniladigan tamoyillar to'plami. Har bir harfi muhim dizayn printsipini anglatadi. Bu tamoyillar kodni o'qish,
> testlash, kengaytirish va texnik xizmat ko'rsatishni osonlashtirish uchun ishlatiladi.
>
> - `S - Single Responsibility Principle (SRP)` - Yagona javobgarlik tamoyili shuni anglatadiki, har bir sinf faqat bitta mas'uliyatga ega bo'lishi kerak. Agar sinf bitta muammo uchun javobgar bo'lsa, uni osonroq boshqarish, sinovdan o'tkazish va tushunish mumkin. <br>
    Masalan:

```
Noto'g'ri kod:

class Product {
    public function calculatePrice() {
        // Narxni hisoblash
    }

    public function saveToDatabase() {
        // Ma'lumotlar bazasiga saqlash
    }
}

To'g'ri kod:

class Product {
    public function calculatePrice() {
        // Narxni hisoblash
    }
}

class ProductRepository {
    public function saveToDatabase(Product $product) {
        // Ma'lumotlar bazasiga saqlash
    }
}


```

> - `O — Open/Closed Principle (OCP)` - Ochiq/Yopiq printsipi — sinflar o'zgartirish uchun yopiq, lekin kengaytirish uchun ochiq bo'lishi kerak. Ya'ni, mavjud sinfni o'zgartirmasdan, yangi funksiyalarni qo'shish mumkin bo'lishi kerak.

```
Noto'g'ri kod:

class Product {
    public function calculateDiscount($type) {
        if ($type == 'standard') {
            return 10;
        } elseif ($type == 'premium') {
            return 20;
        }
    }
}

__________________________________
To'g'ri kod:

interface Discount {
    public function getDiscount();
}

class StandardDiscount implements Discount {
    public function getDiscount() {
        return 10;
    }
}

class PremiumDiscount implements Discount {
    public function getDiscount() {
        return 20;
    }
}

class Product {
    private $discount;

    public function __construct(Discount $discount) {
        $this->discount = $discount;
    }

    public function calculateDiscount() {
        return $this->discount->getDiscount();
    }
}


$res = new Product(new StandardDiscount);
echo $res->calculateDiscount();  ## 10
 
```

> - `L — Liskov Substitution Principle (LSP)` -LSP: "Agar S sinfi T sinfining vorisi bo'lsa, u holda T sinfining obyekti ishlatiladigan har qanday joyda S sinfining obyekti ham ishlatilishi kerak va dastur xulq-atvori o'zgarmasligi kerak."
> - "Har qanday ob'ekt, agar uning klassi yuqori klassdan meros olgan bo'lsa, bu ob'ekt yuqori klassdagi ob'ektlar o'rnini to'liq bosishi mumkin bo'lishi kerak, hech qanday xatti-harakatlarni o'zgartirmasdan yoki buzmasdan." <br>
> - Boshqacha qilib aytganda, agar B klassi A klassidan meros olsa, unda A klassini ishlatadigan dastur B klassini ishlatganda ham xuddi shunday ishlashi kerak. Quyi klasslar yuqori klassning o'rniga osonlik bilan ishlatilishi kerak, bu esa polimorfizmga mos keladi. Boshqacha qilib aytganda, biz har doim quyi sinfni yuqori sinf o'rnida ishlatishimiz kerak, va tizim xuddi yuqori sinfni ishlatgandek to'g'ri ishlashi kerak."

> - **LSPning Maqsadi** - To'g'ri vorislikni ta'minlash: Voris sinflar ota sinfning xatti-harakatini buzmasligi kerak. Kodda voris sinfni ota sinf sifatida almashtirish orqali kodni dinamik boshqarish mumkin bo'lishi kerak.
> - **Misol Sparrow** - klassi Bird sinfining vorisi va u ota sinfning barcha xatti-harakatlarini to'liq takrorlaydi. Shuning uchun, har qanday joyda Bird klassi kerak bo'lsa, Sparrow klassi ham o'sha joyda muammosiz ishlaydi.

```
// Asosiy sinf
class Bird {
    public function fly() {
        echo "I am flying!";
    }
}

// Voris sinf
class Sparrow extends Bird {
    public function fly() {
        echo "I am flying like a sparrow!";
    }
}

// Foydalanish:
function makeBirdFly(Bird $bird) {
    $bird->fly();
}

$bird = new Sparrow();
makeBirdFly($bird); // Sparrow klassi Bird o'rnida ishlatilmoqda va to'g'ri ishlaydi


```

> `I — Interface Segregation Principle (ISP)` - Interfeysni ajratish printsipi — mijozlar ular foydalanmaydigan metodlarga bog'liq bo'lmasliklari kerak. Ya'ni, katta interfeyslarni kichikroq, aniq maqsadli interfeyslarga bo'lish kerak. <br>
> #### Misol:
> Agar sizda printer uchun interfeys bo'lsa, u juda ko'p turli metodlarga ega bo'lishi mumkin. Lekin har doim ham barcha
> printerlar bu metodlarni qo'llay olmasligi mumkin.

```
Noto'g'ri kod:

interface Printer {
    public function printDocument();
    public function scanDocument();
    public function faxDocument();
}

class BasicPrinter implements Printer {
    public function printDocument() {
        echo "Print!";
    }

    public function scanDocument() {
        throw new Exception("Scan not supported!");
    }

    public function faxDocument() {
        throw new Exception("Fax not supported!");
    }
}

__________________________________
To'g'ri kod:

// Interfeyslar
interface Printer {
    public function printDocument();
}

interface Scanner {
    public function scanDocument();
}

interface Fax {
    public function faxDocument();
}

// Faqat printer funksiyasini amalga oshiruvchi sinf
class BasicPrinter implements Printer {
    public function printDocument() {
        echo "Print!";
    }
}

// Faqat skaner funksiyasini amalga oshiruvchi sinf
class BasicScanner implements Scanner {
    public function scanDocument() {
        echo "Scan!";
    }
}

// Faks funksiyasini amalga oshiruvchi sinf
class BasicFax implements Fax {
    public function faxDocument() {
        echo "Fax!";
    }
}

// Barcha funksiyalarni qila oladigan ko'p funksiyali sinf
class MultiFunctionPrinter implements Printer, Scanner, Fax {
    public function printDocument() {
        echo "Print from Multi-function printer!";
    }

    public function scanDocument() {
        echo "Scan from Multi-function printer!";
    }

    public function faxDocument() {
        echo "Fax from Multi-function printer!";
    }
}

// Foydalanish

$printer = new BasicPrinter();
$printer->printDocument(); // Output: Print!

$scanner = new BasicScanner();
$scanner->scanDocument(); // Output: Scan!

$fax = new BasicFax();
$fax->faxDocument(); // Output: Fax!

// Ko'p funksiyali printer
$multiFunctionPrinter = new MultiFunctionPrinter();
$multiFunctionPrinter->printDocument(); // Output: Print from Multi-function printer!
$multiFunctionPrinter->scanDocument();  // Output: Scan from Multi-function printer!
$multiFunctionPrinter->faxDocument();   // Output: Fax from Multi-function printer!


```

> - `D — Dependency Inversion Principle (DIP)` - Bog'liqlikni teskari aylantirish printsipi — yuqori darajali modullar past darajali modullarga bog'liq bo'lmasligi kerak. Ikkalasi ham abstraktsiyalarga bog'liq bo'lishi kerak. Bu, asosan, sinflarni bevosita bir-biriga bog'liq qilishdan qochish uchun ishlatiladi.
> - **Misol**: “Misol uchun, agar bizda xabarlar yuboruvchi bir xizmat bo'lsa (NotificationService), bu xizmat faqat email yoki SMS uchun qattiq bog'liq bo'lishi shart emas. Aksincha, biz umumiy MessageSender interfeysidan foydalanamiz va har qanday yangi xabar turini qo'shish osonlashadi.”
```
// Abstraktsiya (Interfeys)
interface NotificationSender {
    public function send($message);
}

// Past darajadagi modullar
class EmailSender implements NotificationSender {
    public function send($message) {
        echo "Email sent: " . $message;
    }
}

class SmsSender implements NotificationSender {
    public function send($message) {
        echo "SMS sent: " . $message;
    }
}

// Yangi bildirishnoma turi: Push Notification
class PushNotificationSender implements NotificationSender {
    public function send($message) {
        echo "Push Notification sent: " . $message;
    }
}

// Yuqori darajadagi modul
class Notification {
    private $notificationSender;

    public function __construct(NotificationSender $notificationSender) {
        $this->notificationSender = $notificationSender;
    }

    public function send($message) {
        $this->notificationSender->send($message);
    }
}

// Foydalanish:
$emailNotification = new Notification(new EmailSender());
$emailNotification->send("Hello via Email!");

$smsNotification = new Notification(new SmsSender());
$smsNotification->send("Hello via SMS!");

$pushNotification = new Notification(new PushNotificationSender());
$pushNotification->send("Hello via Push Notification!");

```