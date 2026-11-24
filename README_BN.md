# NIKITA AI — Android starter

এটি একটি **starter project**, Play Store-এ প্রকাশের জন্য প্রস্তুত production app নয়।
এতে আছে:
- Kotlin + Jetpack Compose Android chat UI
- Python FastAPI backend
- OpenAI Responses API দিয়ে AI উত্তর
- Android SpeechRecognizer দিয়ে voice input
- Android TextToSpeech দিয়ে উত্তর শোনা

## ১) Backend চালু করুন

Python 3.10+ লাগবে।

```bash
cd backend
python -m venv .venv
# Windows: .venv\Scripts\activate
# macOS/Linux: source .venv/bin/activate
pip install -r requirements.txt
```

`backend/.env.example` কপি করে `.env` নামে রাখুন এবং নিজের API key বসান:

```env
OPENAI_API_KEY=your_key_here
OPENAI_MODEL=gpt-5.5
```

তারপর:

```bash
uvicorn main:app --host 0.0.0.0 --port 8000
```

**API key কখনো Android app-এর মধ্যে রাখবেন না বা কারও সঙ্গে শেয়ার করবেন না।**
এই backend-এ demo CORS খোলা আছে; production-এ নিজের domain-এ সীমিত করুন, authentication,
rate limit, abuse protection এবং HTTPS যোগ করুন।

## ২) Android Studio-তে খুলুন

`android/` ফোল্ডারটি Android Studio-তে Open করুন এবং Gradle sync করুন।

`android/app/src/main/java/com/example/nikita/MainActivity.kt` ফাইলে
`API_BASE_URL` সেট করুন:

- Android Emulator: `http://10.0.2.2:8000`
- বাস্তব Android ফোন: `http://YOUR_COMPUTER_LAN_IP:8000` (ফোন ও কম্পিউটার একই Wi-Fi-তে)

এটি শুধু local development-এর জন্য HTTP ব্যবহার করে। Production-এ HTTPS URL ব্যবহার করুন
এবং `usesCleartextTraffic` বন্ধ করুন।

## ৩) চালান

প্রথমে backend চালু রাখুন, তারপর Android Studio থেকে app Run করুন।
মাইক্রোফোন ব্যবহারের অনুমতি দিন। মাইক্রোফোনের বোতামে চাপ দিয়ে কথা বলুন,
তারপর Send চাপুন। উত্তর এলে Speak চাপলে ফোনের TTS উত্তর পড়ে শোনাবে।

## গুরুত্বপূর্ণ সীমাবদ্ধতা

- এই starter এখন চ্যাট, voice input এবং text-to-speech করে; নিজে থেকে ফোনের সব কাজ করতে পারে না।
- Calendar, message, file ইত্যাদি কাজ যোগ করতে হলে আলাদা অনুমোদিত Android feature/API,
  permission এবং প্রতিটি গুরুত্বপূর্ণ কাজের আগে user confirmation দরকার।
- API ব্যবহারে খরচ হতে পারে; OpenAI API billing ChatGPT subscription থেকে আলাদা হতে পারে।
- App-এ conversation history এখন শুধু app চলাকালীন memory-তে থাকে; স্থায়ীভাবে সংরক্ষণ করে না।
