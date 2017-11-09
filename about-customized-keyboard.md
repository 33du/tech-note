# Customized keyboard of tg bot

```javascript
//creates a keyboard
bot.on(['/keyboard'], msg => {

  let replyMarkup = bot.keyboard([
    ['/east', '/south', '/west', '/north'],
    ['attack', '/hide keyboard']
  ], {resize: true});

  return bot.sendMessage(msg.from.id, 'This is keyboard.', {replyMarkup});

});

//hides keyboard, or use one-timed keyboard instead
bot.on('/hide', msg => {
  return bot.sendMessage(
    msg.from.id, 'Hide keyboard. Type /keyboard to show.', {replyMarkup: 'hide'}
  );
});

bot.start();
```
