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
                        class="flex items-center justi
            <i data-lucide="layout-dashboard"></i>

            الرئيسية

        </button>


        <button
        onclick="showPage('orders')"
        class="w-full flex items-center gap-3 px-4 py-3 rounded-xl text-gray-600 hover:bg-gray-50">

            <i data-lucide="shopping-cart"></i>

            الطلبات

            <span
            id="sideOrdersCount"
            class="mr-auto bg-green-600 text-white text-xs px-2 py-1 rounded-full">
                0
            </span>

        </button>


        <button
        onclick="showPage('products')"
        class="w-full flex items-center gap-3 px-4 py-3 rounded-xl text-gray-600 hover:bg-gray-50">

            <i data-lucide="package"></i>

            المنتجات

        </button>


        <button
        onclick="showPage('categories')"
        class="w-full flex items-center gap-3 px-4 py-3 rounded-xl text-gray-600 hover:bg-gray-50">

            <i data-lucide="layout-grid"></i>

            الأقسام

        </button>

    </nav>


    <div class="p-4 border-t">

        <button
        onclick="window.open('index.html','_blank')"
        class="w-full flex items-center gap-3 px-4 py-3 rounded-xl bg-gray-100 text-gray-700 font-bold">

            <i data-lucide="external-link"></i>

            فتح المتجر

        </button>

    </div>

</aside>


<div
id="sidebarOverlay"
onclick="closeSidebar()"
class="fixed inset-0 bg-black/40 z-[60] hidden">
</div>


<!-- =========================
     MAIN
========================= -->

<div class="md:mr-72 min-h-screen">

    <header
    class="h-20 bg-white border-b flex items-center justify-between px-4 md:px-8 sticky top-0 z-40">

        <div class="flex items-center gap-3">

            <button
            onclick="toggleSidebar()"
            class="md:hidden w-11 h-11 rounded-xl bg-gray-100 flex items-center justify-center">

                <i data-lucide="menu"></i>

            </button>


            <div>

                <h2
                id="pageTitle"
                class="font-black text-xl md:text-2xl">

                    الرئيسية

                </h2>

                <p class="text-xs text-gray-400">
                    إدارة متجر الشيخ إيهاب
                </p>

            </div>

        </div>


        <button
        onclick="loadAll()"
        class="w-11 h-11 rounded-xl bg-green-50 text-green-600 flex items-center justify-center">

            <i data-lucide="refresh-cw"></i>

        </button>

    </header>


    <main class="p-4 md:p-8">


        <!-- =========================
             DASHBOARD
        ========================= -->

        <section id="page-dashboard">

            <div class="grid grid-cols-2 lg:grid-cols-4 gap-4 mb-8">


                <div class="card bg-white rounded-2xl p-5 border">

                    <div class="flex justify-between">

                        <div>

                            <p class="text-sm text-gray-400">
                                الأقسام
                            </p>

                            <strong
                            id="statCategories"
                            class="text-3xl font-black block mt-2">
                                0
                            </strong>

                        </div>

                        <div class="w-12 h-12 rounded-xl bg-blue-50 text-blue-600 flex items-center justify-center">

                            <i data-lucide="layout-grid"></i>

                        </div>

                    </div>

                </div>


                <div class="card bg-white rounded-2xl p-5 border">

                    <div class="flex justify-between">

                        <div>

                            <p class="text-sm text-gray-400">
                                المنتجات
                            </p>

                            <strong
                            id="statProducts"
                            class="text-3xl font-black block mt-2">
                                0
                            </strong>

                        </div>

                        <div class="w-12 h-12 rounded-xl bg-purple-50 text-purple-600 flex items-center justify-center">

                            <i data-lucide="package"></i>

                        </div>

                    </div>

                </div>


                <div class="card bg-white rounded-2xl p-5 border">

                    <div class="flex justify-between">

                        <div>

                            <p class="text-sm text-gray-400">
                                الطلبات
                            </p>

                            <strong
                            id="statOrders"
                            class="text-3xl font-black block mt-2">
                                0
                            </strong>

                        </div>

                        <div class="w-12 h-12 rounded-xl bg-orange-50 text-orange-600 flex items-center justify-center">

                            <i data-lucide="shopping-cart"></i>

                        </div>

                    </div>

                </div>


                <div class="card bg-white rounded-2xl p-5 border">

                    <div class="flex justify-between">

                        <div>

                            <p class="text-sm text-gray-400">
                                المبيعات
                            </p>

                            <strong
                            id="statSales"
                            class="text-2xl font-black block mt-2 text-green-600">
                                0 ج.م
                            </strong>

                        </div>

                        <div class="w-12 h-12 rounded-xl bg-green-50 text-green-600 flex items-center justify-center">

                            <i data-lucide="banknote"></i>

                        </div>

                    </div>

                </div>

            </div>


            <!-- أحدث الطلبات -->

            <div class="bg-white rounded-2xl border overflow-hidden">

                <div class="p-5 border-b flex items-center justify-between">

                    <div>

                        <h3 class="font-black text-lg">
                            أحدث الطلبات
                        </h3>

                        <p class="text-xs text-gray-400 mt-1">
                            آخر الطلبات التي وصلت من الموقع
                        </p>

                    </div>


                    <button
                    onclick="showPage('orders')"
                    class="text-green-600 font-bold text-sm">

                        عرض الكل

                    </button>

                </div>


                <div
                id="latestOrders"
                class="divide-y">

                </div>

            </div>

        </section>


        <!-- =========================
             ORDERS
        ========================= -->

        <section
        id="page-orders"
        class="hidden">

            <div class="bg-white rounded-2xl border overflow-hidden">

                <div class="p-5 border-b">

                    <h3 class="font-black text-xl">
                        إدارة الطلبات
                    </h3>

                    <p class="text-sm text-gray-400 mt-1">
                        اضغط على أي طلب لعرض تفاصيله
                    </p>

                </div>


                <div
                id="ordersList"
                class="divide-y">

                </div>

            </div>

        </section>


        <!-- =========================
             PRODUCTS
        ========================= -->

        <section
        id="page-products"
        class="hidden">

            <div class="flex flex-col md:flex-row gap-4 justify-between mb-5">

                <div>

                    <h3 class="text-xl font-black">
                        المنتجات
                    </h3>

                    <p class="text-sm text-gray-400">
                        إضافة وتعديل وحذف المنتجات
                    </p>

                </div>


                <button
                onclick="openProductModal()"
                class="bg-green-600 text-white px-5 py-3 rounded-xl font-bold flex items-center justify-center gap-2">

                    <i data-lucide="plus"></i>

                    إضافة منتج

                </button>

            </div>


            <div
            id="productsList"
            class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-4">

            </div>

        </section>


        <!-- =========================
             CATEGORIES
        ========================= -->

        <section
        id="page-categories"
        class="hidden">

            <div class="flex flex-col md:flex-row gap-4 justify-between mb-5">

                <div>

                    <h3 class="text-xl font-black">
                        الأقسام
                    </h3>

                    <p class="text-sm text-gray-400">
                        إدارة أقسام المتجر
                    </p>

                </div>


                <button
                onclick="openCategoryModal()"
                class="bg-green-600 text-white px-5 py-3 rounded-xl font-bold flex items-center justify-center gap-2">

                    <i data-lucide="plus"></i>

                    إضافة قسم

                </button>

            </div>


            <div
            id="categoriesList"
            class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-4">

            </div>

        </section>

    </main>

</div>


<!-- =========================
     ORDER MODAL
========================= -->

<div
id="orderModal"
class="fixed inset-0 bg-black/60 z-[100] hidden items-center justify-center p-4">

    <div
    class="modal bg-white rounded-3xl w-full max-w-2xl max-h-[90vh] overflow-y-auto">

        <div
        class="p-5 border-b flex justify-between items-center">

            <div>

                <h3 class="text-xl font-black">
                    تفاصيل الطلب
                </h3>

                <p
                id="modalOrderNumber"
                class="text-sm text-green-600 font-bold mt-1">
                </p>

            </div>


            <button
            onclick="closeOrderModal()"
            class="w-10 h-10 bg-gray-100 rounded-xl flex items-center justify-center">

                <i data-lucide="x"></i>

            </button>

        </div>


        <div
        id="orderDetails"
        class="p-5">

        </div>

    </div>

</div>


<!-- =========================
     CATEGORY MODAL
========================= -->

<div
id="categoryModal"
class="fixed inset-0 bg-black/60 z-[100] hidden items-center justify-center p-4">

    <div class="modal bg-white rounded-3xl w-full max-w-md">

        <div class="p-5 border-b flex justify-between">

            <h3 class="text-xl font-black">

                <span id="categoryModalTitle">
                    إضافة قسم
                </span>

            </h3>


            <button onclick="closeCategoryModal()">

                <i data-lucide="x"></i>

            </button>

        </div>


        <div class="p-5 space-y-4">

            <input
            id="categoryId"
            type="hidden">


            <div>

                <label class="text-sm font-bold block mb-2">
                    اسم القسم
                </label>

                <input
                id="categoryName"
                class="w-full border rounded-xl p-3 outline-none focus:border-green-500"
                placeholder="مثال: شوكولاتة">

            </div>


            <div>

                <label class="text-sm font-bold block mb-2">
                    الوصف
                </label>

                <textarea
                id="categoryDescription"
                rows="3"
                class="w-full border rounded-xl p-3 outline-none focus:border-green-500"
                placeholder="وصف القسم"></textarea>

            </div>


            <div>

                <label class="text-sm font-bold block mb-2">
                    اسم الأيقونة
                </label>

                <input
                id="categoryIcon"
                class="w-full border rounded-xl p-3 outline-none focus:border-green-500"
                placeholder="package"
                value="package">

            </div>


            <button
            onclick="saveCategory()"
            class="w-full bg-green-600 text-white py-3.5 rounded-xl font-black">

                حفظ القسم

            </button>

        </div>

    </div>

</div>


<!-- =========================
     PRODUCT MODAL
========================= -->

<div
id="productModal"
class="fixed inset-0 bg-black/60 z-[100] hidden items-center justify-center p-4">

    <div
    class="modal bg-white rounded-3xl w-full max-w-lg max-h-[90vh] overflow-y-auto">

        <div class="p-5 border-b flex justify-between">

            <h3 class="text-xl font-black">

                <span id="productModalTitle">
                    إضافة منتج
                </span>

            </h3>


            <button onclick="closeProductModal()">

                <i data-lucide="x"></i>

            </button>

        </div>


        <div class="p-5 space-y-4">

            <input
            id="productId"
            type="hidden">


            <div>

                <label class="text-sm font-bold block mb-2">
                    اسم المنتج
                </label>

                <input
                id="productName"
                class="w-full border rounded-xl p-3 outline-none focus:border-green-500">

            </div>


            <div>

                <label class="text-sm font-bold block mb-2">
                    القسم
                </label>

                <select
                id="productCategory"
                class="w-full border rounded-xl p-3 outline-none focus:border-green-500">

                </select>

            </div>


            <div>

                <label class="text-sm font-bold block mb-2">
                    الوصف
                </label>

                <textarea
                id="productDescription"
                rows="3"
                class="w-full border rounded-xl p-3 outline-none focus:border-green-500"></textarea>

            </div>


            <div class="grid grid-cols-2 gap-3">

                <div>

                    <label class="text-sm font-bold block mb-2">
                        السعر
                    </label>

                    <input
                    id="productPrice"
                    type="number"
                    step="0.01"
                    class="w-full border rounded-xl p-3 outline-none focus:border-green-500">

                </div>


                <div>

                    <label class="text-sm font-bold block mb-2">
                        المخزون
                    </label>

                    <input
                    id="productStock"
                    type="number"
                    class="w-full border rounded-xl p-3 outline-none focus:border-green-500">

                </div>

            </div>


            <div>

                <label class="text-sm font-bold block mb-2">
                    رابط الصورة
                </label>

                <input
                id="productImage"
                class="w-full border rounded-xl p-3 outline-none focus:border-green-500"
                placeholder="https://...">

            </div>


            <button
            onclick="saveProduct()"
            class="w-full bg-green-600 text-white py-3.5 rounded-xl font-black">

                حفظ المنتج

            </button>

        </div>

    </div>

</div>


<script>

/* =========================
   SUPABASE
========================= */

const SUPABASE_URL =
"https://zxcvmfgpbjltqdtislvi.supabase.co";

const SUPABASE_KEY =
"sb_publishable_Ud_yA-0XkEv6WvwrM407mA_vUcYZy-f";

const db =
supabase.createClient(
    SUPABASE_URL,
    SUPABASE_KEY
);


/* =========================
   DATA
========================= */

let categories = [];
let products = [];
let orders = [];


/* =========================
   START
========================= */

document.addEventListener(
    "DOMContentLoaded",
    async () => {

        lucide.createIcons();

        await loadAll();

    }
);


/* =========================
   LOAD ALL
========================= */

async function loadAll() {

    await Promise.all([
        loadCategories(),
        loadProducts(),
        loadOrders()
    ]);

    updateStats();
}


/* =========================
   LOAD CATEGORIES
========================= */

async function loadCategories() {

    const result =
    await db
    .from("categories")
    .select("*")
    .order("id");

    if (result.error) {

        console.error(result.error);

        alert(
            "خطأ في تحميل الأقسام:\n"
            + result.error.message
        );

        return;
    }

    categories =
    result.data || [];

    renderCategories();

    fillProductCategories();
}


/* =========================
   LOAD PRODUCTS
========================= */

async function loadProducts() {

    const result =
    await db
    .from("products")
    .select("*")
    .order(
        "id",
        {
            ascending: false
        }
    );

    if (result.error) {

        console.error(result.error);

        alert(
            "خطأ في تحميل المنتجات:\n"
            + result.error.message
        );

        return;
    }

    products =
    result.data || [];

    renderProducts();
}


/* =========================
   LOAD ORDERS
========================= */

async function loadOrders() {

    const result =
    await db
    .from("orders")
    .select("*")
    .order(
        "created_at",
        {
            ascending: false
        }
    );

    if (result.error) {

        console.error(result.error);

        alert(
            "خطأ في تحميل الطلبات:\n"
            + result.error.message
        );

        return;
    }

    orders =
    result.data || [];

    renderOrders();

    renderLatestOrders();
}


/* =========================
   STATS
========================= */

function updateStats() {

    document.getElementById(
        "statCategories"
    ).innerText =
    categories.length;


    document.getElementById(
        "statProducts"
    ).innerText =
    products.length;


    document.getElementById(
        "statOrders"
    ).innerText =
    orders.length;


    const sales =
    orders.reduce(
        (total, order) => {

            return total +
            Number(
                order.total_amount || 0
            );

        },
        0
    );


    document.getElementById(
        "statSales"
    ).innerText =
    sales.toFixed(2)
    + " ج.م";


    document.getElementById(
        "sideOrdersCount"
    ).innerText =
    orders.length;
}


/* =========================
   LATEST ORDERS
========================= */

function renderLatestOrders() {

    const box =
    document.getElementById(
        "latestOrders"
    );


    const latest =
    orders.slice(0, 5);


    if (!latest.length) {

        box.innerHTML = `

            <div class="p-10 text-center text-gray-400">

                لا توجد طلبات حتى الآن

            </div>

        `;

        return;
    }


    box.innerHTML =
    latest
    .map(
        order => orderRow(order)
    )
    .join("");


    lucide.createIcons();
}


/* =========================
   ORDERS
========================= */

function renderOrders() {

    const box =
    document.getElementById(
        "ordersList"
    );


    if (!orders.length) {

        box.innerHTML = `

            <div class="p-14 text-center text-gray-400">

                لا توجد طلبات حتى الآن

            </div>

        `;

        return;
    }


    box.innerHTML =
    orders
    .map(
        order =>
        orderRow(
            order,
            true
        )
    )
    .join("");


    lucide.createIcons();
}


/* =========================
   ORDER ROW
========================= */

function orderRow(
    order,
    full = false
) {

    const date =
    new Date(
        order.created_at
    );


    return `

        <div
        onclick="openOrder(${order.id})"
        class="p-4 md:p-5 hover:bg-gray-50 cursor-pointer">

            <div class="flex items-center gap-3">

                <div
                class="w-11 h-11 rounded-xl bg-green-50 text-green-600 flex items-center justify-center shrink-0">

                    <i data-lucide="shopping-bag"></i>

                </div>


                <div class="min-w-0 flex-1">

                    <div class="flex flex-wrap items-center gap-2">

                        <h4 class="font-black">
                            طلب #${order.id}
                        </h4>

                        ${statusBadge(order.status)}

                    </div>


                    <p class="text-sm text-gray-500 mt-1 truncate">

                        ${escapeHTML(
                            order.customer_name
                        )}

                        ·

                        ${escapeHTML(
                            order.customer_phone
                        )}

                    </p>


                    <p class="text-xs text-gray-400 mt-1">

                        ${formatDate(date)}

                    </p>

                </div>


                <div class="text-left shrink-0">

                    <strong class="text-green-600 font-black">

                        ${Number(
                            order.total_amount || 0
                        ).toFixed(2)}

                        ج.م

                    </strong>

                    <p class="text-xs text-gray-400 mt-1">
                        عرض التفاصيل
                    </p>

                </div>

            </div>

        </div>

    `;
}


/* =========================
   STATUS
========================= */

function statusBadge(status) {

    const map = {

        new: [
            "جديد",
            "bg-blue-50 text-blue-600"
        ],

        processing: [
            "جاري التجهيز",
            "bg-orange-50 text-orange-600"
        ],

        completed: [
            "مكتمل",
            "bg-green-50 text-green-600"
        ],

        cancelled: [
            "ملغي",
            "bg-red-50 text-red-600"
        ]

    };


    const data =
    map[status] ||
    [
        status || "جديد",
        "bg-gray-100 text-gray-600"
    ];


    return `

        <span
        class="text-[11px] px-2.5 py-1 rounded-full font-bold ${data[1]}">

            ${data[0]}

        </span>

    `;
}


/* =========================
   OPEN ORDER
========================= */

async function openOrder(id) {

    const order =
    orders.find(
        item =>
        Number(item.id)
        === Number(id)
    );


    if (!order) return;


    const result =
    await db
    .from("order_items")
    .select("*")
    .eq(
        "order_id",
        order.id
    )
    .order("id");


    if (result.error) {

        alert(
            "لم يتم تحميل تفاصيل الطلب:\n"
            + result.error.message
        );

        return;
    }


    const items =
    result.data || [];


    document.getElementById(
        "modalOrderNumber"
    ).innerText =
    "طلب #" + order.id;


    document.getElementById(
        "orderDetails"
    ).innerHTML = `

        <div class="bg-gray-50 rounded-2xl p-4 mb-5">

            <h4 class="font-black mb-4">
                بيانات العميل
            </h4>


            <div class="grid sm:grid-cols-2 gap-3">

                <div class="bg-white rounded-xl p-3">

                    <p class="text-xs text-gray-400">
                        الاسم
                    </p>

                    <p class="font-bold mt-1">

                        ${escapeHTML(
                            order.customer_name
                        )}

                    </p>

                </div>


                <div class="bg-white rounded-xl p-3">

                    <p class="text-xs text-gray-400">
                        الهاتف
                    </p>

                    <p class="font-bold mt-1">

                        ${escapeHTML(
                            order.customer_phone
                        )}

                    </p>

                </div>


                <div class="bg-white rounded-xl p-3 sm:col-span-2">

                    <p class="text-xs text-gray-400">
                        العنوان
                    </p>

                    <p class="font-bold mt-1">

                        ${escapeHTML(
                            order.customer_address
                        )}

                    </p>

                </div>


                ${
                    order.notes
                    ?
                    `

                    <div class="bg-white rounded-xl p-3 sm:col-span-2">

                        <p class="text-xs text-gray-400">
                            ملاحظات
                        </p>

                        <p class="font-bold mt-1">

                            ${escapeHTML(
                                order.notes
                            )}

                        </p>

                    </div>

                    `
                    :
                    ""
                }

            </div>

        </div>


        <div class="mb-5">

            <label class="text-sm font-bold block mb-2">
                حالة الطلب
            </label>


            <select
            onchange="changeOrderStatus(${order.id},this.value)"
            class="w-full border rounded-xl p-3 outline-none focus:border-green-500">

                <option
                value="new"
                ${order.status === "new" ? "selected" : ""}>
                    جديد
                </option>

                <option
                value="processing"
                ${order.status === "processing" ? "selected" : ""}>
                    جاري التجهيز
                </option>

                <option
                value="completed"
                ${order.status === "completed" ? "selected" : ""}>
                    مكتمل
                </option>

                <option
                value="cancelled"
                ${order.status === "cancelled" ? "selected" : ""}>
                    ملغي
                </option>

            </select>

        </div>


        <div>

            <div class="flex justify-between items-center mb-3">

                <h4 class="font-black">
                    المنتجات المطلوبة
                </h4>

                <span class="text-sm text-gray-400">
                    ${items.length} منتج
                </span>

            </div>


            <div class="space-y-3">

                ${
                    items.length
                    ?

                    items.map(
                        item => `

                        <div
                        class="border rounded-2xl p-4 flex items-center gap-3">

                            <div
                            class="w-11 h-11 bg-green-50 text-green-600 rounded-xl flex items-center justify-center shrink-0">

                                <i data-lucide="package"></i>

                            </div>


                            <div class="flex-1 min-w-0">

                                <p class="font-bold truncate">

                                    ${escapeHTML(
                                        item.product_name
                                    )}

                                </p>


                                <p class="text-xs text-gray-400 mt-1">

                                    السعر:
                                    ${Number(
                                        item.price
                                    ).toFixed(2)}
                                    ج.م

                                    ×

                                    ${item.quantity}

                                </p>

                            </div>


                            <strong class="text-green-600">

                                ${(
                                    Number(
                                        item.price
                                    ) *
                                    Number(
                                        item.quantity
                                    )
                                ).toFixed(2)}

                                ج.م

                            </strong>

                        </div>

                    `
                    ).join("")

                    :

                    `

                    <div class="text-center p-8 text-gray-400">

                        لا توجد تفاصيل للمنتجات

                    </div>

                    `
                }

            </div>

        </div>


        <div
        class="mt-5 bg-green-50 rounded-2xl p-5 flex justify-between items-center">

            <span class="font-black">
                إجمالي الطلب
            </span>

            <strong
            class="text-2xl text-green-600 font-black">

                ${Number(
                    order.total_amount || 0
                ).toFixed(2)}

                ج.م

            </strong>

        </div>


        <button
        onclick="deleteOrder(${order.id})"
        class="w-full mt-4 bg-red-50 text-red-600 py-3 rounded-xl font-bold">

            حذف الطلب

        </button>

    `;


    const modal =
    document.getElementById(
        "orderModal"
    );


    modal.classList.remove(
        "hidden"
    );

    modal.classList.add(
        "flex"
    );


    lucide.createIcons();
}


/* =========================
   CLOSE ORDER
========================= */

function closeOrderModal() {

    const modal =
    document.getElementById(
        "orderModal"
    );


    modal.classList.add(
        "hidden"
    );

    modal.classList.remove(
        "flex"
    );
}


/* =========================
   CHANGE STATUS
========================= */

async function changeOrderStatus(
    id,
    status
) {

    const result =
    await db
    .from("orders")
    .update({
        status: status
    })
    .eq(
        "id",
        id
    );


    if (result.error) {

        alert(
            "لم يتم تحديث الحالة:\n"
            + result.error.message
        );

        return;
    }


    const order =
    orders.find(
        item =>
        Number(item.id)
        === Number(id)
    );


    if (order) {
        order.status = status;
    }


    renderOrders();
    renderLatestOrders();
}


/* =========================
   DELETE ORDER
========================= */

async function deleteOrder(id) {

    if (
        !confirm(
            "هل أنت متأكد من حذف هذا الطلب؟"
        )
    ) {
        return;
    }


    const result =
    await db
    .from("orders")
    .delete()
    .eq(
        "id",
        id
    );


    if (result.error) {

        alert(
            "لم يتم حذف الطلب:\n"
            + result.error.message
        );

        return;
    }


    closeOrderModal();

    await loadOrders();

    updateStats();
}


/* =========================
   CATEGORIES
========================= */

function renderCategories() {

    const box =
    document.getElementById(
        "categoriesList"
    );


    if (!categories.length) {

        box.innerHTML = `

            <div class="col-span-full bg-white rounded-2xl p-10 text-center text-gray-400">

                لا توجد أقسام

            </div>

        `;

        return;
    }


    box.innerHTML =
    categories
    .map(
        category => `

        <div class="card bg-white border rounded-2xl p-5">

            <div class="flex items-start justify-between">

                <div
                class="w-12 h-12 rounded-xl bg-green-50 text-green-600 flex items-center justify-center">

                    <i data-lucide="${category.icon || "package"}"></i>

                </div>


                <div class="flex gap-2">

                    <button
                    onclick="editCategory(${category.id})"
                    class="w-9 h-9 rounded-lg bg-gray-100 flex items-center justify-center">

                        <i
                        data-lucide="pencil"
                        class="w-4 h-4"></i>

                    </button>


                    <button
                    onclick="deleteCategory(${category.id})"
                    class="w-9 h-9 rounded-lg bg-red-50 text-red-600 flex items-center justify-center">

                        <i
                        data-lucide="trash-2"
                        class="w-4 h-4"></i>

                    </button>

                </div>

            </div>


            <h3 class="font-black text-lg mt-4">

                ${escapeHTML(
                    category.name
                )}

            </h3>


            <p class="text-sm text-gray-400 mt-1">

                ${escapeHTML(
                    category.description ||
                    "بدون وصف"
                )}

            </p>

        </div>

    `
    )
    .join("");


    lucide.createIcons();
}


/* =========================
   CATEGORY MODAL
========================= */

function openCategoryModal() {

    document.getElementById(
        "categoryId"
    ).value = "";


    document.getElementById(
        "categoryName"
    ).value = "";


    document.getElementById(
        "categoryDescription"
    ).value = "";


    document.getElementById(
        "categoryIcon"
    ).value = "package";


    document.getElementById(
        "categoryModalTitle"
    ).innerText =
    "إضافة قسم";


    openModal(
        "categoryModal"
    );
}


function closeCategoryModal() {

    closeModal(
        "categoryModal"
    );
}


/* =========================
   EDIT CATEGORY
========================= */

function editCategory(id) {

    const category =
    categories.find(
        item =>
        Number(item.id)
        === Number(id)
    );


    if (!category) return;


    document.getElementById(
        "categoryId"
    ).value =
    category.id;


    document.getElementById(
        "categoryName"
    ).value =
    category.name;


    document.getElementById(
        "categoryDescription"
    ).value =
    category.description || "";


    document.getElementById(
        "categoryIcon"
    ).value =
    category.icon || "package";


    document.getElementById(
        "categoryModalTitle"
    ).innerText =
    "تعديل القسم";


    openModal(
        "categoryModal"
    );
}


/* =========================
   SAVE CATEGORY
========================= */

async function saveCategory() {

    const id =
    document.getElementById(
        "categoryId"
    ).value;


    const name =
    document.getElementById(
        "categoryName"
    )
    .value
    .trim();


    const description =
    document.getElementById(
        "categoryDescription"
    )
    .value
    .trim();


    const icon =
    document.getElementById(
        "categoryIcon"
    )
    .value
    .trim()
    ||
    "package";


    if (!name) {

        alert(
            "اكتب اسم القسم"
        );

        return;
    }


    const data = {

        name: name,

        description:
        description || null,

        icon: icon

    };


    let result;


    if (id) {

        result =
        await db
        .from("categories")
        .update(data)
        .eq(
            "id",
            id
        );

    } else {

        result =
        await db
        .from("categories")
        .insert(data);

    }


    if (result.error) {

        alert(
            "لم يتم حفظ القسم:\n"
            + result.error.message
        );

        return;
    }


    closeCategoryModal();

    await loadCategories();

    updateStats();
}


/* =========================
   DELETE CATEGORY
========================= */

async function deleteCategory(id) {

    if (
        !confirm(
            "هل تريد حذف هذا القسم؟"
        )
    ) {
        return;
    }


    const result =
    await db
    .from("categories")
    .delete()
    .eq(
        "id",
        id
    );


    if (result.error) {

        alert(
            "لم يتم حذف القسم:\n"
            + result.error.message
        );

        return;
    }


    await loadCategories();

    await loadProducts();

    updateStats();
}


/* =========================
   PRODUCTS
========================= */

function renderProducts() {

    const box =
    document.getElementById(
        "productsList"
    );


    if (!products.length) {

        box.innerHTML = `

            <div class="col-span-full bg-white rounded-2xl p-12 text-center text-gray-400">

                لا توجد منتجات

            </div>

        `;

        return;
    }


    box.innerHTML =
    products
    .map(
        product => {

            const category =
            categories.find(
                item =>
                Number(item.id)
                ===
                Number(
                    product.category_id
                )
            );


            return `

                <div class="card bg-white border rounded-2xl overflow-hidden">

                    <div class="relative">

                        <img
                        src="${product.image || "https://placehold.co/600x450?text=Product"}"
                        class="w-full h-48 object-cover"
                        onerror="this.src='https://placehold.co/600x450?text=Product'">


                        <span
                        class="absolute top-3 right-3 bg-white/90 px-3 py-1 rounded-full text-xs font-bold">

                            ${
                                category
                                ?
                                escapeHTML(
                                    category.name
                                )
                                :
                                "بدون قسم"
                            }

                        </span>

                    </div>


                    <div class="p-4">

                        <h3 class="font-black">

                            ${escapeHTML(
                                product.name
                            )}

                        </h3>


                        <p class="text-sm text-gray-400 mt-1">

                            ${escapeHTML(
                                product.description || ""
                            )}

                        </p>


                        <div class="flex justify-between items-center mt-4">

                            <strong class="text-green-600 text-lg">

                                ${Number(
                                    product.price || 0
                                ).toFixed(2)}

                                ج.م

                            </strong>


                            <span class="text-xs text-gray-400">

                                المخزون:
                                ${product.stock || 0}

                            </span>

                        </div>


                        <div class="grid grid-cols-2 gap-2 mt-4">

                            <button
                            onclick="editProduct(${product.id})"
                            class="bg-gray-100 py-2.5 rounded-xl font-bold">

                                تعديل

                            </button>


                            <button
                            onclick="deleteProduct(${product.id})"
                            class="bg-red-50 text-red-600 py-2.5 rounded-xl font-bold">

                                حذف

                            </button>

                        </div>

                    </div>

                </div>

            `;

        }
    )
    .join("");


    lucide.createIcons();
}


/* =========================
   PRODUCT CATEGORIES
========================= */

function fillProductCategories() {

    const select =
    document.getElementById(
        "productCategory"
    );


    select.innerHTML = `

        <option value="">
            اختر القسم
        </option>

        ${
            categories
            .map(
                category =>
                `
                <option value="${category.id}">

                    ${escapeHTML(
                        category.name
                    )}

                </option>
                `
            )
            .join("")
        }

    `;
}


/* =========================
   PRODUCT MODAL
========================= */

function openProductModal() {

    document.getElementById(
        "productId"
    ).value = "";


    document.getElementById(
        "productName"
    ).value = "";


    document.getElementById(
        "productDescription"
    ).value = "";


    document.getElementById(
        "productPrice"
    ).value = "";


    document.getElementById(
        "productStock"
    ).value = "0";


    document.getElementById(
        "productImage"
    ).value = "";


    document.getElementById(
        "productCategory"
    ).value = "";


    document.getElementById(
        "productModalTitle"
    ).innerText =
    "إضافة منتج";


    openModal(
        "productModal"
    );
}


function closeProductModal() {

    closeModal(
        "productModal"
    );
}


/* =========================
   EDIT PRODUCT
========================= */

function editProduct(id) {

    const product =
    products.find(
        item =>
        Number(item.id)
        === Number(id)
    );


    if (!product) return;


    document.getElementById(
        "productId"
    ).value =
    product.id;


    document.getElementById(
        "productName"
    ).value =
    product.name;


    document.getElementById(
        "productDescription"
    ).value =
    product.description || "";


    document.getElementById(
        "productPrice"
    ).value =
    product.price;


    document.getElementById(
        "productStock"
    ).value =
    product.stock || 0;


    document.getElementById(
        "productImage"
    ).value =
    product.image || "";


    document.getElementById(
        "productCategory"
    ).value =
    product.category_id || "";


    document.getElementById(
        "productModalTitle"
    ).innerText =
    "تعديل المنتج";


    openModal(
        "productModal"
    );
}


/* =========================
   SAVE PRODUCT
========================= */

async function saveProduct() {

    const id =
    document.getElementById(
        "productId"
    ).value;


    const name =
    document.getElementById(
        "productName"
    )
    .value
    .trim();


    const category =
    document.getElementById(
        "productCategory"
    ).value;


    const description =
    document.getElementById(
        "productDescription"
    )
    .value
    .trim();


    const price =
    Number(
        document.getElementById(
            "productPrice"
        ).value
    );


    const stock =
    Number(
        document.getElementById(
            "productStock"
        ).value
    );


    const image =
    document.getElementById(
        "productImage"
    )
    .value
    .trim();


    if (!name) {

        alert(
            "اكتب اسم المنتج"
        );

        return;
    }


    if (!category) {

        alert(
            "اختر القسم"
        );

        return;
    }


    if (isNaN(price)) {

        alert(
            "اكتب السعر"
        );

        return;
    }


    const data = {

        category_id:
        Number(category),

        name: name,

        description:
        description || null,

        price: price,

        image:
        image || null,

        stock: stock,

        is_active: true

    };


    let result;


    if (id) {

        result =
        await db
        .from("products")
        .update(data)
        .eq(
            "id",
            id
        );

    } else {

        result =
        await db
        .from("products")
        .insert(data);

    }


    if (result.error) {

        alert(
            "لم يتم حفظ المنتج:\n"
            + result.error.message
        );

        return;
    }


    closeProductModal();

    await loadProducts();

    updateStats();
}


/* =========================
   DELETE PRODUCT
========================= */

async function deleteProduct(id) {

    if (
        !confirm(
            "هل تريد حذف هذا المنتج؟"
        )
    ) {
        return;
    }


    const result =
    await db
    .from("products")
    .delete()
    .eq(
        "id",
        id
    );


    if (result.error) {

        alert(
            "لم يتم حذف المنتج:\n"
            + result.error.message
        );

        return;
    }


    await loadProducts();

    updateStats();
}


/* =========================
   NAVIGATION
========================= */

function showPage(page) {

    const pages = [
        "dashboard",
        "orders",
        "products",
        "categories"
    ];


    pages.forEach(
        item => {

            document
            .getElementById(
                "page-" + item
            )
            .classList.add(
                "hidden"
            );

        }
    );


    document
    .getElementById(
        "page-" + page
    )
    .classList.remove(
        "hidden"
    );


    const titles = {

        dashboard: "الرئيسية",

        orders: "الطلبات",

        products: "المنتجات",

        categories: "الأقسام"

    };


    document.getElementById(
        "pageTitle"
    ).innerText =
    titles[page];


    closeSidebar();

    lucide.createIcons();
}


/* =========================
   SIDEBAR MOBILE
========================= */

function toggleSidebar() {

    document
    .getElementById(
        "sidebar"
    )
    .classList.toggle(
        "open"
    );


    document
    .getElementById(
        "sidebarOverlay"
    )
    .classList.toggle(
        "hidden"
    );
}


function closeSidebar() {

    document
    .getElementById(
        "sidebar"
    )
    .classList.remove(
        "open"
    );


    document
    .getElementById(
        "sidebarOverlay"
    )
    .classList.add(
        "hidden"
    );
}


/* =========================
   MODALS
========================= */

function openModal(id) {

    const modal =
    document.getElementById(
        id
    );


    modal.classList.remove(
        "hidden"
    );


    modal.classList.add(
        "flex"
    );
}


function closeModal(id) {

    const modal =
    document.getElementById(
        id
    );


    modal.classList.add(
        "hidden"
    );


    modal.classList.remove(
        "flex"
    );
}


/* =========================
   HELPERS
========================= */

function formatDate(date) {

    return date.toLocaleString(
        "ar-EG",
        {
            year: "numeric",
            month: "short",
            day: "numeric",
            hour: "2-digit",
            minute: "2-digit"
        }
    );
}


function escapeHTML(value) {

    return String(
        value ?? ""
    )
    .replaceAll("&", "&amp;")
    .replaceAll("<", "&lt;")
    .replaceAll(">", "&gt;")
    .replaceAll('"', "&quot;")
    .replaceAll("'", "&#039;");

}

</script>

</body>
</html>
