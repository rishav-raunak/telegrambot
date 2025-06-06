# telegrambot
from telegram import Update
from telegram.ext import ApplicationBuilder, CommandHandler, ContextTypes
from PIL import Image, ImageDraw, ImageFont

import os

# Get token from environment variable
BOT_TOKEN = os.getenv("BOT_TOKEN")

async def start(update: Update, context: ContextTypes.DEFAULT_TYPE):
    await update.message.reply_text("Welcome! Send /logo YourName to get a logo.")

async def create_logo(update: Update, context: ContextTypes.DEFAULT_TYPE):
    name = ' '.join(context.args)
    if not name:
        await update.message.reply_text("Please use like this: /logo YourName")
        return

    # Create image with name
    img = Image.new('RGB', (500, 250), color=(30, 30, 30))
    draw = ImageDraw.Draw(img)
    font = ImageFont.load_default()
    draw.text((50, 100), name, font=font, fill=(255, 255, 255))

    file_path = "logo.png"
    img.save(file_path)

    await update.message.reply_photo(photo=open(file_path, 'rb'))

if __name__ == '__main__':
    app = ApplicationBuilder().token(BOT_TOKEN).build()
    app.add_handler(CommandHandler("start", start))
    app.add_handler(CommandHandler("logo", create_logo))
    app.run_polling()
