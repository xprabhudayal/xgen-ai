# XGen-AI [BOT](https://t.me/x_genai_bot) 🤖 

Welcome to the XGen-AI Bot project! This bot is built with Python, backed by the llama3 model, and integrates with Telegram to provide AI-powered interactions.

[![xgenai.png](https://i.postimg.cc/Jz3dQcqy/xgenai.png)](https://postimg.cc/ZWRcbNXZ)

## To run the Bot 🤖, use Colab or SageMaker Studio Lab 

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
Here's the graph for the **Training loss vs Step size** 

[![training.png](https://i.postimg.cc/KzWpbn63/training.png)](https://postimg.cc/BLxBc1QJ)

code for the generation of the graph

```python

import matplotlib.pyplot as plt


steps = list(range(1, 61))
training_loss = [2.327300, 1.431800, 1.671600, 1.828400, 1.723400, 1.448000, 1.125600, 1.661500, 1.715100, 1.380100, 
                 1.507000, 1.364800, 1.737800, 1.650400, 1.277000, 1.182300, 1.039400, 1.073800, 1.294300, 1.574200, 
                 1.330000, 1.223400, 1.462400, 0.993300, 1.279900, 1.369200, 1.244500, 1.300900, 1.339500, 1.317200, 
                 1.481800, 1.419600, 1.149300, 1.589900, 1.493600, 1.334200, 1.117900, 1.302200, 1.498100, 1.401000, 
                 1.264400, 1.278900, 1.567100, 1.412500, 1.414700, 1.420700, 1.453300, 1.420000, 1.826700, 1.514000, 
                 1.621700, 1.375200, 1.220700, 1.511000, 1.081900, 1.572400, 1.356300, 1.590700, 1.270300, 1.181600]


plt.figure(figsize=(10, 6))
plt.plot(steps, training_loss, marker='o', linestyle='-', color='b')

plt.title('Training Loss over Steps')
plt.xlabel('Step')
plt.ylabel('Training Loss')

plt.grid(True)
plt.show()

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
    await update.message.reply_text("Hi, I am xGenAI. I'm an enthusiastic assistant and an AI model, backed by Meta Llama3.")

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
