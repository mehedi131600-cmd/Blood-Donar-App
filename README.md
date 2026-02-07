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
        <h1 class="text-2xl font-bold">যুব কল্যাণ রক্তদান ফাউন্ডেশন</h1>
        <p class="text-sm opacity-90 mt-1">মানবতার কল্যাণে আমাদের রক্তদান</p>
    </div>

    <div id="donorList" class="p-4 grid gap-4 pb-24">
        <p class="text-center py-10 text-gray-400">শিট থেকে তথ্য লোড হচ্ছে...</p>
    </div>

    <script>
        // আপনার নতুন Web App URL
        const url = "https://script.google.com/macros/s/AKfycbyT5Wy8zwAZw30r3bNetoQnhhvlxuWYsf8yaBQx_rQwWMCOy5UvmBI8M3jgbVT-7qUc/exec";

        async function loadDonors() {
            try {
                const response = await fetch(url);
                const data = await response.json();
                const list = document.getElementById('donorList');
                list.innerHTML = "";
                
                data.forEach(d => {
                    list.innerHTML += `
                        <div class="bg-white p-4 rounded-2xl shadow-sm border border-gray-100 relative mb-4">
                            <span class="absolute top-0 right-0 bg-gray-100 text-gray-500 text-[10px] px-3 py-1 rounded-bl-xl font-bold italic">SL: ${d.sl}</span>
                            <div class="flex justify-between items-start mb-4 mt-2">
                                <div>
                                    <h3 class="font-bold text-lg text-gray-800">${d.n}</h3>
                                    <p class="text-xs text-gray-500 mt-1">📍 ${d.l}</p>
                                </div>
                                <div class="bg-red-50 px-3 py-1 rounded-lg text-center border border-red-100">
                                    <p class="text-[10px] text-red-400 font-bold uppercase mb-1 leading-none">গ্রুপ</p>
                                    <p class="text-xl font-black text-red-600 leading-none">${d.g}</p>
                                </div>
                            </div>
                            <div class="grid grid-cols-2 gap-3 mb-4 text-center text-[11px]">
                                <div class="bg-slate-50 p-2 rounded-xl text-gray-600"><b>শেষ দান:</b><br>${d.last || 'N/A'}</div>
                                <div class="bg-green-50 p-2 rounded-xl text-green-700"><b>পরবর্তী:</b><br>${d.next || 'N/A'}</div>
                            </div>
                            <a href="tel:${d.p}" class="w-full bg-green-600 text-white py-3 rounded-xl font-bold flex justify-center items-center gap-2 shadow-md">📞 সরাসরি কল করুন</a>
                        </div>`;
                });
            } catch (e) {
                document.getElementById('donorList').innerHTML = "<p class='text-center text-red-500'>সার্ভার কানেকশন সমস্যা!</p>";
            }
        }
        loadDonors();
    </script>
</body>
</html>
