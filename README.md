<!DOCTYPE html>
<html lang="bn">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>যুব কল্যাণ রক্তদান ফাউন্ডেশন - মেম্বার প্যানেল</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Hind+Siliguri:wght@400;600;700&display=swap" rel="stylesheet">
</head>
<body class="bg-slate-50 font-['Hind_Siliguri']">

    <div id="loginSection" class="min-h-screen flex items-center justify-center px-4">
        <div class="bg-white p-8 rounded-3xl shadow-xl w-full max-w-sm border border-gray-100">
            <div class="text-center mb-8">
                <img src="logo.png" onerror="this.src='https://i.ibb.co/C3m2X9Y/1000001730.png'" class="w-20 h-20 mx-auto mb-4 rounded-full border-4 border-red-50">
                <h2 class="text-2xl font-bold text-gray-800">সদস্য লগইন</h2>
                <p class="text-xs text-gray-400 mt-1">শিটে দেওয়া মোবাইল নম্বর ব্যবহার করুন</p>
            </div>

            <div class="space-y-4">
                <input type="tel" id="phoneInput" placeholder="মোবাইল নম্বর (যেমন: 017...)" class="w-full p-4 border rounded-2xl outline-none focus:border-red-500 bg-gray-50">
                <input type="password" id="passInput" placeholder="পাসওয়ার্ড (JKBDF)" class="w-full p-4 border rounded-2xl outline-none focus:border-red-500 bg-gray-50">
                <button onclick="checkLogin()" id="loginBtn" class="w-full bg-red-600 text-white py-4 rounded-2xl font-bold shadow-lg hover:bg-red-700 transition-all">লগইন করুন</button>
            </div>
            <p id="msg" class="text-center text-xs mt-4 font-bold hidden"></p>
        </div>
    </div>

    <div id="appSection" class="hidden min-h-screen pb-10">
        <div class="bg-red-600 p-8 rounded-b-[40px] text-white text-center shadow-lg">
            <h1 class="text-xl font-bold">যুব কল্যাণ রক্তদান ফাউন্ডেশন</h1>
            <p id="userName" class="text-yellow-300 text-sm mt-2 font-bold"></p>
            <button onclick="location.reload()" class="mt-4 text-[10px] bg-white/20 px-3 py-1 rounded-full border border-white/30">লগ আউট</button>
        </div>

        <div class="mx-6 -mt-6 bg-white p-6 rounded-[30px] shadow-2xl relative z-10 border border-red-50">
            <h3 class="text-sm font-bold text-gray-700 mb-4 tracking-wide">রক্তদানের তারিখ আপডেট করুন</h3>
            <input type="date" id="dateInput" class="w-full p-4 border rounded-2xl mb-4 bg-gray-50 font-bold outline-none">
            <button onclick="updateLastDate()" id="saveBtn" class="w-full bg-green-600 text-white py-4 rounded-2xl font-bold shadow-md">তথ্য সেভ করুন</button>
        </div>

        <div id="donorContainer" class="p-6 grid gap-4"></div>
    </div>

    <script>
        // আপনার Apps Script এর লিঙ্কটি এখানে বসান
        const scriptURL = "আপনার_নতুন_DEPLOY_URL_এখানে";
        
        let allDonors = [];
        let loggedUser = null;

        async function checkLogin() {
            const phone = document.getElementById('phoneInput').value.trim();
            const pass = document.getElementById('passInput').value.trim();
            const msg = document.getElementById('msg');
            const btn = document.getElementById('loginBtn');

            if (pass !== "JKBDF") {
                msg.innerText = "❌ পাসওয়ার্ড ভুল! সঠিক পাসওয়ার্ড JKBDF দিন।";
                msg.className = "text-center text-xs mt-4 font-bold text-red-500";
                msg.classList.remove('hidden');
                return;
            }

            try {
                btn.innerText = "যাচাই হচ্ছে...";
                btn.disabled = true;

                const response = await fetch(scriptURL);
                allDonors = await response.json();

                // শিটে থাকা 'p' (Phone) এর সাথে ইনপুট মিলানো
                loggedUser = allDonors.find(u => String(u.p).trim() === phone);

                if (loggedUser) {
                    document.getElementById('loginSection').classList.add('hidden');
                    document.getElementById('appSection').classList.remove('hidden');
                    document.getElementById('userName').innerText = "স্বাগতম, " + loggedUser.n;
                    renderList();
                } else {
                    msg.innerText = "❌ এই মোবাইল নম্বরটি ডাটাবেজে নেই।";
                    msg.className = "text-center text-xs mt-4 font-bold text-red-500";
                    msg.classList.remove('hidden');
                }
            } catch (e) {
                alert("সার্ভার কানেকশন এরর! Apps Script লিঙ্কটি চেক করুন।");
            } finally {
                btn.innerText = "লগইন করুন";
                btn.disabled = false;
            }
        }

        async function updateLastDate() {
            const date = document.getElementById('dateInput').value;
            const btn = document.getElementById('saveBtn');
            if(!date) return alert("দয়া করে তারিখ দিন");

            btn.innerText = "আপডেট হচ্ছে...";
            btn.disabled = true;

            try {
                const response = await fetch(scriptURL, {
                    method: 'POST',
                    body: JSON.stringify({ phone: loggedUser.p, newDate: date })
                });
                alert("সফলভাবে আপডেট হয়েছে!");
                location.reload();
            } catch (e) {
                alert("আপডেট ব্যর্থ হয়েছে!");
                btn.innerText = "তথ্য সেভ করুন";
                btn.disabled = false;
            }
        }

        function renderList() {
            const container = document.getElementById('donorContainer');
            container.innerHTML = "";
            allDonors.forEach(d => {
                container.innerHTML += `
                    <div class="bg-white p-5 rounded-3xl shadow-sm border border-gray-100 flex justify-between items-center">
                        <div class="flex items-center gap-3">
                            <div class="bg-red-600 text-white w-10 h-10 rounded-xl flex items-center justify-center font-bold text-sm">${d.g}</div>
                            <div>
                                <p class="text-sm font-bold text-gray-800">${d.n}</p>
                                <p class="text-[10px] text-gray-400">📍 ${d.l}</p>
                            </div>
                        </div>
                        <p class="text-[10px] font-bold text-gray-300 italic">${d.last || 'তারিখ নেই'}</p>
                    </div>`;
            });
        }
    </script>
</body>
</html>

                if (activeUser) {
                    // যদি মেম্বার হয় তবে চেক করা সে কি মেম্বার কি না (শিটে Role কলাম অনুযায়ী)
                    document.getElementById('loginPage').classList.add('hidden');
                    document.getElementById('mainPage').classList.remove('hidden');
                    document.getElementById('userWelcome').innerText = (role === 'Admin' ? "👑 এডমিন: " : "👋 সদস্য: ") + activeUser.n;
                    
                    // সদস্য এবং এডমিন উভয়েই তারিখ আপডেট করতে পারবে
                    document.getElementById('updateSection').classList.remove('hidden');
                    renderDonors();
                } else {
                    error.innerText = "❌ এই নম্বরটি আমাদের ডাটাবেজে নেই!";
                    error.classList.remove('hidden');
                }
            } catch (e) {
                alert("সার্ভার ত্রুটি! ইন্টারনাল কানেকশন চেক করুন।");
            }
        }

        // তারিখ আপডেট সাবমিট
        async function submitUpdate() {
            const newDate = document.getElementById('dateInput').value;
            const btn = document.getElementById('submitBtn');
            if(!newDate) return alert("অনুগ্রহ করে তারিখ সিলেক্ট করুন");

            btn.disabled = true;
            btn.innerText = "আপডেট হচ্ছে...";

            const payload = { phone: activeUser.p, newDate: newDate };

            try {
                await fetch(apiURL, { method: 'POST', body: JSON.stringify(payload) });
                alert("সফলভাবে আপডেট করা হয়েছে!");
                location.reload();
            } catch (e) {
                alert("আপডেট ব্যর্থ হয়েছে!");
                btn.disabled = false;
                btn.innerText = "সেভ করুন";
            }
        }

        // ডোনার লিস্ট রেন্ডার
        function renderDonors() {
            const query = document.getElementById('search').value.toLowerCase();
            const list = document.getElementById('listContainer');
            list.innerHTML = "";

            const filtered = allData.filter(d => d.n.toLowerCase().includes(query) || d.l.toLowerCase().includes(query));
