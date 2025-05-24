# support_bot.py
import os
from telegram import Update, ReplyKeyboardMarkup
from telegram.ext import ApplicationBuilder, CommandHandler, MessageHandler, filters, ContextTypes

BOT_TOKEN = os.getenv("BOT_TOKEN")
ADMIN_ID = int(os.getenv("ADMIN_ID"))

# Button options
menu_keyboard = ReplyKeyboardMarkup(
    keyboard=[
        ["How to Use Website", "How to Recharge"],
        ["Deposit Not Added", "Withdrawal Problem"],
        ["Investment Plans", "Invite & Earn"],
        ["Gift Code Rules", "Latest Updates"],
        ["Join Telegram Group", "User Feedback"]
    ], resize_keyboard=True
)

RESPONSES = {
    "How to Use Website": "1. Register on the website\n2. Login daily to earn\n3. Withdraw anytime you want.",
    "How to Recharge": "1. Go to Recharge page\n2. Choose amount and method\n3. Complete the payment. For help contact @MessyPowerSupport",
    "Deposit Not Added": "Please send UTR, screenshot and ID to @MessyPowerSupport",
    "Withdrawal Problem": "Send your withdrawal screenshot to @MessyPowerSupport01",
    "Investment Plans": "Visit website > Plans section to view all current investment options.",
    "Invite & Earn": "Share your referral link and earn for each signup!",
    "Gift Code Rules": "Gift codes are available daily for active users only. Check Telegram group.",
    "Latest Updates": "Follow our Telegram group and website dashboard for daily updates.",
    "Join Telegram Group": "Click here: https://t.me/yourgroup",
    "User Feedback": "Please share your suggestions or issues by messaging admin @Profitking2111"
}

async def start(update: Update, context: ContextTypes.DEFAULT_TYPE):
    await update.message.reply_text(
        "Welcome to Support Bot! Choose an option below:",
        reply_markup=menu_keyboard
    )

async def handle_message(update: Update, context: ContextTypes.DEFAULT_TYPE):
    user_text = update.message.text.strip()
    response = RESPONSES.get(user_text, "Please choose a valid option from the menu.")
    await update.message.reply_text(response)

def main():
    app = ApplicationBuilder().token(BOT_TOKEN).build()
    app.add_handler(CommandHandler("start", start))
    app.add_handler(MessageHandler(filters.TEXT & ~filters.COMMAND, handle_message))
    app.run_polling()

if __name__ == '__main__':
    main()
