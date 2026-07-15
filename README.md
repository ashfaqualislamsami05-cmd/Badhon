 <!DOCTYPE html>
<html lang="bn">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>বাঁধন - Badhon Foundation</title>
    <!-- Tailwind CSS for styling -->
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <!-- CORE DATABASE & STORAGE ENGINES -->
    <script src="https://www.gstatic.com/firebasejs/8.10.1/firebase-app.js"></script>
    <script src="https://www.gstatic.com/firebasejs/8.10.1/firebase-database.js"></script>
    <script src="https://www.gstatic.com/firebasejs/8.10.1/firebase-storage.js"></script>
</head>
<body class="bg-gray-50 text-gray-800 font-sans min-h-screen flex flex-col">

    <!-- Navbar -->
    <header class="bg-teal-600 text-white shadow-md sticky top-0 z-40">
        <div class="container mx-auto px-4 py-4 flex justify-between items-center">
            <h1 id="nav-logo" class="text-3xl font-bold tracking-wide">বাঁধন</h1>
            <button id="lang-toggle" onclick="toggleLanguage()" class="bg-white text-teal-700 font-semibold px-4 py-2 rounded-full shadow hover:bg-teal-50 transition duration-200">
                English
            </button>
        </div>
    </header>

    <!-- Main Content -->
    <main class="container mx-auto px-4 py-8 flex-grow max-w-4xl">
        <div class="flex justify-between items-center mb-6">
            <h2 id="feed-title" class="text-2xl font-bold text-gray-700">সাম্প্রতিক আবেদনসমূহ (Recent Appeals)</h2>
            <button onclick="openModal()" class="bg-teal-600 hover:bg-teal-700 text-white rounded-full p-4 shadow-lg flex items-center justify-center transition transform hover:scale-105" title="Add New Request">
                <i class="fa-solid fa-plus text-xl"></i>
            </button>
        </div>

        <!-- Global Live Feed Container -->
        <div id="posts-container" class="space-y-6">
            <div id="loading-text" class="text-center text-gray-500 py-10 animate-pulse">লোড হচ্ছে (Loading database appeals...)</div>
        </div>
    </main>

    <!-- Add Post Modal -->
    <div id="post-modal" class="fixed inset-0 bg-black bg-opacity-50 z-50 flex items-center justify-center hidden p-4">
        <div class="bg-white rounded-2xl max-w-lg w-full max-h-[90vh] overflow-y-auto p-6 relative shadow-2xl">
            <button onclick="closeModal()" class="absolute top-4 right-4 text-gray-400 hover:text-gray-600 text-xl">
                <i class="fa-solid fa-xmark"></i>
            </button>
            <h2 id="modal-heading" class="text-2xl font-bold text-teal-700 mb-4">সাহায্যের আবেদন করুন</h2>
            
            <form id="fund-form" onsubmit="handleFormSubmit(event)" class="space-y-4">
                <div>
                    <label id="lbl-name" class="block text-sm font-semibold text-gray-600 mb-1">রোগীর নাম (Patient's Name)</label>
                    <input type="text" id="form-name" required class="w-full px-4 py-2 border rounded-lg focus:ring-2 focus:ring-teal-500 outline-none">
                </div>
                <div class="grid grid-cols-2 gap-4">
                    <div>
                        <label id="lbl-disease" class="block text-sm font-semibold text-gray-600 mb-1">রোগ/প্রয়োজন (Disease/Need)</label>
                        <input type="text" id="form-disease" required class="w-full px-4 py-2 border rounded-lg focus:ring-2 focus:ring-teal-500 outline-none">
                    </div>
                    <div>
                        <label id="lbl-stage" class="block text-sm font-semibold text-gray-600 mb-1">বর্তমান অবস্থা/ধাপ (Current Stage)</label>
                        <input type="text" id="form-stage" placeholder="e.g., Stage 3" class="w-full px-4 py-2 border rounded-lg focus:ring-2 focus:ring-teal-500 outline-none">
                    </div>
                </div>
                <div class="grid grid-cols-2 gap-4">
                    <div>
                        <label id="lbl-goal" class="block text-sm font-semibold text-gray-600 mb-1">টাকার পরিমাণ (Goal Amount ৳)</label>
                        <input type="number" id="form-goal" required class="w-full px-4 py-2 border rounded-lg focus:ring-2 focus:ring-teal-500 outline-none">
                    </div>
                    <div>
                        <label id="lbl-raised" class="block text-sm font-semibold text-gray-600 mb-1">সংগৃহীত টাকা (Raised Amount ৳)</label>
                        <input type="number" id="form-raised" value="0" class="w-full px-4 py-2 border rounded-lg focus:ring-2 focus:ring-teal-500 outline-none">
                    </div>
                </div>
                <div>
                    <label id="lbl-docs" class="block text-sm font-semibold text-gray-600 mb-1">মেডিকেল ডকুমেন্টস (Upload Photos/Videos)</label>
                    <input type="file" id="form-docs" accept="image/*,video/*" multiple class="w-full text-sm text-gray-500 file:mr-4 file:py-2 file:px-4 file:rounded-full file:border-0 file:text-sm file:font-semibold file:bg-teal-50 file:text-teal-700 hover:file:bg-teal-100">
                </div>
                <div>
                    <label id="lbl-payment" class="block text-sm font-semibold text-gray-600 mb-1">পেমেন্ট মাধ্যম (bKash/Nagad/Bank details)</label>
                    <textarea id="form-payment" required rows="2" placeholder="e.g., bKash: 018XXXXXXXX..." class="w-full px-4 py-2 border rounded-lg focus:ring-2 focus:ring-teal-500 outline-none"></textarea>
                </div>
                <div>
                    <label id="lbl-phone" class="block text-sm font-semibold text-gray-600 mb-1">মোবাইল নাম্বার (Contact Number)</label>
                    <input type="tel" id="form-phone" required class="w-full px-4 py-2 border rounded-lg focus:ring-2 focus:ring-teal-500 outline-none">
                </div>
                <div>
                    <label id="lbl-desc" class="block text-sm font-semibold text-gray-600 mb-1">বিবিধ (Description)</label>
                    <textarea id="form-desc" rows="3" class="w-full px-4 py-2 border rounded-lg focus:ring-2 focus:ring-teal-500 outline-none"></textarea>
                </div>
                <button type="submit" id="btn-submit" class="w-full bg-teal-600 hover:bg-teal-700 text-white font-bold py-3 rounded-lg shadow transition duration-200">পোস্ট করুন (Submit Post)</button>
            </form>
        </div>
    </div>

    <!-- Footer -->
    <footer class="bg-gray-800 text-gray-400 py-4 text-center text-sm border-t border-gray-700 mt-10">
        <p>&copy; 2026 <span id="footer-logo">বাঁধন ফাউন্ডেশন</span>. All Rights Reserved.</p>
    </footer>

    <!-- Logic Engine -->
    <script>
        // -------------------------------------------------------------------------
        // ⚠️ REMEMBER TO PASTE YOUR PERSONAL FIREBASE CONFIGURATION OBJECT HERE
        // -------------------------------------------------------------------------
        const firebaseConfig = {
  apiKey: "AIzaSyDhw-wmVIppsagWsTpRbNvhV8jg5FZtuDM",
  authDomain: "badhon-94226.firebaseapp.com",
  projectId: "badhon-94226",
  storageBucket: "badhon-94226.firebasestorage.app",
  messagingSenderId: "233697472337",
  appId: "1:233697472337:web:d6c253efd9a4c9baa4ed70",
  measurementId: "G-6NZTD9MR5E"
};

// Initialize Firebase
const app = initializeApp(firebaseConfig);
const analytics = getAnalytics(app);
        };
        
        // Initialize Firebase Engines
        firebase.initializeApp(firebaseConfig);
        const database = firebase.database();
        const storage = firebase.storage(); // New high-speed cloud file asset engine

        let currentLang = 'bn';

        const translations = {
            en: {
                logo: "Badhon", feedTitle: "Recent Funding Appeals", modalHeading: "Apply for Financial Fund",
                lblName: "Patient Name", lblDisease: "Disease / Need", lblStage: "Current Stage",
                lblGoal: "Target Amount Needed (৳)", lblRaised: "Amount Raised (৳)",
                lblDocs: "Medical Documents (Photos/Videos)", lblPayment: "Payment Methods (bKash/Nagad/Bank)",
                lblPhone: "Mobile Number", lblDesc: "Description", btnSubmit: "Publish Post",
                langBtn: "বাংলা", timeText: "Live Appeal", diseaseLabel: "Disease:", stageLabel: "Stage:",
                contactLabel: "Contact:", docsHeading: "Medical Documents:"
            },
            bn: {
                logo: "বাঁধন", feedTitle: "সাম্প্রতিক আবেদনসমূহ", modalHeading: "সাহায্যের আবেদন করুন",
                lblName: "রোগীর নাম (Patient's Name)", lblDisease: "রোগ/প্রয়োজন (Disease/Need)", lblStage: "বর্তমান অবস্থা/ধাপ (Current Stage)",
                lblGoal: "টাকার পরিমাণ (Goal Amount ৳)", lblRaised: "সংগৃহীত টাকা (Raised Amount ৳)",
                lblDocs: "মেডিকেল ডকুমেন্টস (Upload Photos/Videos)", lblPayment: "পেমেন্ট মাধ্যম (bKash/Nagad/Bank details)",
                lblPhone: "মোবাইল নাম্বার (Contact Number)", lblDesc: "বিবিধ", btnSubmit: "পোস্ট করুন",
                langBtn: "English", timeText: "চলতি আবেদন", diseaseLabel: "রোগ:", stageLabel: "ধাপ:",
                contactLabel: "যোগাযোগ:", docsHeading: "মেডিকেল ডকুমেন্টস:"
            }
        };

        function toggleLanguage() {
            currentLang = currentLang === 'bn' ? 'en' : 'bn';
            const t = translations[currentLang];

            document.getElementById('nav-logo').innerText = t.logo;
            document.getElementById('footer-logo').innerText = t.logo + (currentLang === 'en' ? ' Foundation' : ' ফাউন্ডেশন');
            document.getElementById('feed-title').innerText = t.feedTitle;
            document.getElementById('lang-toggle').innerText = t.langBtn;
            
            document.getElementById('modal-heading').innerText = t.modalHeading;
            document.getElementById('lbl-name').innerText = t.lblName;
            document.getElementById('lbl-disease').innerText = t.lblDisease;
            document.getElementById('lbl-stage').innerText = t.lblStage;
            document.getElementById('lbl-goal').innerText = t.lblGoal;
            document.getElementById('lbl-raised').innerText = t.lblRaised;
            document.getElementById('lbl-docs').innerText = t.lblDocs;
            document.getElementById('lbl-payment').innerText = t.lblPayment;
            document.getElementById('lbl-phone').innerText = t.lblPhone;
            document.getElementById('lbl-desc').innerText = t.lblDesc;
            document.getElementById('btn-submit').innerText = t.btnSubmit;

            document.querySelectorAll('.data-disease-label').forEach(el => el.innerText = t.diseaseLabel);
            document.querySelectorAll('.data-stage-label').forEach(el => el.innerText = t.stageLabel);
            document.querySelectorAll('.data-contact-label').forEach(el => el.innerText = t.contactLabel);
            document.querySelectorAll('.data-docs-heading').forEach(el => el.innerText = t.docsHeading);
        }

        function openModal() { document.getElementById('post-modal').classList.remove('hidden'); }
        function closeModal() {
            document.getElementById('post-modal').classList.add('hidden');
            document.getElementById('fund-form').reset();
        }

        // DATABASE APPFEDEEER: Loads posts instantly via lightweight URL references
        database.ref('posts').orderByChild('timestamp').on('value', (snapshot) => {
            const container = document.getElementById('posts-container');
            container.innerHTML = ''; 
            
            if (!snapshot.exists()) {
                container.innerHTML = `<div class="text-center text-gray-400 py-10">কোনো পোস্ট পাওয়া যায়নি (No active posts found)</div>`;
                return;
            }

            let postList = [];
            snapshot.forEach((childSnapshot) => {
                postList.push(childSnapshot.val());
            });

            postList.reverse().forEach((data) => {
                const t = translations[currentLang];
                const percentage = Math.min((data.raised / data.goal) * 100, 100).toFixed(1);

                let descriptionHTML = data.description ? `<p class="text-gray-700 text-sm mb-4 bg-gray-50 p-3 rounded-lg border border-gray-100 whitespace-pre-line">${data.description}</p>` : '';
                
                let docsHTML = '';
                if (data.documents && data.documents.length > 0) {
                    docsHTML = `<div class="mb-4"><p class="text-xs font-semibold text-gray-500 mb-2 data-docs-heading">${t.docsHeading}</p><div class="grid grid-cols-2 gap-2">`;
                    data.documents.forEach((doc) => {
                        // Intelligent Fallback: Check if structural data format is direct text string or nested object
                        const docUrl = doc.url || doc;
                        const docType = doc.type || (docUrl.includes('video') || docUrl.startsWith('data:video') ? 'video/' : 'image/');

                        if (docType.startsWith('image/')) {
                            docsHTML += `<a href="${docUrl}" target="_blank"><img src="${docUrl}" class="rounded-lg h-28 w-full object-cover border border-gray-200 shadow-sm hover:opacity-90 transition" alt="Document"></a>`;
                        } else if (docType.startsWith('video/')) {
                            docsHTML += `<video src="${docUrl}" controls class="rounded-lg h-28 w-full object-cover border border-gray-200 shadow-sm"></video>`;
                        }
                    });
                    docsHTML += `</div></div>`;
                }

                const postHTML = `
                    <div class="bg-white rounded-xl shadow-md p-6 border border-gray-100 transition hover:shadow-lg animate-fade-in">
                        <div class="flex justify-between items-start mb-4">
                            <div>
                                <h3 class="text-xl font-bold text-gray-900">${data.name}</h3>
                                <p class="text-sm text-teal-600 font-medium mt-1">
                                    <span class="data-disease-label">${t.diseaseLabel}</span> ${data.disease} | 
                                    <span class="data-stage-label">${t.stageLabel}</span> ${data.stage}
                                </p>
                            </div>
                            <span class="text-xs text-teal-600 font-semibold bg-teal-50 px-2 py-1 rounded">${t.timeText}</span>
                        </div>
                        
                        ${descriptionHTML}
                        
                        <p class="text-gray-600 text-sm mb-4"><strong class="data-contact-label">${t.contactLabel}</strong> ${data.phone}</p>
                        
                        <div class="mb-4">
                            <div class="flex justify-between text-xs font-semibold text-gray-500 mb-1">
                                <span>Raised: ৳${Number(data.raised).toLocaleString()}</span>
                                <span>Goal: ৳${Number(data.goal).toLocaleString()}</span>
                            </div>
                            <div class="w-full bg-gray-200 rounded-full h-2.5">
                                <div class="bg-teal-500 h-2.5 rounded-full" style="width: ${percentage}%"></div>
                            </div>
                        </div>

                        ${docsHTML}

                        <div class="bg-gray-50 p-3 rounded-lg text-sm border border-gray-100 whitespace-pre-line">
                            <strong>${currentLang === 'en' ? 'Payment Methods:' : 'পেমেন্ট মাধ্যম:'}</strong>\n${data.payment}
                        </div>
                    </div>`;
                container.insertAdjacentHTML('beforeend', postHTML);
            });
        });

        // HIGH-SPEED MULTI-THREAD UPLOADER ENGINE
        async function handleFormSubmit(event) {
            event.preventDefault();
            
            const submitBtn = document.getElementById('btn-submit');
            submitBtn.disabled = true;
            submitBtn.innerText = "Uploading files...";

            const name = document.getElementById('form-name').value;
            const disease = document.getElementById('form-disease').value;
            const stage = document.getElementById('form-stage').value || 'N/A';
            const goal = parseFloat(document.getElementById('form-goal').value);
            const raised = parseFloat(document.getElementById('form-raised').value) || 0;
            const payment = document.getElementById('form-payment').value;
            const phone = document.getElementById('form-phone').value;
            const description = document.getElementById('form-desc').value;
            const docsInput = document.getElementById('form-docs');
            
            let fileArray = [];
            try {
                if (docsInput.files && docsInput.files.length > 0) {
                    for (let i = 0; i < docsInput.files.length; i++) {
                        const file = docsInput.files[i];
                        
                        // Create a secure unique name tag path inside the filing cabinet
                        const uniqueFileName = 'medical_docs/' + Date.now() + '_' + file.name;
                        const storageRef = storage.ref().child(uniqueFileName);
                        
                        // Push binary data up directly (Super Fast)
                        submitBtn.innerText = `Uploading File (${i+1}/${docsInput.files.length})...`;
                        const uploadSnapshot = await storageRef.put(file);
                        
                        // Capture public URL links references
                        const downloadUrl = await uploadSnapshot.ref.getDownloadURL();
                        
                        fileArray.push({
                            url: downloadUrl,
                            type: file.type
                        });
                    }
                }

                submitBtn.innerText = "Saving Post details...";
                const newPostData = {
                    name: name, disease: disease, stage: stage, goal: goal,
                    raised: raised, payment: payment, phone: phone,
                    description: description, documents: fileArray, timestamp: Date.now()
                };

                // Push small text footprint map profile directly to DB
                await database.ref('posts').push(newPostData);
                
                submitBtn.disabled = false;
                closeModal();

            } catch (error) {
                alert("Upload Error: " + error.message);
                submitBtn.disabled = false;
                submitBtn.innerText = "পোস্ট করুন (Submit Post)";
            }
        }
    </script>

    <style>
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        .animate-fade-in { animation: fadeIn 0.3s ease-out forwards; }
    </style>
</body>
</html>

                                
                  
