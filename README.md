# 🎮 The Etching Press

> **Compile narrative worlds into playable AI text adventures**

A visual world-builder and game engine that transforms your lore, characters, and rules 
into fully playable AI-powered RPGs with automatic state tracking.

![The Etching Press Interface](https://github.com/user-attachments/assets/786f9df1-1eab-4bf9-908f-2ac0183a2e80)

![Running Game](https://github.com/user-attachments/assets/7726f6c4-9a1b-4cdc-b8dc-8f5e8ba11e23)

---

## ✨ Features

- 🎨 **Visual World Builder** — Define your universe through an intuitive web interface
- 🤖 **Multi-Provider AI Support** — Works with Claude, Gemini, or GPT (you choose)
- 📊 **Glyph-Based State Management** — AI outputs structured tags that auto-update game state
- 💾 **Save/Load System** — Persistent game states via JSON export
- 🔄 **Smart Context Handling** — Automatic conversation archiving to manage token limits
- 📦 **Zero-Config Deployment** — Everything bundles into a single downloadable project
- 🎭 **Genre-Agnostic** — Fantasy, sci-fi, noir, horror, therapy journaling — anything works

---

## 🎯 What Problem Does This Solve?

Traditional AI chat interfaces don't track game state. You can play text adventures, but:
- ❌ Inventory/stats/location drift out of sync
- ❌ No visual representation of world state
- ❌ Conversations don't persist across sessions
- ❌ You're locked into one AI provider

**The Etching Press fixes all of this.**

---

## 🔮 The Glyph System (How It Works)

Instead of just narrating, the AI outputs **invisible structured data tags**:
```
You enter the crypt. The air grows cold.
[Location: The Forgotten Crypt | Visibility: Low | Danger: 8]
[Player: Health: -5 | Status: Chilled]
```

The game engine:
1. **Extracts** these glyphs with regex
2. **Parses** key-value pairs (numbers, booleans, strings)
3. **Updates** the live game state displayed in sidebars
4. **Strips** them from the narrative text
5. **Applies** logic (numbers accumulate, other values override)

**Result:** The AI naturally manages game mechanics while telling a story.

---

## 🚀 Quick Start

### **1. Download the Builder**
Clone this repo or [download the HTML file](link-to-file).

### **2. Open `etching-press.html` in your browser**
No server needed — it's a standalone tool.

### **3. Build Your World**
Fill in:
- **Identity** — Who is the AI? What tone should it use?
- **Glyphs** — Define data structures (`[Health: 100]`, `[Location: City]`)
- **Laws** — Immutable rules the AI must follow
- **Lore** — Characters, locations, backstory

### **4. Compile & Download**
Hit **"📦 COMPILE & DOWNLOAD GAME"** — you'll get a `.zip` with:
```
AI_Game_Project/
├── app.py                 # Flask backend
├── requirements.txt       # Python dependencies
├── .env                   # API key configuration
└── templates/
    └── index.html         # Playable game interface
```

### **5. Set Up & Run**
```bash
# Install dependencies
pip install -r requirements.txt

# Edit .env with your API keys
# (At minimum, add ONE key — Claude, Gemini, or GPT)

# Run the server
python app.py

# Open http://localhost:5000 in your browser
```

**That's it.** Your game is live.

---

## 🎮 Example Worlds

### **Fantasy: The Greywake**
```
Glyphs:
[Player: HP: 100 | Sanity: 100 | Location: Starting Village]
[Inventory: Gold: 50 | Weapon: Rusty Sword]
[World: Day: 1 | Threat Level: 2]

Laws:
- Death is permanent. If HP reaches 0, the story ends.
- Sanity affects perception. Below 30, hallucinations occur.

Characters:
- **The Witness** — A blind oracle who speaks in riddles
- **Kael the Sellsword** — Mercenary with flexible morals
```

### **Noir Detective: Rain City**
```
Glyphs:
[Investigation: Clues: 0 | Suspects: 0]
[Reputation: Heat: 0 | Street Cred: 5]
[Time: Hour: 8 | Day: Monday]

Laws:
- Time advances with each action. Locations close at night.
- Heat above 10 = Police raid, game over.

Characters:
- **Dame Holloway** — Femme fatale with a dangerous secret
- **Officer Briggs** — Corrupt cop on the take
```

---

## 🛠️ Configuration

### **API Keys** (`.env` file)
```bash
# Add at least ONE of these:
GEMINI_API_KEY=your-key-here
ANTHROPIC_API_KEY=your-key-here
OPENAI_API_KEY=your-key-here
```

### **Model Selection**
```bash
# Narrator (the main game AI)
NARRATOR_MODEL=gemini-3.1-pro-preview
# Options: claude-sonnet-4-6 | gemini-3.1-pro-preview | gpt-5.4

# Image Generation (optional, not yet implemented)
IMAGE_MODEL=gpt-image-1.5
# Options: imagen-4.0-generate-001 | gpt-image-1.5
```

---

## 📚 Advanced Usage

### **Custom Glyph Parsers**
The default parser handles:
```
[Key: Value]                    → Simple assignment
[Key: Value | Key2: Value2]     → Multiple keys
[Category: Key: Value]          → Nested structure
[Player: HP: -10]               → Numeric delta (adds/subtracts)
```

**Want custom logic?** Edit the `extractAndParseGlyphs()` function in `index.html`.

### **Adding New AI Providers**
1. Add the SDK to `requirements.txt`
2. Add a new provider case in `app.py`'s `chat()` function
3. Update `.env` with the new key

### **Styling the Game UI**
All CSS is in the `<style>` block of the generated `index.html`. Variables:
```css
--bg-main: #09090b;       /* Main background */
--text-header: #38bdf8;   /* Accent color */
--accent: #3b82f6;        /* Primary action color */
```

---

## 🏗️ Architecture
```
┌─────────────────────────────────────────────┐
│  The Etching Press (World Builder)          │
│  - Visual editor for prompts/lore/glyphs    │
│  - Compiles system prompt                   │
│  - Generates game UI HTML                   │
└─────────────────────────────────────────────┘
                    ↓
         📦 Downloads AI_Game_Project.zip
                    ↓
┌─────────────────────────────────────────────┐
│  Flask Backend (app.py)                     │
│  - /api/chat → Send action, get response    │
│  - /api/archive → Summarize old logs        │
│  - Multi-provider abstraction layer         │
└─────────────────────────────────────────────┘
                    ↓
┌─────────────────────────────────────────────┐
│  Game UI (index.html)                       │
│  - Three-panel layout                       │
│  - Glyph extraction & state updates         │
│  - Save/load game state                     │
└─────────────────────────────────────────────┘
```

---

## 📜 License

MIT License — see [LICENSE](LICENSE) for details.

---

## 🙏 Acknowledgments

Built with:
- [Flask](https://flask.palletsprojects.com/) — Backend framework
- [JSZip](https://stuk.github.io/jszip/) — Client-side ZIP generation
- [Google Generative AI](https://ai.google.dev/) — Gemini API
- [Anthropic](https://www.anthropic.com/) — Claude API
- [OpenAI](https://openai.com/) — GPT API

---

## 🔗 Links

- **Reddit:** [r/AIPlayableFiction](https://www.reddit.com/r/AIPlayableFiction/)

---

**Built by Blake Brandon**  
*"Define the parameters of your universe. Compile your reality."*
```
