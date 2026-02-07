<!DOCTYPE html>
<html lang="bn">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>যুব কল্যাণ রক্তদান ফাউন্ডেশন</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Hind+Siliguri:wght@400;600;700&display=swap" rel="stylesheet">
    <style>
        body { font-family: 'Hind Siliguri', sans-serif; background-color: #f8fafc; }
    </style>
</head>
<body class="bg-gray-50 pb-10">

    <div class="bg-red-600 text-white p-6 text-center shadow-lg sticky top-0 z-20">
        <h1 class="text-2xl font-bold">যুব কল্যাণ রক্তদান ফাউন্ডেশন</h1>
        <p class="text-sm opacity-90 mt-1 italic">মানবতার কল্যাণে আমাদের রক্তদান</p>
        
        <div class="mt-4 pt-3 border-t border-red-400">
            <p class="text-[11px] uppercase tracking-widest opacity-80">যেকোনো প্রয়োজনে যোগাযোগ করুন</p>
            <p class="text-md font-bold text-yellow-300">প্রতিষ্ঠাতা পরিচালক: মোঃ মেহেদী হাসান</p>
            <a href="tel:01888354739" class="inline-block mt-2 bg-white text-red-600 px-4 py-1 rounded-full text-sm font-bold shadow-md active:scale-95 transition-transform">
                📞 01888354739
            </a>
        </div>
    </div>

    <div class="m-4 p-4 bg-white border-l-4 border-red-500 rounded-xl shadow-sm">
        <h4 class="font-bold text-red-600 text-sm">📢 নতুন সদস্য হতে আগ্রহী?</h4>
        <p class="text-xs text-gray-600 mt-1">আপনি যদি আমাদের ফাউন্ডেশনের সদস্য হয়ে মানবতার সেবায় অংশ নিতে চান, তবে উপরে দেওয়া নাম্বারে অবশ্যই যোগাযোগ করুন।</p>
    </div>

    <div class="mx-4 p-4 bg-white shadow-md rounded-2xl sticky top-[180px] z-10 border border-gray-100">
        <input type="text" id="searchInput" onkeyup="filterDonors()" placeholder="নাম বা এলাকা লিখে খুঁজুন..." class="w-full p-3 border border-gray-200 rounded-xl mb-3 outline-none focus:ring-2 focus:ring-red-500 text-sm">
        <select id="groupFilter" onchange="filterDonors()" class="w-full p-3 border border-gray-200 rounded-xl font-bold text-red-600 outline-none text-sm bg-white text-center">
            <option value="">সব রক্তের গ্রুপ</option>
            <option value="A+">A+</option><option value="A-">A-</option>
            <option value="B+">B+</option><option value="B-">B-</option>
            <option value="O+">O+</option><option value="O-">O-</option>
            <option value="AB+">AB+</option><option value="AB-">AB-</option>
        </select>
    </div>

    <div id="loading" class="text-center py-20">
        <div class="animate-spin inline-block w-8 h-8 border-4 border-red-500 border-t-transparent rounded-full mb-2"></div>
        <p class="text-gray-500 font-bold">তথ্য লোড হচ্ছে...</p>
    </div>

    <div id="donorList" class="p-4 grid gap-4 hidden"></div>

    <script>
        const url = "https://script.google.com/macros/s/AKfycbyT5Wy8zwAZw30r3bNetoQnhhvlxuWYsf8yaBQx_rQwWMCOy5UvmBI8M3jgbVT-7qUc/exec";
        let allDonors = [];

        function getDonationStatus(lastDateStr) {
            if (!lastDateStr || lastDateStr === "" || lastDateStr === "N/A") return { text: "তথ্য নেই", class: "text-gray-500 bg-gray-50 border-gray-100" };
            const lastDate = new Date(lastDateStr);
            if (isNaN(lastDate)) return { text: "সঠিক তারিখ নেই", class: "text-gray-500 bg-gray-50" };
            const today = new Date();
            const diffTime = (today - lastDate);
            const diffDays = Math.ceil(diffTime / (1000 * 60 * 60 * 24)); 
            if (diffDays >=
                            </div>
                        </div>
                        <div class="grid grid-cols-2 gap-3 mb-5 text-center text-[10px]">
                            <div class="bg-slate-50 p-2 rounded-xl border border-slate-100">
                                <p class="font-bold uppercase opacity-60 mb-1">শেষ রক্তদান</p>
                                <p class="text-xs font-bold text-slate-700">${d.last || 'N/A'}</p>
                            </div>
                            <div class="${status.class} p-2 rounded-xl border">
                                <p class="font-bold uppercase opacity-70 mb-1">বর্তমান অবস্থা</p>
                                <p class="text-xs font-bold">${status.text}</p>
                            </div>
                        </div>
                        <a href="tel:${d.p}" class="w-full bg-red-600 text-white py-4 rounded-2xl font-bold flex justify-center items-center gap-2 shadow-lg active:scale-95 transition-all">
                            📞 ডোনারকে কল করুন
                        </a>
                    </div>`;
            });
        }

        function filterDonors() {
            let input = document.getElementById('searchInput').value.toLowerCase();
            let group = document.getElementById('groupFilter').value;
            let filtered = allDonors.filter(d => 
                (String(d.n).toLowerCase().includes(input) || String(d.l).toLowerCase().includes(input)) && 
                (group === "" || String(d.g).trim() === group)
            );
            displayDonors(filtered);
        }

        loadDonors();
    </script>
</body>
</html>
                                <p class="text-xs font-bold">${status.text}</p>
                            </div>
                        </div>
                        <a href="tel:${d.p}" class="w-full bg-green-600 text-white py-4 rounded-2xl font-bold flex justify-center items-center gap-2 shadow-lg active:bg-green-700">
                            📞 সরাসরি কল দিন
                        </a>
                    </div>`;
            });
        }

        function filterDonors() {
            let input = document.getElementById('searchInput').value.toLowerCase();
            let group = document.getElementById('groupFilter').value;
            let filtered = allDonors.filter(d => 
                (String(d.n).toLowerCase().includes(input) || String(d.l).toLowerCase().includes(input)) && 
                (group === "" || String(d.g).trim() === group)
            );
            displayDonors(filtered);
        }

        loadDonors();
    </script>
</body>
</html>
