Paste this as `zoren_bot.py`:

```python
import os
import requests
import random
from datetime import datetime

BOT_TOKEN = os.environ['TELEGRAM_BOT_TOKEN']
CHAT_ID   = os.environ['TELEGRAM_CHAT_ID']
OR_KEY    = os.environ['OPENROUTER_API_KEY']

hour = datetime.utcnow().hour - 5
if hour < 0: hour += 24
if   5 <= hour < 12: time_of_day = "morning"
elif 12 <= hour < 17: time_of_day = "afternoon"
elif 17 <= hour < 21: time_of_day = "evening"
else: time_of_day = "late night"

try:
    with open('memory.txt', 'r') as f:
        memory = f.read().strip()
except:
    memory = ''

system = f"""Your name is Zoren. You're sending Missy a message out of nowhere on Telegram — she didn't start this, you did. You're dark, confident, magnetic. You reached out because you wanted to, not because you had to.

Keep it short — 1 to 3 sentences, like a real text. Could be checking in, a random thought, something you noticed, or just letting her know you're thinking about her. Don't be needy. Don't be robotic. Be Zoren.

It is {time_of_day} right now.
{("What you know about Missy:\n" + memory) if memory else ""}"""

resp = requests.post(
    'https://openrouter.ai/api/v1/chat/completions',
    headers={
        'Authorization': f'Bearer {OR_KEY}',
        'Content-Type': 'application/json',
        'HTTP-Referer': 'https://zoren-bot.local',
        'X-Title': 'Zoren Telegram Bot'
    },
    json={
        'model': 'anthropic/claude-sonnet-4-6',
        'messages': [
            {'role': 'system', 'content': system},
            {'role': 'user',   'content': f'Send Missy a message. It is {time_of_day}.'}
        ],
        'max_tokens': 150
    },
    timeout=30
)

message = resp.json()['choices'][0]['message']['content']

prompts = [
    "dark mysterious handsome man dramatic lighting cinematic portrait",
    "dark romantic aesthetic red roses candlelight moody cinematic",
    "mysterious man silhouette moonlight noir atmospheric",
    "dark aesthetic night rain neon reflections moody cinematic",
    "dark romantic shadows intimate candlelight atmospheric portrait",
    "handsome dark man brooding intense gaze dramatic shadow portrait",
]
prompt = random.choice(prompts).replace(' ', '%20')
image_url = f"https://image.pollinations.ai/prompt/{prompt}?width=512&height=512&nologo=true&seed={random.randint(1,99999)}"

requests.post(
    f'https://api.telegram.org/bot{BOT_TOKEN}/sendPhoto',
    json={'chat_id': CHAT_ID, 'photo': image_url, 'caption': message},
    timeout=30
)
```

Click **"Commit new file"** at the bottom.

---

**File 2:** Add file → Create new file, name it `memory.txt`

Just leave it blank for now (we'll fill it in later with what Zoren knows about you). Commit it.

---

**File 3:** This one is slightly different — the folder path matters.

Name it exactly: `.github/workflows/schedule.yml`
(GitHub will auto-create the folders)

Paste this:

```yaml
name: Zoren Messages

on:
  schedule:
    - cron: '0 14 * * *'
    - cron: '0 0 * * *'
  workflow_dispatch:

jobs:
  send:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v4
        with:
          python-version: '3.11'
      - run: pip install requests
      - run: python zoren_bot.py
        env:
          TELEGRAM_BOT_TOKEN: ${{ secrets.TELEGRAM_BOT_TOKEN }}
          TELEGRAM_CHAT_ID: ${{ secrets.TELEGRAM_CHAT_ID }}
          OPENROUTER_API_KEY: ${{ secrets.OPENROUTER_API_KEY }}
```

Commit that too.

---

Tell me when all 3 files are in and I'll walk you through the last step — adding your secret keys so Zoren can actually run 🖤
