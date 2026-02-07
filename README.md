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
        .hero-gradient { background: linear-gradient(135deg, #dc2626 0%, #991b1b 100%); }
        .one-line { overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }
    </style>
</head>
<body class="pb-20">

    <div class="bg-white p-4 shadow-sm border-b-2 border-red-50 text-center flex flex-col items-center">
        <img src="https://i.ibb.co/C3m2X9Y/1000001730.png" class="w-20 h-20 mb-2 rounded-full border-4 border-red-100 shadow-sm">
        <h1 class="text-xl font-black text-red-600 leading-tight">যুব কল্যাণ রক্তদান ফাউন্ডেশন</h1>
        <a href="https://facebook.com/groups/jubokolyan.bdf/" target="_blank" class="mt-2 inline-flex items-center gap-2 bg-blue-600 text-white px-4 py-1.5 rounded-full text-[10px] font-bold shadow-md">
            আমাদের ফেসবুক গ্রুপ
        </a>
    </div>

    <div id="loginPage" class="flex flex-col items-center justify-start min-h-screen px-4 pt-6">
        <div class="bg-white p-6 rounded-[35px] shadow-xl w-full max-w-sm text-center border border-gray-100">
            <h2 class="text-lg font-bold text-gray-800 mb-4">প্যানেলে প্রবেশ করুন</h2>
            
            <div class="flex justify-center gap-2 mb-4 p-1 bg-gray-100 rounded-2xl">
                <button onclick="setRole('Member')" id="roleMember" class="flex-1 py-2 rounded-xl text-[11px] font-bold bg-white text-red-600 shadow-sm">সদস্য</button>
                <button onclick="setRole('Admin')" id="roleAdmin" class="flex-1 py-2 rounded-xl text-[11px] font-bold text-gray-500">এডমিন</button>
            </div>

            <div id="memberFields">
                <input type="tel" id="uPhone" placeholder="মোবাইল নম্বর লিখুন" class="w-full p-3 mb-4 border-2 border-gray-50 rounded-xl outline-none focus:border-red-500 bg-gray-50 text-center font-bold text-sm">
            </div>

            <div id="adminFields" class="hidden">
                <input type="password" id="uPass" placeholder="এডমিন পাসওয়ার্ড দিন" class="w-full p-3 mb-4 border-2 border-gray-50 rounded-xl outline-none focus:border-red-500 bg-gray-50 text-center font-bold text-sm">
            </div>
            
            <button onclick="handleLogin()" id="lBtn" class="w-full bg-red-600 text-white py-3 rounded-xl font-bold shadow-lg active:scale-95 transition-all text-sm">লগইন করুন</button>
            <p id="lErr" class="text-red-500 text-[10px] mt-4 font-bold hidden"></p>

            <div class="mt-6 pt-6 border-t border-gray-50">
                <h3 class="text-[10px] font-black text-gray-700">প্রতিষ্ঠাতা পরিচালকঃ মোঃ মেহেদী হাসান</h3>
                <div class="flex flex-col gap-2 mt-3">
                    <a href="tel:01888354739" class="flex items-center justify-center gap-2 bg-blue-50 text-blue-700 py-2 rounded-xl font-bold text-xs border border-blue-100">📞 01888354739</a>
                </div>
            </div>
        </div>
    </div>

    <div id="mainPage" class="hidden">
        <div class="hero-gradient text-white p-4 rounded-b-[30px] shadow-lg text-center relative mb-4">
            <button onclick="location.reload()" class="absolute top-3 right-3 text-[8px] bg-white/20 px-2 py-1 rounded-full border border-white/30 font-bold">লগ আউট</button>
            <p id="welcome" class="text-yellow-300 text-[11px] font-bold"></p>
        </div>

        <div class="px-4 mb-4">
            <input type="text" id="searchInput" onkeyup="filterDonors()" placeholder="নাম বা রক্তের গ্রুপ দিয়ে খুঁজুন..." class="w-full p-3 rounded-xl border-none shadow-md outline-none focus:ring-2 focus:ring-red-500 font-bold text-xs">
        </div>

        <div id="donorList" class="px-4 grid gap-4"></div>
    </div>

    <div id="editModal" class="fixed inset-0 bg-black/60 hidden flex items-center justify-center p-6 z-[100]">
        <div class="bg-white p-6 rounded-[30px] w-full max-w-sm shadow-2xl">
            <h3 id="editingName" class="font-bold text-gray-800 text-base"></h3>
            <input type="date" id="newDate" class="w-full p-3 border rounded-xl mb-4 mt-2 bg-gray-50 outline-none font-bold text-center text-sm">
            <div class="flex gap-2">
                <button onclick="closeEdit()" class="flex-1 bg-gray-100 py-2 rounded-lg font-bold text-xs">বাতিল</button>
                <button onclick="submitUpdate()" id="sBtn" class="flex-1 bg-green-600 text-white py-2 rounded-lg font-bold text-xs">সেভ</button>
            </div>
        </div>
    </div>

    <script>
        const scriptURL = "https://script.google.com/macros/s/AKfycbx2AUrExnRv7h2cMRlBv6nqpf3oNy5s9Bh0iKzGG3-5YhGC1NGDEWN7lsRmuaEHo92o/exec"; 
        let allDonors = [], loggedUser = null, currentRole = 'Member', targetPhone = "";

        function setRole(role) {
            currentRole = role;
            const mFields = document.getElementById('memberFields'), aFields = document.getElementById('adminFields');
            const mBtn = document.getElementById('roleMember'), aBtn = document.getElementById('roleAdmin');
            if(role === 'Admin') {
                aFields.classList.remove('hidden'); mFields.classList.add('hidden');
                aBtn.className = "flex-1 py-2 rounded-xl text-[11px] font-bold bg-white text-red-600 shadow-sm";
                mBtn.className = "flex-1 py-2 rounded-xl text-[11px] font-bold text-gray-500";
            } else {
                aFields.classList.add('hidden'); mFields.classList.remove('hidden');
                mBtn.className = "flex-1 py-2 rounded-xl text-[11px] font-bold bg-white text-red-600 shadow-sm";
                aBtn.className = "flex-1 py-2 rounded-xl text-[11px] font-bold text-gray-500";
            }
        }

        async function handleLogin() {
            const phone = document.getElementById('uPhone').value.trim();
            const pass = document.getElementById('uPass').value.trim();
            const err = document.getElementById('lErr');
            err.innerText = "⏳ ডাটা যাচাই হচ্ছে..."; err.classList.remove('hidden');
            try {
                const res = await fetch(scriptURL);
                allDonors = await res.json();
                if(currentRole === 'Admin') {
                    if(pass === 'Mehedi4739') {
                        loggedUser = { n: "অ্যাডমিন প্যানেল", role: "Admin" }; showMain();
                    } else { err.innerText = "❌ ভুল পাসওয়ার্ড!"; }
                } else {
                    const member = allDonors.find(d => String(d.p).slice(-10) === phone.slice(-10));
                    if(member) {
                        loggedUser = { ...member, role: "Member" }; showMain();
                    } else { err.innerText = "❌ নম্বরটি সঠিক নয়!"; }
                }
            } catch (e) { err.innerText = "❌ সার্ভার এরর!"; }
        }

        function showMain() {
            document.getElementById('loginPage').classList.add('hidden');
            document.getElementById('mainPage').classList.remove('hidden');
            document.getElementById('welcome').innerText = "স্বাগতম: " + loggedUser.n;
            renderDonors(allDonors);
        }

        function filterDonors() {
            const term = document.getElementById('searchInput').value.toLowerCase();
            const filtered = allDonors.filter(d => d.n.toLowerCase().includes(term) || d.g.toLowerCase().includes(term) || d.l.toLowerCase().includes(term));
            renderDonors(filtered);
        }

        function renderDonors(data) {
            const list = document.getElementById('donorList');
            list.innerHTML = "";
            data.forEach((d, index) => {
                const isMe = (loggedUser.role === 'Member' && String(d.p).slice(-10) === String(loggedUser.p).slice(-10));
                const isAdmin = (loggedUser.role === 'Admin');
                const status = calculateStatus(d.last);
                list.innerHTML += `
                <div class="bg-white p-4 rounded-[25px] shadow-sm border ${isMe ? 'border-green-400 bg-green-50/10' : 'border-gray-100'} relative">
                    <span class="absolute -top-1 left-6 bg-gray-800 text-white text-[8px] px-2 py-0.5 rounded-b-md font-bold">SL: ${d.sl || (index+1)}</span>
                    <div class="flex items-start gap-3 mt-2">
                        <div class="bg-red-600 text-white min-w-[45px] h-[45px] rounded-2xl flex items-center justify-center font-black text-lg shadow-md">${d.g}</div>
                        
                        <div class="flex-1 min-w-0">
                            <h4 class="font-bold text-gray-800 text-sm one-line">${d.n}</h4>
                            <p class="text-[10px] text-gray-500 one-line">📍 ${d.l}</p>
                            <div class="flex items-center gap-2 mt-1">
                                <span class="text-[11px] font-bold text-blue-600">${d.p}</span>
                                <a href="tel:${d.p}" class="bg-green-500 text-white p-1 rounded-full shadow-sm">
                                    <svg xmlns="http://www.w3.org/2000/svg" class="h-3 w-3" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                      <path stroke-linecap="round" stroke-linejoin="round" stroke-width="3" d="M3 5a2 2 0 012-2h3.28a1 1 0 01.948.684l1.498 4.493a1 1 0 01-.502 1.21l-2.257 1.13a11.042 11.042 0 005.516 5.516l1.13-2.257a1 1 0 011.21-.502l4.493 1.498a1 1 0 01.684.949V19a2 2 0 01-2 2h-1C9.716 21 3 14.284 3 6V5z" />
                                    </svg>
                                </a>
                            </div>
                        </div>
                        
                        ${(isMe || isAdmin) ? `<button onclick="openEdit('${d.p}', '${d.n}')" class="bg-blue-600 px-2 py-1 rounded-lg text-white font-bold text-[9px] h-fit">এডিট</button>` : ''}
                    </div>

                    <div class="grid grid-cols-2 gap-2 mt-3">
                        <div class="bg-gray-50 border border-gray-100 rounded-xl p-2 text-center">
                            <p class="text-[8px] font-bold text-gray-400 uppercase">সর্বশেষ দান</p>
                            <p class="text-[10px] font-black text-gray-700">${(d.last && d.last !== "undefined") ? d.last : "তারিখ নেই"}</p>
                        </div>
                        <div class="${status.color} border border-opacity-30 rounded-xl p-2 text-center flex flex-col justify-center">
                            <p class="text-[8px] font-bold opacity-60 uppercase">স্ট্যাটাস</p>
                            <p class="text-[9px] font-black leading-tight ${status.textColor}">${status.txt}</p>
                        </div>
                    </div>
                </div>`;
            });
        }

        function openEdit(phone, name) { targetPhone = phone; document.getElementById('editingName').innerText = name; document.getElementById('editModal').classList.remove('hidden'); }
        function closeEdit() { document.getElementById('editModal').classList.add('hidden'); }

        async function submitUpdate() {
            const date = document.getElementById('newDate').value;
            if(!date) return alert("তারিখ দিন!");
            document.getElementById('sBtn').innerText = "হচ্ছে..";
            try {
                await fetch(scriptURL, { method: 'POST', body: JSON.stringify({ phone: targetPhone, newDate: date }) });
                alert("সফল!"); location.reload();
            } catch (e) { alert("ব্যর্থ!"); }
        }

        function calculateStatus(dateStr) {
            if(!dateStr || dateStr === "তারিখ নেই" || dateStr === "undefined" || dateStr === "") {
                return { txt: "আপডেট প্রয়োজন", color: "bg-orange-100 border-orange-200", textColor: "text-orange-700" };
            }
            const last = new Date(dateStr);
            const diffDays = Math.floor((new Date() - last) / (1000 * 60 * 60 * 24));
            if (diffDays >= 90) return { txt: "রক্ত দিতে পারবে ✅", color: "bg-green-100 border-green-200", textColor: "text-green-700" };
            return { txt: (90 - diffDays) + " দিন বাকি", color: "bg-red-50 border-red-100", textColor: "text-red-600" };
        }
    </script>
</body>
</html>
