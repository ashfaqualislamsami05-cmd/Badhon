<!DOCTYPE html>
<html lang="bn">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>বাঁধন - Badhon Foundation</title>
    <!-- Tailwind CSS for modern styling -->
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
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
        <!-- Floating Add Button for Mobile/Desktop -->
        <div class="flex justify-between items-center mb-6">
            <h2 id="feed-title" class="text-2xl font-bold text-gray-700">সাম্প্রতিক আবেদনসমূহ (Recent Appeals)</h2>
            <button onclick="openModal()" class="bg-teal-600 hover:bg-teal-700 text-white rounded-full p-4 shadow-lg flex items-center justify-center transition transform hover:scale-105" title="Add New Request">
                <i class="fa-solid fa-plus text-xl"></i>
            </button>
        </div>

        <!-- Dynamic Feed Container (Newest First) -->
        <div id="posts-container" class="space-y-6">
            <!-- Demo Post (Sample data to show how it looks) -->
            <div class="bg-white rounded-xl shadow-md p-6 border border-gray-100 transition hover:shadow-lg">
                <div class="flex justify-between items-start mb-4">
                    <div>
                        <h3 class="text-xl font-bold text-gray-900">রাকিবুল হাসান (Rakibul Hasan)</h3>
                        <p class="text-sm text-teal-600 font-medium mt-1"><span class="data-disease-label">রোগ:</span> ব্লাড ক্যান্সার (Blood Cancer) | <span class="data-stage-label">ধাপ:</span> Stage 2</p>
                    </div>
                    <span class="text-xs text-gray-400 bg-gray-100 px-2 py-1 rounded">Just Now</span>
                </div>
                
                <p class="text-gray-700 text-sm mb-4 bg-gray-50 p-3 rounded-lg border border-gray-100">ঢাকা বিশ্ববিদ্যালয়ের একজন মেধাবী ছাত্র। জরুরি চিকিৎসার জন্য আর্থিক সহায়তা প্রয়োজন। চিকিৎসকদের পরামর্শ অনুযায়ী দ্রুত থেরাপি শুরু করতে হবে। (A brilliant student of DU needing urgent financial support for medical therapy.)</p>
                
                <!-- Progress Bar -->
                <div class="mb-4">
                    <div class="flex justify-between text-xs font-semibold text-gray-500 mb-1">
                        <span>Raised: ৳৫০,০০০</span>
                        <span>Goal: ৳৫,০০,০০০</span>
                    </div>
                    <div class="w-full bg-gray-200 rounded-full h-2.5">
                        <div class="bg-teal-500 h-2.5 rounded-full" style="width: 10%"></div>
                    </div>
                </div>

                <div class="bg-gray-50 p-3 rounded-lg text-sm grid grid-cols-1 md:grid-cols-2 gap-2 border border-gray-100">
                    <div><strong>bKash/Nagad:</strong> 017XXXXXXXX</div>
                    <div><strong>Bank:</strong> DBBL, Acc: 123.XXX.XXXX</div>
                </div>
            </div>
        </div>
    </main>

    <!-- Add Post Modal (Hidden by default) -->
    <div id="post-modal" class="fixed inset-0 bg-black bg-opacity-50 z-50 flex items-center justify-center hidden p-4">
        <div class="bg-white rounded-2xl max-w-lg w-full max-h-[90vh] overflow-y-auto p-6 relative shadow-2xl animate-fade-in">
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
                    <textarea id="form-payment" required rows="2" placeholder="e.g., bKash: 018XXXXXXXX, Bank Account details..." class="w-full px-4 py-2 border rounded-lg focus:ring-2 focus:ring-teal-500 outline-none"></textarea>
                </div>
                <div>
                    <label id="lbl-phone" class="block text-sm font-semibold text-gray-600 mb-1">মোবাইল নাম্বার (Contact Number)</label>
                    <input type="tel" id="form-phone" required class="w-full px-4 py-2 border rounded-lg focus:ring-2 focus:ring-teal-500 outline-none">
                </div>
                <!-- New Description / বিবিধ Options Area Below Phone Number -->
                <div>
                    <label id="lbl-desc" class="block text-sm font-semibold text-gray-600 mb-1">বিবিধ (Description)</label>
                    <textarea id="form-desc" rows="3" placeholder="..." class="w-full px-4 py-2 border rounded-lg focus:ring-2 focus:ring-teal-500 outline-none"></textarea>
                </div>
                <button type="submit" id="btn-submit" class="w-full bg-teal-600 hover:bg-teal-700 text-white font-bold py-3 rounded-lg shadow transition duration-200">পোস্ট করুন (Submit Post)</button>
            </form>
        </div>
    </div>

    <!-- Footer -->
    <footer class="bg-gray-800 text-gray-400 py-4 text-center text-sm border-t border-gray-700 mt-10">
        <p>&copy; 2026 <span id="footer-logo">固定 বাঁধন ফাউন্ডেশন</span>. All Rights Reserved.</p>
    </footer>

    <!-- Translation & Interface JavaScript Logic -->
    <script>
        let currentLang = 'bn';

        const translations = {
            en: {
                logo: "Badhon",
                feedTitle: "Recent Funding Appeals",
                modalHeading: "Apply for Financial Fund",
                lblName: "Patient / Requester Name",
                lblDisease: "Disease / Core Need",
                lblStage: "Current Condition / Stage",
                lblGoal: "Target Amount Needed (৳)",
                lblRaised: "Amount Raised So Far (৳)",
                lblDocs: "Medical Verification Documents (Photos/Videos)",
                lblPayment: "Payment Methods (bKash, Nagad, Bank Details)",
                lblPhone: "Mobile Number",
                lblDesc: "Description",
                btnSubmit: "Publish Post",
                langBtn: "বাংলা",
                timeText: "Just Now",
                diseaseLabel: "Disease:",
                stageLabel: "Stage:",
                contactLabel: "Contact:",
                docsHeading: "Medical Documents:"
            },
            bn: {
                logo: "বাঁধন",
                feedTitle: "সাম্প্রতিক আবেদনসমূহ",
                modalHeading: "সাহায্যের আবেদন করুন",
                lblName: "রোগীর নাম (Patient's Name)",
                lblDisease: "রোগ/প্রয়োজন (Disease/Need)",
                lblStage: "বর্তমান অবস্থা/ধাপ (Current Stage)",
                lblGoal: "টাকার পরিমাণ (Goal Amount ৳)",
                lblRaised: "সংগৃহীত টাকা (Raised Amount ৳)",
                lblDocs: "মেডিকেল ডকুমেন্টস (Upload Photos/Videos)",
                lblPayment: "পেমেন্ট মাধ্যম (bKash/Nagad/Bank details)",
                lblPhone: "মোবাইল নাম্বার (Contact Number)",
                lblDesc: "বিবিধ",
                btnSubmit: "পোস্ট করুন",
                langBtn: "English",
                timeText: "এইমাত্র",
                diseaseLabel: "রোগ:",
                stageLabel: "ধাপ:",
                contactLabel: "যোগাযোগ:",
                docsHeading: "মেডিকেল ডকুমেন্টস:"
            }
        };

        function toggleLanguage() {
            currentLang = currentLang === 'bn' ? 'en' : 'bn';
            const t = translations[currentLang];

            document.getElementById('nav-logo').innerText = t.logo;
            document.getElementById('footer-logo').innerText = t.logo + (currentLang === 'en' ? ' Foundation' : ' ফাউন্ডেশন');
            document.getElementById('feed-title').innerText = t.feedTitle;
            document.getElementById('lang-toggle').innerText = t.langBtn;
            
            // Modal Elements Translation
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

            // Card Text Elements Live Dynamic Update
            document.querySelectorAll('.data-disease-label').forEach(el => el.innerText = t.diseaseLabel);
            document.querySelectorAll('.data-stage-label').forEach(el => el.innerText = t.stageLabel);
            document.querySelectorAll('.data-contact-label').forEach(el => el.innerText = t.contactLabel);
            document.querySelectorAll('.data-docs-heading').forEach(el => el.innerText = t.docsHeading);
        }

        function openModal() {
            document.getElementById('post-modal').classList.remove('hidden');
        }

        function closeModal() {
            document.getElementById('post-modal').classList.add('hidden');
            document.getElementById('fund-form').reset();
        }

        function handleFormSubmit(event) {
            event.preventDefault();
            
            // Collect Form Values
            const name = document.getElementById('form-name').value;
            const disease = document.getElementById('form-disease').value;
            const stage = document.getElementById('form-stage').value || 'N/A';
            const goal = parseFloat(document.getElementById('form-goal').value);
            const raised = parseFloat(document.getElementById('form-raised').value) || 0;
            const payment = document.getElementById('form-payment').value;
            const phone = document.getElementById('form-phone').value;
            const description = document.getElementById('form-desc').value;
            const docsInput = document.getElementById('form-docs');
            
            const t = translations[currentLang];

            // Core Logic for Processing Verification Previews
            let docsHTML = '';
            if (docsInput.files && docsInput.files.length > 0) {
                docsHTML = `
                    <div class="mb-4">
                        <p class="text-xs font-semibold text-gray-500 mb-2 data-docs-heading">${t.docsHeading}</p>
                        <div class="grid grid-cols-2 gap-2">
                `;
                
                for (let i = 0; i < docsInput.files.length; i++) {
                    const file = docsInput.files[i];
                    const fileUrl = URL.createObjectURL(file);
                    
                    if (file.type.startsWith('image/')) {
                        docsHTML += `
                            <a href="${fileUrl}" target="_blank" title="Click to view full image">
                                <img src="${fileUrl}" class="rounded-lg h-28 w-full object-cover border border-gray-200 hover:opacity-90 transition shadow-sm" alt="Document">
                            </a>`;
                    } else if (file.type.startsWith('video/')) {
                        docsHTML += `
                            <video src="${fileUrl}" controls class="rounded-lg h-28 w-full object-cover border border-gray-200 shadow-sm"></video>`;
                    }
                }
                docsHTML += `</div></div>`;
            }
            
            // Calculate Progress Status Layout
            const percentage = Math.min((raised / goal) * 100, 100).toFixed(1);

            // Conditional Description Component Render
            let descriptionHTML = '';
            if(description.trim() !== "") {
                descriptionHTML = `
                    <p class="text-gray-700 text-sm mb-4 bg-gray-50 p-3 rounded-lg border border-gray-100 whitespace-pre-line">${description}</p>
                `;
            }

            // Construct new clean template structure layout
            const postHTML = `
                <div class="bg-white rounded-xl shadow-md p-6 border border-teal-100 transition hover:shadow-lg animate-fade-in">
                    <div class="flex justify-between items-start mb-4">
                        <div>
                            <h3 class="text-xl font-bold text-gray-900">${name}</h3>
                            <p class="text-sm text-teal-600 font-medium mt-1">
                                <span class="data-disease-label">${t.diseaseLabel}</span> ${disease} | 
                                <span class="data-stage-label">${t.stageLabel}</span> ${stage}
                            </p>
                        </div>
                        <span class="text-xs text-teal-600 font-semibold bg-teal-50 px-2 py-1 rounded">${t.timeText}</span>
                    </div>
                    
                    <!-- Main Text Box Area / Description Rendering -->
                    ${descriptionHTML}
                    
                    <p class="text-gray-600 text-sm mb-4"><strong class="data-contact-label">${t.contactLabel}</strong> ${phone}</p>
                    
                    <!-- Progress Bar Component -->
                    <div class="mb-4">
                        <div class="flex justify-between text-xs font-semibold text-gray-500 mb-1">
                            <span>Raised: ৳${raised.toLocaleString()}</span>
                            <span>Goal: ৳${goal.toLocaleString()}</span>
                        </div>
                        <div class="w-full bg-gray-200 rounded-full h-2.5">
                            <div class="bg-teal-500 h-2.5 rounded-full" style="width: ${percentage}%"></div>
                        </div>
                    </div>

                    <!-- Medical Docs Content Component Layout -->
                    ${docsHTML}

                    <div class="bg-gray-50 p-3 rounded-lg text-sm border border-gray-100 whitespace-pre-line">
                        <strong>${currentLang === 'en' ? 'Payment Methods:' : 'পেমেন্ট মাধ্যম:'}</strong>\n${payment}
                    </div>
                </div>
            `;

            // Enforce Chronological Feed Alignment (Insert right at the top)
            const container = document.getElementById('posts-container');
            container.insertAdjacentHTML('afterbegin', postHTML);

            closeModal();
        }
    </script>

    <style>
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }
        .animate-fade-in {
            animation: fadeIn 0.3s ease-out forwards;
        }
    </style>
</body>
</html>
