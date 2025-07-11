---
cssclasses:
  - hide-properties
obsidianUIMode: preview
---

# 🧭 [[01 Roadmap|Roadmap]] • [[09 📦 CARD SECTIONS|← Back]] • [[00 HUB|Home Page]] 🏠

---

# BLOCK 10 🛠️ SCRIPT INTEGRATION

> 💡 This step is **required** to launch the cards. Here, you connect the core script and the localization file.

---

## 🔹 Connecting the Core Script

This line is responsible for connecting the script core:

```js
const scriptPath = "scripts/universal-cards-core.js";
```

### 📌 Rules:

- `scriptPath` is a **relative path** to the script from the root of your Obsidian vault.
- Make sure the path points to the **latest version** of `universal-cards-core.js`.
- **Case sensitivity matters**: `"Script/"`, `"script/"`, and `"SCRIPT/"` are treated as different folders.

---

## 🔹 Connecting Language Files

This is done almost the same way as with the core script.  
You specify the file paths like this:

```js
const langCorePath = "scripts/universal-cards-lang-core.js";
const langUserPath = "scripts/universal-cards-lang-user.js";
```

### 📌 Rules:

- `langCorePath` — this is the system localization file that gets updated along with the script. **Do not edit this file manually.**
- `langUserPath` — this is the user localization file where you can **override any strings** or **add your own languages**. **This file is preserved during updates.**

**Attention!** Loading a custom text file is optional.  
In other words, you don’t have to include the line:

```js
const langUserPath = "scripts/universal-cards-lang-user.js";
```

---

## 🔧 Initialization Block

After defining the paths, **make sure to add the following block** (do not edit it — it must remain exactly as shown):

```js
const langCoreFile = app.vault.getAbstractFileByPath(langCorePath);
if (!langCoreFile) {
  dv.paragraph(`❌ Core localization file not found: ${langCorePath}`);
  return;
}
eval(await app.vault.read(langCoreFile));

if (typeof langUserPath === "string" && langUserPath.trim() !== "") {
  const langUserFile = app.vault.getAbstractFileByPath(langUserPath);
  if (langUserFile) {
    eval(await app.vault.read(langUserFile));
  } else {
    window.UNIVERSAL_CARDS_LANG_USER = {};
  }
} else {
  window.UNIVERSAL_CARDS_LANG_USER = {};
}

const scriptFile = app.vault.getAbstractFileByPath(scriptPath);
if (!scriptFile || typeof scriptFile.path !== "string") {
  dv.paragraph(
    `❌ Script not found or the path is incorrect. Your path: ${scriptPath}`
  );
  return;
}
const scriptContent = await app.vault.read(scriptFile);
eval(scriptContent);

if (typeof initializeSectionMatches === "function") {
  initializeSectionMatches(config);
}
if (typeof runUniversalCards === "function") {
  runUniversalCards(dv, config);
}
```

### 🔚 Where Should This Be Placed?

Immediately **after the configuration block**:

```js
const config = {
  // configuration parameters
};

// Script and localization loading goes here
```

---

🧪 Example Structure:

```py
📁 My_Movies/
├── 📁 scripts/
│   └── 📄 universal-cards-core.js
│   └── 📄 universal-cards-lang-core.js
│   └── 📄 universal-cards-lang-user.js
├── 📁 YAML folder/
│   └── 📄 Demo 1.md
└── 📄 demo-config.md ← this is the config
```

In this case, your paths would be:

```js
const scriptPath = "My_Movies/scripts/universal-cards-core.js";
const langCorePath = "My_Movies/scripts/universal-cards-lang-core.js";
const langUserPath = "My_Movies/scripts/universal-cards-lang-user.js";
```

---

# 🧭 [[01 Roadmap|Roadmap]] • [[09 📦 CARD SECTIONS|← Back]] • [[00 HUB|Home Page]] 🏠
