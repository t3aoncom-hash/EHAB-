<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>لوحة التحكم - الشيخ إيهاب</title>

<script src="https://cdn.tailwindcss.com"></script>
<script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2"></script>
<script src="https://unpkg.com/lucide@latest"></script>

<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Tajawal:wght@400;500;600;700;800;900&display=swap" rel="stylesheet">

<style>
* {
    box-sizing: border-box;
}

body {
    font-family: "Tajawal", sans-serif;
    background: #f5f7f6;
}

.sidebar {
    transition: .25s ease;
}

.card {
    transition: .2s ease;
}

.card:hover {
    transform: translateY(-2px);
}

.modal {
    animation: show .2s ease;
}

@keyframes show {
    from {
        opacity: 0;
        transform: translateY(10px);
    }

    to {
        opacity: 1;
        transform: translateY(0);
    }
}

::-webkit-scrollbar {
    width: 7px;
}

::-webkit-scrollbar-thumb {
    background: #16a34a;
    border-radius: 20px;
}

@media (max-width: 767px) {

    .sidebar {
        transform: translateX(100%);
        position: fixed;
        z-index: 70;
    }

    .sidebar.open {
        transform: translateX(0);
    }
}
</style>
</head>

<body>

<!-- =========================
     SIDEBAR
========================= -->

<aside
id="sidebar"
class="sidebar fixed right-0 top-0 bottom-0 w-72 bg-white border-l border-gray-200 flex flex-col">

    <div class="p-6 border-b">

        <div class="flex items-center gap-3">

            <div class="w-12 h-12 rounded-2xl bg-green-600 text-white flex items-center justify-center">
                <i data-lucide="shopping-bag"></i>
            </div>

            <div>

                <h1 class="font-black text-xl">
                    الشيخ <span class="text-green-600">إيهاب</span>
                </h1>

                <p class="text-xs text-gray-400">
                    لوحة التحكم
                </p>

            </div>

        </div>

    </div>


    <nav class="p-4 space-y-2 flex-1">

        <button
        onclick="showPage('dashboard')"
        class="w-full flex items-center gap-3 px-4 py-3 rounded-xl bg-green-50 text-green-700 font-bold">

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
