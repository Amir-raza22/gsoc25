[<img width="100" src="https://cdn.simpleicons.org/googlesummerofcode" />](#) &nbsp;[<img width="100" src="https://www.gstatic.com/images/branding/productlogos/chrome_chromium/v8/192px.svg" />](#)  
Google Summer of Code 2025 for Chromium  

# 🚀 Chrome Extension APIs
Hello, this is Amit! I created this repository as a dedicated hub for my final work product submission, which illustrates and highlights my contributions to Chromium during Google Summer of Code - 2025.  

---

## 💡Project Links
-  [GSoC Project Page](https://summerofcode.withgoogle.com/programs/2025/projects/Z7zhQ3EO)
-  [Project Proposal](https://docs.google.com/document/d/1XU1QfxlAaMTtltbn0AeuzqW03Fjzkamwy0F3yjdhA0A/edit?usp=sharing)
-  [WECG Github](https://github.com/w3c/webextensions)
-  [Changes Made](https://chromium-review.googlesource.com/q/owner:ponnan2112@gmail.com)

## 🛠️ Project Overview  
The goal: **Improve the Chrome Extensions developer experience** by enhancing debugging tools, APIs, and extension workflows.  

Main focus areas:  
- Improvements to **`chrome://extensions`** page (the hub where developers manage/debug their extensions).  
- New/Improved **Extension APIs** (aligning with WECG standards).  
- Better **debugging experience** for Manifest V3 extensions.  

---

## 🔨 My Contributions  

### 1. Extensions Page – Better Error Visibility  
> Imagine seeing **"Errors"** on the extensions page every time, even if there were only some harmless warnings. Frustrating, right?  
> This was the very first bug I worked on in Chromium — and it taught me a lot about how UI labels are tied to backend logic.  

- **Before:** Always showed “Errors” → confusing when only warnings existed.  
- **After:** Now shows **“Warnings”** when appropriate.  
- Result → Developers instantly know whether it’s a critical issue or just a small warning.  

📸 *UI Update:*  
<table>
  <tr>
    <td align="center"><b>Before</b></td>
    <td align="center"><b>After</b></td>
  </tr>
  <tr>
    <td><img src="assets/warnings_before.png" width="400"/></td>
    <td><img src="assets/warnings_after.png" width="400"/></td>
  </tr>
</table>

🔗 [Code Change](https://chromium-review.googlesource.com/c/chromium/src/+/6639214)  

---

### 2. Debugging Made Easier – *View in DevTools* Button  
> I remember spending time trying to open the **right DevTools** for debugging extension errors. It wasn’t straightforward.  
> So I added a shortcut: a shiny new **"View in DevTools"** button. One click → you’re in the exact DevTools context you need.  

- Added a **button in the extension error page** to open the **right DevTools target directly**.  
- No more manually finding the right SW or background page!  

📸 *UI Update:*  
<table>
  <tr>
    <td align="center"><b>Before</b></td>
    <td align="center"><b>After</b></td>
  </tr>
  <tr>
    <td><img src="assets/devtools_before.png" width="400"/></td>
    <td><img src="assets/devtools_after.png" width="400"/></td>
  </tr>
</table>

🔗 [Code Change](https://chromium-review.googlesource.com/c/chromium/src/+/6702715)  

---

### 3. RuntimeError Enhancements  
> Debugging service workers in extensions is already tricky. I worked on adding metadata (`isServiceWorker`, `canInspect`) so Chromium knows *where exactly the error belongs*.  
> This way, even if a Service Worker is inactive, DevTools can spin it up and take you to the right place.  

- Added:  
  - `isServiceWorker` → flags if error belongs to a Service Worker.  
  - `canInspect` → allows DevTools inspection if error is from a SW.  
- Enables correct DevTools target selection (even for **inactive SWs**).  

🔗 [Code Change](https://chromium-review.googlesource.com/c/chromium/src/+/6830747)  

---

### 4. New API: `getSystemUILanguage()` *(WIP)*  
> Ever wondered how an extension knows your system’s UI language? I implemented an API that gives this info as a proper **BCP 47 language tag** — cross-platform and standards-compliant.  

- Returns OS language as **BCP 47 tag** (`en-US`, `fr-FR`, etc.).  
- Special handling for `C`/`POSIX` → maps to `en-US`.  
- Cross-platform support: **Windows, macOS, Linux**.  
- Added **comprehensive test coverage**.  

🔗 [Code Change](https://chromium-review.googlesource.com/c/chromium/src/+/6790185)  

---

### 5. Alarms API – Persistence Across Sessions *(WIP)*  
- Added **`persistAcrossSessions` flag** in `chrome.alarms.create`.  
- Default: `true` → alarms survive browser restarts.  
- Session-only alarms vanish after restart.  

📸 *Example Flow:*  
```js
chrome.alarms.create("dailyCheck", { delayInMinutes: 5, persistAcrossSessions: true });
```

🔗 [Code Change](https://chromium-review.googlesource.com/c/chromium/src/+/6843535)  

---

### 6. DNR – Request Header Matching (Partial)  
> Think of content-blocking extensions. They often need to filter requests based on headers like `User-Agent`.  
> I laid the groundwork for this by adding **IDL types and parsing logic** — ensuring headers are valid, consistent, and safe to use.  

- Added **IDL types and parsing** for request headers.  
- Validates header names, values, and excluded values.  
- Ensures rules remain consistent (e.g., no conflicts between include/exclude headers).  

🔗 [Code Change](https://chromium-review.googlesource.com/c/chromium/src/+/6756929)  

---

### 7. DNR – Regex Substitution Transforms *(Work in Progress)*  
> Imagine capturing a query string like `q=hello%20world`. Without decoding, it looks messy.  
> With my WIP feature, you can transform regex groups (e.g., `decodeURIComponent`) before substitution → `"hello world"` instead of `"hello%20world"`.  

- Extends **Declarative Net Request** substitutions with transform operations.  
- First operation: `decodeURIComponent`.  
- Designed to be extensible for future transforms (e.g., `toLowerCase`, `encodeURIComponent`).  

📜 Example manifest:  
```json
"regexSubstitutionTransforms": [
  { "group": 1, "operation": "decodeURIComponent" }
]
```
🔗 [Code Change](https://chromium-review.googlesource.com/c/chromium/src/+/6904039)  

---

## 🏄‍♂️ Challenges  
Working on Chromium was both exciting and challenging. Some of the toughest hurdles I faced:  

- 🧩 **Huge Codebase** → Navigating millions of lines of C++ and JS while understanding the extensions architecture.  
- 🔄 **DevTools Integration** → Figuring out how DevTools connects with background contexts and service workers.  
- 🧪 **Testing Complexity** → Understanding Chromium’s testing framework was tough. I had to learn how to design and add the right test files that covered edge cases thoroughly, while making sure I didn’t break or alter existing logic and flows.  
- 📏 **Standards vs. Implementation** → Balancing WECG specifications with Chrome-specific requirements.  
- ⏱️ **Debugging Build Issues** → Chromium builds are long and resource-heavy; fixing small errors often meant multiple rebuilds.  

---

## 🔮 Future Goals  
My journey with Chromium doesn’t stop here. I’m looking forward to:  

- ✅ Completing **DNR header indexing** for full request header matching.  
- ⚙️ Finishing **regex substitution transforms** to allow flexible query parameter handling.  
- 🌱 Exploring more opportunities to contribute to the **Chromium Extensions team** — whether through **GSoC 2026** or independent contributions outside the program.  

---

## ☀️ Mentors  
I am deeply grateful to my mentors:  

- **Oliver Dunk**  
- **Solomon Kinard**  
- **Sreeja Kamishetty**

for their guidance, patience, and detailed reviews. Every discussion helped me sharpen my approach and think like a browser engineer.  

<div align="center">

| ![](https://lh3.googleusercontent.com/a-/ALV-UjUzUEflsvjipGd7cvZEsFUHskmlfPymb4qDCb2t3fhdt4oaM5iB=s160-p-k-rw-no) | ![](https://avatars.githubusercontent.com/u/297660?v=4) | ![](https://lh3.googleusercontent.com/a-/ALV-UjWTqIdwc30XPXzudx8IHbPc2GZiXWXXPs5sXTWwp6sU6ks_yJuLKA=s160-p-k-rw-no) |
|:---:|:---:|:---:|
| **Oliver Dunk**<br>[✉️ Email](mailto:oliverdunk@google.com)<br>[🔗 GitHub](https://github.com/oliverdunk) | **Solomon Kinard**<br>[✉️ Email](mailto:solomonkinard@google.com)<br>[🔗 GitHub](https://github.com/solomonkinard) | **Sreeja Kamishetty**<br>[✉️ Email](mailto:sreejakshetty@google.com)<br>[🔗 GitHub](https://github.com/sreejakamishetty) |

</div>

---

## 📌 A Quick Recap of My GSoC Journey  
- 🚀 Contributed to the **Chromium codebase** for GSoC’25 under the Chrome Extensions team.  
- 🛠️ Gained hands-on experience with **browser internals, debugging flows, and API design**.  
- 🧪 Learned to write **robust tests that catch edge cases without disrupting existing flows**.  
- 🎯 Managed complexity in a **massive open-source project**.  
- 💡 Built confidence in collaborating with mentors and the community.  
- 🌱 Most importantly → I grew from being just a user of Chrome to **a contributor to the browser itself**.  

---
#Amir Raza new contributer


