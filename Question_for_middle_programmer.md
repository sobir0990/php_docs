# Middle

## Foydalanuvchi brauzerga URL manzilini kiritganda qanday jarayonlar sodir bo'ladi?

> Foydalanuvchi brauzerga URL kiritganda, quyidagi asosiy jarayonlar sodir bo‘ladi:
> - Keshni tekshirish: Brauzer avval keshda shu sahifa yoki uning resurslari bor-yo‘qligini tekshiradi. Agar mavjud
    bo‘lsa, ular keshdan yuklanadi.
> - DNS orqali manzilni aniqlash: Agar keshda ma’lumot topilmasa, brauzer URLdagi domen nomini IP-manzilga o‘zgartirish
    uchun DNS-serverga so‘rov yuboradi.
> - TCP ulanishini o‘rnatish: IP-manzil olingach, brauzer server bilan TCP ulanishi (3-qadamli kelishuv) orqali aloqani
    boshlaydi.
> - SSL/TLS xavfsiz ulanishi (agar HTTPS ishlatilsa): HTTPS bo‘lsa, brauzer va server shifrlash uchun xavfsiz aloqani
    o‘rnatishadi va serverning SSL sertifikatini tekshirishadi.
> - HTTP so‘rov yuborish: Brauzer serverga kerakli resurslarni olish uchun HTTP so‘rov yuboradi.
> - Serverda so‘rovni qayta ishlash: Server so‘rovni qabul qilib, ma’lumotlar bazasidan yoki fayllardan sahifani yoki
    resurslarni shakllantirib, javob qaytaradi.
> - HTTP javobini olish: Brauzer serverdan javobni oladi va uni tahlil qiladi. Bu javob HTML, CSS, JavaScript va boshqa
    resurslarni o‘z ichiga oladi.
> - Sahifani ko‘rsatish: Brauzer HTML va CSS fayllarni qayta ishlaydi, sahifaning tuzilmasini yaratadi va ekranga
    chiqaradi.
> - JavaScript ishlashi: Agar sahifada JavaScript bo‘lsa, u sahifa yuklanayotganda yoki yuklangandan keyin ishlaydi,
    sahifaning interaktiv qismlarini boshqaradi.

## Variativ va spread operatorlar

> - `Variativ` funksiyalar: Ular noma'lum miqdordagi argumentlarni qabul qilish imkonini beradi va ... operatori
    yordamida ishlatiladi.

```
<?php
// O'rtacha qiymatni hisoblash uchun variativ funksiya
function ortacha(...$sonlar) {
    $jami = array_sum($sonlar);
    $soni = count($sonlar);
    
    if ($soni > 0) {
        return $jami / $soni;
    }
    
    return 0; // Agar argumentlar bo'lmasa, natija 0 bo'ladi
}

echo ortacha(4, 5, 6); // 5
echo ortacha(10, 20, 30, 40); // 25



___________________________________

$menu = [
    'pizza' => 12000,
    'sushi' => 20000,
    'burger' => 8000,
    'salad' => 5000
];


function hisoblaNarx(...$taomlar) {
    global $menu; // Global o'zgaruvchini olish

    $jami = 0; // Jami narxni saqlash

    foreach ($taomlar as $taom) {
        if (array_key_exists($taom, $menu)) { // Agar taom menyuda mavjud bo'lsa
            $jami += $menu[$taom]; // Narxni qo'shish
        }
    }

    return $jami;
}

?>

```

> - `Spread` operatori: Bu operator massiv elementlarini alohida qiymatlar sifatida tarqatish imkonini beradi va u
    funksiya chaqiruvlarida yoki massivlarni birlashtirishda qo‘llaniladi.

```
<?php
// Funksiya uchta qiymatni chiqaradi
function printThreeNumbers($a, $b, $c) {
    echo "Raqamlar: $a, $b, $c\n";
}

$numbers = [1, 2, 3];

// Spread operatoridan foydalanib, massivdagi qiymatlarni argument sifatida uzatamiz
printThreeNumbers(...$numbers); // Natija: Raqamlar: 1, 2, 3
?>

_______________

// Mijozning buyurtmasi
$buyurtma1 = ['pizza', 'sushi'];
$buyurtma2 = ['burger', 'salad', 'pizza'];

// Spread operatori yordamida buyurtmalarni hisoblaymiz
$sum1 = hisoblaNarx(...$buyurtma1);
$sum2 = hisoblaNarx(...$buyurtma2);


```

## OWASP

> OWASP Top 10 ro'yxati veb-ilovalar uchun eng kritik 10 ta xavflarni o'z ichiga oladi. Siz bu xavflarni bilish va
> ularni qanday bartaraf etishni tushunishingiz kerak. Bu ro'yxatdagi xavflarga quyidagilar kiradi:
> - A1: Injection — SQL injektsiyasi, XSS (Cross-Site Scripting) kabi zaifliklar.
> - A2: Broken Authentication — zaif autentifikatsiya mexanizmlari.
> - A3: Sensitive Data Exposure — maxfiy ma'lumotlarni xavfsiz saqlamaslik.
> - A4: XML External Entities (XXE) — XML ni noto'g'ri parselash.
> - A5: Broken Access Control — noto'g'ri kirish nazorati. (role bilan tekshirish)

```
SQL injektsiyasi

$username = $_POST['username'];
$password = $_POST['password'];
$query = "SELECT * FROM users WHERE username = '$username' AND password = '$password'";

# Agar foydalanuvchi admin' -- deb kiritsa, SQL so'rovi kutilmagan natijalar beradi.


# Yechim: Parametrli so'rovlar (Prepared Statements) dan foydalaning:

$stmt = $pdo->prepare("SELECT * FROM users WHERE username = :username AND password = :password");
$stmt->execute(['username' => $username, 'password' => $password]);

______________________________________________

A2: Broken Authentication
Agar parollar saqlanayotgan bo'lsa, ularni to'g'ridan-to'g'ri ma'lumotlar bazasida saqlash xavfli.
Yechim: Parollarni saqlashdan oldin ularni hash() funksiyasi bilan xesh qiling:

$hashedPassword = password_hash($password, PASSWORD_DEFAULT);
$query = "INSERT INTO users (username, password) VALUES ('$username', '$hashedPassword')";

A3: Sensitive Data Exposure
______________________________________________

Agar foydalanuvchi ma'lumotlari, masalan, kredit karta raqamlari, HTTP protokoli orqali yuborilsa, ularni o'g'irlash oson. Masalan:
http://example.com/checkout?card_number=1234567890123456
Yechim: Har doim HTTPS ni qo'llang va ma'lumotlarni shifrlang.



______________________________________________

XML External Entities (XXE) zaifligi XML hujjatlarini noto'g'ri ishlash orqali tizimga tahdid solishi mumkin. 
Ushbu zaiflikni bartaraf etish uchun XML parserlarida xavfsiz sozlamalarni qo'llash va foydalanuvchi ma'lumotlarini yaxshi nazorat qilish muhimdir.
Yechim: Tashqi entitylarni o'chirib qo'ying: XML parserda tashqi entitylarni o'chirib qo'yish orqali hujumlar oldini olish mumkin.

libxml_disable_entity_loader(true);
$doc = new DOMDocument();
$doc->loadXML($xml);

```

__________________________________________________________

[README PHP.md](README%20PHP.md)
## Zaiflikning qanday turlarini bilasiz? O'zingizni ulardan qanday himoya qilish kerak?

> - `SQL Injection (SQL Injektsiyasi)` - bu hujumchi veb-ilovaga SQL so'rovlarida kiritilgan ma'lumotlar orqali
    ma'lumotlar bazasiga zarar etkazish imkoniyatini beradi.
>  #### Himoya
>  Parametrli so'rovlar (Prepared Statements) dan foydalaning <br> 
>  ORM (Object-Relational Mapping) kutubxonalaridan foydalaning. <br><br>
> - `Cross-Site Scripting (XSS)` - bu hujumchi foydalanuvchi bra[README PHP.md](README%20PHP.md)uzeriga zararli skriptlarni kiritib, foydalanuvchining
    shaxsiy ma'lumotlarini o'g'irlashi yoki muayyan harakatlarni bajarishga undashi mumkin
>  #### Himoya
> - `Cross-Site Request Forgery (CSRF)` -  bu hujumchi foydalanuvchining hisob qaydnomasi orqali noxush harakatlarni amalga oshirishga olib keladigan hujum.
>  #### Himoya
>  CSRF tokenlaridan foydalanish. <br>
>  POST so'rovlariga maxsus tokenlarni kiritish. <br>
> - `Insufficient Logging and Monitoring` - Tizimda loglarni to'g'ri yozmaslik yoki monitoring qilmaslik natijasida
    xavfsizlik xatolarini erta aniqlash imkoniyatining yo'qligi.
> - `Security Misconfiguration` - Tizimning noto'g'ri sozlanishi va zaifliklarga olib keladigan konfiguratsiya xatolari.
> - `Sensitive Data Exposure` (Nozik Ma'lumotlarni O'g'irlash) - Nozik ma'lumotlar (kredit karta raqamlari, parollar va boshqa shaxsiy ma'lumotlar) shifrlanmagan holda saqlanishi yoki uzatilishi.
> - `Insecure Deserialization` Tizimda ishlatiladigan ma'lumotlar formatini noto'g'ri deserializatsiya qilish orqali hujumlarni amalga oshirish.
> #### Himoya
>  Deserialization jarayonida malumotlarni tekshirish <br>
> Deserializatsiya Nima?
> Serializatsiya — bu obyektlarni yoki ma'lumotlarni bir ma'lumotlar formatiga o'zgartirish jarayoni (masalan, JSON yoki XML). 
> Deserializatsiya esa bu jarayonning teskari holati bo'lib, ya'ni saqlangan ma'lumotlarni yana dasturdagi obyektga aylantirish jarayoni.

__________________________________________________________

## Usul idempotentligi nima? Qaysi HTTP usullari REST uchun idempotent hisoblanadi?
## Idempotent HTTP usullari
> - `GET`
> - `PUT`
> - `DELETE`
> - `HEAD` GET so'rovi kabi, lekin faqat resurs haqidagi metama'lumotlarni (headerlarni) qaytaradi. HEAD so'rovi ham idempotent bo'lib, bir necha marta chaqirilsa ham serverdagi resursni o'zgartirmaydi.
> - `OPTIONS` Bu so'rov serverdagi resurs qaysi usullarni qo'llab-quvvatlashini bilish uchun ishlatiladi. OPTIONS so'rovi ham idempotent bo'lib, u serverdagi resursni o'zgartirmaydi.
> - `POST` bunga kirmaydi
> POST odatda yangi resurs yaratish uchun ishlatiladi. Har bir POST so'rovi yangi resurs yaratib, 
> - serverning holatini o'zgartirishi mumkin. Shu sababli, POST so'rovi idempotent emas,
> - chunki bir xil POST so'rovini bir necha marta yuborish har safar yangi resurs yaratishi yoki
> - server holatini o'zgartirishi mumkin.
> #### Xulosa
> Idempotent usullar bir necha marta chaqirilganda bir xil natija beradi, serverdagi resursning holati o'zgarmaydi yoki o'zgarish bir xil bo'ladi. REST arxitekturasida GET, PUT, DELETE, HEAD va OPTIONS idempotent usullar hisoblanadi. POST esa idempotent emas, chunki u server holatini har safar o'zgartirishi mumkin.

## Stateless nima
> Stateless esa aksincha — bunda hech qanday holat (state) saqlanmaydi. Har bir so'rov mustaqil bo'lib, avvalgi so'rovlardan hech narsa eslanmaydi. Shuni quyidagicha tushuntirish mumkin:
> har safar bearer token yuboradi
```
POST /login
Authorization: Bearer abc123456
```
> `Stateful` (sessiya asosidagi tizim) — bu holat saqlanadigan tizim, ya'ni server foydalanuvchining oldingi so'rovlarini eslaydi va sessiya ma'lumotlarini saqlaydi.

## Rafleksiya (reflection)
> Rafleksiya (reflection) — bu PHP dasturlash tilida ob'ektlar va klasslar strukturasini va xulq-atvorini dastur ishga tushirilganda o'rganish va o'zgartirish imkonini beruvchi mexanizmdir. Rafleksiya yordamida klasslar, metodlar, xususiyatlar, interfeyslar va boshqa dastur elementlari haqida ma'lumot olish mumkin. Shuningdek, dinamik ravishda metodlarni chaqirish va xususiyatlarni o'zgartirish imkoniyatini beradi.
> #### Rafleksiya uchun asosiy klasslar va metodlar:
> - `ReflectionClass` - klasslarni o'rganish uchun:
> - `getName()` — klass nomini qaytaradi.
> - `getMethods()` — metodlar ro'yxatini qaytaradi.
> - `getProperties()` — xususiyatlar ro'yxatini qaytaradi.
> - `newInstance()` — klassdan ob'ekt yaratadi.

```
class Person {
    private $name;
    public $age;

    public function __construct($name, $age) {
        $this->name = $name;
        $this->age = $age;
    }
}

// ReflectionClass yordamida Person klassini tahlil qilamiz
$reflector = new ReflectionClass('Person');

// Xususiyatlar ro'yxatini olamiz
$properties = $reflector->getProperties();
foreach ($properties as $property) {
    echo "Xususiyat nomi: " . $property->getName() . PHP_EOL;
    echo "Publicmi? " . ($property->isPublic() ? "Ha" : "Yo'q") . PHP_EOL;
    echo "Privatemi? " . ($property->isPrivate() ? "Ha" : "Yo'q") . PHP_EOL;
    echo "Protectedmi? " . ($property->isProtected() ? "Ha" : "Yo'q") . PHP_EOL;
    echo "-----------------------" . PHP_EOL;
}

```


## Redis va Memcache
> - `Redis`: Kesh va xotira ichida ma'lumotlar bazasi sifatida ishlatilishi mumkin bo'lgan kuchli vosita bo'lib, murakkab ma'lumot tuzilmalarini saqlaydi va diskka saqlashni ham qo'llab-quvvatlaydi.
> - `Memcached`: Faqat kesh sifatida ishlatiladi va sodda kalit-qiymat juftliklarini saqlaydi. Bu tezkor va samarali, lekin Redis kabi murakkab ma'lumot tuzilmalarini yoki diskka saqlash imkonini bermaydi. <br>
> `Redis` murakkab ma'lumot tuzilmalarini (massivlar, to'plamlar, hashlar) qo'llab-quvvatlaydi, `Memcached` esa faqat satrlar bilan ishlaydi.


## У нас есть важный PHP-файл, его надо запускать каждые 20 секунд, как бы вы это сделали?
```
Shell skripti yarating, masalan, run_php.sh nomi bilan:

#!/bin/bash
while true; do
    php /path/to/your/file.php
    sleep 20
done

Shell skriptini ishga tushirish uchun Crontab sozlang: Crontab faylini oching:
crontab -e


Quyidagi qatorni qo‘shing, bu har safar tizim qayta ishga tushganda (reboot qilganda) shell skriptini ishga tushiradi:
@reboot /bin/bash /path/to/run_php.sh


php da yozsa
while (true) {
    // PHP faylingizdagi asosiy kod
    echo "Skript ishlayapti\n";
    
    // 20 soniya kutish
    sleep(20);
}
```