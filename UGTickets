import discord
from discord.ext.commands import Bot
from discord.ext import commands
import asyncio

prefix = 'THE PREFIX OF THE BOT'
client = commands.Bot(command_prefix=prefix)

tickets = []
channel = 0 # THIS IS THE CHANNEL'S ID

@client.event
async def on_ready():
        print('Logged in!')

@client.event
async def on_message(msg):
    if msg.guild == None:
        embed = discord.Embed(colour=0x1FE658)
        if msg.author.id in tickets:
            embed.description = ':x: You can\'t make a new ticket until you get a reply for the last one.'
            await msg.author.send(embed=embed)
        elif len(msg.content) > 1500:
            embed.description = ':exclamation: The message cannot be longer than 1500 characters.'
            await msg.author.send(embed=embed)
        else:
            tickets.append(msg.author.id)
            embed.description = ':small_red_triangle_down: `User:` {}\n:small_red_triangle: `ID:` {}\n\n{}'.format(msg.author, msg.author.id, msg.content)
            await client.get_channel(channel).send(embed=embed)
            embed.description = ':white_check_mark: The ticket has been sent!\n__You\'ll get a reply as soon as possible.__'
            await msg.author.send(embed=embed)

@client.command()
async def reply(ctx, target = None, *, text = None):
    embed = discord.Embed(colour=0x1FE658)
    if ctx.message.channel.id != channel:
        embed.description = ':exclamation: This command can only be used in <#{}>.'.format(channel)
        await ctx.send(embed=embed)
    elif target == None or text == None:
        embed.description = ':exclamation: Invalid usage.\n__Proper usage:__ `{}reply <user ID> <text>`.'.format(prefix)
        await ctx.send(embed=embed)
    else:
        try:
            if int(target) not in tickets:
                embed.description = ':exclamation: No tickets found for that user.'
                await ctx.send(embed=embed)
            else:
                user = await client.fetch_user(int(target))
                if len(text) > 1500:
                    embed.description = ':exclamation: The text cannot be longer than 1500 characters.'
                    await msg.author.send(embed=embed)
                elif '#Me' in text:
                    m = ':bust_in_silhouette: **{}**:\m{}'.format(ctx.message.author, text)
                else:
                    m = ':bust_in_silhouette: **Reply**:\m{}'.format(text)
                embed.description = m
                try:
                    await user.send(embed=embed)
                    tickets.remove(int(target))
                    embed.description = ':white_check_mark: Reply sent!'
                    await ctx.send(embed=embed)
                except:
                    tickets.remove(int(target))
                    embed.description = ':exclamation: Unable to reply to that user. Their ticket has been removed.'
                    await ctx.send(embed=embed)
        except:
            embed.description = ':exclamation: Invalid user ID given.'
            await ctx.send(embed=embed)

client.run('TOKEN')
