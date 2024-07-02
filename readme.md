# XGen-AI Bot 🤖

Welcome to the XGen-AI Bot project! This bot is built with Python, backed by the llama3 model, and integrates with Telegram to provide AI-powered interactions.

## Table of Contents

- [Overview](#overview)
- [Installation](#installation)
- [Usage](#usage)
- [Configuration](#configuration)
- [Finetuning](#finetuning)
- [Telegram Integration](#telegram-integration)
- [License](#license)
- [Contact](#contact)

## Overview 🌟

The XGen-AI Bot uses Telegram API to leverage the capabilities of Meta Llama 3 💝


- Fine-tuned with ShareGPT format for Inference 🧠
- Integrated with Telegram for interactive AI-powered chats 📱

## Installation 🛠️

Install the required packages by running:

```python
pip install python-telegram-bot transformers bitsandbytes datasets
pip install "unsloth[colab-new] @ git+https://github.com/unslothai/unsloth.git"
pip install --no-deps xformers "trl<0.9.0" peft accelerate bitsandbytes
```
## Usage 🚀
To use the bot, follow these steps:

- Clone the repository.
- Navigate to the project directory.
- Open and run main.ipynb in Jupyter Notebook.
- Can also run in Google Colab

## Configuration 🚀
The bot uses a quantized version of the LLM (Large Language Model), making it lightweight in resource usage compared to the original LLAMA3 model.

``` python 
from unsloth import FastLanguageModel
import torch
from huggingface_hub import notebook_login

model, tokenizer = FastLanguageModel.from_pretrained(
    model_name="unsloth/llama-3-8b-Instruct-bnb-4bit",
    max_seq_length=2048,
    dtype=None,
    load_in_4bit=True,
    cache_dir="./llama"
)

```
## Finetuning ⏲️
The notebook includes steps for fine-tuning the model using the Alpaca format (chat-based template).

Loading the Alpaca Format
```python
from unsloth.chat_templates import get_chat_template

tokenizer = get_chat_template(
    tokenizer,
    chat_template="llama-3",
    mapping={"role": "from", "content": "value", "user": "human", "assistant": "gpt"},
)

def formatting_prompts_func(examples):
    convos = examples["conversations"]
    texts = [tokenizer.apply_chat_template(convo, tokenize=False) for convo in convos]
    return {"text": texts}

```

## Telegram Integration 📱
The bot integrates with Telegram using the python-telegram-bot library.

- **Start**: Welcomes the user with a random greeting.
- **About**: Provides information about XGenAI.

```python
from telegram import Update
from telegram.ext import ApplicationBuilder, CommandHandler, ContextTypes, MessageHandler, filters
import random

async def start(update: Update, context: ContextTypes.DEFAULT_TYPE):
    greetings = [
        "Hey there! XgenAI at your service, what's going on?",
        "Oh hi! It's XgenAI here. What can I do for you today?",
        "XgenAI here! Ready for some fun? What can we discuss today?",
        "Hi there, you've reached XgenAI. How can I assist you?",
        "Namaste, XgenAI here! What would you like to talk about?"
    ]
    greet = random.choice(greetings)
    await update.message.reply_text(greet)

async def about(update: Update, context: ContextTypes.DEFAULT_TYPE):
    await update.message.reply_text("xGenAI is an integration of Telegram and Google's generative AI model.")

async def generate_prompt(update: Update, context: ContextTypes.DEFAULT_TYPE):
    text = update.message.text
    if text.startswith("."):
        text = text.replace('.', '')
        res = generate(text)
        await update.message.reply_text(res)
    else:
        await update.message.reply_text(handle_res(text))

# AI model based function
def generate(text: str) -> str:
    res = model.generate_content(text)
    return res.text

# Message handler
def handle_res(text: str) -> str:
    return "Prompt should be like this -> .prompt" if not text.startswith(".") else ""

if __name__ == '__main__':
    app = ApplicationBuilder().token('YOUR_TELEGRAM_BOT_TOKEN 💝').build()
    app.add_handler(CommandHandler('start', start))
    app.add_handler(CommandHandler('about', about))
    app.add_handler(MessageHandler(filters.TEXT, generate_prompt))
    app.run_polling()
```
## License 📄
This project is licensed under the MIT License.

## Contact 📧
For any inquiries, please reach me at : [MAIL](mailto:pradachan@tuta.io )


Made with 💖
