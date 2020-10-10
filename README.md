# bot-for-sanitation
from telegram.ext import Updater,CommandHandler,MessageHandler,Filters
from Adafruit_IO import Client
import os
def turnoff(update, context):
  context.bot.send_message(chat_id=update.effective_chat.id, text="sanitation off")
  context.bot.send_photo(chat_id=update.effective_chat.id,photo='https://pngimg.com/uploads/bulb/bulb_PNG1241.png')
  send_value(0)
def turnon(update, context):
  context.bot.send_message(chat_id=update.effective_chat.id, text="sanitation on")
  context.bot.send_photo(chat_id=update.effective_chat.id,photo='https://img.icons8.com/plasticine/2x/light-on.png')
  send_value(1)
def send_value(value):
  feed = aio.feeds('sree')
  aio.send_data(feed.key,value)
def input_message(update, context):
  text=update.message.text.upper()
  text=update.message.text
  if text == 'turn on':
    send_value(1)
    context.bot.send_message(chat_id=update.effective_chat.id,text="sanitation turned on")
    context.bot.send_photo(chat_id=update.effective_chat.id,photo='https://img.icons8.com/plasticine/2x/light-on.png')
  elif text == 'turn off':
    send_value(0)
    context.bot.send_message(chat_id=update.effective_chat.id,text="sanitation turned off")
    context.bot.send_photo(chat_id=update.effective_chat.id,photo='https://pngimg.com/uploads/bulb/bulb_PNG1241.png')
def Hi(update,context):
  start_message='''
  Hello i am ur bot please type 
  turnoff :To turn off sanitation
  turnon  :To turn on  sanitation
'''
  context.bot.send_message(chat_id=update.effective_chat.id, text=start_message)
ADAFRUIT_IO_USERNAME = os.getenv ('ADAFRUIT_IO_USERNAME')
ADAFRUIT_IO_KEY =os.getenv('ADAFRUIT_IO_KEY')
TOKEN =os.getenv ('TOKEN ')
aio = Client(ADAFRUIT_IO_USERNAME,ADAFRUIT_IO_KEY)
updater=Updater(TOKEN,use_context=True)
dispatcher = updater.dispatcher
dispatcher.add_handler(CommandHandler('turnoff',turnoff))
dispatcher.add_handler(CommandHandler('turnon',turnon))
dispatcher.add_handler(CommandHandler('Hi',Hi))
dispatcher.add_handler(MessageHandler(Filters.text & (~Filters.command),input_message))
updater.start_polling()
updater.idle()
