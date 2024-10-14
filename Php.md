> - `include`: Foydalanuvchiga kodni qo'shadi. Agar fayl topilmasa, ogohlantiradi va dasturni davom ettiradi.
> - `require`: Faylni qo'shadi. Fayl topilmasa, xatolik (fatal error) chiqaradi va dasturni to‘xtatadi.
> - `include_once`: Fayl faqat bir marta qo'shiladi, boshqa urinishda ishlatilmaydi.
> - `require_once`: Faylni faqat bir marta qo'shadi, fayl topilmasa, xatolik chiqaradi.

## 3. What is PDO in PHP and how does it work?

> `PDO` (PHP Data Objects) — bu PHPda ma'lumotlar bazasi bilan bog'lanish uchun ishlatiladigan abstraktsiya qatlami. U
> sizga turli xil ma'lumotlar bazalari bilan bir xil kod orqali ishlashga imkon beradi (masalan, MySQL, PostgreSQL,
> SQLite). PDO o'zining tayinlangan xususiyatlari bilan xavfsiz SQL so'rovlarini amalga oshirishda yordam beradi va SQL
> injection xavfini kamaytiradi.
>

```
try {
    $pdo = new PDO('mysql:host=localhost;dbname=testdb', 'username', 'password');
    $pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);

    $stmt = $pdo->prepare('SELECT * FROM users WHERE id = :id');
    $stmt->execute(['id' => 1]);
    $user = $stmt->fetch();
} catch (PDOException $e) {
    echo 'Connection failed: ' . $e->getMessage();
}

```

## SQLda JOIN turlari qanday va ular qachon ishlatiladi?

> - `INNER JOIN`: Ikkala jadvaldagi mos keluvchi ma'lumotlarni qaytaradi.
> - `LEFT JOIN`: Chap jadvaldagi barcha ma'lumotlarni va o‘ng jadvaldan mos keluvchi ma'lumotlarni qaytaradi.
> - `RIGHT JOIN`: O‘ng jadvaldagi barcha ma'lumotlarni va chap jadvaldan mos keluvchi ma'lumotlarni qaytaradi.
> - `FULL OUTER JOIN`: Har ikkala jadvaldagi barcha ma'lumotlarni qaytaradi, mos kelmasligi mumkin.

## PHPda ma'lumotlar xavfsizligini ta'minlash uchun qanday amallarni bajarish kerak?

> - SQL injectiondan himoya qilish uchun tayinlangan (prepared) so'rovlar ishlatish.
> - XSS (Cross-Site Scripting) xavfiga qarshi ma'lumotlarni filtrlash va chiqish ma'lumotlarini qochirish (escape).
> - CSRF (Cross-Site Request Forgery) xavfiga qarshi tokenlar (CSRF token) qo'shish.
> - Ma'lumotlarni shifrlash va saqlash (masalan, parollar uchun password_hash() funksiyasini ishlatish).
> - SSL sertifikatlari yordamida xavfsiz ulanish (HTTPS) ta'minlash.

## Nima uchun SOLID tamoyillari muhim va PHPda qanday qo‘llaniladi?

> - Single Responsibility Principle (SRP) — Har bir klass faqat bitta vazifani bajarishi kerak.
> - Open/Closed Principle (OCP) — Klasslar yangi funksiyalar qo‘shishda ochiq, lekin o‘zgartirishga yopiq bo‘lishi
    kerak.
> - Liskov Substitution Principle (LSP) — Kichik sinflar asosiy sinfga mos kelishi va uni almashtira olishi kerak.
> - Interface Segregation Principle (ISP) — Katta interfeyslarni kichik interfeyslarga bo‘lib ishlatish kerak.
> - Dependency Inversion Principle (DIP) — Modullar yuqori darajadagi abstractions (interfeys) bilan bog'lanishi kerak,
    past darajadagi implemntatsiyalar bilan emas.

## PHPda error handling qanday amalga oshiriladi?

> - try...catch bloki yordamida xatolarni ushlab olish mumkin.
> - set_error_handler() yordamida xatolarni o'ziga xos usulda boshqarish.
> - error_log() orqali xatolarni log-faylga yozish.

```
try {
    $result = 10 / 0;
} catch (DivisionByZeroError $e) {
    echo 'Error: ' . $e->getMessage();
}

```

## Composer nima va u qanday ishlaydi?

> Composer — bu PHP loyihalari uchun paket menejeri. U orqali loyihaga kerakli kutubxonalarni osonlikcha qo'shish va
> boshqarish mumkin.

## MySQLda indekslar nima? Ularning turlari qanday va qachon qo‘llanadi?

> - PRIMARY KEY: Biror jadvadagi har bir qatorni noyob identifikatsiya qilish uchun ishlatiladi.
> - UNIQUE: Takrorlanmagan qiymatlar bo'lishini ta'minlaydi.
> - INDEX: So‘rovlarni tezlashtirish uchun ishlatiladi, lekin qiymatlar takrorlanishi mumkin.
> - FULLTEXT: Matn qidirish uchun mo‘ljallangan indeks.

## SQL Injection nima va undan qanday himoyalanish mumkin?

> - Javob: SQL Injection — foydalanuvchi kiritadigan ma'lumot orqali SQL so'rovini buzish hujumi. Himoyalanish usullari:
> - Prepared Statements: PDO yoki MySQLi dan foydalanish.
> - Ma'lumotlarni eskaplash: Foydalanuvchi kiritgan ma'lumotlarni xavfsiz holga keltirish uchun
    mysqli_real_escape_string funksiyasidan foydalanish.

## GROUP BY va HAVING farqi nima?

> - GROUP BY: Qatorlarni ma'lum ustunlar bo'yicha guruhlash uchun ishlatiladi.
> - HAVING: GROUP BY dan keyin filtr qo‘llash uchun ishlatiladi. WHERE qatori qatorlarni filtrlashdan oldin qo‘llanadi,
    HAVING esa guruhlangan natijalarga nisbatan qo‘llanadi.

## MySQLda EXPLAIN buyrug‘i nima va u nima uchun ishlatiladi?

> EXPLAIN buyrug‘i so‘rovni bajarish rejasi va indekslardan qanday foydalanishini ko'rsatadi. U yordamida so‘rovni
> optimizatsiya qilish va qaysi indekslar samaraliroq ekanligini aniqlash mumkin.

## Normalizatsiya nima va uning bosqichlari qanday?

> Normalizatsiya ma'lumotlar bazasi dizaynini takomillashtirish, ortiqcha ma'lumotlarni kamaytirish va ma'lumotlarning
> yaxlitligini ta'minlash uchun ishlatiladi. Uning bosqichlari: <br>
> Birlamchi Normal Forma (1NF): Har bir qator bitta qiymatni ifodalaydi. <br>
> Ikkinchi Normal Forma (2NF): Har bir qator birlamchi kalitga to‘liq bog‘liq bo‘ladi. <br>
> Uchinchi Normal Forma (3NF): Qatorlar orasidagi transitsiyaviy bog‘liqliklar bartaraf etiladi.

## View nima va u qachon qo‘llanadi?

> View — bu jadvaldan yoki boshqa so‘rovlardan olingan ma'lumotlar asosida yaratilgan virtual jadval. U ma'lumotlarni
> soddalashtirish va xavfsizlikni oshirish uchun ishlatiladi, chunki foydalanuvchilar to‘g‘ridan-to‘g‘ri jadvallarga
> kirishmaydi, balki view orqali kirishadi.

## Deadlock nima va qanday bartaraf qilinadi?

> Deadlock — bu ikki yoki undan ko‘p transaksiyalar bir-birining resurslarini kutayotgan holat. Buni bartaraf qilish
> uchun transaksiyalarni kichikroq bo‘limlarga bo‘lish, blokirovka ketma-ketligini to‘g‘ri boshqarish, yoki LOCK_TIMEOUT
> ni o‘rnatish mumkin.

## Index optimizatsiyasi qanday amalga oshiriladi?

> Indekslarni to‘g‘ri qo‘llash so‘rovlarni tezlashtiradi. Asosan so‘rovda ishlatiladigan ustunlarga indeks qo‘yish
> kerak, lekin indekslarni ortiqcha qo‘llashdan qochish kerak, chunki ular jadvalga yozish vaqtida qo‘shimcha xarajat
> talab qiladi.


##  Sharding va Replication nima va qachon qo'llaniladi?
> - Sharding: Ma'lumotlar bazasini katta hajmdagi ma'lumotlarni boshqarish uchun bir nechta serverlarga bo‘lish.
> - Replication: Ma'lumotlar bazasini bir nechta nusxaga ko‘chirish va ulardan foydalanuvchilar o‘rtasida yukni taqsimlash. Odatda o‘qish operatsiyalarini tezlashtirish uchun ishlatiladi.
