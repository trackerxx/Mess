# Messenger WebView App

এই প্রজেক্ট দিয়ে GitHub Actions ব্যবহার করে একটা সিম্পল Android APK বানানো যাবে যেটা `messenger.com` লোড করে — কিন্তু **desktop version হিসেবে**, যাতে full desktop Messenger UI (chat list + conversation পাশাপাশি) পাওয়া যায়, আর সেই সাথে সেটা phone-এর স্ক্রিনে **auto-fit** হয়ে যাবে, zoom করা লাগবে না।

## কীভাবে এটা কাজ করে (আগের Facebook app থেকে যা বদলানো হয়েছে)

1. **Desktop User-Agent spoof**: `MainActivity.kt`-এ `settings.userAgentString` সেট করা আছে একটা desktop Chrome UA দিয়ে। এর ফলে `messenger.com` মনে করে একটা কম্পিউটার থেকে ভিজিট হচ্ছে, তাই মোবাইল সাইটে redirect না করে সরাসরি desktop UI পাঠায়।
2. **Auto-fit scaling**: Desktop পেজে মোবাইলের মতো "viewport" ট্যাগ থাকে না, তাই WebView জানে না কতটা ছোট করে দেখাতে হবে। তাই প্রতিবার পেজ লোড হওয়ার পর একটা ছোট JavaScript চালানো হয় যেটা পেজের আসল width বের করে viewport ট্যাগে বসিয়ে দেয়। এটা `settings.loadWithOverviewMode = true` আর `settings.useWideViewPort = true`-এর সাথে মিলে পুরো desktop layout-টাকে স্ক্রিনে ফিট করে দেয়।
3. চাইলে এখনো pinch-zoom করা যাবে (`setSupportZoom(true)`), কিন্তু শুরুতেই এটা ফিট অবস্থায় থাকবে, বাড়তি zoom করা লাগবে না।

## ব্যবহারের ধাপ

1. GitHub-এ একটা নতুন **repository** বানান (public বা private, দুটোই চলবে)।
2. এই ফোল্ডারের সব ফাইল ও সাব-ফোল্ডার (`.github` সহ) সেই repository-তে আপলোড করুন।
3. আপলোড হয়ে গেলে GitHub repo-র উপরে **"Actions"** ট্যাবে যান।
4. "Build APK" workflow-টা অটোমেটিক রান হবে (push করার সাথে সাথে)। যদি না হয়, "Run workflow" বাটনে ক্লিক করুন।
5. রান শেষ হলে, workflow run-এর পেজে নিচে **"Artifacts"** সেকশনে `messenger-webview-app-debug` নামে একটা ফাইল পাবেন — ওটা ডাউনলোড করুন।
6. ডাউনলোড হওয়া zip-এর ভেতরে `app-debug.apk` থাকবে — এটাই আপনার অ্যাপ।
7. ফোনে এই APK কপি করে ইনস্টল করুন ("Install from unknown sources" পারমিশন allow করে দিন)।

## নোট

- এই APK একটা "debug" ভার্সন — নিজের ব্যবহারের জন্য যথেষ্ট, Play Store-এ দেওয়ার জন্য উপযুক্ত নয়।
- অ্যাপ ওপেন করলে সরাসরি `https://www.messenger.com` (desktop version) লোড হবে এবং লগইন সেশন মনে রাখবে।
- যদি কোনো নির্দিষ্ট অংশ (যেমন কোনো বাটন বা পপ-আপ) এখনো স্ক্রিনে ফিট না হয়, তাহলে বলুন — `fitToScreenJs` স্ক্রিপ্টটা আরও টিউন করা যাবে।
