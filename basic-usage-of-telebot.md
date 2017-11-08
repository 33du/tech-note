#The basic usage of telebot API

Install via npm in project directory:
```
npm install telebot --save
```

Create a .js file like index.js, and inside it require the api and get a (echo)bot:
```javascript
const TeleBot = require('telebot')
const bot = new TeleBot('TELEGRAM_BOT_TOKEN') //Token from BotFather

bot.on('text', (msg) => msg.reply.text(msg.text)) //bot.on(<event>, <function>) handles telebot events

bot.start() //start polling
```

Then run:
```
node index.js
```
and you can talk with the bot!
