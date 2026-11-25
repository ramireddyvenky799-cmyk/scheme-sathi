# SchemeSathi

A voice-enabled government welfare scheme assistant for rural India, in Hindi and English. Speak or answer 5 simple icon questions, and SchemeSathi shows the schemes you may be eligible for, reads out the benefits and steps, and builds your document checklist.

Frontend only: plain HTML, CSS and JavaScript. No backend, no build step, no dependencies.

## Run it

1. Unzip the folder.
2. Open `index.html` in **Chrome or Edge** (best support for voice input).
3. If the microphone is blocked when opened as a file, serve the folder instead:
   ```
   npx serve schemesathi
   ```
   and open the address it prints (localhost allows microphone access).

On a phone, host the folder anywhere with HTTPS (microphone access requires a secure page) and open the link in Chrome.

## How to use

1. **Speak:** tap the microphone and say something like *"Main chhota kisan hoon, meri aay 1 lakh se kam hai"*. No microphone? Type the sentence in the box instead.
2. **Check:** the app shows what it understood as icon chips. Tap **Show schemes**, or **Answer remaining questions** to fill in the rest.
3. **Or answer 5 questions:** category, income, occupation, gender, state. Each question is read aloud.
4. **Results:** each scheme card shows the benefit, documents and steps. Tap **Listen** to hear them (tap again to stop).
5. **Checklist:** tick documents off, then **Print**, **Download** (.txt) or share on **WhatsApp**.
6. Use the **EN / हिं** button at the top to switch language.

## Features

| Feature | Where |
|---|---|
| Voice query (Hindi `hi-IN`, English fallback) with live transcript | `app.js` |
| 5-question icon questionnaire | `schemes.js` (`Q`), `app.js` |
| Rule-based matching engine, 15 schemes | `engine.js`, `schemes.js` |
| Spoken explainer (SpeechSynthesis) | `app.js` |
| Personalised document checklist (print / download / WhatsApp) | `app.js` |
| Hindi / English toggle, dark high-contrast theme | `app.js`, `style.css` |

Not included: nearest CSC finder and the voice-guided form-filling simulator (stretch goals).

## Project files

| File | Purpose |
|---|---|
| `index.html` | Page layout: Home, Quiz and Results views |
| `style.css` | Dark theme, large touch targets, print styles |
| `schemes.js` | Data: 15 schemes, documents, questions, Hindi/English text |
| `engine.js` | Spoken-query parser and eligibility engine (no DOM) |
| `app.js` | UI, voice input, speech output, checklist, language toggle |
| `PLAN.md` | Build plan |

## Customising

**Add or edit a scheme:** add a line to `SCHEMES` in `schemes.js`. Rules are plain conditions; leave one out to mean "no condition":

```js
s('id', 'Name EN', 'नाम HI', 'Benefit EN', 'लाभ HI',
  { occupation: ['farmer'], maxIncome: 250000, gender: 'female', category: ['sc','st'], states: ['up'] },
  ['aadhaar', 'land', 'bank'],            // document keys from DOCS
  'How to apply EN', 'आवेदन कैसे करें HI')
```

**Add a document:** add an entry to `DOCS` in `schemes.js`.
**Add a state:** add it to `STATES` in `schemes.js` (and keywords are picked up automatically from its names).
**Change wording:** edit the `T` object in `schemes.js`.

## How matching works

A scheme is shown only if every condition in its rules is met by a **known** answer. If the citizen hasn't answered a question that a scheme depends on (for example gender for a women-only scheme), that scheme is not shown until it is answered.

## Limitations

- Voice recognition needs a Chrome-based browser and an internet connection; accuracy varies with accent and noise. The confirmation chips let users spot and fix mistakes.
- Spoken Hindi needs a Hindi voice on the device. If none is installed, the app says so and the on-screen text cards still work.
- Benefit amounts and eligibility rules are simplified for demonstration. Always confirm on the official scheme portal or at a Common Service Centre (CSC) before applying.
