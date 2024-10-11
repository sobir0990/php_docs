## Solid Prinsiplari

> `SOLID` tamoyillari — bu obyektga yo‘naltirilgan dasturlashda kodni yaxshi tuzish va loyihalash uchun asosiy
> qo'llaniladigan tamoyillar to'plami. Har bir harfi muhim dizayn printsipini anglatadi. Bu tamoyillar kodni o'qish,
> testlash, kengaytirish va texnik xizmat ko'rsatishni osonlashtirish uchun ishlatiladi.
>
> - `S - Single Responsibility Principle (SRP)` - Yagona javobgarlik tamoyili shuni anglatadiki, har bir sinf faqat
    bitta
    mas'uliyatga ega bo'lishi kerak. Agar sinf bitta muammo uchun javobgar bo'lsa, uni osonroq boshqarish, sinovdan
    o'tkazish va tushunish mumkin. <br>
    > Masalan:

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

> - `O — Open/Closed Principle (OCP)` - Ochiq/Yopiq printsipi — sinflar o'zgartirish uchun yopiq, lekin kengaytirish
    uchun ochiq bo'lishi kerak. Ya'ni, mavjud sinfni o'zgartirmasdan, yangi funksiyalarni qo'shish mumkin bo'lishi
    kerak.

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

```