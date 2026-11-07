MY MARKET CRM V3 — EASY SETUP (বাংলা)

এই সংস্করণে Admin-এর জন্য আলাদা admins Collection হাতে তৈরি করতে হবে না।
Admin Email allow-list দিয়ে চেনা হবে। তাই একই Admin Email দুই জায়গায় বসানো বাধ্যতামূলক।

গুরুত্বপূর্ণ:
- এটি Firebase-এর সঙ্গে সংযোগ না করা পর্যন্ত Cloud app হিসেবে কাজ করবে না।
- Firebase Web config অবশ্যই বসাতে হবে।
- Admin email-টি index.html এবং firestore.rules—দুই জায়গায় একই হতে হবে।
- Rules-এ Admin email প্রকাশ্য থাকে; তাই শুধু নির্দিষ্ট, নিয়ন্ত্রিত অফিসিয়াল Email ব্যবহার করুন।
- Bank-এর অনুমোদন ছাড়া সংবেদনশীল গ্রাহক/ঋণ/NID/অ্যাকাউন্ট তথ্য ব্যক্তিগত Firebase-এ রাখবেন না।

ধাপ ১ — Authentication:
Firebase Console > Build > Authentication > Get started
Sign-in method > Email/Password > Enable > Save

ধাপ ২ — Firestore:
Build > Firestore Database > Create database
Production mode নির্বাচন করুন এবং Create করুন।

ধাপ ৩ — Firebase Web App Config:
Project Overview > </> Add app > Web app Register
Firebase config থেকে apiKey, authDomain, projectId, storageBucket, messagingSenderId, appId কপি করুন।
index.html ফাইলে firebaseConfig অংশে PASTE_ / YOUR_PROJECT_ID মানগুলোর জায়গায় নিজের মান বসান।
storageBucket-এ Firebase Console যে মান দেয়, সেটিই হুবহু ব্যবহার করুন।

ধাপ ৪ — Admin Email:
index.html-এ:
const ADMIN_EMAILS=['shafiq.jhe22@gmail.com']
এখানে Admin-এর Login Email বসান।
firestore.rules ফাইলেও একই Email বসান:
request.auth.token.email in ['shafiq.jhe22@gmail.com']

ধাপ ৫ — Rules Publish:
Firestore Database > Rules
বর্তমান Rules পুরো Select All করে মুছুন।
এই ZIP-এর firestore.rules-এর সম্পূর্ণ কোড Paste করুন।
Publish চাপুন।

ধাপ ৬ — GitHub Pages:
Repository-র root-এ index.html, manifest.json, sw.js, firestore.rules আপলোড করুন।
Settings > Pages > Deploy from a branch > main > /(root) > Save।
Authentication > Settings > Authorized domains-এ আপনার GitHub Pages domain যোগ করুন।

ধাপ ৭ — প্রথম Admin Login:
প্রকাশিত GitHub Pages URL খুলুন।
Officer Registration দিয়ে Admin-এর নির্দিষ্ট Email এবং Password দিয়ে account তৈরি করুন।
তারপর একই Email/Password দিয়ে Login করুন।
Allow-list-এ থাকা Email হলে অ্যাপ নিজে users/{UID} Admin profile তৈরি করবে; admins Collection হাতে তৈরি করতে হবে না।

ধাপ ৮ — Officer:
অফিসাররা নিজেদের Email দিয়ে Registration করবেন। তাদের status pending থাকবে।
Admin Panel থেকে যাচাই করে Approve/Reject করুন।

গুরুত্বপূর্ণ সীমাবদ্ধতা:
- Admin email allow-list-এ থাকলে সে Admin হবে। এই Email নিরাপদে নিয়ন্ত্রণ করুন।
- এই starter version-এ Admin Email পরিবর্তন করতে হলে index.html ও firestore.rules দুটোই আপডেট করে Publish করতে হবে।
- Firebase Console-এর Rules ও App-এর কোডে একই Admin Email না থাকলে Admin access কাজ করবে না।
- Password Firebase Authentication-এ থাকে; Firestore-এ Password রাখা হয় না।


এই প্যাকেজে নির্ধারিত Admin Email: shafiq.jhe22@gmail.com
