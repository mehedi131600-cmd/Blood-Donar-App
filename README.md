<!DOCTYPE html>
<html lang="bn">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>যুব কল্যাণ রক্তদান ফাউন্ডেশন - অ্যাডমিন ও মেম্বার পোর্টাল</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Hind+Siliguri:wght@400;600;700&display=swap" rel="stylesheet">
    <style>
        body { font-family: 'Hind Siliguri', sans-serif; background-color: #f1f5f9; }
        .hero-gradient { background: linear-gradient(135deg, #dc2626 0%, #991b1b 100%); }
    </style>
</head>
<body class="pb-20">

    <div id="loginPage" class="flex flex-col items-center justify-center min-h-screen px-6">
        <div class="bg-white p-8 rounded-[40px] shadow-2xl w-full max-w-sm text-center border border-gray-100">
            <img src="https://i.ibb.co/C3m2X9Y/1000001730.png" class="w-20 h-20 mx-auto mb-4 rounded-full border-4 border-red-50">
            <h2 class="text-2xl font-bold text-gray-800 mb-2">প্রবেশ করুন</h2>
            
            <div class="flex justify-center gap-4 mb-6 mt-4">
                <button onclick="setRole('Member')" id="roleMember" class="px-4 py-2 rounded-full text-xs font-bold bg-red-600 text-white shadow-md">সদস্য</button>
                <button onclick="setRole('Admin')" id="roleAdmin" class="px-4 py-2 rounded-full text-xs font-bold bg-gray-100 text-gray-500 border border-gray-200">এডমিন</button>
            </div>

            <input type="tel" id="uPhone" placeholder="মোবাইল নম্বর লিখুন" class="w-full p-4 mb-3 border-2 border-gray-50 rounded-2xl outline-none focus:border-red-500 bg-gray-50 text-center font-bold">
            <input type="password" id="uPass" placeholder="এডমিন পাসওয়ার্ড" class="hidden w-full p-4 mb-6 border-2 border-gray-50 rounded-2xl outline-none focus:border-red-500 bg-gray-50 text-center font-bold">
            
            <button onclick="handleLogin()" id="lBtn" class="w-full bg-red-600 text-white py-4 rounded-2xl font-bold shadow-lg active:scale-95 transition-all">লগইন করুন</button>
            <p id="lErr" class="text-red-500 text-[10px] mt-4 font-bold hidden"></p>
        </div>
    </div>

    <div id="mainPage" class="hidden">
        <div class="hero-gradient text-white p-8 rounded-b-[50px] shadow-lg text-center relative mb-8">
            <button onclick="location.reload()" class="absolute top-5 right-5 text-[10px] bg-white/20 px-3 py-1 rounded-full border border-white/30 font-bold">লগ আউট</button>
            <h1 class="text-lg font-bold uppercase tracking-tight">যুব কল্যাণ রক্তদান ফাউন্ডেশন</h1>
            <p id="welcome" class="text-yellow-300 text-[11px] mt-2 font-bold"></p>
        </div>

        <div id="editModal" class="fixed inset-0 bg-black/60 hidden flex items-center justify-center p-6 z-50">
            <div class="bg-white p-6 rounded-[35px] w-full max-w-sm shadow-2xl">
                <h3 id="editingName" class="font-bold text-gray-800 mb-1"></h3>
                <p class="text-[10px] text-gray-400 mb-4 font-bold uppercase tracking-widest">রক্তদানের তারিখ আপডেট করুন</p>
                <input type="date" id="newDate" class="w-full p-4 border rounded-2xl mb-4 bg-gray-50 outline-none font-bold text-gray-700">
                <div class="flex gap-2">
                    <button onclick="closeEdit()" class="flex-1 bg-gray-100 py-3 rounded-xl font-bold text-xs">বাতিল</button>
                    <button onclick="submitUpdate()" id="sBtn" class="flex-1 bg-green-600 text-white py-3 rounded-xl font-bold text-xs">সেভ করুন</button>
                </div>
            </div>
        </div>

        <div class="px-8 mb-4 flex items-center justify-between">
            <h3 class="text-xs font-black text-gray-400 uppercase tracking-widest">রক্তদাতাদের তালিকা</h3>
            <span id="adminBadge" class="hidden bg-black text-white text-[8px] px-2 py-0.5 rounded font-bold uppercase">Admin Mode</span>
        </div>

        <div id="donorList" class="px-6 grid gap-6"></div>
    </div>

    <script>
        const scriptURL = "https://script.google.com/macros/s/AKfycbx2AUrExnRv7h2cMRlBv6nqpf3oNy5s9Bh0iKzGG3-5YhGC1NGDEWN7lsRmuaEHo92o/exec"; 
        
        let allDonors = [];
        let loggedUser = null;
        let currentRole = 'Member';
        let targetPhone = ""; // এডিটের জন্য টার্গেট ফোন

        function setRole(role) {
            currentRole = role;
            const passInput = document.getElementById('uPass');
            const mBtn = document.getElementById('roleMember');
            const aBtn = document.getElementById('roleAdmin');

            if(role === 'Admin') {
                passInput.classList.remove('hidden');
                aBtn.className = "px-4 py-2 rounded-full text-xs font-bold bg-red-600 text-white shadow-md";
                mBtn.className = "px-4 py-2 rounded-full text-xs font-bold bg-gray-100 text-gray-500 border border-gray-200";
            } else {
                passInput.classList.add('hidden');
                mBtn.className = "px-4 py-2 rounded-full text-xs font-bold bg-red-600 text-white shadow-md";
                aBtn.className = "px-4
                
