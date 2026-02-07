<!DOCTYPE html>
<html lang="bn">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>যুব কল্যাণ রক্তদান ফাউন্ডেশন</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Hind+Siliguri:wght@400;600&display=swap" rel="stylesheet">
    <style>
        body { font-family: 'Hind Siliguri', sans-serif; background-color: #f3f4f6; }
        .donor-card { transition: all 0.3s ease; }
    </style>
</head>
<body>

    <div class="bg-red-600 text-white p-6 text-center shadow-lg sticky top-0 z-20">
        <h1 class="text-2xl font-bold">যুব কল্যাণ রক্তদান ফাউন্ডেশন</h1>
        <p class="text-sm opacity-90">রক্তদাতার পূর্ণাঙ্গ তালিকা</p>
    </div>

    <div class="p-4 bg-white shadow-md sticky top-[80px] z-10">
        <input type="text" id="searchInput" onkeyup="filterDonors()" placeholder="নাম বা এলাকা লিখে খুঁজুন..." class="w-full p-3 border border-gray-300 rounded-lg mb-3 focus:ring-2 focus:ring-red-500 outline-none">
        
        <select id="groupFilter" onchange="filterDonors()" class="w-full p-3 border border-gray-300 rounded-lg font-bold text-red-600 outline-none">
            <option value="">সব গ্রুপের রক্তদাতা</option>
            <option value="A+">A+</option>
            <option value="B+">B+</option>
            <option value="O+">O+</option>
            <option value="O-">O-</option>
            <option value="AB+">AB+</option>
        </select>
    </div>

    <div id="donorList" class="p-4 grid gap-4 pb-24">
        </div>

    <script>
        const donors = [
            {n:"মোঃ মেহেদী হাসান", l:"শাখারীকাঠী বাজার", g:"A+", p:"01888354739", last:"জানা নেই", next:"যেকোনো সময়"},
            {n:"শাহিন স্থলপ্রহরী", l:"নুরুল্লাপুর", g:"A+", p:"01893618660", last:"-", next:"01-Mar-2025"},
            {n:"নয়ন শেখ", l:"নুরুল্লাপুর", g:"A+", p:"01609046098", last:"23-May-2025", next:"20-Aug-2025"},
            {n:"কাওসার সিকদার", l:"নুরুল্লাপুর", g:"A+", p:"01835392316", last:"09-Mar-2025", next:"08-Jun-2025"},
            {n:"সুকান্ত দাস", l:"শাখারীকাঠী বাজার", g:"A+", p:"01914432663", last:"07-May-2025", next:"05-Aug-2025"},
            {n:"মোঃ সজীব শেখ", l:"নুরুল্লাপুর", g:"O-", p:"01603319041", last:"11-Mar-2025", next:"10-Jun-2025"},
            {n:"শেখ আলী আজিম", l:"কেশরামপুর", g:"O+", p:"01708642778", last:"31-Mar-2025", next:"30-Jun-2025"},
            {n:"মোঃ রকি শেখ", l:"ছোট খাজুরা", g:"B+", p:"01962393394", last:"21-Mar-2025", next:"20-Jun-2025"},
            {n:"আতিকুর রহমান", l:"ঢুলিগাতী", g:"B+", p:"01947553479", last:"16-Mar-2025", next:"14-Jun-2025"},
            {n:"সাদ্দাম", l:"নরুল্লাপুর", g:"B+", p:"01843443443", last:"16-May-2025", next:"14-Aug-2025"},
            {n:"রুবেল তালুকদার", l:"আমতলা", g:"AB+", p:"01400375835", last:"12-Jun-2025", next:"11-Sep-2025"}
        ];

        function displayDonors(data) {
            const list = document.getElementById('donorList');
            list.innerHTML = "";
            data.forEach(d => {
                list.innerHTML += `
                    <div class="donor-card bg-white p-4 rounded-xl shadow-md border-l-8 border-red-600">
                        <div class="flex justify-between items-center mb-3">
                            <div>
                                <h3 class="font-bold text-lg text-gray-800">${d.n}</h3>
                                <p class="text-xs text-gray-500 italic">📍 ${d.l}</p>
                            </div>
                            <div class="text-center">
                                <p class="text-[10px] text-gray-400 font-bold uppercase">রক্তের গ্রুপ</p>
                                <p class="text-2xl font-black text-red-600 leading-none">${d.g}</p>
                            </div>
                        </div>
                        
                        <div class="flex gap-2 mb-4">
                            <div class="flex-1 bg-gray-100 p-2 rounded text-center">
                                <p class="text-[9px] text-gray-500 uppercase">শেষ রক্তদান</p>
                                <p class="text-xs font-bold text-gray-700">${d.last}</p>
                            </div>
                            <div class="flex-1 bg-red-50 p-2 rounded text-center">
                                <p class="text-[9px] text-gray-400 uppercase">পরবর্তী রক্তদান</p>
                                <p class="text-xs font-bold text-red-600">${d.next}</p>
                            </div>
                        </div>

                        <a href="tel:${d.p}" class="w-full bg-green-600 text-white py-3 rounded-lg font-bold flex justify-center items-center gap-2 hover:bg-green-700">
                            📞 সরাসরি কল করুন
                        </a>
                    </div>
                `;
            });
        }

        function filterDonors() {
            let input = document.getElementById('searchInput').value.toLowerCase();
            let group = document.getElementById('groupFilter').value;
            let filtered = donors.filter(d => (d.n.toLowerCase().includes(input) || d.l.toLowerCase().includes(input)) && (group === "" || d.g === group));
            displayDonors(filtered);
        }

        displayDonors(donors);
    </script>
</body>
</html>
                return matchName && matchGroup;
            });
            displayDonors(filtered);
        }

        displayDonors(donors);
    </script>
</body>
</html>
                let matchName = d.n.toLowerCase().includes(input) || d.l.toLowerCase().includes(input);
                let matchGroup = group === "" || d.g === group;
                return matchName && matchGroup;
            });
            displayDonors(filtered);
        }

        // Initial Load
        displayDonors(donors);
    </script>
</body>
</html>

