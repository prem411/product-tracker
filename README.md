```html
<!DOCTYPE html>
<html lang="bn">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>প্রোডাক্ট প্রাইস ও ইনফো ট্র্যাকার</title>
  <!-- Tailwind CSS -->
  <script src="https://cdn.tailwindcss.com"></script>
  <!-- Font Awesome Icons -->
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
  <!-- Google Fonts (Hind Siliguri for Bengali typography) -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Hind+Siliguri:wght@300;400;500;600;700&display=swap" rel="stylesheet">

  <style>
    body {
      font-family: 'Hind Siliguri', sans-serif;
      background-color: #f8fafc;
    }
    .glass-card {
      background: rgba(255, 255, 255, 0.9);
      backdrop-filter: blur(10px);
    }
    /* Custom Scrollbar */
    ::-webkit-scrollbar {
      width: 6px;
    }
    ::-webkit-scrollbar-track {
      background: #f1f5f9;
    }
    ::-webkit-scrollbar-thumb {
      background: #cbd5e1;
      border-radius: 4px;
    }
    ::-webkit-scrollbar-thumb:hover {
      background: #94a3b8;
    }
  </style>
</head>
<body class="text-slate-800 min-h-screen flex flex-col justify-between">

  <!-- Navigation Bar -->
  <header class="bg-gradient-to-r from-emerald-600 to-teal-700 text-white shadow-lg sticky top-0 z-30">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-4 flex justify-between items-center">
      <div class="flex items-center space-x-3 space-x-reverse">
        <div class="bg-white/20 p-2.5 rounded-xl backdrop-blur-md">
          <i class="fa-solid me-1 fa-tags text-2xl text-emerald-100"></i>
        </div>
        <div>
          <h1 class="text-xl sm:text-2xl font-bold tracking-wide">দাম ট্র্যাকার</h1>
          <p class="text-xs text-emerald-100 hidden sm:block">আপনার প্রয়োজনীয় সকল পণ্যের দাম ও তথ্য এক জায়গায়</p>
        </div>
      </div>
      <div class="flex items-center space-x-2 sm:space-x-3">
        <button onclick="openModal()" class="bg-white text-emerald-700 hover:bg-emerald-50 px-3 sm:px-4 py-2 rounded-xl text-sm font-semibold transition shadow flex items-center gap-1.5 active:scale-95">
          <i class="fa-solid fa-plus text-emerald-600"></i>
          <span>নতুন পণ্য</span>
        </button>
      </div>
    </div>
  </header>

  <!-- Main Content Container -->
  <main class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-6 w-full flex-grow">
    
    <!-- Top Summary Banner -->
    <div class="grid grid-cols-1 sm:grid-cols-3 gap-4 mb-6">
      <div class="bg-white p-5 rounded-2xl shadow-sm border border-slate-100 flex items-center justify-between">
        <div>
          <p class="text-xs font-medium text-slate-500 uppercase tracking-wider">মোট সংরক্ষিত পণ্য</p>
          <h3 id="stat-total-count" class="text-2xl font-bold text-slate-800 mt-1">০ টি</h3>
        </div>
        <div class="w-12 h-12 bg-emerald-50 text-emerald-600 rounded-xl flex items-center justify-center text-xl">
          <i class="fa-solid fa-boxes-stacked"></i>
        </div>
      </div>

      <div class="bg-white p-5 rounded-2xl shadow-sm border border-slate-100 flex items-center justify-between">
        <div>
          <p class="text-xs font-medium text-slate-500 uppercase tracking-wider">মোট আনুমানিক মূল্য</p>
          <h3 id="stat-total-value" class="text-2xl font-bold text-emerald-600 mt-1">৳ ০</h3>
        </div>
        <div class="w-12 h-12 bg-emerald-50 text-emerald-600 rounded-xl flex items-center justify-center text-xl">
          <i class="fa-solid fa-bangladeshi-taka-sign"></i>
        </div>
      </div>

      <div class="bg-white p-5 rounded-2xl shadow-sm border border-slate-100 flex items-center justify-between">
        <div>
          <p class="text-xs font-medium text-slate-500 uppercase tracking-wider">গড় মূল্য</p>
          <h3 id="stat-avg-price" class="text-2xl font-bold text-teal-600 mt-1">৳ ০</h3>
        </div>
        <div class="w-12 h-12 bg-teal-50 text-teal-600 rounded-xl flex items-center justify-center text-xl">
          <i class="fa-solid fa-calculator"></i>
        </div>
      </div>
    </div>

    <!-- Filter & Search Controls -->
    <div class="bg-white p-4 sm:p-5 rounded-2xl shadow-sm border border-slate-100 mb-6 flex flex-col md:flex-row gap-4 justify-between items-center">
      <!-- Search Input -->
      <div class="relative w-full md:w-96">
        <span class="absolute inset-y-0 left-0 flex items-center pl-3.5 pointer-events-none text-slate-400">
          <i class="fa-solid fa-magnifying-glass"></i>
        </span>
        <input type="text" id="search-input" oninput="filterAndRenderProducts()" placeholder="পণ্যের নাম বা নোট দিয়ে খুঁজুন..." 
          class="w-full pl-10 pr-4 py-2.5 text-sm bg-slate-50 border border-slate-200 rounded-xl focus:bg-white focus:outline-none focus:ring-2 focus:ring-emerald-500 focus:border-transparent transition">
      </div>

      <!-- Filters & Sorting -->
      <div class="flex flex-wrap items-center gap-3 w-full md:w-auto justify-between md:justify-end">
        <!-- Category Filter -->
        <select id="filter-category" onchange="filterAndRenderProducts()" class="py-2.5 px-3 text-sm bg-slate-50 border border-slate-200 rounded-xl focus:outline-none focus:ring-2 focus:ring-emerald-500 text-slate-700">
          <option value="ALL">সকল ক্যাটাগরি</option>
          <option value="মুদি পণ্য">মুদি পণ্য</option>
          <option value="ইলেকট্রনিক্স">ইলেকট্রনিক্স</option>
          <option value="পোশাক">পোশাক</option>
          <option value="মেডিসিন ও হেলথ">মেডিসিন ও হেলথ</option>
          <option value="গৃহস্থালি">গৃহস্থালি</option>
          <option value="অন্যান্য">অন্যান্য</option>
        </select>

        <!-- Sorting -->
        <select id="sort-order" onchange="filterAndRenderProducts()" class="py-2.5 px-3 text-sm bg-slate-50 border border-slate-200 rounded-xl focus:outline-none focus:ring-2 focus:ring-emerald-500 text-slate-700">
          <option value="NEWEST">সর্বশেষ যোগ করা</option>
          <option value="PRICE_LOW">দাম: কম থেকে বেশি</option>
          <option value="PRICE_HIGH">দাম: বেশি থেকে কম</option>
          <option value="NAME">নাম অনুসারে (A-Z)</option>
        </select>

        <!-- Extra Action Menu -->
        <div class="relative inline-block text-left">
          <button onclick="toggleExtraMenu()" class="p-2.5 bg-slate-100 hover:bg-slate-200 rounded-xl text-slate-600 transition" title="অতিরিক্ত অপশন">
            <i class="fa-solid fa-ellipsis-vertical px-1"></i>
          </button>
          <div id="extra-menu" class="hidden absolute right-0 mt-2 w-48 bg-white rounded-xl shadow-lg border border-slate-100 py-1 z-20">
            <button onclick="loadSampleData()" class="w-full text-left px-4 py-2 text-sm text-slate-700 hover:bg-slate-50 flex items-center gap-2">
              <i class="fa-solid fa-vial text-emerald-600"></i> ডেমো তথ্য লোড করুন
            </button>
            <button onclick="exportData()" class="w-full text-left px-4 py-2 text-sm text-slate-700 hover:bg-slate-50 flex items-center gap-2">
              <i class="fa-solid fa-file-export text-blue-600"></i> ব্যাকআপ নামান (Export)
            </button>
            <label class="w-full text-left px-4 py-2 text-sm text-slate-700 hover:bg-slate-50 flex items-center gap-2 cursor-pointer">
              <i class="fa-solid fa-file-import text-amber-600"></i> ব্যাকআপ আনুন (Import)
              <input type="file" id="import-file" accept=".json" class="hidden" onchange="importData(event)">
            </label>
            <hr class="my-1 border-slate-100">
            <button onclick="confirmClearAll()" class="w-full text-left px-4 py-2 text-sm text-red-600 hover:bg-red-50 flex items-center gap-2">
              <i class="fa-solid fa-trash-can"></i> সব মুছে ফেলুন
            </button>
          </div>
        </div>
      </div>
    </div>

    <!-- Product Grid Section -->
    <div id="product-grid" class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-5">
      <!-- Product Cards will be rendered dynamically via JavaScript -->
    </div>

    <!-- Empty State Container -->
    <div id="empty-state" class="hidden text-center py-16 px-4 bg-white rounded-2xl border border-dashed border-slate-300">
      <div class="w-20 h-20 bg-emerald-50 text-emerald-500 rounded-full flex items-center justify-center mx-auto mb-4 text-3xl">
        <i class="fa-solid fa-basket-shopping"></i>
      </div>
      <h3 class="text-lg font-semibold text-slate-700">কোনো পণ্য খুঁজে পাওয়া যায়নি!</h3>
      <p class="text-slate-500 text-sm mt-1 max-w-md mx-auto">নতুন পণ্য যোগ করতে উপরের "নতুন পণ্য" বোতামে চাপ দিন অথবা আপনার ফিল্টার পরিবর্তন করুন।</p>
      <button onclick="openModal()" class="mt-4 bg-emerald-600 hover:bg-emerald-700 text-white px-5 py-2.5 rounded-xl text-sm font-medium transition shadow">
        <i class="fa-solid fa-plus me-1"></i> প্রথম পণ্য যোগ করুন
      </button>
    </div>

  </main>

  <!-- Footer -->
  <footer class="bg-white border-t border-slate-200 py-4 mt-8 text-center text-xs text-slate-500">
    <div class="max-w-7xl mx-auto px-4">
      <p>প্রোডাক্ট প্রাইস ও ইনফো ট্র্যাকার &copy; ২০২৬ | আপনার ব্রাউজারে তথ্য নিরাপদে সংরক্ষিত থাকে।</p>
    </div>
  </footer>

  <!-- Add/Edit Product Modal -->
  <div id="product-modal" class="fixed inset-0 bg-slate-900/60 backdrop-blur-sm hidden flex items-center justify-center p-4 z-50">
    <div class="bg-white rounded-2xl shadow-xl w-full max-w-lg overflow-hidden transform transition-all">
      <!-- Modal Header -->
      <div class="bg-slate-50 px-6 py-4 border-b border-slate-100 flex justify-between items-center">
        <h3 id="modal-title" class="text-lg font-bold text-slate-800 flex items-center gap-2">
          <i class="fa-solid fa-square-plus text-emerald-600"></i> নতুন পণ্য যোগ করুন
        </h3>
        <button onclick="closeModal()" class="text-slate-400 hover:text-slate-600 p-1.5 rounded-lg hover:bg-slate-100 transition">
          <i class="fa-solid fa-xmark text-lg"></i>
        </button>
      </div>

      <!-- Modal Form -->
      <form id="product-form" onsubmit="handleFormSubmit(event)" class="p-6 space-y-4">
        <input type="hidden" id="product-id">

        <!-- Product Name -->
        <div>
          <label class="block text-sm font-medium text-slate-700 mb-1">প্রোডাক্টের নাম <span class="text-red-500">*</span></label>
          <input type="text" id="prod-name" required placeholder="যেমন: মিনিকেট চাল (৫ কেজি)" 
            class="w-full px-3.5 py-2.5 text-sm bg-slate-50 border border-slate-200 rounded-xl focus:bg-white focus:outline-none focus:ring-2 focus:ring-emerald-500 transition">
        </div>

        <!-- Price & Category Grid -->
        <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
          <!-- Price -->
          <div>
            <label class="block text-sm font-medium text-slate-700 mb-1">দাম (৳ টাকা) <span class="text-red-500">*</span></label>
            <div class="relative">
              <span class="absolute inset-y-0 left-0 pl-3.5 flex items-center text-slate-400 text-sm">৳</span>
              <input type="number" id="prod-price" min="0" step="any" required placeholder="৩৫০" 
                class="w-full pl-8 pr-3.5 py-2.5 text-sm bg-slate-50 border border-slate-200 rounded-xl focus:bg-white focus:outline-none focus:ring-2 focus:ring-emerald-500 transition">
            </div>
          </div>

          <!-- Category -->
          <div>
            <label class="block text-sm font-medium text-slate-700 mb-1">ক্যাটাগরি</label>
            <select id="prod-category" class="w-full px-3.5 py-2.5 text-sm bg-slate-50 border border-slate-200 rounded-xl focus:bg-white focus:outline-none focus:ring-2 focus:ring-emerald-500 transition">
              <option value="মুদি পণ্য">মুদি পণ্য</option>
              <option value="ইলেকট্রনিক্স">ইলেকট্রনিক্স</option>
              <option value="পোশাক">পোশাক</option>
              <option value="মেডিসিন ও হেলথ">মেডিসিন ও হেলথ</option>
              <option value="গৃহস্থালি">গৃহস্থালি</option>
              <option value="অন্যান্য">অন্যান্য</option>
            </select>
          </div>
        </div>

        <!-- Image Input Selection -->
        <div>
          <label class="block text-sm font-medium text-slate-700 mb-1">ছবি যোগ করুন</label>
          <div class="flex gap-2 mb-2 text-xs">
            <button type="button" id="tab-upload" onclick="switchImageTab('upload')" class="px-3 py-1.5 rounded-lg bg-emerald-100 text-emerald-800 font-medium">ফাইল আপলোড</button>
            <button type="button" id="tab-url" onclick="switchImageTab('url')" class="px-3 py-1.5 rounded-lg bg-slate-100 text-slate-600 font-medium">ওয়েব ইউআরএল (URL)</button>
          </div>

          <!-- File Upload Option -->
          <div id="img-upload-container" class="space-y-2">
            <input type="file" id="prod-file" accept="image/*" onchange="previewFileImage(event)" 
              class="w-full text-xs text-slate-500 file:mr-3 file:py-2 file:px-4 file:rounded-xl file:border-0 file:text-xs file:font-semibold file:bg-emerald-50 file:text-emerald-700 hover:file:bg-emerald-100">
          </div>

          <!-- URL Input Option -->
          <div id="img-url-container" class="hidden">
            <input type="url" id="prod-url" oninput="previewUrlImage()" placeholder="https://example.com/image.jpg" 
              class="w-full px-3.5 py-2.5 text-sm bg-slate-50 border border-slate-200 rounded-xl focus:bg-white focus:outline-none focus:ring-2 focus:ring-emerald-500 transition">
          </div>

          <!-- Image Preview -->
          <div class="mt-2 flex items-center gap-3">
            <div class="w-16 h-16 rounded-xl border border-slate-200 bg-slate-50 flex items-center justify-center overflow-hidden flex-shrink-0">
              <img id="img-preview" src="" alt="প্রিভিউ" class="hidden w-full h-full object-cover">
              <i id="img-preview-icon" class="fa-regular fa-image text-slate-300 text-2xl"></i>
            </div>
            <p class="text-xs text-slate-400">ছবি না দিলে ডিফল্ট ছবি ব্যবহৃত হবে।</p>
          </div>
        </div>

        <!-- Notes / Description -->
        <div>
          <label class="block text-sm font-medium text-slate-700 mb-1">নোট / অতিরিক্ত তথ্য (ঐচ্ছিক)</label>
          <textarea id="prod-note" rows="2" placeholder="কোথা থেকে কেনা, ব্র্যান্ডের নাম বা বিশেষ কোনো বার্তা..." 
            class="w-full px-3.5 py-2 text-sm bg-slate-50 border border-slate-200 rounded-xl focus:bg-white focus:outline-none focus:ring-2 focus:ring-emerald-500 transition"></textarea>
        </div>

        <!-- Modal Actions -->
        <div class="pt-3 flex justify-end gap-2 border-t border-slate-100">
          <button type="button" onclick="closeModal()" class="px-4 py-2.5 text-sm font-medium text-slate-600 bg-slate-100 hover:bg-slate-200 rounded-xl transition">বাতিল</button>
          <button type="submit" class="px-5 py-2.5 text-sm font-medium text-white bg-emerald-600 hover:bg-emerald-700 rounded-xl transition shadow">সংরক্ষণ করুন</button>
        </div>
      </form>
    </div>
  </div>

  <!-- Delete Confirmation Modal -->
  <div id="delete-modal" class="fixed inset-0 bg-slate-900/60 backdrop-blur-sm hidden flex items-center justify-center p-4 z-50">
    <div class="bg-white rounded-2xl shadow-xl w-full max-w-sm p-6 text-center">
      <div class="w-14 h-14 bg-red-100 text-red-500 rounded-full flex items-center justify-center mx-auto mb-4 text-2xl">
        <i class="fa-solid fa-triangle-exclamation"></i>
      </div>
      <h3 class="text-lg font-bold text-slate-800">আপনি কি নিশ্চিত?</h3>
      <p class="text-sm text-slate-500 mt-2">এই পণ্যটি তালিকা থেকে চিরতরে মুছে ফেলা হবে।</p>
      <div class="mt-6 flex justify-center gap-3">
        <button onclick="closeDeleteModal()" class="px-4 py-2 text-sm font-medium text-slate-600 bg-slate-100 hover:bg-slate-200 rounded-xl transition">বাতিল</button>
        <button id="confirm-delete-btn" class="px-4 py-2 text-sm font-medium text-white bg-red-600 hover:bg-red-700 rounded-xl transition">হ্যাঁ, মুছে ফেলুন</button>
      </div>
    </div>
  </div>

  <!-- Notification Toast -->
  <div id="toast" class="fixed bottom-5 right-5 bg-slate-800 text-white px-4 py-3 rounded-xl shadow-lg flex items-center gap-3 transform translate-y-20 opacity-0 transition-all duration-300 z-50 pointer-events-none">
    <i id="toast-icon" class="fa-solid fa-circle-check text-emerald-400 text-lg"></i>
    <span id="toast-msg" class="text-sm font-medium">মেসেজ</span>
  </div>

  <!-- JavaScript Logic -->
  <script>
    // LocalStorage Key
    const STORAGE_KEY = 'bangla_product_tracker_data';

    // Global State
    let products = [];
    let currentImageBase64 = '';
    let itemToDeleteId = null;

    // Default Sample Data
    const sampleProducts = [
      {
        id: '1',
        name: 'মিনিকেট চাল (৫ কেজি)',
        price: 360,
        category: 'মুদি পণ্য',
        image: 'https://images.unsplash.com/photo-1586201375761-83865001e31c?w=400&q=80',
        note: 'স্বপ্নে ছাড়ের সময়ে কেনা ভাল।',
        createdAt: Date.now() - 3600000 * 24 * 2
      },
      {
        id: '2',
        name: 'ওয়্যারলেস ইয়ারবডস',
        price: 1850,
        category: 'ইলেকট্রনিক্স',
        image: 'https://images.unsplash.com/photo-1590658268037-6bf12165a8df?w=400&q=80',
        note: 'ব্যাস খুবই ভালো, ব্যাটারি ব্যাকআপ ৬ ঘণ্টা।',
        createdAt: Date.now() - 3600000 * 24
      },
      {
        id: '3',
        name: 'সুতি পাঞ্জাবি (এল সাইজ)',
        price: 1450,
        category: 'পোশাক',
        image: 'https://images.unsplash.com/photo-1602810318383-e386cc2a3ccf?w=400&q=80',
        note: 'আড়ং বা আড়ং এর আউটলেট থেকে দেখা।',
        createdAt: Date.now()
      }
    ];

    // Default Fallback Image
    const DEFAULT_IMAGE = 'https://images.unsplash.com/photo-1546069901-ba9599a7e63c?w=400&q=80';

    // Initialize Application
    window.onload = function() {
      loadProductsFromStorage();
    };

    // Load Data from LocalStorage
    function loadProductsFromStorage() {
      const stored = localStorage.getItem(STORAGE_KEY);
      if (stored) {
        try {
          products = JSON.parse(stored);
        } catch (e) {
          products = [...sampleProducts];
        }
      } else {
        products = [...sampleProducts];
        saveProductsToStorage();
      }
      filterAndRenderProducts();
    }

    // Save Data to LocalStorage
    function saveProductsToStorage() {
      localStorage.setItem(STORAGE_KEY, JSON.stringify(products));
    }

    // Numbers in Bengali conversion helper
    function toBengaliNumeral(num) {
      const bengaliDigits = ['০', '১', '২', '৩', '৪', '৫', '৬', '৭', '৮', '৯'];
      return num.toString().replace(/\d/g, d => bengaliDigits[d]);
    }

    // Format Currency
    function formatCurrency(amount) {
      const formatted = new Intl.NumberFormat('bn-BD').format(Math.round(amount));
      return '৳ ' + formatted;
    }

    // Render Products Grid & Update Statistics
    function filterAndRenderProducts() {
      const searchVal = document.getElementById('search-input').value.trim().toLowerCase();
      const catVal = document.getElementById('filter-category').value;
      const sortVal = document.getElementById('sort-order').value;

      // Filter
      let filtered = products.filter(item => {
        const matchesSearch = item.name.toLowerCase().includes(searchVal) || 
                              (item.note && item.note.toLowerCase().includes(searchVal));
        const matchesCat = (catVal === 'ALL') || (item.category === catVal);
        return matchesSearch && matchesCat;
      });

      // Sort
      if (sortVal === 'PRICE_LOW') {
        filtered.sort((a, b) => a.price - b.price);
      } else if (sortVal === 'PRICE_HIGH') {
        filtered.sort((a, b) => b.price - a.price);
      } 
