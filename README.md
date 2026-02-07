<!DOCTYPE html>
<html lang="bn">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>যুব কল্যাণ রক্তদান ফাউন্ডেশন</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Hind+Siliguri:wght@400;600;700&display=swap" rel="stylesheet">
    <style>body { font-family: 'Hind Siliguri', sans-serif; }</style>
</head>
<body class="bg-gray-50 pb-20">

    <div class="bg-red-600 text-white p-6 text-center shadow-lg">
        <h1 class="text-2xl font-bold uppercase">যুব কল্যাণ রক্তদান ফাউন্ডেশন</h1>
        <div class="flex justify-center gap-3 mt-5">
            <button onclick="toggleForm('memberPanel')" class="bg-white text-green-700 px-4 py-2 rounded-xl text-[11px] font-bold shadow-lg">➕ ডোনার যোগ করুন</button>
            <button id="adminLoginBtn" onclick="toggleForm('adminModal')" class="bg-red-900 text-white px-4 py-2 rounded-xl text-[10px]">🔑 অ্যাডমিন লগইন</button>
            <button id="logoutBtn" onclick="logout()" class="hidden bg-black text-white px-4 py-2 rounded-xl text-[10px]">🚪 লগআউট</button>
        </div>
    </div>

    <div id="adminModal" class="fixed inset-0 bg-black/70 hidden z-50 flex items-center justify-center p-4">
        <div class="bg-white p-6 rounded-3xl w-full max-w-sm text-center">
            <h2 class="font-bold mb-4">অ্যাডমিন প্রবেশ</h2>
            <input type="password" id="adminPass" placeholder="পাসওয়ার্ড দিন" class="w-full p-3 border rounded-xl mb-4 text-center outline-none">
            <button onclick="loginAdmin()" class="w-full bg-red-600 text-white py-2 rounded-xl font-bold">প্রবেশ</button>
        </div>
    </div>

    <div id="loading" class="text-center py-20"><p>লোড হচ্ছে...</p></div>
    <div id="donorList" class="p-4 grid gap-4 hidden"></div>

    <script>
        const url = "আপনার_নতুন_এপস_স্ক্রিপ্ট_লিঙ্ক"; // এখানে লিঙ্ক বসান
        let allDonors = [];
        let isAdmin = false;

        function toggleForm(id) { document.getElementById(id).classList.toggle('hidden'); }

        function loginAdmin() {
            if(document.getElementById('adminPass').value === "1234") {
                isAdmin = true;
                toggleForm('adminModal');
                document.getElementById('adminLoginBtn').classList.add('hidden');
                document.getElementById('logoutBtn').classList.remove('hidden');
                displayDonors(allDonors);
                alert("অ্যাডমিন মোড চালু হয়েছে! এখন আপনি ডিলিট করতে পারবেন।");
            } else { alert("ভুল পাসওয়ার্ড!"); }
        }

        function logout() {
            isAdmin = false;
            location.reload();
        }

        async function loadDonors() {
            const response = await fetch(url);
            allDonors = await response.json();
            displayDonors(allDonors);
            document.getElementById('loading').classList.add('hidden');
            document.getElementById('donorList').classList.remove('hidden');
        }

        async function deleteDonor(rowId) {
            if(confirm("আপনি কি নিশ্চিতভাবে এই ডোনার ডিলিট করতে চান?")) {
                await fetch(url, { method: 'POST', mode: 'no-cors', body: JSON.stringify({action: 'delete', rowId: rowId}) });
                alert("ডিলিট হয়েছে! ১ মিনিট পর রিফ্রেশ দিন।");
                location.reload();
            }
        }

        function displayDonors(data) {
            const list = document.getElementById('donorList');
            list.innerHTML = "";
            data.forEach(d => {
                let deleteBtn = isAdmin ? `<button onclick="deleteDonor(${d.rowId})" class="mt-2 w-full bg-gray-100 text-red-600 py-1 rounded-xl text-[10px] font-bold border border-red-100">🗑 ডিলিট করুন</button>` : "";
                
                list.innerHTML += `
                <div class="bg-white p-5 rounded-3xl shadow-sm border border-gray-100 mb-2">
                    <div class="flex justify-between items-start">
                        <div><h3 class="font-bold text-lg">${d.n}</h3><p class="text-[10px] text-gray-400">📍 ${d.l}</p></div>
                        <div class="bg-red-50 px-3 py-1 rounded-xl text-red-600 font-bold">${d.g}</div>
                    </div>
                    <a href="tel:${d.p}" class="block mt-4 w-full bg-red-600 text-white py-2 rounded-2xl font-bold text-center text-sm">📞 কল দিন</a>
                    ${deleteBtn}
                </div>`;
            });
        }
        loadDonors();
    </script>
</body>
</html>
