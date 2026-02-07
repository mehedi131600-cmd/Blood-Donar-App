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
<body class="bg-gray-50">

    <div class="bg-red-600 text-white p-6 text-center shadow-lg sticky top-0 z-20">
        <h1 class="text-2xl font-bold uppercase tracking-wide">যুব কল্যাণ রক্তদান ফাউন্ডেশন</h1>
        <p class="text-sm opacity-90 mt-1 italic">মানবতার কল্যাণে আমাদের রক্তদান</p>
    </div>

    <div id="loading" class="text-center py-20">
        <div class="animate-spin inline-block w-8 h-8 border-4 border-red-500 border-t-transparent rounded-full mb-2"></div>
        <p class="text-gray-500 font-bold">সার্ভার থেকে তথ্য আসছে, একটু অপেক্ষা করুন...</p>
    </div>

    <div id="donorList" class="p-4 grid gap-4 pb-24 hidden">
        </div>

    <script>
        // আপনার নতুন এবং সঠিক Web App URL
        const url = "https://script.google.com/macros/s/AKfycbyT5Wy8zwAZw30r3bNetoQnhhvlxuWYsf8yaBQx_rQwWMCOy5UvmBI8M3jgbVT-7qUc/exec";

        async function loadDonors() {
            try {
                const response = await fetch(url);
                const data = await response.json();
                
                const list = document.getElementById('donorList');
                const loader = document.getElementById('loading');
                
                list.innerHTML = "";
                
                if(data.length === 0) {
                    loader.innerHTML = "<p class='text-red-500'>শিটে কোনো ডোনারের নাম পাওয়া যায়নি।</p>";
                    return;
                }

                data.forEach(d => {
                    list.innerHTML += `
                        <div class="bg-white p-5 rounded-3xl shadow-sm border border-gray-100 relative mb-2">
                            <span class="absolute top-0 right-0 bg-gray-100 text-gray-400 text-[9px] px-3 py-1 rounded-bl-2xl font-bold italic tracking-tighter">SL: ${d.sl}</span>
                            
                            <div class="flex justify-between items-start mb-4 mt-2">
                                <div class="w-2/3">
                                    <h3 class="font-bold text-xl text-gray-800 leading-tight">${d.n}</h3>
                                    <p class="text-xs text-gray-500 mt-1 flex items-center">
                                        <span class="mr-1">📍</span> ${d.l}
                                    </p>
                                </div>
                                <div class="bg-red-50 px-4 py-2 rounded-2xl text-center border border-red-100 min-w-[65px]">
                                    <p class="text-[10px] text-red-400 font-bold uppercase leading-none mb-1">গ্রুপ</p>
                                    <p class="text-2xl font-black text-red-600 leading-none">${d.g}</p>
                                </div>
                            </div>

                            <div class="grid grid-cols-2 gap-3 mb-5 text-center">
                                <div class="bg-slate-50 p-2 rounded-xl border border-slate-100">
                                    <p class="text-[9px] text-slate-400 font-bold uppercase mb-1">শেষ রক্তদান</p>
                                    <p class="text-xs font-bold text-slate-700">${d.last || 'জানা নেই'}</p>
                                </div>
                                <div class="bg-green-50 p-2 rounded-xl border border-green-100">
                                    <p class="text-[9px] text-green-400 font-bold uppercase mb-1">পরবর্তী দান</p>
                                    <p class="text-xs font-bold text-green-600">${d.next || 'এখনই পারবেন'}</p>
                                </div>
                            </div>

                            <a href="tel:${d.p}" class="w-full bg-green-600 text-white py-4 rounded-2xl font-bold flex justify-center items-center gap-2 shadow-lg active:bg-green-700 transition-all">
                                📞 সরাসরি কল দিন
                            </a>
                        </div>
                    `;
                });

                loader.classList.add('hidden');
                list.classList.remove('hidden');

            } catch (e) {
                document.getElementById('loading').innerHTML = `
                    <div class="p-6 text-red-500">
                        <p class="font-bold text-lg">ডাটা লোড হচ্ছে না!</p>
                        <p class="text-xs mt-2 italic text-gray-400 underline" onclick="location.reload()">আবার চেষ্টা করুন</p>
                    </div>`;
            }
        }
        loadDonors();
    </script>
</body>
</html>
