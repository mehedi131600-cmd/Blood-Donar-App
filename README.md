<!DOCTYPE html>
<html lang="bn">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>যুব কল্যাণ রক্তদান ফাউন্ডেশন - মেম্বার পোর্টাল</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Hind+Siliguri:wght@400;600;700&display=swap" rel="stylesheet">
    <style>body { font-family: 'Hind Siliguri', sans-serif; background-color: #f8fafc; }</style>
</head>
<body>

    <div id="loginPage" class="flex flex-col items-center justify-center min-h-screen px-6">
        <div class="bg-white p-8 rounded-[40px] shadow-2xl w-full max-w-sm text-center">
            <img src="logo.png" onerror="this.src='https://i.ibb.co/C3m2X9Y/1000001730.png'" class="w-20 h-20 mx-auto mb-4 rounded-full border-4 border-red-50">
            <h2 class="text-2xl font-bold text-gray-800 mb-6">সদস্য লগইন</h2>
            <input type="tel" id="uPhone" placeholder="মোবাইল নম্বর লিখুন" class="w-full p-4 mb-6 border rounded-2xl outline-none focus:border-red-500 bg-gray-50 text-center font-bold">
            <button onclick="handleLogin()" id="lBtn" class="w-full bg-red-600 text-white py-4 rounded-2xl font-bold shadow-lg">লগইন করুন</button>
            <p id="lErr" class="text-red-500 text-xs mt-4 font-bold hidden"></p>
        </div>
    </div>

    <div id="mainPage" class="hidden">
        <div class="bg-red-600 text-white p-8 rounded-b-[40px] shadow-lg text-center relative mb-8">
            <button onclick="location.reload()" class="absolute top-5 right-5 text-[10px] bg-white/20 px-3 py-1 rounded-full border border-white/30">লগ আউট</button>
            <h1 class="text-lg font-bold">যুব কল্যাণ রক্তদান ফাউন্ডেশন</h1>
            <p id="welcome" class="text-yellow-300 text-sm mt-2 font-bold"></p>
        </div>

        <div id="list" class="p-4 grid gap-4"></div>
    </div>

    <div id="editBox" class="fixed inset-0 bg-black/50 hidden flex items-center justify-center p-6 z-50">
        <div class="bg-white p-6 rounded-[30px] w-full max-w-sm shadow-2xl">
            <h3 class="font-bold text-gray-700 mb-4 text-center">রক্তদানের তারিখ আপডেট করুন</h3>
            <input type="date" id="newDate" class="w-full p-4 border rounded-2xl mb-4 bg-gray-50 outline-none font-bold">
            <div class="flex gap-2">
                <button onclick="closeEdit()" class="flex-1 bg-gray-100 py-3 rounded-xl font-bold text-sm">বাতিল</button>
                <button onclick="updateData()" id="sBtn" class="flex-1 bg-green-600 text-white py-3 rounded-xl font-bold text-sm">সেভ করুন</button>
            </div>
        </div>
    </div>

    <script>
        const apiURL = "আপনার_নতুন_DEPLOY_URL_এখানে"; 
        let allDonors = [];
        let loggedUser = null;

        async function handleLogin() {
            const phone = document.getElementById('uPhone').value.trim();
            const err = document.getElementById('lErr');
            if(!phone) return alert("নম্বর দিন!");
            err.innerText = "⏳ যাচাই হচ্ছে..."; err.classList.remove('hidden');

            try {
                const res = await fetch(apiURL);
                allDonors = await res.json();
                loggedUser = allDonors.find(d => String(d.p).slice(-10) === phone.slice(-10));

                if(loggedUser) {
                    document.getElementById('loginPage').classList.add('hidden');
                    document.getElementById('mainPage').classList.remove('hidden');
                    document.getElementById('welcome').innerText = "স্বাগতম, " + loggedUser.n;
                    renderList();
                } else { err.innerText = "❌ এই নম্বরটি তালিকায় নেই!"; }
            } catch (e) { err.innerText = "❌ সার্ভার কানেকশন এরর!"; }
        }

        function renderList() {
            const container = document.getElementById('list');
            container.innerHTML = "";
            allDonors.forEach(d => {
                const isMe = (d.p === loggedUser.p);
                const status = checkStatus(d.last);
                container.innerHTML += `
                <div class="bg-white p-5 rounded-[30px] shadow-sm border ${isMe ? 'border-red-200' : 'border-gray-100'} flex justify-between items-center relative">
                    <div class="flex items-center gap-3">
                        <div class="bg-red-600 text-white w-10 h-10 rounded-xl flex items-center justify-center font-bold text-sm">${d.g}</div>
                        <div>
                            <h4 class="font-bold text-gray-800 text-sm">${d.n} ${isMe ? '<span class="text-[8px] bg-red-100 text-red-600 px-1 rounded">আপনি</span>' : ''}</h4>
                            <p class="text-[9px] text-gray-400">📍 ${d.l}</p>
                        </div>
                    </div>
                    <div class="text-right flex flex-col items-end gap-1">
                        <p class="text-[9px] font-bold ${status ? 'text-green-600' : 'text-red-500'}">${status ? 'রক্ত দিতে পারবে' : 'অপেক্ষা করুন'}</p>
                        <p class="text-[8px] text-gray-400 italic">শেষ: ${d.last}</p>
                        ${isMe ? `<button onclick="openEdit()" class="bg-blue-50 text-blue-600 text-[9px] font-bold px-3 py-1 rounded-full border border-blue-100">এডিট করুন</button>` : ''}
                    </div>
                </div>`;
            });
        }

        function checkStatus(dateStr) {
            if(!dateStr || dateStr === "তারিখ নেই") return true;
            const diff = Math.floor((new Date() - new Date(dateStr)) / (1000*60*60*24));
            return diff >= 90;
        }

        function openEdit() { document.getElementById('editBox').classList.remove('hidden'); }
        function closeEdit() { document.getElementById('editBox').classList.add('hidden'); }

        async function updateData() {
            const date = document.getElementById('newDate').value;
            const btn = document.getElementById('sBtn');
            if(!date) return alert("তারিখ দিন!");
            btn.disabled = true; btn.innerText = "সেভ হচ্ছে...";

            try {
                await fetch(apiURL, { method: 'POST', body: JSON.stringify({ phone: loggedUser.p, newDate: date }) });
                alert("সফলভাবে আপডেট হয়েছে!"); location.reload();
            } catch (e) { alert("ব্যর্থ হয়েছে!"); btn.disabled = false; btn.innerText = "সেভ করুন"; }
        }
    </script>
</body>
</html>
