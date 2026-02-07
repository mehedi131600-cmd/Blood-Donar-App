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
<body class="bg-gray-50 pb-20">

    <div class="bg-red-600 text-white p-8 text-center shadow-lg">
        <h1 class="text-3xl font-bold uppercase tracking-tight">যুব কল্যাণ রক্তদান ফাউন্ডেশন</h1>
        <p class="text-sm opacity-90 mt-1 italic">মানবতার কল্যাণে আমাদের রক্তদান</p>
        
        <div class="mt-6 pt-4 border-t border-red-400">
            <p class="text-lg font-bold text-yellow-300">প্রতিষ্ঠাতা পরিচালক: মোঃ মেহেদী হাসান</p>
            <a href="tel:01888354739" class="inline-block mt-2 bg-white text-red-600 px-6 py-2 rounded-full text-sm font-bold shadow-md">📞 কল করুন: 01888354739</a>
        </div>
    </div>

    <div class="m-4 p-4 bg-white shadow-md rounded-2xl border border-gray-100">
        <input type="text" id="searchInput" onkeyup="filterDonors()" placeholder="নাম বা এলাকা লিখে খুঁজুন..." class="w-full p-3 border border-gray-200 rounded-xl mb-3 outline-none text-sm text-center shadow-inner focus:border-red-300">
        <select id="groupFilter" onchange="filterDonors()" class="w-full p-3 border border-gray-200 rounded-xl font-bold text-red-600 outline-none text-sm bg-white text-center cursor-pointer">
            <option value="">সব রক্তের গ্রুপ</option>
            <option value="A+">A+</option><option value="B+">B+</option><option value="O+">O+</option><option value="AB+">AB+</option>
            <option value="A-">A-</option><option value="B-">B-</option><option value="O-">O-</option><option value="AB-">AB-</option>
        </select>
    </div>

    <div id="loading" class="text-center py-20 text-gray-400">তথ্য লোড হচ্ছে...</div>
    <div id="donorList" class="p-4 grid gap-4 hidden"></div>

    <script>
        // আপনার সঠিক লিঙ্কটি এখানে দেওয়া হয়েছে
        const url = "https://script.google.com/macros/s/AKfycbzbHWB2_5S77EH29tXH9JGIZD7CiotsAYtUl778hn36ymr2OoQ_-N31X8K_NNr7uOag/exec"; 

        async function loadDonors() {
            const loadingDiv = document.getElementById('loading');
            try {
                const response = await fetch(url);
                const data = await response.json();
                
                if (data && data.length > 0) {
                    displayDonors(data);
                    loadingDiv.classList.add('hidden');
                    document.getElementById('donorList').classList.remove('hidden');
                } else {
                    loadingDiv.innerText = "শিটে কোনো ডাটা নেই। অনুগ্রহ করে গুগল শিটে ডোনারের নাম যোগ করুন।";
                }
            } catch (e) { 
                console.error(e);
                loadingDiv.innerHTML = "<p class='text-red-500'>লিঙ্ক ত্রুটি! <br> আপনার Apps Script লিঙ্কটি চেক করুন অথবা 'Anyone' এক্সেস দিন।</p>"; 
            }
        }

        function getStatus(lastDateStr) {
            if (!lastDateStr || lastDateStr.trim() === "" || lastDateStr === "undefined") {
                return { text: "N/A", class: "text-gray-400 bg-gray-50 border-gray-100", last: "তারিখ আপডেট করা হয়নি" };
            }
            const lastDate = new Date(lastDateStr);
            if (isNaN(lastDate)) return { text: "N/A", class: "text-gray-400 bg-gray-50", last: "তারিখ আপডেট করা হয়নি" };
            
            const diffDays = Math.floor((new Date() - lastDate) / (1000 * 60 * 60 * 24));
            const formatted = lastDate.toLocaleDateString('en-GB', { day: '2-digit', month: 'short', year: 'numeric' });
            
            if (diffDays >= 90) return { text: "রক্ত দিতে পারবে", class: "text-green-600 bg-green-50 border-green-100", last: formatted };
            return { text: (90 - diffDays) + " দিন বাকি", class: "text-red-600 bg-red-50 border-red-100", last: formatted };
        }

        function displayDonors(data) {
            const list = document.getElementById('donorList');
            list.innerHTML = "";
            data.forEach(d => {
                const status = getStatus(d.last);
                list.innerHTML += `
                <div class="bg-white p-5 rounded-3xl shadow-sm border border-gray-100 mb-2">
                    <div class="flex justify-between items-start mb-2">
                        <div class="w-2/3">
                            <h3 class="font-bold text-xl text-gray-800">${d.n}</h3>
                            <p class="text-sm text-gray-600 mt-1 font-semibold flex items-center gap-1">
                                <span class="text-red-500">📍</span> ${d.l}
                            </p>
                        </div>
                        <div class="bg-red-50 px-3 py-1 rounded-xl text-red-600 font-black text-2xl border border-red-100 shadow-sm">${d.g}</div>
                    </div>
                    <hr class="my-3 border-gray-50">
                    <div class="grid grid-cols-2 gap-3 mb-4 text-center">
                        <div class="bg-slate-50 p-2 rounded-xl border border-slate-100">
                            <p class="text-[8px] uppercase font-bold text-slate-400">শেষ রক্তদান</p>
                            <p class="text-[10px] font-bold text-slate-700">${status.last}</p>
                        </div>
                        <div class="${status.class} p-2 rounded-xl border">
                            <p class="text-[8px] uppercase font-bold opacity-70">অবস্থা</p>
                            <p class="text-[11px] font-bold">${status.text}</p>
                        </div>
                    </div>
                    <a href="tel:${d.p}" class="block w-full bg-red-600 text-white py-3 rounded-2xl font-bold text-center text-sm shadow-md active:scale-95 transition-all">📞 কল দিন</a>
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
