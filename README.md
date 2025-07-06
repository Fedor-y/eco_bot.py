# eco_bot.py
import telebot
from telebot.types import InlineKeyboardMarkup, InlineKeyboardButton
import time
from conf import TG_TOKEN


bot = telebot.TeleBot(token=TG_TOKEN)

@bot.message_handler(commands=['start', 'help'])
def hello_mes(message):
    bot.send_message(message.chat.id, 'привет, у меня есть команда (/eco)')


    
def get_inline_keyboard():
    markup = InlineKeyboardMarkup()
    markup.add(InlineKeyboardButton('пластиковая бутылка', callback_data='bt1'))
    markup.add(InlineKeyboardButton('стекло', callback_data='bt2'))
    markup.add(InlineKeyboardButton('банановая кожура', callback_data='bt3'))
    markup.add(InlineKeyboardButton('бумага', callback_data='bt4'))
    markup.add(InlineKeyboardButton('ал банка', callback_data='bt5'))    
    return markup


                                                                #---ECO---
@bot.message_handler(commands=['eco'])

def eco_items(message):
    bot.send_message(message.chat.id, 'выбери предмет', reply_markup=get_inline_keyboard())

@bot.callback_query_handler(func=lambda call: call.data in ['bt1', 'bt2', 'bt3', 'bt4', 'bt5'])
def handle_eco_callback(call):
    items = {
        'bt1': '450 лет',
        'bt2': '1000 лет',
        'bt3': '3 недели',
        'bt4': '3 месяца',
        'bt5': '200 лет'
    }
    bot.answer_callback_query(call.id)
    bot.send_message(call.message.chat.id, items[call.data])

# новая функция по экологии :






bot.infinity_polling()
