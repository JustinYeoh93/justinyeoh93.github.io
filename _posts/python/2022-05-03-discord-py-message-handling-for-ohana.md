---
title: "Discord automation: Message handling with Python for Ohana!"
date:   2022-05-02 14:10:26 +0900
category: python
tags: python discord
---
# Introduction
There is a small group of supporters of the streamer [Hitohira Hana](http://hitohira-hana.com/) that came together to develop random tools to support her stream.

This is one those tools and its sole goal to act as the support for [Nightbot](https://nightbot.tv/) in our [discord](https://discord.gg/kEwSAa9D) server. 

# Courtesy 
This could not have been achievable without the work done on the open source [discord.py](https://discordpy.readthedocs.io/en/stable/).

# TLDR
GitHub repository found here: [ohana-discord-bot](https://github.com/ohanagirl/ohana-discord-bot)

# Features
List of features we hope to add with this bot.
- custom message responses

## Custom message response
This section holds all the customer responses we are doing.

|Message Pattern|Response|
|-|-|
|/\s?pon\s?/gmi|No pon!|

# Code Architecture
The code flow is pretty straight forward.
1. Connect the client to discord
2. Listen to events
3. Handle specific events

## Connect the client to discord
We connect the client using the below snippet.

```python
# main.py
def main():
    TOKEN = os.getenv("DISCORD_TOKEN")
    client = NewDiscordClient() # from client.py
    client.run(TOKEN)

# client.py
def NewDiscordClient(*args, **kwargs) -> discord.Client:
    intents = discord.Intents.all()
    client = CustomClient(intents=intents)

    return client
```

This instantiates a new discord client that will be ran with a bot token.

## Listen to events
We will now build the client to listen to various events. 

```python
# client.py
class CustomClient(discord.Client):
    async def on_ready(self):
        print(f'{self.user} has connected')

    async def on_message(self, message: discord.Message):
        # Don't process any messages that is not directed to TEST_GUILD
        if ENVIRONMENT != "prod":
            if message.guild.name != TEST_GUILD:
                print(
                    f'Messages to other than "{TEST_GUILD}" will not be processed in non-prod environment')
                return

        # Don't talk to yourself
        if message.author == self.user:
            return

        # Handle messages
        await onMessageHandler.handleMessage(message=message)
```

We handle any messages by guilds that the bot is a part of. 

We also added checks to prevent the bot from processing any messages that come from other guilds in the event we put it into maintenance mode.

## Handle specific events
Our main goal is to handle the `on_message` events. This is all compiled and stored in our `onMessageHandler.handleMessage` function.

```python
# controller.py
async def handleMessage(message: Message):
    # Add your handlers in the entity section and await them here
    await pon.NoPon(message=message)
    
# entity.py
async def NoPon(message: Message):
    print(message.content)
    res = re.search("\W*pon\W*", "pon!", re.IGNORECASE)
    if res:
        await message.channel.send("No pon!")
```

We then pass all the business logic specific functions that we want to handle into the `handleMessage` function.

The above example is an event where if someone says the word that matches the regex `pon`, we will respond to the same channel with the message `"No pon!"`. 

# Conclusion
This structure allows us to create a very collaborative environment with other teams. 

They do not need to know the specifics of Python to make changes to the business logic. 

We will add commands in another article.