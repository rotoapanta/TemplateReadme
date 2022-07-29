![Python Version](https://img.shields.io/pypi/pyversions/3)
[![GitHub issues](https://img.shields.io/github/issues/rotoapanta/prueba2)](https://github.com/rotoapanta/prueba2/issues)
![GitHub repo size](https://img.shields.io/github/repo-size/rotoapanta/prueba2)
![GitHub last commit](https://img.shields.io/github/last-commit/rotoapanta/prueba2)
![GitHub forks](https://img.shields.io/github/forks/rotoapanta/prueba2?style=plastic)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
![GitHub commit merge status](https://img.shields.io/github/commit-status/rotoapanta/prueba2/main/6a500cc65d)
![Discord](https://img.shields.io/discord/996422496842694726?style=plastic)
[![Discord Invite](https://img.shields.io/badge/discord-join%20now-green)](https://discord.gg/pp73rEVE)

# <p align="center">Prueba de Insignias (Badges) Github

<p align="center">This is an implementation of a Readme file as a Template  <a href="https://core.telegram.org/bots/api">Telegram Bot API</a>.</p>
<p align="center">Both synchronous and asynchronous.</p>

## <p align="center">Supported Bot API version: <a href="https://core.telegram.org/bots/api#june-20-2022">6.1</a>!

<h2><a href='https://pytba.readthedocs.io/en/latest/index.html'>Official documentation</a></h2>

## Contents

  * [Getting started](#getting-started)
  * [Writing your first bot](#writing-your-first-bot)
    * [Prerequisites](#prerequisites)
    * [A simple echo bot](#a-simple-echo-bot)
  * [General API Documentation](#general-api-documentation)
    * [Types](#types)
    * [Methods](#methods)
    * [General use of the API](#general-use-of-the-api)
      * [Message handlers](#message-handlers)
      * [Edited Message handler](#edited-message-handler)
      * [Channel Post handler](#channel-post-handler)

## Getting started

This API is tested with Python 3.6-3.10 and Pypy 3.
There are two ways to install the library:

* Installation using pip (a Python package manager):

```
$ pip install pyTelegramBotAPI
```
* Installation from source (requires git):

```
$ git clone https://github.com/eternnoir/pyTelegramBotAPI.git
$ cd pyTelegramBotAPI
$ python setup.py install
```
or:
```
$ pip install git+https://github.com/eternnoir/pyTelegramBotAPI.git
```

```sh
cd dillinger
npm i
node app
```


It is generally recommended to use the first option.

*While the API is production-ready, it is still under development and it has regular updates, do not forget to update it regularly by calling*
```
pip install pytelegrambotapi --upgrade
```

## Writing your first bot

### Prerequisites

It is presumed that you [have obtained an API token with @BotFather](https://core.telegram.org/bots#botfather). We will call this token `TOKEN`.
Furthermore, you have basic knowledge of the Python programming language and more importantly [the Telegram Bot API](https://core.telegram.org/bots/api).

### A simple echo bot

The TeleBot class (defined in \__init__.py) encapsulates all API calls in a single class. It provides functions such as `send_xyz` (`send_message`, `send_document` etc.) and several ways to listen for incoming messages.

Create a file called `echo_bot.py`.
Then, open the file and create an instance of the TeleBot class.
```python
import telebot

bot = telebot.TeleBot("TOKEN", parse_mode=None) # You can set parse_mode by default. HTML or MARKDOWN
```
*Note: Make sure to actually replace TOKEN with your own API token.*

After that declaration, we need to register some so-called message handlers. Message handlers define filters which a message must pass. If a message passes the filter, the decorated function is called and the incoming message is passed as an argument.

Let's define a message handler which handles incoming `/start` and `/help` commands.
```python
@bot.message_handler(commands=['start', 'help'])
def send_welcome(message):
	bot.reply_to(message, "Howdy, how are you doing?")
```
A function which is decorated by a message handler __can have an arbitrary name, however, it must have only one parameter (the message)__.

Let's add another handler:
```python
@bot.message_handler(func=lambda m: True)
def echo_all(message):
	bot.reply_to(message, message.text)
```
This one echoes all incoming text messages back to the sender. It uses a lambda function to test a message. If the lambda returns True, the message is handled by the decorated function. Since we want all messages to be handled by this function, we simply always return True.

*Note: all handlers are tested in the order in which they were declared*

We now have a basic bot which replies a static message to "/start" and "/help" commands and which echoes the rest of the sent messages. To start the bot, add the following to our source file:
```python
bot.infinity_polling()
```
Alright, that's it! Our source file now looks like this:
```python
import telebot

bot = telebot.TeleBot("YOUR_BOT_TOKEN")

@bot.message_handler(commands=['start', 'help'])
def send_welcome(message):
	bot.reply_to(message, "Howdy, how are you doing?")

@bot.message_handler(func=lambda message: True)
def echo_all(message):
	bot.reply_to(message, message.text)

bot.infinity_polling()
```
To start the bot, simply open up a terminal and enter `python echo_bot.py` to run the bot! Test it by sending commands ('/start' and '/help') and arbitrary text messages.

## General API Documentation

### Types

All types are defined in types.py. They are all completely in line with the [Telegram API's definition of the types](https://core.telegram.org/bots/api#available-types), except for the Message's `from` field, which is renamed to `from_user` (because `from` is a Python reserved token). Thus, attributes such as `message_id` can be accessed directly with `message.message_id`. Note that `message.chat` can be either an instance of `User` or `GroupChat` (see [How can I distinguish a User and a GroupChat in message.chat?](#how-can-i-distinguish-a-user-and-a-groupchat-in-messagechat)).

The Message object also has a `content_type`attribute, which defines the type of the Message. `content_type` can be one of the following strings:
`text`, `audio`, `document`, `photo`, `sticker`, `video`, `video_note`, `voice`, `location`, `contact`, `new_chat_members`, `left_chat_member`, `new_chat_title`, `new_chat_photo`, `delete_chat_photo`, `group_chat_created`, `supergroup_chat_created`, `channel_chat_created`, `migrate_to_chat_id`, `migrate_from_chat_id`, `pinned_message`.

You can use some types in one function. Example:

```content_types=["text", "sticker", "pinned_message", "photo", "audio"]```

### Methods

All [API methods](https://core.telegram.org/bots/api#available-methods) are located in the TeleBot class. They are renamed to follow common Python naming conventions. E.g. `getMe` is renamed to `get_me` and `sendMessage` to `send_message`.

### General use of the API

Outlined below are some general use cases of the API.

#### Message handlers
A message handler is a function that is decorated with the `message_handler` decorator of a TeleBot instance. Message handlers consist of one or multiple filters.
Each filter must return True for a certain message in order for a message handler to become eligible to handle that message. A message handler is declared in the following way (provided `bot` is an instance of TeleBot):
```python
@bot.message_handler(filters)
def function_name(message):
	bot.reply_to(message, "This is a message handler")
```
`function_name` is not bound to any restrictions. Any function name is permitted with message handlers. The function must accept at most one argument, which will be the message that the function must handle.
`filters` is a list of keyword arguments.
A filter is declared in the following manner: `name=argument`. One handler may have multiple filters.
TeleBot supports the following filters:

|name|argument(s)|Condition|
|:---:|---| ---|
|content_types|list of strings (default `['text']`)|`True` if message.content_type is in the list of strings.|
|regexp|a regular expression as a string|`True` if `re.search(regexp_arg)` returns `True` and `message.content_type == 'text'` (See [Python Regular Expressions](https://docs.python.org/2/library/re.html))|
|commands|list of strings|`True` if `message.content_type == 'text'` and `message.text` starts with a command that is in the list of strings.|
|chat_types|list of chat types|`True` if `message.chat.type` in your filter|
|func|a function (lambda or function reference)|`True` if the lambda or function reference returns `True`|
	
Here are some examples of using the filters and message handlers:

```python
import telebot
bot = telebot.TeleBot("TOKEN")

# Handles all text messages that contains the commands '/start' or '/help'.
@bot.message_handler(commands=['start', 'help'])
def handle_start_help(message):
	pass

# Handles all sent documents and audio files
@bot.message_handler(content_types=['document', 'audio'])
def handle_docs_audio(message):
	pass

# Handles all text messages that match the regular expression
@bot.message_handler(regexp="SOME_REGEXP")
def handle_message(message):
	pass

# Handles all messages for which the lambda returns True
@bot.message_handler(func=lambda message: message.document.mime_type == 'text/plain', content_types=['document'])
def handle_text_doc(message):
	pass

# Which could also be defined as:
def test_message(message):
	return message.document.mime_type == 'text/plain'

@bot.message_handler(func=test_message, content_types=['document'])
def handle_text_doc(message):
	pass

# Handlers can be stacked to create a function which will be called if either message_handler is eligible
# This handler will be called if the message starts with '/hello' OR is some emoji
@bot.message_handler(commands=['hello'])
@bot.message_handler(func=lambda msg: msg.text.encode("utf-8") == SOME_FANCY_EMOJI)
def send_something(message):
    pass
```
**Important: all handlers are tested in the order in which they were declared**

#### Edited Message handler
Handle edited messages
`@bot.edited_message_handler(filters) # <- passes a Message type object to your function`

#### Channel Post handler
Handle channel post messages
`@bot.channel_post_handler(filters) # <- passes a Message type object to your function`

#### Edited Channel Post handler
Handle edited channel post messages
`@bot.edited_channel_post_handler(filters) # <- passes a Message type object to your function`

#### Callback Query Handler
Handle callback queries
```python
@bot.callback_query_handler(func=lambda call: True)
def test_callback(call): # <- passes a CallbackQuery type object to your function
    logger.info(call)
```

#### Shipping Query Handler
Handle shipping queries
`@bot.shipping_query_handeler() # <- passes a ShippingQuery type object to your function`

#### Pre Checkout Query Handler
Handle pre checkoupt queries
`@bot.pre_checkout_query_handler() # <- passes a PreCheckoutQuery type object to your function`

#### Poll Handler
Handle poll updates
`@bot.poll_handler() # <- passes a Poll type object to your function`

#### Poll Answer Handler
Handle poll answers
`@bot.poll_answer_handler() # <- passes a PollAnswer type object to your function`

#### My Chat Member Handler
Handle updates of a the bot's member status in a chat
`@bot.my_chat_member_handler() # <- passes a ChatMemberUpdated type object to your function`

#### Chat Member Handler
Handle updates of a chat member's status in a chat
`@bot.chat_member_handler() # <- passes a ChatMemberUpdated type object to your function`
*Note: "chat_member" updates are not requested by default. If you want to allow all update types, set `allowed_updates` in `bot.polling()` / `bot.infinity_polling()` to `util.update_types`*

#### Chat Join Request Handler	
Handle chat join requests using:
`@bot.chat_join_request_handler() # <- passes ChatInviteLink type object to your function`

### Inline Mode

More information about [Inline mode](https://core.telegram.org/bots/inline).

#### Inline handler

Now, you can use inline_handler to get inline queries in telebot.

```python

@bot.inline_handler(lambda query: query.query == 'text')
def query_text(inline_query):
    # Query message is text
```

#### Chosen Inline handler

Use chosen_inline_handler to get chosen_inline_result in telebot. Don't forgot add the /setinlinefeedback
command for @Botfather.

More information : [collecting-feedback](https://core.telegram.org/bots/inline#collecting-feedback)

```python
@bot.chosen_inline_handler(func=lambda chosen_inline_result: True)
def test_chosen(chosen_inline_result):
    # Process all chosen_inline_result.
```

#### Answer Inline Query

```python
@bot.inline_handler(lambda query: query.query == 'text')
def query_text(inline_query):
    try:
        r = types.InlineQueryResultArticle('1', 'Result', types.InputTextMessageContent('Result message.'))
        r2 = types.InlineQueryResultArticle('2', 'Result2', types.InputTextMessageContent('Result message2.'))
        bot.answer_inline_query(inline_query.id, [r, r2])
    except Exception as e:
        print(e)

```

### Additional API features

#### Middleware Handlers

A middleware handler is a function that allows you to modify requests or the bot context as they pass through the 
Telegram to the bot. You can imagine middleware as a chain of logic connection handled before any other handlers are
executed. Middleware processing is disabled by default, enable it by setting `apihelper.ENABLE_MIDDLEWARE = True`. 

```python
apihelper.ENABLE_MIDDLEWARE = True

@bot.middleware_handler(update_types=['message'])
def modify_message(bot_instance, message):
    # modifying the message before it reaches any other handler 
    message.another_text = message.text + ':changed'

@bot.message_handler(commands=['start'])
def start(message):
    # the message is already modified when it reaches message handler
    assert message.another_text == message.text + ':changed'
```
There are other examples using middleware handler in the [examples/middleware](examples/middleware) directory.

#### Class-based middlewares
There are class-based middlewares. 
Basic class-based middleware looks like this:
```python
class Middleware(BaseMiddleware):
    def __init__(self):
        self.update_types = ['message']
    def pre_process(self, message, data):
        data['foo'] = 'Hello' # just for example
        # we edited the data. now, this data is passed to handler.
        # return SkipHandler() -> this will skip handler
        # return CancelUpdate() -> this will cancel update
    def post_process(self, message, data, exception=None):
        print(data['foo'])
        if exception: # check for exception
            print(exception)
```
