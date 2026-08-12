# ⚽ Match Organizer Bot

A feature‑rich Discord bot for managing football (soccer) matches.  
Built with `discord.py` 2.x and SQLite, it handles **reaction‑based registration**, **interactive lineup boards** with substitution queues, and full staff role control.

---

## 🚀 Features

- ✅ **Reaction Registration** – Users join by clicking ✅ on a match post.
- 🧑‍🤝‍🧑 **Interactive Lineup** – Click any position to claim it; if occupied, you join the sub queue.
- 🔄 **Automatic Substitution** – When a player leaves, the first sub takes their place.
- 👑 **Staff Permissions** – Designate a role via `/set_staff_role`; owners and admins also have access.
- 🛠️ **Custom Formations** – Create any lineup (up to 23 positions) with a comma‑separated list.
- 📦 **Single‑File** – Everything in one Python script + SQLite database.

---

## 📦 Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/yourusername/match-bot.git
   cd match-bot
```

2. **Install dependencies**

```
pip install discord.py python-dotenv
```
3. **Set up your bot token**
Create a `.env` file:

```
DISCORD_TOKEN=your-bot-token-here
```
4. **Run the bot**

```
python bot.py
```

---

## ⚙️ Configuration

After the bot is online, use the **slash command** `/set_staff_role` to define which role can manage matches.
Only the server owner or members with Administrator permission can set this.

---

## 📝 Commands

| Command ↕▾ | Description ↕▾ |
|---|---|
| −`/set_staff_role role:@role` | Set the staff role (owner/admin only). |
| −`/friendly` or `!friendly` | Open a match registration post with a ✅ reaction. |
| −`/lineup` or `!lineup` | Create an interactive lineup board. |
| −`/lineup 11` | 11v11 formation. |
| −`/lineup GK,CB,CB,CM,ST` | Custom formation. |
⚙

All lineup commands are **staff‑only**.

---

## 🧑‍💻 Usage Example

1. **Start registration**
Staff runs `/friendly` → bot posts a message with ✅ reaction.
2. **Players react** to register – the embed updates in real time.
3. **Create lineup**
Staff runs `/lineup` → a board appears with clickable positions.
4. **Claim a spot** – click any available position.
If it’s taken, you’re added to the sub queue for that position.
5. **Clear or customize** – staff can reset the board or use the **Custom Lineup** button to change positions.

---

## 🛠️ Technologies

- Python 3.8+
- discord.py 2.x
- SQLite
- python-dotenv

---

## 📄 License

MIT © 2026 – feel free to use and modify.

---

## 🤝 Contributing

Pull requests and issues are welcome!
For major changes, please open an issue first to discuss what you would like to change.

---

## 📬 Support

Join our [Discord server](https://discord.gg/your-server) or open an issue on GitHub.

