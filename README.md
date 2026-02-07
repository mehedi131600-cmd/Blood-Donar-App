<!DOCTYPE html>
<html lang="bn">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>যুব কল্যাণ রক্তদান ফাউন্ডেশন - ডোনার লিস্ট</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Hind+Siliguri:wght@400;600&display=swap" rel="stylesheet">
    <style>
        body { font-family: 'Hind Siliguri', sans-serif; background-color: #f3f4f6; }
        .donor-card { transition: transform 0.2s; }
        .donor-card:active { transform: scale(0.98); }
    </style>
</head>
<body>

    <div class="bg-red-600 text-white p-6 text-center shadow-lg sticky top-0 z-20">
        <h1 class="text-2xl font-bold">যুব কল্যাণ রক্তদান ফাউন্ডেশন</h1>
        <p class="text-sm opacity-90 font-bold">ডোনার তালিকা ও রক্তদানের সময়সূচী</p>
    </div>

    <div class="p-4 bg-white shadow-md sticky top-[88px] z-10">
        <input type="text" id="searchInput" onkeyup="filterDonors()" placeholder="নাম বা এলাকা লিখে খুঁজুন..." class="w-full p-3 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-red-500 mb-3">
        
        <select id="groupFilter" onchange="filterDonors()" class="w-full p-3 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-red-500 font-bold text-red-600">
            <option value="">সব রক্তের গ্রুপ</option>
            <option value="A+">A+</option>
            <option value="B+">B+</option>
            <option value="O+">O+</option>
            <option value="O-">O-</option>
            <option value="AB+">AB+</option>
        </select>
    </div>

    <div id="donorList" class="p-4 grid gap-4 pb-20">
        </div>

    <script>
        // ডোনার লিস্ট (তথ্য যোগ করার জায়গা)
        // n = নাম, l = এলাকা, g = গ্রুপ, p = ফোন, last = শেষ রক্তদান, next = পরবর্তী রক্তদান
        const donors = [
            {n:"শাহিন স্থলপ্রহরী", l:"নুরুল্লাপুর", g:"A+", p:"01893618660", last:"N/A", next:"01-Mar-2025"},
            {n:"নয়ন শেখ", l:"নুরুল্লাপুর", g:"A+", p:"01609046098", last:"23-May-2025", next:"20-Aug-2025"},
            {n:"কাওসার সিকদার", l:"নুরুল্লাপুর", g:"A+", p:"01835392316", last:"09-Mar-2025", next:"08-Jun-2025"},
            {n:"সুকান্ত দাস", l:"শাখারীকাঠী বাজার", g:"A+", p:"01914432663", last:"07-May-2025", next:"05-Aug-2025"},
            {n:"মোঃ সজীব শেখ", l:"নুরুল্লাপুর", g:"O-", p:"01603319041", last:"11-Mar-2025", next:"10-Jun-2025"},
            {n:"শেখ আলী আজিম", l:"কেশরামপুর", g:"O+", p:"01708642778", last:"31-Mar-2025", next:"30-Jun-2025"},
            {n:"মোঃ রকি শেখ", l:"ছোট খাজুরা", g:"B+", p:"01962393394", last:"21-Mar-2025", next:"20-Jun-2025"},
            {n:"আতিকুর রহমান", l:"ঢুলিগাতী", g:"B+", p:"01947553479", last:"16-Mar-2025", next:"14-Jun-2025"},
            {n:"সাদ্দাম", l:"নরুল্লাপুর", g:"B+", p:"01843443443", last:"16-May-2025", next:"14-Aug-2025"},
            {n:"রুবেল তালুকদার", l:"আমতলা", g:"AB+", p:"01400375835", last:"12-Jun-2025", next:"11-Sep-2025"},
            // নতুন ডোনার যুক্ত করতে নিচের লাইনটি কপি করে তথ্য বসান
            {n:"মোঃ মেহেদী হাসান", l:"শাখারীকাঠী বাজার", g:"A+", p:"01888354739", last:"N/A", next:"যেকোনো সময়"}
        ];

        function displayDonors(data) {
            const list = document.getElementById('donorList');
            list.innerHTML = "";
            data.forEach(d => {
                list.innerHTML += `
                    <div class="donor-card bg-white p-4 rounded-xl shadow-sm border-l-8 border-red-500 flex flex-col gap-3">
                        <div class="flex justify-between items-start">
                            <div>
                                <h3 class="font-bold text-lg text-gray-800">${d.n}</h3>
                                <p class="text-sm text-gray-600 font-semibold">গ্রুপ: <span class="text-red-600 text-xl">${d.g}</span></p>
                                <p class="text-xs text-gray-500 italic">📍 ${d.l}</p>
                            </div>
                            <a href="tel:${d.p}" class="bg-green-500 text-white p-3 rounded-full shadow-md flex items-center justify-center">
                                📞
                            </a>
                        </div>
                        
                        <div class="grid grid-cols-2 gap-2 mt-2 pt-2 border-t border-gray-100">
                            <div class="text-center bg-gray-50 p-2 rounded-lg">
                                <p class="text-[10px] text-gray-500 uppercase">শেষ রক্তদান</p>
                                <p class="text-sm font-bold text-blue-600">${d.last || 'জানা নেই'}</p>
                            </div>
                            <div class="text-center bg-red-50 p-2 rounded-lg">
                                <p class="text-[10px] text-gray-500 uppercase">পরবর্তী রক্তদান</p>
                                <p class="text-sm font-bold text-red-600">${d.next || 'যেকোনো সময়'}</p>
                            </div>
                        </div>
                    </div>
                `;
            });
        }

        function filterDonors() {
            let input = document.getElementById('searchInput').value.toLowerCase();
            let group = document.getElementById('groupFilter').value;
            
            let filtered = donors.filter(d => {
                let matchName = d.n.toLowerCase().includes(input) || d.l.toLowerCase().includes(input);
                let matchGroup = group === "" || d.g === group;
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

