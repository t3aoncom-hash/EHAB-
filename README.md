<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover">

<title>الشيخ إيهاب</title>

<script src="https://cdn.tailwindcss.com"></script>
<script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2"></script>
<script src="https://unpkg.com/lucide@latest"></script>

<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Tajawal:wght@400;500;600;700;800;900&display=swap" rel="stylesheet">

<script>
tailwind.config = {
    theme: {
        extend: {
            fontFamily: {
                sans: ["Tajawal", "sans-serif"]
            },
            colors: {
                brand: {
                    500: "#22c55e",
                    600: "#16a34a",
                    700: "#15803d"
                }
            }
        }
    }
}
</script>

<style>
* {
    box-sizing: border-box;
}

html {
    scroll-behavior: smooth;
}

body {
    font-family: "Tajawal", sans-serif;
    margin: 0;
    padding-bottom: 76px;
    overflow-x: hidden;
}

body.cart-open {
    overflow: hidden;
}

::-webkit-scrollbar {
    width: 7px;
}

::-webkit-scrollbar-thumb {
    background: #22c55e;
    border-radius: 20px;
}

.product-card,
.category-card {
    transition: .25s ease;
}

.product-card:hover,
.category-card:hover {
    transform: translateY(-4px);
}

.hero-pattern {
    background-image:
        radial-gradient(
            circle at 10% 20%,
            rgba(255,255,255,.12) 0 70px,
            transparent 71px
        ),
        radial-gradient(
            circle at 90% 80%,
            rgba(255,255,255,.08) 0 100px,
            transparent 101px
        );
}

.drawer {
    animation: drawer .25s ease;
}

@keyframes drawer {
    from {
        transform: translateX(100%);
    }

    to {
        transform: translateX(0);
    }
}

.modal {
    animation: fade .2s ease;
}

@keyframes fade {
    from {
        opacity: 0;
    }

    to {
        opacity: 1;
    }
}

.line-clamp-2 {
    display: -webkit-box;
    -webkit-line-clamp: 2;
    -webkit-box-orient: vertical;
    overflow: hidden;
}

.bottom-nav {
    position: fixed;
    left: 0;
    right: 0;
    bottom: 0;
    z-index: 9999;
    height: 70px;
    padding-bottom: env(safe-area-inset-bottom);
    background: rgba(255,255,255,.97);
    backdrop-filter: blur(12px);
    -webkit-backdrop-filter: blur(12px);
    border-top: 1px solid #e5e7eb;
    box-shadow: 0 -5px 20px rgba(0,0,0,.08);
}

.bottom-nav-item {
    width: 62px;
    height: 55px;
    transition: .2s ease;
}

.bottom-nav-item:active {
    transform: scale(.92);
}

.cart-hidden-nav {
    display: none !important;
}

@media (min-width: 768px) {

    .bottom-nav-item {
        width: 75px;
    }

}

@media (max-width: 767px) {

    #products {
        grid-template-columns: repeat(2, minmax(0, 1fr));
        gap: 10px;
    }

    .product-card {
        border-radius: 20px;
    }

    .product-card img {
        height: 145px;
    }

    .product-card .p-5 {
        padding: 12px;
    }

    .product-card h3 {
        font-size: 13px;
        min-height: 40px;
    }

    .product-card strong {
        font-size: 16px;
    }

    .product-card button {
        padding: 9px 10px;
        font-size: 11px;
    }

}

@media (max-width: 380px) {

    #products {
        gap: 8px;
    }

    .product-card img {
        height: 125px;
    }

}
</style>
</head>

<body class="bg-gray-50 text-gray-800">

<!-- HEADER -->
<header class="sticky top-0 z-40 bg-white/95 backdrop-blur-md border-b border-gray-100">

    <div class="max-w-7xl mx-auto px-4 h-[72px] flex items-center justify-between gap-4">

        <!-- LOGO -->
        <button
            onclick="goHome()"
            class="flex items-center gap-3 shrink-0">

            <div class="w-11 h-11 rounded-2xl bg-green-600 text-white flex items-center justify-center shadow-lg">
                <i data-lucide="shopping-bag"></i>
            </div>

            <div class="text-right">

                <h1 class="font-black text-lg text-gray-900">
                    الشيخ
                    <span class="text-green-600">
                        إيهاب
                    </span>
                </h1>

                <p class="text-[10px] text-gray-400">
                    كل احتياجات بيتك
                </p>

            </div>

        </button>

        <!-- DESKTOP SEARCH -->
        <div class="hidden md:block flex-1 max-w-xl">

            <div class="relative">

                <input
                    id="desktop-search"
                    oninput="searchProducts(this.value)"
                    placeholder="ابحث عن منتج..."
                    class="w-full bg-gray-100 rounded-2xl py-3.5 pr-12 pl-4 outline-none focus:ring-2 focus:ring-green-500">

                <i
                    data-lucide="search"
                    class="absolute right-4 top-3.5 w-5 h-5 text-gray-400">
                </i>

            </div>

        </div>

    </div>

    <!-- MOBILE SEARCH -->
    <div class="md:hidden px-4 pb-3">

        <div class="relative">

            <input
                id="mobile-search"
                oninput="searchProducts(this.value)"
                placeholder="ابحث عن منتج..."
                class="w-full bg-gray-100 rounded-xl py-3 pr-11 pl-4 outline-none focus:ring-2 focus:ring-green-500">

            <i
                data-lucide="search"
                class="absolute right-3.5 top-3.5 w-5 h-5 text-gray-400">
            </i>

        </div>

    </div>

</header>

<!-- MAIN -->
<main class="max-w-7xl mx-auto px-4 py-6 md:py-10">

    <!-- HERO -->
    <section
        class="hero-pattern relative overflow-hidden rounded-[28px] bg-gradient-to-br from-green-600 via-green-700 to-emerald-900 text-white p-7 md:p-12 mb-10">

        <div class="relative z-10 max-w-2xl">

            <span
                class="inline-flex items-center gap-2 bg-white/15 border border-white/10 px-4 py-2 rounded-full text-xs font-bold mb-5">

                <span class="w-2 h-2 bg-white rounded-full"></span>

                توصيل سريع

            </span>

            <h2 class="text-3xl md:text-5xl font-black leading-tight">

                كل مقاضي بيتك

                <br>

                <span class="text-green-200">
                    في مكان واحد
                </span>

            </h2>

            <p class="mt-5 text-green-50 max-w-lg leading-7">

                اختار المنتجات اللي محتاجها،
                ضيفها للسلة،
                وابعت طلبك بسهولة.

            </p>

            <button
                onclick="scrollProducts()"
                class="mt-7 bg-white text-green-700 px-6 py-3.5 rounded-xl font-black">

                تسوق الآن

            </button>

        </div>

        <div class="absolute left-[-30px] bottom-[-40px] opacity-10">

            <i
                data-lucide="shopping-cart"
                class="w-72 h-72">
            </i>

        </div>

    </section>

    <!-- CATEGORIES -->
    <section id="categoriesSection" class="mb-10">

        <div class="mb-5">

            <p class="text-green-600 text-sm font-bold mb-1">
                تصفح المتجر
            </p>

            <h2 class="text-2xl md:text-3xl font-black">
                الأقسام
            </h2>

        </div>

        <div
            id="categories"
            class="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-5 gap-3 md:gap-4">
        </div>

    </section>

    <!-- PRODUCTS -->
    <section id="productsSection">

        <div class="flex items-end justify-between mb-5">

            <div>

                <p class="text-green-600 text-sm font-bold mb-1">
                    منتجاتنا
                </p>

                <h2 class="text-2xl md:text-3xl font-black">
                    جميع المنتجات
                </h2>

            </div>

            <span
                id="productCount"
                class="text-sm text-gray-400">
                0 منتج
            </span>

        </div>

        <div
            id="products"
            class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-5">
        </div>

    </section>

</main>

<!-- BOTTOM NAV -->
<nav id="bottomNav" class="bottom-nav">

    <div class="max-w-xl mx-auto h-full px-4 flex items-center justify-around">

        <!-- HOME -->
        <button
            onclick="goHome()"
            class="bottom-nav-item flex flex-col items-center justify-center gap-1 text-green-600">

            <i data-lucide="home"></i>

            <span class="text-[11px] font-bold">
                الرئيسية
            </span>

        </button>

        <!-- CATEGORIES -->
        <button
            onclick="scrollCategories()"
            class="bottom-nav-item flex flex-col items-center justify-center gap-1 text-gray-400">

            <i data-lucide="layout-grid"></i>

            <span class="text-[11px] font-bold">
                الأقسام
            </span>

        </button>

        <!-- SEARCH -->
        <button
            onclick="focusSearch()"
            class="bottom-nav-item flex flex-col items-center justify-center gap-1 text-gray-400">

            <i data-lucide="search"></i>

            <span class="text-[11px] font-bold">
                البحث
            </span>

        </button>

        <!-- CART -->
        <button
            onclick="toggleCart()"
            class="bottom-nav-item relative flex flex-col items-center justify-center gap-1 text-gray-400">

            <div class="relative">

                <i data-lucide="shopping-cart"></i>

                <span
                    id="bottomCartCount"
                    class="absolute -top-2 -right-3 min-w-[16px] h-[16px] px-1 bg-green-600 text-white text-[9px] rounded-full flex items-center justify-center">
                    0
                </span>

            </div>

            <span class="text-[11px] font-bold">
                السلة
            </span>

        </button>

    </div>

</nav>

<!-- CART OVERLAY -->
<div
    id="cartOverlay"
    onclick="toggleCart()"
    class="fixed inset-0 bg-black/50 z-[9990] hidden">
</div>

<!-- CART -->
<div
    id="cartDrawer"
    class="drawer fixed top-0 right-0 bottom-0 w-full sm:max-w-md bg-white z-[9995] hidden flex-col shadow-2xl">

    <div class="p-5 border-b flex items-center justify-between">

        <div>

            <h2 class="font-black text-xl">
                سلة المشتريات
            </h2>

            <p class="text-xs text-gray-400 mt-1">
                راجع طلبك قبل الإرسال
            </p>

        </div>

        <button
            onclick="toggleCart()"
            class="w-10 h-10 rounded-xl bg-gray-100 flex items-center justify-center">

            <i data-lucide="x"></i>

        </button>

    </div>

    <div
        id="cartItems"
        class="flex-1 overflow-y-auto p-5 space-y-3">
    </div>

    <div class="p-5 border-t bg-gray-50">

        <div class="flex justify-between mb-4">

            <span class="text-gray-500">
                الإجمالي
            </span>

            <strong
                id="cartTotal"
                class="text-xl text-green-600">
                0 ج.م
            </strong>

        </div>

        <button
            onclick="openCheckout()"
            class="w-full bg-green-600 hover:bg-green-700 text-white py-4 rounded-2xl font-black">

            إتمام الطلب

        </button>

    </div>

</div>

<!-- CHECKOUT -->
<div
    id="checkoutModal"
    class="fixed inset-0 bg-black/60 z-[10000] hidden items-center justify-center p-4">

    <div
        class="modal bg-white rounded-3xl w-full max-w-lg max-h-[90vh] overflow-y-auto">

        <div class="p-6 border-b flex justify-between items-center">

            <div>

                <h2 class="text-2xl font-black">
                    تأكيد الطلب
                </h2>

                <p class="text-sm text-gray-400 mt-1">
                    اكتب بيانات التوصيل
                </p>

            </div>

            <button
                onclick="closeCheckout()"
                class="w-10 h-10 rounded-xl bg-gray-100">

                <i data-lucide="x"></i>

            </button>

        </div>

        <div class="p-6">

            <label class="block font-bold text-sm mb-2">
                الاسم بالكامل
            </label>

            <input
                id="customerName"
                class="w-full border border-gray-200 rounded-xl p-3.5 mb-4 outline-none focus:border-green-500"
                placeholder="مثال: محمد أحمد">

            <label class="block font-bold text-sm mb-2">
                رقم الهاتف
            </label>

            <input
                id="customerPhone"
                type="tel"
                class="w-full border border-gray-200 rounded-xl p-3.5 mb-4 outline-none focus:border-green-500"
                placeholder="01xxxxxxxxx">

            <label class="block font-bold text-sm mb-2">
                العنوان
            </label>

            <textarea
                id="customerAddress"
                rows="3"
                class="w-full border border-gray-200 rounded-xl p-3.5 mb-4 outline-none focus:border-green-500"
                placeholder="المحافظة - المنطقة - الشارع - رقم المنزل"></textarea>

            <label class="block font-bold text-sm mb-2">
                ملاحظات
            </label>

            <textarea
                id="customerNotes"
                rows="2"
                class="w-full border border-gray-200 rounded-xl p-3.5 mb-5 outline-none focus:border-green-500"
                placeholder="أي ملاحظات إضافية (اختياري)"></textarea>

            <div
                class="bg-green-50 rounded-2xl p-4 flex justify-between items-center mb-5">

                <span class="font-bold">
                    إجمالي الطلب
                </span>

                <strong
                    id="checkoutTotal"
                    class="text-xl text-green-600">
                    0 ج.م
                </strong>

            </div>

            <button
                id="submitOrderButton"
                onclick="submitOrder()"
                class="w-full bg-green-600 hover:bg-green-700 text-white py-4 rounded-2xl font-black">

                تأكيد وإرسال الطلب

            </button>

        </div>

    </div>

</div>

<!-- SUCCESS -->
<div
    id="successModal"
    class="fixed inset-0 bg-black/60 z-[11000] hidden items-center justify-center p-4">

    <div
        class="bg-white rounded-3xl p-8 text-center max-w-sm w-full">

        <div
            class="w-20 h-20 bg-green-100 text-green-600 rounded-full flex items-center justify-center mx-auto mb-5">

            <i data-lucide="check" class="w-10 h-10"></i>

        </div>

        <h2 class="text-2xl font-black">
            تم إرسال طلبك بنجاح
        </h2>

        <p class="text-gray-500 mt-2">
            رقم طلبك
        </p>

        <div
            id="successOrderId"
            class="text-green-600 text-2xl font-black mt-1">
        </div>

        <button
            onclick="closeSuccess()"
            class="w-full mt-6 bg-green-600 text-white py-3.5 rounded-xl font-bold">

            العودة للمتجر

        </button>

    </div>

</div>

<script>

/* =====================================================
   SUPABASE
===================================================== */

const SUPABASE_URL =
"https://zxcvmfgpbjltqdtislvi.supabase.co";

const SUPABASE_KEY =
"sb_publishable_Ud_yA-0XkEv6WvwrM407mA_vUcYZy-f";

const db =
supabase.createClient(
    SUPABASE_URL,
    SUPABASE_KEY
);


/* =====================================================
   DATA
===================================================== */

let categories = [];
let products = [];
let cart = [];


/* =====================================================
   LOAD STORE
===================================================== */

async function loadStore() {

    const categoriesResult =
        await db
        .from("categories")
        .select("*")
        .order("id");

    if (categoriesResult.error) {

        console.error(categoriesResult.error);

        alert("حدث خطأ أثناء تحميل الأقسام");

        return;
    }

    categories =
        categoriesResult.data || [];


    const productsResult =
        await db
        .from("products")
        .select("*")
        .eq("is_active", true)
        .order("id", {
            ascending: false
        });

    if (productsResult.error) {

        console.error(productsResult.error);

        alert("حدث خطأ أثناء تحميل المنتجات");

        return;
    }

    products =
        productsResult.data || [];


    renderCategories();

    renderProducts(products);
}


/* =====================================================
   CATEGORIES
===================================================== */

function renderCategories() {

    const box =
        document.getElementById("categories");

    if (!categories.length) {

        box.innerHTML = `
            <div class="col-span-full text-center py-10 text-gray-400">
                لا توجد أقسام
            </div>
        `;

        return;
    }


    box.innerHTML =
        categories.map(category => `

            <button
                onclick="filterCategory(${category.id})"
                class="category-card bg-white border border-gray-100 rounded-2xl p-4 text-center shadow-sm hover:shadow-md">

                <div
                    class="w-14 h-14 mx-auto rounded-2xl bg-green-50 text-green-600 flex items-center justify-center mb-3">

                    <i
                        data-lucide="${category.icon || "package"}">
                    </i>

                </div>

                <h3 class="font-black text-sm">
                    ${escapeHTML(category.name)}
                </h3>

                <p class="text-[11px] text-gray-400 mt-1 line-clamp-2">
                    ${escapeHTML(category.description || "")}
                </p>

            </button>

        `).join("");


    lucide.createIcons();
}


/* =====================================================
   PRODUCTS
===================================================== */

function renderProducts(list) {

    const box =
        document.getElementById("products");

    document.getElementById("productCount").innerText =
        `${list.length} منتج`;


    if (!list.length) {

        box.innerHTML = `
            <div class="col-span-full bg-white rounded-2xl p-12 text-center text-gray-400">
                لا توجد منتجات في هذا القسم حاليًا
            </div>
        `;

        return;
    }


    box.innerHTML =
        list.map(product => `

            <article
                class="product-card bg-white rounded-3xl overflow-hidden border border-gray-100 shadow-sm">

                <div class="relative">

                    <img
                        src="${product.image || "https://placehold.co/600x450?text=Product"}"
                        class="w-full h-48 object-cover"
                        onerror="this.src='https://placehold.co/600x450?text=Product'">

                </div>

                <div class="p-5">

                    <h3
                        class="font-black text-base line-clamp-2 min-h-[48px]">

                        ${escapeHTML(product.name)}

                    </h3>

                    ${
                        product.description
                        ?
                        `
                        <p class="text-xs text-gray-400 mt-2 line-clamp-2">
                            ${escapeHTML(product.description)}
                        </p>
                        `
                        :
                        ""
                    }

                    <div
                        class="flex items-center justify-between gap-2 mt-5">

                        <div>

                            <p class="text-xs text-gray-400">
                                السعر
                            </p>

                            <strong
                                class="text-xl text-green-600 font-black">

                                ${Number(product.price).toFixed(2)}

                                <span class="text-xs">
                                    ج.م
                                </span>

                            </strong>

                        </div>

                        <button
                            onclick="addToCart(${product.id})"
                            class="bg-green-600 hover:bg-green-700 text-white px-4 py-3 rounded-xl font-bold flex items-center gap-2">

                            <i
                                data-lucide="plus"
                                class="w-4 h-4">
                            </i>

                            السلة

                        </button>

                    </div>

                </div>

            </article>

        `).join("");


    lucide.createIcons();
}


/* =====================================================
   CATEGORY FILTER
===================================================== */

function filterCategory(categoryId) {

    const filtered =
        products.filter(
            product =>
                Number(product.category_id) ===
                Number(categoryId)
        );


    renderProducts(filtered);

    scrollProducts();
}


/* =====================================================
   SEARCH
===================================================== */

function searchProducts(value) {

    const query =
        value.trim().toLowerCase();


    const filtered =
        products.filter(product =>

            product.name
            .toLowerCase()
            .includes(query)

            ||

            (
                product.description || ""
            )
            .toLowerCase()
            .includes(query)

        );


    renderProducts(filtered);
}


/* =====================================================
   CART
===================================================== */

function addToCart(id) {

    const product =
        products.find(
            item =>
                Number(item.id) === Number(id)
        );


    if (!product) return;


    const existing =
        cart.find(
            item =>
                Number(item.id) === Number(id)
        );


    if (existing) {

        existing.quantity++;

    } else {

        cart.push({
            ...product,
            quantity: 1
        });

    }


    updateCart();

    toggleCart();
}


/* =====================================================
   QUANTITY
===================================================== */

function changeQuantity(id, amount) {

    const item =
        cart.find(
            product =>
                Number(product.id) === Number(id)
        );


    if (!item) return;


    item.quantity += amount;


    if (item.quantity <= 0) {

        cart =
            cart.filter(
                product =>
                    Number(product.id) !== Number(id)
            );
    }


    updateCart();
}


/* =====================================================
   CART TOTAL
===================================================== */

function cartTotal() {

    return cart.reduce(
        (total, item) =>
            total +
            Number(item.price) *
            item.quantity,
        0
    );
}


/* =====================================================
   UPDATE CART
===================================================== */

function updateCart() {

    const count =
        cart.reduce(
            (total, item) =>
                total + item.quantity,
            0
        );


    document.getElementById(
        "bottomCartCount"
    ).innerText = count;


    const total =
        cartTotal();


    document.getElementById(
        "cartTotal"
    ).innerText =
        total.toFixed(2) + " ج.م";


    document.getElementById(
        "checkoutTotal"
    ).innerText =
        total.toFixed(2) + " ج.م";


    const box =
        document.getElementById("cartItems");


    if (!cart.length) {

        box.innerHTML = `

            <div class="text-center py-16">

                <div
                    class="w-16 h-16 bg-gray-100 rounded-full mx-auto flex items-center justify-center mb-4">

                    <i
                        data-lucide="shopping-cart"
                        class="w-7 h-7 text-gray-400">
                    </i>

                </div>

                <p class="font-bold">
                    السلة فارغة
                </p>

                <p class="text-sm text-gray-400 mt-1">
                    أضف المنتجات التي تريد شراءها
                </p>

            </div>

        `;

        lucide.createIcons();

        return;
    }


    box.innerHTML =
        cart.map(item => `

            <div
                class="border border-gray-100 rounded-2xl p-3">

                <div class="flex gap-3">

                    <img
                        src="${item.image || "https://placehold.co/100x100"}"
                        class="w-20 h-20 rounded-xl object-cover">

                    <div class="flex-1">

                        <h3 class="font-bold text-sm">
                            ${escapeHTML(item.name)}
                        </h3>

                        <p class="text-green-600 font-black mt-1">
                            ${Number(item.price).toFixed(2)}
                            ج.م
                        </p>

                        <div class="flex items-center gap-2 mt-3">

                            <button
                                onclick="changeQuantity(${item.id},-1)"
                                class="w-8 h-8 rounded-lg bg-gray-100 font-black">
                                -
                            </button>

                            <span
                                class="font-black min-w-5 text-center">
                                ${item.quantity}
                            </span>

                            <button
                                onclick="changeQuantity(${item.id},1)"
                                class="w-8 h-8 rounded-lg bg-green-600 text-white font-black">
                                +
                            </button>

                        </div>

                    </div>

                </div>

            </div>

        `).join("");


    lucide.createIcons();
}


/* =====================================================
   TOGGLE CART
   الشريط السفلي يختفي عند فتح السلة
===================================================== */

function toggleCart() {

    const drawer =
        document.getElementById("cartDrawer");

    const overlay =
        document.getElementById("cartOverlay");

    const bottomNav =
        document.getElementById("bottomNav");


    const isHidden =
        drawer.classList.contains("hidden");


    if (isHidden) {

        drawer.classList.remove("hidden");

        drawer.classList.add("flex");

        overlay.classList.remove("hidden");


        // إخفاء الشريط السفلي
        bottomNav.classList.add("cart-hidden-nav");


        // منع تحرك الصفحة خلف السلة
        document.body.classList.add("cart-open");

    } else {

        drawer.classList.add("hidden");

        drawer.classList.remove("flex");

        overlay.classList.add("hidden");


        // إعادة الشريط السفلي
        bottomNav.classList.remove("cart-hidden-nav");


        // إعادة حركة الصفحة
        document.body.classList.remove("cart-open");
    }
}


/* =====================================================
   CHECKOUT
===================================================== */

function openCheckout() {

    if (!cart.length) {

        alert("السلة فارغة");

        return;
    }


    const modal =
        document.getElementById("checkoutModal");


    modal.classList.remove("hidden");

    modal.classList.add("flex");


    document.getElementById(
        "checkoutTotal"
    ).innerText =
        cartTotal().toFixed(2) + " ج.م";
}


function closeCheckout() {

    const modal =
        document.getElementById("checkoutModal");


    modal.classList.add("hidden");

    modal.classList.remove("flex");
}


/* =====================================================
   SUBMIT ORDER
===================================================== */

async function submitOrder() {

    const name =
        document.getElementById(
            "customerName"
        ).value.trim();


    const phone =
        document.getElementById(
            "customerPhone"
        ).value.trim();


    const address =
        document.getElementById(
            "customerAddress"
        ).value.trim();


    const notes =
        document.getElementById(
            "customerNotes"
        ).value.trim();


    if (!name) {

        alert("من فضلك اكتب الاسم");

        return;
    }


    if (!phone) {

        alert("من فضلك اكتب رقم الهاتف");

        return;
    }


    if (!address) {

        alert("من فضلك اكتب العنوان");

        return;
    }


    if (!cart.length) {

        alert("السلة فارغة");

        return;
    }


    const button =
        document.getElementById(
            "submitOrderButton"
        );


    button.disabled = true;

    button.innerText =
        "جاري إرسال الطلب...";


    const total =
        cartTotal();


    const orderResult =
        await db
        .from("orders")
        .insert({

            customer_name:
                name,

            customer_phone:
                phone,

            customer_address:
                address,

            total_amount:
                total,

            status:
                "new",

            notes:
                notes || null

        })
        .select()
        .single();


    if (orderResult.error) {

        console.error(
            orderResult.error
        );

        alert(
            "لم يتم إرسال الطلب:\n" +
            orderResult.error.message
        );

        button.disabled = false;

        button.innerText =
            "تأكيد وإرسال الطلب";

        return;
    }


    const order =
        orderResult.data;


    const orderItems =
        cart.map(
            item => ({

                order_id:
                    order.id,

                product_id:
                    item.id,

                product_name:
                    item.name,

                price:
                    Number(item.price),

                quantity:
                    item.quantity

            })
        );


    const itemsResult =
        await db
        .from("order_items")
        .insert(orderItems);


    if (itemsResult.error) {

        console.error(
            itemsResult.error
        );

        alert(
            "تم إنشاء الطلب ولكن حدث خطأ في حفظ المنتجات."
        );

        button.disabled = false;

        button.innerText =
            "تأكيد وإرسال الطلب";

        return;
    }


    closeCheckout();


    document.getElementById(
        "successOrderId"
    ).innerText =
        "#" + order.id;


    const success =
        document.getElementById(
            "successModal"
        );


    success.classList.remove("hidden");

    success.classList.add("flex");


    cart = [];

    updateCart();


    document.getElementById(
        "customerName"
    ).value = "";

    document.getElementById(
        "customerPhone"
    ).value = "";

    document.getElementById(
        "customerAddress"
    ).value = "";

    document.getElementById(
        "customerNotes"
    ).value = "";


    button.disabled = false;

    button.innerText =
        "تأكيد وإرسال الطلب";
}


/* =====================================================
   SUCCESS
===================================================== */

function closeSuccess() {

    const modal =
        document.getElementById(
            "successModal"
        );


    modal.classList.add("hidden");

    modal.classList.remove("flex");
}


/* =====================================================
   BOTTOM NAV
===================================================== */

function goHome() {

    window.scrollTo({
        top: 0,
        behavior: "smooth"
    });


    renderProducts(products);
}


function scrollCategories() {

    document
        .getElementById("categoriesSection")
        .scrollIntoView({
            behavior: "smooth"
        });
}


function scrollProducts() {

    document
        .getElementById("productsSection")
        .scrollIntoView({
            behavior: "smooth"
        });
}


/* =====================================================
   SEARCH
===================================================== */

function focusSearch() {

    const input =
        window.innerWidth >= 768
        ?
        document.getElementById(
            "desktop-search"
        )
        :
        document.getElementById(
            "mobile-search"
        );


    // إظهار حقل البحث بدون تحريك الصفحة لأعلى
    input.focus({
        preventScroll: true
    });


    input.scrollIntoView({
        behavior: "smooth",
        block: "center"
    });
}


/* =====================================================
   منع تحرك الشريط مع الكيبورد على الموبايل
===================================================== */

let originalViewportHeight =
    window.innerHeight;


window.addEventListener(
    "resize",
    function() {

        if (
            document.body.classList.contains(
                "cart-open"
            )
        ) {
            return;
        }

        // الشريط يظل مثبتًا أسفل الشاشة
        const nav =
            document.getElementById(
                "bottomNav"
            );

        if (nav) {
            nav.style.position = "fixed";
            nav.style.bottom = "0";
        }

    }
);


/* =====================================================
   SECURITY
===================================================== */

function escapeHTML(value) {

    return String(value ?? "")
        .replaceAll("&", "&amp;")
        .replaceAll("<", "&lt;")
        .replaceAll(">", "&gt;")
        .replaceAll('"', "&quot;")
        .replaceAll("'", "&#039;");
}


/* =====================================================
   START
===================================================== */

lucide.createIcons();

loadStore();

updateCart();

</script>

</body>
</html>
