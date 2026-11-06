MY MARKET CRM V3 CLOUD — SETUP GUIDE (বাংলা)
============================================

এই ZIP-এ যা আছে
----------------
1. index.html       : V3 Cloud CRM-এর মূল অ্যাপ
2. firestore.rules  : Firebase Cloud Firestore-এর নিরাপত্তা বিধি
3. manifest.json    : Android-এ Add to Home Screen/PWA তথ্য
4. sw.js            : App shell caching

গুরুত্বপূর্ণ: এই সংস্করণটি Firebase Web App config বসানো ও নিচের Security Rules Publish করার পরই Cloud-এ কাজ করবে। Config না বসানো অবস্থায় এটি Firebase-এ সংযুক্ত হবে না।

পর্ব ১ — Firebase Authentication চালু করুন
------------------------------------------
1. https://console.firebase.google.com/ খুলে আপনার তৈরি Project নির্বাচন করুন।
2. Build > Authentication > Get started চাপুন।
3. Sign-in method-এ Email/Password নির্বাচন করুন।
4. Enable করে Save করুন।
5. Authentication > Settings > Authorized domains-এ আপনার GitHub Pages domain যোগ করুন। উদাহরণ: username.github.io (https:// ছাড়া)।

পর্ব ২ — Cloud Firestore তৈরি করুন
---------------------------------
1. Firebase Console > Build > Firestore Database খুলুন।
2. Create database চাপুন।
3. Production mode নির্বাচন করুন (Test mode নয়)।
4. আপনার উপযুক্ত Location নির্বাচন করুন। বাংলাদেশি ব্যবহারকারীদের জন্য কাছাকাছি Asia region পাওয়া গেলে সেটি বিবেচনা করুন; Location একবার তৈরি হলে সহজে বদলানো যায় না।
5. Create চাপুন।

পর্ব ৩ — Security Rules বসান (অত্যন্ত গুরুত্বপূর্ণ)
--------------------------------------------------
1. Firestore Database > Rules ট্যাব খুলুন।
2. ZIP-এর firestore.rules ফাইল খুলে সব লেখা কপি করুন।
3. Firebase Rules editor-এর পুরোনো লেখা সম্পূর্ণ মুছে এটি Paste করুন।
4. Publish চাপুন।
5. Rules কখনোই Test mode / allow read, write: if true করে রাখবেন না।

পর্ব ৪ — Web App Config সংগ্রহ করুন
----------------------------------
1. Firebase Project Overview-এ যান।
2. Add app / Web (</>) আইকন চাপুন।
3. App nickname দিন: My Market CRM V3
4. Register app চাপুন। Hosting এখনই সেটআপ করার দরকার নেই।
5. Firebase SDK setup-এর Config অংশে apiKey, authDomain, projectId, storageBucket, messagingSenderId, appId পাবেন।
6. ZIP-এর index.html ফাইল খুলুন। নিচের মতো অংশ খুঁজুন:
   const firebaseConfig={apiKey:'PASTE_API_KEY', ... };
7. Firebase Console থেকে পাওয়া ছয়টি value ঠিক জায়গায় বসান। প্রতিটি value-এর চারপাশে single quote রাখুন। উদাহরণ:
   apiKey:'আপনার-apiKey',
   authDomain:'your-project.firebaseapp.com',
   projectId:'your-project',
   storageBucket:'your-project.firebasestorage.app',
   messagingSenderId:'123456789',
   appId:'1:123456789:web:abcdef'
8. Save করুন। Firebase config-এর apiKey Web App identifier; Service Account private key নয়। Service Account JSON, private key বা Admin SDK credential কখনো GitHub-এ দেবেন না।

পর্ব ৫ — প্রথম Admin অ্যাকাউন্ট তৈরি
----------------------------------
নিরাপত্তার জন্য অ্যাপের Registration form দিয়ে প্রথমে নিজের অফিসিয়াল Email দিয়ে একটি অ্যাকাউন্ট তৈরি করুন। এরপর:
1. Firebase Console > Firestore Database > Data খুলুন।
2. users collection-এ সদ্য তৈরি হওয়া UID document খুঁজুন।
3. ওই document-এর status field-কে approved করুন (role officer থাকলেও সমস্যা নেই)।
4. admins নামে নতুন collection তৈরি করুন।
5. Document ID হিসেবে আপনার Authentication UID হুবহু দিন। Document-এ একটি field দিন: enabled (boolean) = true।
6. App-এ ফিরে Logout/Login করুন। এখন আপনার অ্যাকাউন্ট Admin হিসেবে কাজ করবে।

UID কোথায় পাবেন?
Firebase Console > Authentication > Users-এ আপনার Email-এর পাশে User UID থাকে। users document-এর ID-ও একই UID।

পর্ব ৬ — GitHub-এ কোড আপলোড ও প্রকাশ
------------------------------------
1. GitHub-এ New repository তৈরি করুন; যেমন: my-market-crm-v3।
2. ZIP extract করুন। index.html, manifest.json, sw.js, firestore.rules, README_BN.txt ফাইলগুলো repository-এর root-এ Upload files দিয়ে আপলোড করুন।
3. Commit changes করুন।
4. Repository > Settings > Pages খুলুন।
5. Build and deployment > Source: Deploy from a branch নির্বাচন করুন।
6. Branch: main এবং Folder: /(root) নির্বাচন করে Save করুন।
7. প্রকাশিত URL খুলুন। প্রথমবার প্রকাশ হতে কয়েক মিনিট লাগতে পারে।
8. Firebase Authentication > Settings > Authorized domains-এ ওই URL-এর domain (যেমন username.github.io) যুক্ত করুন।

পর্ব ৭ — অফিসারদের ব্যবহার
-------------------------
1. প্রত্যেক অফিসার নিজের মোবাইল থেকে GitHub Pages URL খুলবেন।
2. Officer Registration চাপবেন।
3. নাম, Employee ID, Designation, Branch, Mobile, Email ও Password দিয়ে Submit করবেন।
4. Admin অ্যাপের Admin Panel-এ গিয়ে Employee ID ও Branch যাচাই করে Approve/Reject করবেন।
5. Approve হলে অফিসার Logout/Login করে CRM ব্যবহার করতে পারবেন।
6. Officer শুধু নিজের Customer, Survey ও Visit দেখতে/পরিবর্তন করতে পারবেন। Admin সব অফিসারের ডেটা দেখতে পারবেন।
7. Suspend করলে Officer-এর নতুন CRM ডেটা access বন্ধ হবে। Reactivate করলে আবার চালু হবে।

Cloud-এ ডেটা কীভাবে জমা হবে?
--------------------------------
- Customer Save করলে Firestore-এর customers collection-এ যাবে।
- Survey Save করলে surveys collection-এ যাবে।
- Visit Save করলে visits collection-এ যাবে।
- প্রতিটি রেকর্ডে ownerId, officerName, employeeId, branch সংরক্ষিত হয়।
- Officer-এর রেকর্ডগুলো ownerId দিয়ে আলাদা করা হয় এবং Firestore Rules-ও সেই সীমা enforce করে। শুধু UI-তে লুকানো নয়।
- Admin Panel-এর Officer-wise ও Branch-wise summary এই রেকর্ড থেকে তৈরি হয়।

Export / PDF
------------
Reports থেকে CSV Export করুন। CSV Microsoft Excel-এ খুলবে। PDF-এর জন্য Print / Save PDF চাপুন, তারপর Android/Chrome বা কম্পিউটারে Save as PDF নির্বাচন করুন।

Password
--------
Forgot Password দিয়ে Firebase email reset link পাঠানো যায়। Change Password দিয়ে লগইন থাকা অবস্থায় Password পরিবর্তন করা যায়। নিরাপত্তার জন্য শক্তিশালী Password ব্যবহার করুন। এই প্রাথমিক V3-তে ৯০ দিনের বাধ্যতামূলক rotation enforcement নেই; শুধু password পরিবর্তনের সুবিধা আছে। ৯০ দিনের policy বাধ্যতামূলক করতে পরবর্তী ধাপে Cloud Functions/সার্ভার-সাইড enforcement যোগ করতে হবে।

বর্তমান সংস্করণের সীমা
---------------------
- এটি V3-এর কার্যকর Cloud foundation: registration, approval, login, password reset/change, officer role separation, customer/survey/visit CRUD, admin status control, dashboard summary, CSV/PDF export।
- Advanced chart drill-down, CSV bulk officer provisioning, audit log, automated 90-day password enforcement, automated scheduled reminders, Cloud Functions-based admin workflows এখনো অন্তর্ভুক্ত নয়।
- PWA offline shell cache করে; Cloud-এ লিখতে/পড়তে ইন্টারনেট প্রয়োজন।
- প্রথমবার Admin setup অবশ্যই Firebase Console থেকে করতে হবে।

ব্যাংকিং ডেটা নিরাপত্তা সতর্কতা
-----------------------------
ব্যাংকের অনুমোদন ও IT/Security review ছাড়া গ্রাহকের Account Number, NID, ঋণের গোপন তথ্য, আর্থিক statement বা সংবেদনশীল ব্যক্তিগত তথ্য কোনো ব্যক্তিগত Firebase project-এ সংরক্ষণ করবেন না। অনুমোদিত Market Survey তথ্য ও প্রয়োজনীয় ন্যূনতম business contact data-তেই সীমাবদ্ধ রাখুন। GitHub Pages-এ প্রকাশিত HTML/JS সবাই দেখতে পারে; তাই সেখানে কোনো password, private key বা customer data লিখবেন না।

সমস্যা হলে দ্রুত যাচাই
---------------------
- Firebase config ঠিক বসেছে কি?
- Authentication-এ Email/Password Enable আছে কি?
- Firestore Database তৈরি হয়েছে কি?
- firestore.rules Publish হয়েছে কি?
- GitHub Pages domain Firebase Authorized domains-এ আছে কি?
- Admin UID document-এর ID সঠিক কি?
- Admin-এর users/{uid} document-এ status=approved কি?

Firebase official guides:
https://firebase.google.com/docs/web/setup
https://firebase.google.com/docs/auth/web/start
https://firebase.google.com/docs/firestore/security/get-started
GitHub Pages guide:
https://docs.github.com/en/pages/quickstart
