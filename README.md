<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Aurum Boutique | Luxury E-commerce</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://unpkg.com/lucide@latest"></script>
    <link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@700&family=Inter:wght@300;400;600&display=swap" rel="stylesheet">
    <style>
        :root {
            --primary: #000000;
            --accent: #d4af37; /* Gold accent for luxury feel */
            --bg-light: #fdfdfd;
        }
        /* Updated Menu Theme */
        #menu-overlay {
            background-color: #0a0a0a;
            color: #ffffff;
        }
        
        .menu-link {
            position: relative;
            transition: all 0.3s ease;
        }
        
        .menu-link::after {
            content: '';
            position: absolute;
            bottom: 0;
            left: 0;
            width: 0;
            height: 2px;
            background: var(--accent);
            transition: width 0.3s ease;
        }
        
        .menu-link:hover::after {
            width: 100%;
        }

        .payment-card {
            border: 2px solid transparent;
            transition: all 0.3s ease;
            cursor: pointer;
        }
        
        .payment-card.active {
            border-color: var(--accent);
            background-color: #fff;
            box-shadow: 0 4px 12px rgba(0,0,0,0.05);
        }

        .brand-name { font-family: 'Playfair Display', serif; }

        /* Custom Scrollbar */
        .no-scrollbar::-webkit-scrollbar { display: none; }
        .no-scrollbar { -ms-overflow-style: none; scrollbar-width: none; }

        .btn-luxury {
            position: relative;
            overflow: hidden;
            transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
        }
        .btn-luxury:hover {
            transform: translateY(-2px);
            box-shadow: 0 10px 20px rgba(0,0,0,0.1);
        }

        .spinner {
            width: 18px;
            height: 18px;
            border: 2px solid rgba(255,255,255,0.3);
            border-top-color: #fff;
            border-radius: 50%;
            animation: spin 0.8s linear infinite;
        }
        @keyframes spin { to { transform: rotate(360deg); } }

        .drawer {
            transition: transform 0.5s cubic-bezier(0.32, 0.72, 0, 1);
        }

        .product-card-container {
            transition: all 0.5s cubic-bezier(0.2, 1, 0.3, 1);
            cursor: pointer;
        }

        .product-card-container:hover {
            transform: translateY(-15px) scale(1.02);
            box-shadow: 0 30px 60px -12px rgba(0, 0, 0, 0.15);
            z-index: 10;
        }

        .product-card img {
            transition: transform 0.7s cubic-bezier(0.4, 0, 0.2, 1);
        }
        .product-card:hover img {
            transform: scale(1.08);
        }

        .action-overlay {
            background: linear-gradient(to top, rgba(0,0,0,0.6) 0%, transparent 100%);
            opacity: 0;
            transition: all 0.4s ease;
            transform: translateY(10px);
        }

        .product-card-container:hover .action-overlay {
            opacity: 1;
            transform: translateY(0);
        }

        /* Overlay blur */
        .blur-overlay {
            backdrop-filter: blur(8px);
            background-color: rgba(255,255,255,0.8);
        }
    </style>
</head>
<body class="overflow-x-hidden">
    <!-- Full Menu Overlay (Single definition) -->
    <div id="menu-overlay" class="fixed inset-0 z-[200] translate-x-full drawer flex flex-col" style="background-color: #0a0a0a; color: #ffffff;">
        <div class="p-6 flex items-center justify-between border-b border-white/10">
            <span class="brand-name text-2xl text-white">Aurum.</span>
            <button onclick="toggleMenu(false)" class="p-2 hover:bg-white/10 rounded-full text-white transition-colors"><i data-lucide="x"></i></button>
        </div>
        <div class="flex-1 overflow-y-auto p-10 space-y-10">
            <div class="space-y-4">
                <p class="text-xs uppercase tracking-[0.3em] text-gray-500 font-bold">Experience</p>
                <a href="#" onclick="toggleMenu(false); showStore(); filterCategory('All')" class="menu-link block text-5xl font-bold hover:text-accent transition-colors">Shop All</a>
                <a href="#about" onclick="toggleMenu(false)" class="menu-link block text-5xl font-bold hover:text-accent transition-colors">Our Story</a>
                <a href="#contact" onclick="toggleMenu(false)" class="menu-link block text-5xl font-bold hover:text-accent transition-colors">Contact</a>
            </div>
            
            <div class="pt-10 border-t border-white/10">
                <p class="text-xs uppercase tracking-[0.3em] text-gray-500 mb-6">Collections</p>
                <div class="grid grid-cols-2 gap-y-4 text-xl">
                    <button onclick="showStore(); filterCategory('Apparel'); toggleMenu(false)" class="text-left hover:text-accent transition-colors">Apparel</button>
                    <button onclick="showStore(); filterCategory('Electronics'); toggleMenu(false)" class="text-left hover:text-accent transition-colors">Electronics</button>
                    <button onclick="showStore(); filterCategory('Accessories'); toggleMenu(false)" class="text-left hover:text-accent transition-colors">Accessories</button>
                    <button onclick="showStore(); filterCategory('Footwear'); toggleMenu(false)" class="text-left hover:text-accent transition-colors">Footwear</button>
                    <button onclick="showStore(); filterCategory('Home'); toggleMenu(false)" class="text-left hover:text-accent transition-colors">Home Decor</button>
                    <button onclick="showStore(); filterCategory('Beauty'); toggleMenu(false)" class="text-left hover:text-accent transition-colors">Beauty</button>
                </div>
            </div>
        </div>
    </div>

    <!-- Navigation Bar -->
    <nav class="fixed top-0 w-full z-[150] blur-overlay border-b border-gray-100 px-6 py-4 flex items-center justify-between">
        <div class="flex items-center gap-8">
            <button onclick="toggleMenu(true)" class="p-2 hover:bg-gray-100 rounded-full transition-colors">
                <i data-lucide="menu"></i>
            </button>
            <span class="brand-name text-3xl tracking-tighter cursor-pointer" onclick="showStore()">Aurum<span class="text-accent">.</span></span>
        </div>
        
        <div class="flex items-center gap-2 sm:gap-6">
            <button onclick="toggleSearch(true)" class="p-2 hover:bg-gray-100 rounded-full">
                <i data-lucide="search" class="w-5 h-5"></i>
            </button>
            <button onclick="toggleWishlist(true)" class="p-2 hover:bg-gray-100 rounded-full relative">
                <i data-lucide="heart" class="w-5 h-5"></i>
                <span id="wishlist-count" class="absolute -top-1 -right-1 bg-red-500 text-white text-[9px] w-4 h-4 rounded-full flex items-center justify-center hidden">0</span>
            </button>
            <button onclick="toggleCart(true)" class="p-2 hover:bg-gray-100 rounded-full relative">
                <i data-lucide="shopping-bag" class="w-5 h-5"></i>
                <span id="cart-count" class="absolute -top-1 -right-1 bg-black text-white text-[9px] w-4 h-4 rounded-full flex items-center justify-center hidden">0</span>
            </button>
        </div>
    </nav>

    <!-- Full Menu Overlay -->
    <div id="menu-overlay" class="fixed inset-0 z-[200] bg-white translate-x-full drawer flex flex-col">
        <div class="p-6 flex items-center justify-between border-b">
            <span class="brand-name text-2xl">Navigation</span>
            <button onclick="toggleMenu(false)" class="p-2 hover:bg-gray-100 rounded-full"><i data-lucide="x"></i></button>
        </div>
        <div class="flex-1 overflow-y-auto p-10 space-y-10">
            <div class="space-y-4">
                <p class="text-xs uppercase tracking-[0.3em] text-gray-400">Store</p>
                <a href="#" onclick="toggleMenu(false); showStore()" class="block text-5xl font-bold hover:text-gray-400 transition-colors">Shop All</a>
                <a href="#about" onclick="toggleMenu(false)" class="block text-5xl font-bold hover:text-gray-400 transition-colors">Our Story</a>
                <a href="#contact" onclick="toggleMenu(false)" class="block text-5xl font-bold hover:text-gray-400 transition-colors">Contact</a>
            </div>
            <div class="pt-10 border-t border-gray-100">
                <p class="text-xs uppercase tracking-[0.3em] text-gray-400 mb-6">Collections</p>
                <div class="grid grid-cols-2 gap-y-4 text-xl">
                    <button onclick="filterCategory('Apparel'); toggleMenu(false)" class="text-left hover:text-accent transition-colors">Apparel</button>
                    <button onclick="filterCategory('Electronics'); toggleMenu(false)" class="text-left hover:text-accent transition-colors">Electronics</button>
                    <button onclick="filterCategory('Accessories'); toggleMenu(false)" class="text-left hover:text-accent transition-colors">Accessories</button>
                    <button onclick="filterCategory('Footwear'); toggleMenu(false)" class="text-left hover:text-accent transition-colors">Footwear</button>
                    <button onclick="filterCategory('Home'); toggleMenu(false)" class="text-left hover:text-accent transition-colors">Home Decor</button>
                    <button onclick="filterCategory('Beauty'); toggleMenu(false)" class="text-left hover:text-accent transition-colors">Beauty</button>
                </div>
            </div>
        </div>
    </div>

    <!-- Search Overlay -->
    <div id="search-overlay" class="fixed inset-0 z-[250] bg-white translate-y-full drawer flex flex-col">
        <div class="p-6 flex items-center gap-4 border-b">
            <i data-lucide="search" class="w-6 h-6 text-gray-400"></i>
            <input type="text" id="search-input" placeholder="Search our collection..." class="flex-1 text-2xl outline-none" oninput="handleSearch(this.value)">
            <button onclick="toggleSearch(false)" class="p-2 hover:bg-gray-100 rounded-full"><i data-lucide="x"></i></button>
        </div>
        <div id="search-results" class="flex-1 overflow-y-auto p-8 grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-8">
            <!-- Dynamic search results here -->
        </div>
    </div>

    <main id="main-store" class="pt-20">
        <!-- Hero Banner -->
        <section class="relative h-[80vh] flex items-center justify-center overflow-hidden bg-black text-white">
            <img id="banner-img" src="https://images.unsplash.com/photo-1441986300917-64674bd600d8?auto=format&fit=crop&q=80&w=1600" class="absolute inset-0 w-full h-full object-cover opacity-60 transition-all duration-1000 scale-100">
            <div class="relative z-10 text-center px-4 max-w-4xl">
                <h1 id="banner-title" class="brand-name text-6xl md:text-9xl mb-8 opacity-0 translate-y-10 transition-all duration-1000">Excellence.</h1>
                <p class="text-lg md:text-xl font-light mb-12 tracking-[0.4em] uppercase">Defined by Quality, Inspired by You</p>
                <button onclick="document.getElementById('shop-section').scrollIntoView({behavior:'smooth'})" class="px-12 py-5 border border-white text-xs uppercase tracking-[0.3em] hover:bg-white hover:text-black transition-all duration-500 rounded-full">Explore Collection</button>
            </div>
        </section>

        <!-- Shop Section -->
        <section id="shop-section" class="max-w-7xl mx-auto px-4 py-24">
            <div class="flex flex-col md:flex-row md:items-end justify-between mb-16 gap-8">
                <div>
                    <span class="text-xs uppercase tracking-[0.4em] text-gray-400 mb-2 block">Curated Selection</span>
                    <h2 class="brand-name text-5xl">Our Products</h2>
                </div>
                <!-- WORKING CATEGORIES -->
                <div class="flex gap-4 overflow-x-auto no-scrollbar pb-2">
                    <button onclick="filterCategory('All')" class="cat-btn px-8 py-3 rounded-full border border-black text-xs font-bold uppercase tracking-widest bg-black text-white transition-all whitespace-nowrap">All</button>
                    <button onclick="filterCategory('Apparel')" class="cat-btn px-8 py-3 rounded-full border border-gray-200 text-xs font-bold uppercase tracking-widest hover:border-black transition-all whitespace-nowrap">Apparel</button>
                    <button onclick="filterCategory('Electronics')" class="cat-btn px-8 py-3 rounded-full border border-gray-200 text-xs font-bold uppercase tracking-widest hover:border-black transition-all whitespace-nowrap">Electronics</button>
                    <button onclick="filterCategory('Accessories')" class="cat-btn px-8 py-3 rounded-full border border-gray-200 text-xs font-bold uppercase tracking-widest hover:border-black transition-all whitespace-nowrap">Accessories</button>
                    <button onclick="filterCategory('Footwear')" class="cat-btn px-8 py-3 rounded-full border border-gray-200 text-xs font-bold uppercase tracking-widest hover:border-black transition-all whitespace-nowrap">Footwear</button>
                    <button onclick="filterCategory('Beauty')" class="cat-btn px-8 py-3 rounded-full border border-gray-200 text-xs font-bold uppercase tracking-widest hover:border-black transition-all whitespace-nowrap">Beauty</button>
                </div>
            </div>

            <!-- PRODUCT GRID -->
            <div id="product-grid" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-x-8 gap-y-16">
                <!-- Injected via JS -->
            </div>
        </section>

        <!-- About Section -->
        <section id="about" class="bg-gray-50 py-32 border-y border-gray-100">
            <div class="max-w-4xl mx-auto px-4 text-center">
                <span class="text-xs uppercase tracking-[0.5em] text-accent mb-6 block">Our Heritage</span>
                <h2 class="brand-name text-6xl mb-10 leading-tight">The Art of Pure Craftsmanship</h2>
                <p class="text-gray-500 leading-relaxed text-xl mb-12 font-light italic">"Aurum Boutique was founded to bridge the gap between temporary trends and timeless quality. Every item in our catalog is a testament to the skill of artisans worldwide."</p>
                <div class="grid grid-cols-1 md:grid-cols-3 gap-16 pt-10 border-t border-gray-200">
                    <div>
                        <div class="mb-6 flex justify-center"><i data-lucide="shield-check" class="w-10 h-10 text-black"></i></div>
                        <h4 class="font-bold text-lg mb-3">Authentic</h4>
                        <p class="text-sm text-gray-400">Every piece is verified by our in-house experts.</p>
                    </div>
                    <div>
                        <div class="mb-6 flex justify-center"><i data-lucide="globe" class="w-10 h-10 text-black"></i></div>
                        <h4 class="font-bold text-lg mb-3">Global</h4>
                        <p class="text-sm text-gray-400">Ethically sourced materials from across 5 continents.</p>
                    </div>
                    <div>
                        <div class="mb-6 flex justify-center"><i data-lucide="refresh-cw" class="w-10 h-10 text-black"></i></div>
                        <h4 class="font-bold text-lg mb-3">Sustainable</h4>
                        <p class="text-sm text-gray-400">Packaging and shipping designed with the planet in mind.</p>
                    </div>
                </div>
            </div>
        </section>

        <!-- NEW: Contact Section -->
        <section id="contact" class="py-32 bg-white">
            <div class="max-w-7xl mx-auto px-4">
                <div class="grid grid-cols-1 lg:grid-cols-2 gap-24">
                    <div>
                        <span class="text-xs uppercase tracking-[0.4em] text-accent font-bold mb-4 block">Get in Touch</span>
                        <h2 class="brand-name text-6xl mb-8">Visit Our Flagship Studio</h2>
                        <p class="text-gray-500 text-lg mb-12 max-w-md">Our concierge team is available to assist you with personalized styling, product inquiries, and bulk corporate gifting.</p>
                        
                        <div class="space-y-8">
                            <div class="flex items-start gap-6">
                                <div class="w-12 h-12 bg-gray-50 rounded-2xl flex items-center justify-center flex-shrink-0">
                                    <i data-lucide="map-pin" class="w-5 h-5"></i>
                                </div>
                                <div>
                                    <h4 class="font-bold mb-1">Islamabad Studio</h4>
                                    <p class="text-gray-400 text-sm">F-7 Markaz, Blue Area, Islamabad, Pakistan</p>
                                </div>
                            </div>
                            <div class="flex items-start gap-6">
                                <div class="w-12 h-12 bg-gray-50 rounded-2xl flex items-center justify-center flex-shrink-0">
                                    <i data-lucide="phone" class="w-5 h-5"></i>
                                </div>
                                <div>
                                    <h4 class="font-bold mb-1">Contact</h4>
                                    <p class="text-gray-400 text-sm">+92 51 123 4567<br>concierge@aurum.pk</p>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="bg-gray-50 p-10 rounded-[3rem] border border-gray-100">
                        <form class="space-y-6">
                            <div class="grid grid-cols-2 gap-6">
                                <div class="space-y-2">
                                    <label class="text-[10px] uppercase font-bold tracking-widest text-gray-400 px-2">Name</label>
                                    <input type="text" placeholder="John Doe" class="w-full p-4 rounded-2xl border-transparent border bg-white focus:border-accent outline-none transition-all">
                                </div>
                                <div class="space-y-2">
                                    <label class="text-[10px] uppercase font-bold tracking-widest text-gray-400 px-2">Email</label>
                                    <input type="email" placeholder="john@example.com" class="w-full p-4 rounded-2xl border-transparent border bg-white focus:border-accent outline-none transition-all">
                                </div>
                            </div>
                            <div class="space-y-2">
                                <label class="text-[10px] uppercase font-bold tracking-widest text-gray-400 px-2">Inquiry Type</label>
                                <select class="w-full p-4 rounded-2xl border-transparent border bg-white focus:border-accent outline-none transition-all appearance-none">
                                    <option>General Inquiry</option>
                                    <option>Order Support</option>
                                    <option>Bespoke Request</option>
                                </select>
                            </div>
                            <div class="space-y-2">
                                <label class="text-[10px] uppercase font-bold tracking-widest text-gray-400 px-2">Message</label>
                                <textarea rows="4" placeholder="How can we assist you?" class="w-full p-4 rounded-2xl border-transparent border bg-white focus:border-accent outline-none transition-all"></textarea>
                            </div>
                            <button type="button" class="w-full py-5 bg-black text-white rounded-2xl font-bold uppercase tracking-widest hover:bg-accent transition-all">Send Message</button>
                        </form>
                    </div>
                </div>
            </div>
        </section>
    </main>

    <!-- CHECKOUT VIEW (UPDATED PAYMENT) -->
    <section id="checkout-view" class="hidden pt-32 pb-24 px-4 min-h-screen bg-white">
        <div class="max-w-6xl mx-auto">
            <button onclick="showStore()" class="flex items-center gap-3 text-xs uppercase font-bold tracking-widest mb-12 hover:opacity-50 transition-opacity">
                <i data-lucide="arrow-left" class="w-4 h-4"></i> Continue Shopping
            </button>
            <div class="grid grid-cols-1 lg:grid-cols-12 gap-16">
                <!-- Checkout Form -->
                <div class="lg:col-span-7">
                    <h2 class="brand-name text-5xl mb-10">Checkout</h2>
                    <form onsubmit="handlePlaceOrder(event)" class="space-y-8">
                        <div class="space-y-4">
                            <h3 class="font-bold text-lg uppercase tracking-widest border-b pb-2">1. Delivery Details</h3>
                            <div class="grid grid-cols-2 gap-4">
                                <input type="text" placeholder="First Name" required class="w-full p-5 border-gray-100 border bg-gray-50 rounded-2xl focus:bg-white focus:border-black outline-none transition-all">
                                <input type="text" placeholder="Last Name" required class="w-full p-5 border-gray-100 border bg-gray-50 rounded-2xl focus:bg-white focus:border-black outline-none transition-all">
                            </div>
                            <input type="email" placeholder="Email Address" required class="w-full p-5 border-gray-100 border bg-gray-50 rounded-2xl focus:bg-white focus:border-black outline-none transition-all">
                            <input type="text" placeholder="Shipping Address" required class="w-full p-5 border-gray-100 border bg-gray-50 rounded-2xl focus:bg-white focus:border-black outline-none transition-all">
                        </div>
                        
                        <div class="space-y-4 pt-6">
                            <h3 class="font-bold text-lg uppercase tracking-widest border-b pb-2">2. Payment Method</h3>
                            <div class="grid grid-cols-1 sm:grid-cols-3 gap-4">
                                <div onclick="selectPayment('card')" id="pay-card" class="payment-card active p-6 bg-gray-50 rounded-2xl flex flex-col items-center gap-3 text-center">
                                    <i data-lucide="credit-card" class="w-6 h-6"></i>
                                    <span class="text-xs font-bold uppercase tracking-tighter">Debit/Credit</span>
                                </div>
                                <div onclick="selectPayment('bank')" id="pay-bank" class="payment-card p-6 bg-gray-50 rounded-2xl flex flex-col items-center gap-3 text-center">
                                    <i data-lucide="building" class="w-6 h-6"></i>
                                    <span class="text-xs font-bold uppercase tracking-tighter">Bank Transfer</span>
                                </div>
                                <div onclick="selectPayment('cod')" id="pay-cod" class="payment-card p-6 bg-gray-50 rounded-2xl flex flex-col items-center gap-3 text-center">
                                    <i data-lucide="wallet" class="w-6 h-6"></i>
                                    <span class="text-xs font-bold uppercase tracking-tighter">Cash on Delivery</span>
                                </div>
                            </div>
                            
                            <div id="card-details" class="pt-4 space-y-4">
                                <input type="text" placeholder="Name on Card" class="w-full p-5 border-gray-100 border bg-gray-50 rounded-2xl outline-none transition-all">
                                <input type="text" placeholder="Card Number" class="w-full p-5 border-gray-100 border bg-gray-50 rounded-2xl outline-none transition-all">
                                <div class="grid grid-cols-2 gap-4">
                                    <input type="text" placeholder="MM/YY" class="w-full p-5 border-gray-100 border bg-gray-50 rounded-2xl outline-none transition-all">
                                    <input type="text" placeholder="CVV" class="w-full p-5 border-gray-100 border bg-gray-50 rounded-2xl outline-none transition-all">
                                </div>
                            </div>

                            <div id="bank-details" class="pt-4 space-y-4 hidden">
                                <div class="p-6 bg-accent/5 border border-accent/20 rounded-2xl">
                                    <p class="text-xs font-bold uppercase tracking-widest text-accent mb-2">Beneficiary Details</p>
                                    <p class="text-sm font-bold">Bank: HBL Prestige</p>
                                    <p class="text-sm">Account Title: Aurum Boutique Pvt Ltd</p>
                                    <p class="text-sm">IBAN: PK72HABL00678901234567</p>
                                </div>
                                <p class="text-[10px] text-gray-400 italic">Please transfer the total amount to the account above and upload a screenshot of the receipt below or email it to accounts@aurum.pk.</p>
                                <div class="border-2 border-dashed border-gray-200 rounded-2xl p-8 text-center hover:border-accent transition-colors cursor-pointer">
                                    <i data-lucide="upload-cloud" class="w-8 h-8 mx-auto mb-2 text-gray-300"></i>
                                    <p class="text-xs font-bold text-gray-400">Upload Transfer Receipt</p>
                                </div>
                            </div>

                            <div id="cod-details" class="pt-4 hidden">
                                <div class="p-6 bg-gray-50 border border-gray-100 rounded-2xl flex items-start gap-4">
                                    <i data-lucide="info" class="w-5 h-5 text-gray-400 mt-0.5"></i>
                                    <div>
                                        <p class="text-sm font-bold mb-1">Pay on Delivery</p>
                                        <p class="text-xs text-gray-500 leading-relaxed">You will pay the total amount in cash to the courier at the time of delivery. Please ensure you have the exact change ready.</p>
                                    </div>
                                </div>
                            </div>
                        </div>

                        <button type="submit" id="place-order-btn" class="w-full py-6 bg-black text-white font-bold uppercase tracking-[0.2em] rounded-2xl hover:bg-gray-900 transition-all flex items-center justify-center gap-4 mt-10">
                            Complete Order
                        </button>
                    </form>
                </div>
                
                <!-- Order Summary -->
                <div class="lg:col-span-5">
                    <div class="sticky top-32 p-10 bg-gray-50 rounded-[2rem] border border-gray-100">
                        <h3 class="font-bold text-2xl mb-8 brand-name">Order Summary</h3>
                        <div id="checkout-summary" class="space-y-6 max-h-[400px] overflow-y-auto no-scrollbar mb-8">
                            <!-- Items here -->
                        </div>
                        <div class="border-t border-gray-200 pt-8 space-y-4">
                            <div class="flex justify-between text-gray-500">
                                <span>Shipping</span>
                                <span class="font-bold text-black">FREE</span>
                            </div>
                            <div class="flex justify-between items-center text-3xl font-bold">
                                <span>Total</span>
                                <span id="checkout-total">Rs. 0</span>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- Cart Drawer -->
    <div id="cart-drawer" class="fixed inset-0 z-[300] pointer-events-none">
        <div id="cart-overlay" onclick="toggleCart(false)" class="absolute inset-0 bg-black/40 opacity-0 transition-opacity duration-500 pointer-events-none"></div>
        <div class="absolute top-0 right-0 h-full w-full max-w-md bg-white shadow-2xl translate-x-full drawer flex flex-col pointer-events-auto">
            <div class="p-8 border-b flex items-center justify-between">
                <div class="flex items-center gap-4">
                    <button onclick="toggleCart(false)" class="p-2 hover:bg-gray-100 rounded-full transition-colors"><i data-lucide="chevron-right"></i></button>
                    <h2 class="brand-name text-3xl">Bag</h2>
                </div>
                <span id="cart-count-label" class="text-xs font-bold uppercase tracking-widest text-gray-400">0 Items</span>
            </div>
            <div id="cart-items" class="flex-1 overflow-y-auto p-8 space-y-8">
                <!-- Cart items dynamic -->
            </div>
            <div class="p-10 border-t bg-gray-50 rounded-t-[2rem]">
                <div class="flex justify-between items-center mb-8">
                    <span class="text-gray-500 uppercase tracking-widest text-sm">Estimated Total</span>
                    <span id="cart-total-display" class="text-3xl font-bold">Rs. 0</span>
                </div>
                <button onclick="initiateCheckout()" class="w-full py-6 bg-black text-white font-bold uppercase tracking-[0.2em] rounded-2xl hover:bg-gray-900 transition-all">Go to Checkout</button>
            </div>
        </div>
    </div>

    <!-- Product Modal (Details) -->
    <div id="product-modal" class="fixed inset-0 z-[400] hidden flex items-center justify-center p-4">
        <div class="absolute inset-0 bg-black/80 backdrop-blur-sm" onclick="closeProductModal()"></div>
        <div class="bg-white max-w-5xl w-full rounded-[2.5rem] relative z-10 overflow-hidden flex flex-col md:flex-row max-h-[90vh]">
            <div class="md:w-1/2 h-[40vh] md:h-auto bg-gray-100">
                <img id="modal-img" src="" class="w-full h-full object-cover">
            </div>
            <div class="md:w-1/2 p-12 flex flex-col justify-center">
                <button onclick="closeProductModal()" class="absolute top-8 right-8 p-3 hover:bg-gray-100 rounded-full transition-all"><i data-lucide="x"></i></button>
                <span id="modal-cat" class="text-xs uppercase tracking-[0.4em] text-accent font-bold mb-4">Category</span>
                <h2 id="modal-title" class="brand-name text-5xl mb-6">Product Name</h2>
                <p id="modal-price" class="text-3xl font-bold mb-8">Rs. 0</p>
                <div class="prose prose-sm text-gray-500 mb-10 leading-relaxed">
                    Designed for individuals who demand perfection. This piece represents our commitment to durability, aesthetics, and superior materials.
                </div>
                <div class="space-y-4">
                    <div class="flex items-center gap-4">
                        <div class="flex-1 flex items-center border border-gray-200 rounded-xl p-2">
                            <button onclick="updateModalQty(-1)" class="p-2"><i data-lucide="minus" class="w-4 h-4"></i></button>
                            <span id="modal-qty" class="flex-1 text-center font-bold">1</span>
                            <button onclick="updateModalQty(1)" class="p-2"><i data-lucide="plus" class="w-4 h-4"></i></button>
                        </div>
                    </div>
                    <button id="modal-add-btn" class="w-full py-5 bg-black text-white rounded-2xl font-bold uppercase tracking-widest flex items-center justify-center gap-4 hover:bg-gray-900 transition-all">
                        Add to Bag
                    </button>
                </div>
            </div>
        </div>
    </div>

    <script>
        let cart = [];
        let wishlist = [];
        let modalQty = 1;
        let currentBanner = 0;

        const products = [
            { id: 1, name: "Silk Drape Shirt", price: 67000, category: "Apparel", img: "https://images.unsplash.com/photo-1596755094514-f87e34085b2c?w=600" },
            { id: 2, name: "Cashmere Overcoat", price: 238000, category: "Apparel", img: "https://images.unsplash.com/photo-1539533018447-63fcce2678e3?w=600" },
            { id: 3, name: "Linen Tailored Pants", price: 50000, category: "Apparel", img: "https://images.unsplash.com/photo-1624371414361-e6e0ed26296c?w=600" },
            { id: 4, name: "Velvet Evening Gown", price: 310000, category: "Apparel", img: "https://images.unsplash.com/photo-1566174053879-31528523f8ae?w=600" },
            { id: 5, name: "Merino Wool Knit", price: 45000, category: "Apparel", img: "https://images.unsplash.com/photo-1620799140408-edc6dcb6d633?w=600" },
            { id: 6, name: "Deconstructed Blazer", price: 185000, category: "Apparel", img: "https://images.unsplash.com/photo-1591047139829-d91aecb6caea?w=600" },
            { id: 7, name: "Raw Silk Kimono", price: 92000, category: "Apparel", img: "https://images.unsplash.com/photo-1583844041148-fa53f7462f0e?w=600" },
            { id: 8, name: "Classic Trench Coat", price: 155000, category: "Apparel", img: "https://images.unsplash.com/photo-1591047139829-d91aecb6caea?w=600" },
            
            { id: 9, name: "Onyx Pro Headphones", price: 139000, category: "Electronics", img: "https://images.unsplash.com/photo-1505740420928-5e560c06d30e?w=600" },
            { id: 10, name: "Titanium Smart Watch", price: 215000, category: "Electronics", img: "https://images.unsplash.com/photo-1523275335684-37898b6baf30?w=600" },
            { id: 11, name: "Zenith Wireless Speaker", price: 89000, category: "Electronics", img: "https://images.unsplash.com/photo-1608156639585-34a0a56ee6c9?w=600" },
            { id: 12, name: "Aluminum Tablet Stand", price: 12000, category: "Electronics", img: "https://images.unsplash.com/photo-1544244015-0df4b3ffc6b0?w=600" },
            { id: 13, name: "Mechanical Slim Keyboard", price: 42000, category: "Electronics", img: "https://images.unsplash.com/photo-1587829741301-dc798b83dadc?w=600" },
            { id: 14, name: "Pro Studio Microphone", price: 76000, category: "Electronics", img: "https://images.unsplash.com/photo-1590602847861-f357a9332bbc?w=600" },
            { id: 15, name: "E-Ink Minimalist Reader", price: 55000, category: "Electronics", img: "https://images.unsplash.com/photo-1589998059171-988d887df646?w=600" },
            { id: 16, name: "Noise Cancelling Buds", price: 48000, category: "Electronics", img: "https://images.unsplash.com/photo-1590658268037-6bf12165a8df?w=600" },
            
            { id: 17, name: "Gold Cuff Bracelet", price: 117000, category: "Accessories", img: "https://images.unsplash.com/photo-1611591437281-460bfbe1220a?w=600" },
            { id: 18, name: "Python Leather Clutch", price: 295000, category: "Accessories", img: "https://images.unsplash.com/photo-1584917865442-de89df76afd3?w=600" },
            { id: 19, name: "Aviator Lens Shades", price: 62000, category: "Accessories", img: "https://images.unsplash.com/photo-1572635196237-14b3f281503f?w=600" },
            { id: 20, name: "Sapphire Drop Earrings", price: 440000, category: "Accessories", img: "https://images.unsplash.com/photo-1635767790021-950e1eee3300?w=600" },
            { id: 21, name: "Silk Patterned Scarf", price: 22000, category: "Accessories", img: "https://images.unsplash.com/photo-1584917865442-de89df76afd3?w=600" },
            { id: 22, name: "Croc-Embossed Belt", price: 35000, category: "Accessories", img: "https://images.unsplash.com/photo-1624222247344-550fb8ec5521?w=600" },
            { id: 23, name: "Silver Signet Ring", price: 58000, category: "Accessories", img: "https://images.unsplash.com/photo-1605100804763-247f67b3557e?w=600" },
            { id: 24, name: "Minimal Card Holder", price: 18000, category: "Accessories", img: "https://images.unsplash.com/photo-1627123424574-724758594e93?w=600" },
            
            { id: 25, name: "Chelsea Suede Boots", price: 117000, category: "Footwear", img: "https://images.unsplash.com/photo-1638247025967-b4e38f787b76?w=600" },
            { id: 26, name: "Italian Leather Loafers", price: 145000, category: "Footwear", img: "https://images.unsplash.com/photo-1614252235316-8c857d38b5f4?w=600" },
            { id: 27, name: "Minimalist Sneaker", price: 78000, category: "Footwear", img: "https://images.unsplash.com/photo-1560769629-975ec94e6a86?w=600" },
            { id: 28, name: "Stiletto Velvet Pumps", price: 110000, category: "Footwear", img: "https://images.unsplash.com/photo-1543163521-1bf539c55dd2?w=600" },
            { id: 29, name: "Hand-Painted Derbies", price: 195000, category: "Footwear", img: "https://images.unsplash.com/photo-1533867617858-e7b97e060509?w=600" },
            { id: 30, name: "Urban Tech Runner", price: 65000, category: "Footwear", img: "https://images.unsplash.com/photo-1542291026-7eec264c27ff?w=600" },
            { id: 31, name: "Shearling Lined Mules", price: 88000, category: "Footwear", img: "https://images.unsplash.com/photo-1595950653106-6c9ebd614d3a?w=600" },
            { id: 32, name: "Classic Monk Straps", price: 132000, category: "Footwear", img: "https://images.unsplash.com/photo-1449505278894-297fdb3edbc1?w=600" },
            
            { id: 33, name: "Marble Pillar Lamp", price: 126000, category: "Home", img: "https://images.unsplash.com/photo-1507473885765-e6ed057f782c?w=600" },
            { id: 34, name: "Hand-Knotted Silk Rug", price: 580000, category: "Home", img: "https://images.unsplash.com/photo-1575414003591-ece8d0416c7a?w=600" },
            { id: 35, name: "Abstract Bronze Sculpture", price: 210000, category: "Home", img: "https://images.unsplash.com/photo-1554188248-986adbb73be4?w=600" },
            { id: 36, name: "Scented Soy Candle Set", price: 15000, category: "Home", img: "https://images.unsplash.com/photo-1602810318383-e386cc2a3ccf?w=600" },
            { id: 37, name: "Velvet Accent Chair", price: 245000, category: "Home", img: "https://images.unsplash.com/photo-1567538096630-e0c55bd6374c?w=600" },
            { id: 38, name: "Ceramic Minimalist Vase", price: 28000, category: "Home", img: "https://images.unsplash.com/photo-1581783898377-1c85bf937427?w=600" },
            { id: 39, name: "Cashmere Throw Blanket", price: 89000, category: "Home", img: "https://images.unsplash.com/photo-1512418490979-92798ccc1340?w=600" },
            { id: 40, name: "Glass Decanter Set", price: 45000, category: "Home", img: "https://images.unsplash.com/photo-1514362545857-3bc16c4c7d1b?w=600" },
            
            { id: 41, name: "Gold Infused Serum", price: 39000, category: "Beauty", img: "https://images.unsplash.com/photo-1570172619644-dfd03ed5d881?w=600" },
            { id: 42, name: "Oud & Sandalwood Parfum", price: 82000, category: "Beauty", img: "https://images.unsplash.com/photo-1541643600914-78b084683601?w=600" },
            { id: 43, name: "Hydrating Rose Mask", price: 12000, category: "Beauty", img: "https://images.unsplash.com/photo-1556228578-8c7c2f2392b4?w=600" },
            { id: 44, name: "Jade Face Roller", price: 8500, category: "Beauty", img: "https://images.unsplash.com/photo-1619451334792-150fd785ee74?w=600" },
            { id: 45, name: "Silk Eye Mask Kit", price: 14000, category: "Beauty", img: "https://images.unsplash.com/photo-1598440499033-547b19fc162a?w=600" },
            { id: 46, name: "Mineral Night Cream", price: 28000, category: "Beauty", img: "https://images.unsplash.com/photo-1556229010-6c3f2c9ca5f8?w=600" },
            { id: 47, name: "Botanical Body Oil", price: 19500, category: "Beauty", img: "https://images.unsplash.com/photo-1608248597279-f99d160bfcbc?w=600" },
            { id: 48, name: "Charcoal Detox Scrub", price: 11000, category: "Beauty", img: "https://images.unsplash.com/photo-1594418073511-09a473a2462e?w=600" }
        ];

        function toggleMenu(show) {
            const overlay = document.getElementById('menu-overlay');
            if (show) {
                overlay.classList.remove('translate-x-full');
            } else {
                overlay.classList.add('translate-x-full');
            }
        }

        function showStore() {
            document.getElementById('main-store').classList.remove('hidden');
            document.getElementById('checkout-view').classList.add('hidden');
            window.scrollTo({ top: 0, behavior: 'smooth' });
        }

        function initiateCheckout() {
            if (cart.length === 0) return alert("Your bag is empty");
            toggleCart(false);
            document.getElementById('main-store').classList.add('hidden');
            document.getElementById('checkout-view').classList.remove('hidden');
            updateCheckoutSummary();
            window.scrollTo({ top: 0, behavior: 'smooth' });
        }

        function renderProducts(data, targetId = 'product-grid') {
            const container = document.getElementById(targetId);
            if (!container) return;

            container.innerHTML = data.map(p => {
                const isLoved = wishlist.some(w => w.id === p.id);
                return `
                    <div class="product-card-container group bg-white rounded-[2rem] overflow-hidden">
                        <div class="relative aspect-[3/4] overflow-hidden bg-gray-100">
                            <img src="${p.img}" alt="${p.name}" class="w-full h-full object-cover transition-transform duration-700 group-hover:scale-110">
                            
                            <!-- Action Overlay -->
                            <div class="action-overlay absolute inset-0 flex flex-col justify-end p-6 gap-3">
                                <button onclick="addToBagDirect(${p.id})" class="w-full py-3 bg-white text-black text-[10px] font-bold uppercase tracking-widest rounded-xl hover:bg-accent hover:text-white transition-all flex items-center justify-center gap-2">
                                    <i data-lucide="shopping-bag" class="w-3 h-3"></i> Add to Bag
                                </button>
                                <button onclick="openProductModal(${p.id})" class="w-full py-3 bg-black/20 backdrop-blur-md text-white border border-white/30 text-[10px] font-bold uppercase tracking-widest rounded-xl hover:bg-white/40 transition-all flex items-center justify-center gap-2">
                                    <i data-lucide="eye" class="w-3 h-3"></i> Details
                                </button>
                            </div>

                            <button onclick="event.stopPropagation(); toggleLove(${p.id})" class="absolute top-4 right-4 z-10 w-10 h-10 bg-white/80 backdrop-blur-md rounded-full flex items-center justify-center hover:bg-white transition-all shadow-sm">
                                <i data-lucide="heart" class="w-5 h-5 ${isLoved ? 'fill-red-500 text-red-500' : 'text-black'}"></i>
                            </button>
                        </div>
                        <div class="p-6 text-center" onclick="openProductModal(${p.id})">
                            <p class="text-[10px] uppercase tracking-[0.2em] text-accent font-bold mb-2">${p.category}</p>
                            <h3 class="font-bold text-lg mb-1">${p.name}</h3>
                            <p class="text-gray-400 font-medium">Rs. ${p.price.toLocaleString()}</p>
                        </div>
                    </div>
                `;
            }).join('');
            lucide.createIcons();
        }

        function addToBagDirect(id) {
            const product = products.find(p => p.id === id);
            const exists = cart.find(item => item.id === id);
            if (exists) exists.qty += 1;
            else cart.push({ ...product, qty: 1 });
            updateCartUI();
            toggleCart(true);
        }

        function filterCategory(cat) {
            // Smoothly scroll to the shop section when a category is selected
            document.getElementById('shop-section').scrollIntoView({ behavior: 'smooth' });
            
            const filtered = cat === 'All' ? products : products.filter(p => p.category === cat);
            renderProducts(filtered);
            
            // Update active state for filter buttons
            document.querySelectorAll('.cat-btn').forEach(btn => {
                const isActive = (cat === 'All' && btn.innerText === 'All') || btn.innerText.includes(cat);
                btn.classList.toggle('bg-black', isActive);
                btn.classList.toggle('text-white', isActive);
                btn.classList.toggle('border-black', isActive);
                btn.classList.toggle('border-gray-200', !isActive);
            });
        }

        function handleSearch(query) {
            const results = products.filter(p => 
                p.name.toLowerCase().includes(query.toLowerCase()) || 
                p.category.toLowerCase().includes(query.toLowerCase())
            );
            renderProducts(results, 'search-results');
        }

        function selectPayment(method) {
            document.querySelectorAll('.payment-card').forEach(c => c.classList.remove('active'));
            document.getElementById(`pay-${method}`).classList.add('active');
            
            // Toggle visibility of specific detail sections
            document.getElementById('card-details').classList.toggle('hidden', method !== 'card');
            document.getElementById('bank-details').classList.toggle('hidden', method !== 'bank');
            document.getElementById('cod-details').classList.toggle('hidden', method !== 'cod');
        }

        function updateCheckoutSummary() {
            const container = document.getElementById('checkout-summary');
            const totalDisplay = document.getElementById('checkout-total');
            let total = 0;

            container.innerHTML = cart.map(item => {
                total += item.price * item.qty;
                return `
                    <div class="flex items-center gap-4 py-2">
                        <img src="${item.img}" class="w-12 h-12 object-cover rounded-lg">
                        <div class="flex-1">
                            <p class="text-sm font-bold">${item.name}</p>
                            <p class="text-xs text-gray-400">${item.qty} x Rs. ${item.price.toLocaleString()}</p>
                        </div>
                        <span class="font-bold">Rs. ${(item.price * item.qty).toLocaleString()}</span>
                    </div>
                `;
            }).join('');
            totalDisplay.innerText = `Rs. ${total.toLocaleString()}`;
        }

        function rotateBanner() {
            const images = [
                "https://images.unsplash.com/photo-1441986300917-64674bd600d8?auto=format&fit=crop&q=80&w=1600",
                "https://images.unsplash.com/photo-1441984904996-e0b6ba687e04?auto=format&fit=crop&q=80&w=1600"
            ];
            const titles = ["Excellence.", "Elegance.", "Aurum."];
            currentBanner = (currentBanner + 1) % images.length;
            
            const bannerImg = document.getElementById('banner-img');
            const bannerTitle = document.getElementById('banner-title');
            
            bannerImg.style.opacity = '0';
            bannerTitle.style.opacity = '0';
            bannerTitle.style.transform = 'translateY(20px)';
            
            setTimeout(() => {
                bannerImg.src = images[currentBanner];
                bannerTitle.innerText = titles[currentBanner % titles.length];
                bannerImg.style.opacity = '0.6';
                bannerTitle.style.opacity = '1';
                bannerTitle.style.transform = 'translateY(0)';
            }, 1000);
        }

        function addToCart(id) {
            const product = products.find(p => p.id === id);
            const exists = cart.find(item => item.id === id);
            if (exists) exists.qty += modalQty;
            else cart.push({ ...product, qty: modalQty });
            modalQty = 1;
            updateCartUI();
            toggleCart(true);
        }

        function removeFromCart(id) {
            cart = cart.filter(item => item.id !== id);
            updateCartUI();
        }

        function toggleLove(id) {
            const index = wishlist.findIndex(p => p.id === id);
            if(index > -1) wishlist.splice(index, 1);
            else wishlist.push(products.find(p => p.id === id));
            
            const count = document.getElementById('wishlist-count');
            count.innerText = wishlist.length;
            count.classList.toggle('hidden', wishlist.length === 0);
            renderProducts(products); // Refresh icons
        }

        function updateCartUI() {
            const container = document.getElementById('cart-items');
            const totalDisplay = document.getElementById('cart-total-display');
            const navCount = document.getElementById('cart-count');
            
            let total = 0;
            let qty = 0;
            
            container.innerHTML = cart.map(item => {
                total += item.price * item.qty;
                qty += item.qty;
                return `
                    <div class="flex items-center gap-4 py-4 border-b border-gray-100">
                        <img src="${item.img}" class="w-16 h-20 object-cover rounded-xl">
                        <div class="flex-1">
                            <h4 class="text-sm font-bold uppercase">${item.name}</h4>
                            <p class="text-xs text-gray-400">Rs. ${item.price.toLocaleString()} x ${item.qty}</p>
                        </div>
                        <button onclick="removeFromCart(${item.id})" class="text-red-400 hover:text-red-600"><i data-lucide="trash-2" class="w-4 h-4"></i></button>
                    </div>
                `;
            }).join('') || '<p class="text-center py-10 text-gray-400">Your bag is empty.</p>';
            
            totalDisplay.innerText = `Rs. ${total.toLocaleString()}`;
            navCount.innerText = qty;
            navCount.classList.toggle('hidden', qty === 0);
            lucide.createIcons();
        }

        function openProductModal(id) {
            const p = products.find(prod => prod.id === id);
            if (!p) return;
            document.getElementById('modal-img').src = p.img;
            document.getElementById('modal-title').innerText = p.name;
            document.getElementById('modal-price').innerText = `Rs. ${p.price.toLocaleString()}`;
            document.getElementById('modal-cat').innerText = p.category;
            document.getElementById('product-modal').classList.remove('hidden');
            document.getElementById('modal-add-btn').onclick = () => addToCart(p.id);
        }

        function closeProductModal() {
            document.getElementById('product-modal').classList.add('hidden');
        }

        function toggleCart(show) {
            const drawer = document.getElementById('cart-drawer').querySelector('.drawer');
            const overlay = document.getElementById('cart-overlay');
            drawer.style.transform = show ? 'translateX(0)' : 'translateX(100%)';
            overlay.classList.toggle('opacity-0', !show);
            overlay.classList.toggle('pointer-events-none', !show);
        }

        function toggleSearch(show) {
            document.getElementById('search-overlay').style.transform = show ? 'translateY(0)' : 'translateY(100%)';
        }

        function toggleWishlist(show) {
            if(show && wishlist.length > 0) {
                renderProducts(wishlist, 'search-results');
                toggleSearch(true);
            }
        }

        function updateModalQty(val) {
            modalQty = Math.max(1, modalQty + val);
            document.getElementById('modal-qty').innerText = modalQty;
        }

        function handlePlaceOrder(e) {
            e.preventDefault();
            const btn = document.getElementById('place-order-btn');
            btn.innerHTML = '<div class="spinner"></div> Processing...';
            setTimeout(() => {
                alert("Order Placed Successfully!");
                cart = [];
                updateCartUI();
                showStore();
                btn.innerHTML = 'Complete Order';
            }, 2000);
        }

        window.onload = () => {
            renderProducts(products);
            setInterval(rotateBanner, 5000);
            lucide.createIcons();
        };
    </script>
</body>
</html>
