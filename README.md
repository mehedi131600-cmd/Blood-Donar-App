<!DOCTYPE html>
<html lang="bn">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>যুব কল্যাণ রক্তদান ফাউন্ডেশন - প্যানেল</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Hind+Siliguri:wght@400;600;700&display=swap" rel="stylesheet">
    <style>
        body { font-family: 'Hind Siliguri', sans-serif; background-color: #f8fafc; }
        .hero-gradient { background: linear-gradient(135deg, #dc2626 0%, #991b1b 100%); }
    </style>
</head>
<body class="pb-10">

    <div id="loginPage" class="flex flex-col items-center justify-center min-h-screen px-6">
        <div class="bg-white p-8 rounded-[40px] shadow-2xl w-full max-w-sm text-center border border-gray-100">
            <img src="https://i.ibb.co/C3m2X9Y/1000001730.png" class="w-24 h-24 mx-auto mb-6 rounded-full border-4 border-red-50 shadow-md">
            <h2 class="text-2xl font-bold text-gray-800 mb-2">সদস্য লগইন</h2>
            <p class="text-xs text-gray-400 mb-8 font-medium">শিটে দেওয়া মোবাইল নম্বর দিয়ে প্রবেশ করুন</p>
            
            <input type="tel" id="uPhone" placeholder="মোবাইল নম্বর লিখুন" class="w-full p-4 mb-6 border-2 border-gray-100 rounded-2xl outline-none focus:border-red-500 bg-gray-50 text-center font-bold text-lg transition-all">
            
            <button onclick="handleLogin()" id="lBtn" class="w-full bg-red-600 text-white py-4 rounded-2xl font-bold shadow-lg active:scale-95 transition-all">প্রবেশ করুন</button>
            <p id="lErr" class="text-red-500 text-[10px] mt-4 font-bold hidden"></p>
        </div>
    </div>

    <div id="mainPage" class="hidden">
        <div class="hero-gradient text-white p-8 rounded-b-[50px] shadow-lg text-center relative mb-8">
            <button onclick="location.reload()" class="absolute top-5 right-5 text-[10px] bg-white/20 px-3 py-1 rounded-full border border-white/30 font-bold">লগ আউট</button>
            <h1 class="text-xl font-bold uppercase tracking-tight">যুব কল্যাণ রক্তদান ফাউন্ডেশন</h1>
            <p id="welcome" class="text-yellow-300 text-sm mt-2 font-bold"></p>
        </div>

        <div id="updateBox" class="mx-6 bg-white p-6 rounded-[35px] shadow-xl border-t-4 border-green-500 relative z-10 mb-8">
            <div class="flex items-center gap-2 mb-4">
                <span class="bg-green-100 p-2 rounded-full text-green-600 text-sm">📝</span>
                <h3 class="text-sm font-bold text-gray-700">রক্তদানের তারিখ আপডেট করুন</h3>
            </div>
            <input type="date" id="newDate" class="w-full p-4 border-2 border-gray-50 rounded-2xl mb-4 bg-gray-50 outline-none font-bold text-gray-700 text-center">
            <button onclick="updateMyDate()" id="sBtn" class="w-full bg-green-600 text-white py-4 rounded-2xl font-bold shadow-md active:scale-95 transition-all">তথ্য সেভ করুন</button>
        </div>

        <div class="px-6 mb-4">
            <h3 class="text-sm font-black text-gray-400 uppercase tracking-widest">ডোনার তালিকা</h3>
        </div>

        <div id="donorList" class="px-6 grid gap-4"></div>
    </div>

    <script>
        // আপনার দেওয়া নতুন স্ক্রিপ্ট URL
        const scriptURL = "https://script.google.com/macros/s/AKfycbzSSOHmngHKEwy9ygE0ZXAT-ApK2_v8-RbdsTgagBUHVpAjb73Ro5pb80d0qpgedSWy/exec"; 
        
        let allDonors = [];
        let loggedUser = null;

        async function handleLogin() {
            const phone = document.getElementById('uPhone').value.trim();
            const err = document.getElementById('lErr');
            const btn = document.getElementById('lBtn');

            if(!phone) { alert("মোবাইল নম্বর লিখুন!"); return; }

            err.innerText = "⏳ ডাটা যাচাই হচ্ছে...";
            err.classList.remove('hidden');
            btn.disabled = true;

            try {
                const res = await fetch(scriptURL);
                allDonors = await res.json();
                
                // শিটের ফোন কলামের (E) শেষ ১০ সংখ্যা মিলিয়ে দেখা হচ্ছে
                loggedUser = allDonors.find(d => String(d.p).slice(-10) === phone.slice(-10));

                if(loggedUser) {
                    document.getElementById('loginPage').classList.add('hidden');
                    document.getElementById('mainPage').classList.remove('hidden');
                    document.getElementById('welcome').innerText = "স্বাগতম, " + loggedUser.n;
                    renderDonors();
                } else {
                    err.innerText = "❌ এই নম্বরটি আমাদের তালিকায় নেই!";
                }
            } catch (e) {
                err.innerText = "❌ সার্ভার সমস্যা! Apps Script এ 'Anyone' এক্সেস চেক করুন।";
            } finally {
                btn.disabled = false;
                btn.innerText = "প্রবেশ করুন";
            }
        }

        async function updateMyDate() {
            const date = document.getElementById('newDate').value;
            const btn = document.getElementById('sBtn');
            if(!date) return alert("দয়া করে তারিখ সিলেক্ট করুন!");

            btn.disabled = true;
            btn.innerText = "সেভ হচ্ছে...";

            try {
                const response = await fetch(scriptURL, { 
                    method: 'POST', 
                    body: JSON.stringify({ phone: loggedUser.p, newDate: date }) 
                });
                alert("আপনার রক্তদানের তারিখ সফলভাবে সেভ হয়েছে!");
                location.reload();
            } catch (e) {
                alert("ব্যর্থ হয়েছে! আবার চেষ্টা করুন।");
                btn.disabled = false;
                btn.innerText = "তথ্য সেভ করুন";
            }
        }

        function renderDonors() {
            const list = document.getElementById('donorList');
            list.innerHTML = "";
            
            allDonors.forEach(d => {
                const isMe = (String(d.p).slice(-10) === String(loggedUser.p).slice(-10));
                const status = calculateStatus(d.last);
                
                list.innerHTML += `
                <div class="bg-white p-5 rounded-[30px] shadow-sm border ${isMe ? 'border-green-300 ring-2 ring-green-50' : 'border-gray-50'} flex justify-between items-center relative">
                    ${isMe ? '<span class="absolute -top-2 left-6 bg-green-500 text-white text-[8px] px-2 py-0.5 rounded-full font-bold uppercase tracking-widest shadow-sm">আপনি</span>' : ''}
                    <div class="flex items-center gap-4">
                        <div class="bg-red-600 text-white w-12 h-12 rounded-2xl flex items-center justify-center font-black text-lg shadow-md">${d.g}</div>
                        <div>
                            <h4 class="font-bold text-gray-800 text-sm">${d.n}</h4>
                            <p class="text-[10px] text-gray-400 font-medium tracking-wide">📍 ${d.l}</p>
                        </div>
                    </div>
                    <div class="text-right">
                        <p class="text-[10px] font-bold ${status.can ? 'text-green-600' : 'text-red-500'}">${status.txt}</p>
                        <p class="text-[9px] text-gray-300 italic">${d.last}</p>
                    </div>
                </div>`;
            });
        }

        function calculateStatus(dateStr) {
            if(!dateStr || dateStr === "তারিখ নেই" || dateStr === "undefined") return { txt: "রক্ত দিতে পারবে", can: true };
            // তারিখ ফরম্যাট dd MMM yyyy থেকে রূপান্তর
            const last = new Date(dateStr);
            if (isNaN(last)) return { txt: "রক্ত দিতে পারবে", can: true };
            const diff = Math.floor((new Date() - last) / (1000*60*60*24));
            return diff >= 90 ? { txt: "রক্ত দিতে পারবে", can: true } : { txt: (90-diff) + " দিন বাকি", can: false };
        }
    </script>
</body>
</html>
