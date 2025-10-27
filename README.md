# n8n AI ChatBot (Telegram + OpenRouter)

This repository contains an n8n workflow for a conversational AI chatbot that integrates with **Telegram**. It uses a **LangChain Agent** to process user input, leverages a chat model from **OpenRouter**, and includes conversational memory.

## 🤖 Workflow Overview

This workflow powers a Telegram bot that can hold a conversation.

1.  **Trigger:** It starts when a user sends a message to the bot on **Telegram**.
2.  **Process:** The user's message text is sent to an **AI Agent (LangChain)**.
3.  **Think:** The AI Agent is equipped with:
      * **An LLM:** The `cognitivecomputations/dolphin3.0-mistral-24b` model, provided via the **OpenRouter Chat Model** node.
      * **Memory:** A **Simple Memory** node (Window Buffer) to remember the recent conversation history.
4.  **Respond:** The agent's generated output is sent back to the original user on Telegram as a reply.

## ✨ Features

  * **Telegram Integration:** Receives and replies to messages.
  * **LangChain Agent:** Uses a LangChain agent for robust prompt processing.
  * **OpenRouter Model:** Easily configurable to use any model from OpenRouter. This workflow is pre-configured to use `cognitivecomputations/dolphin3.0-mistral-24b`.
  * **Conversational Memory:** Remembers the context of the conversation with each user.

## 📋 Prerequisites

To use this workflow, you will need:

  * An active **n8n instance** (self-hosted or cloud).
  * A **Telegram Bot API Token**. You can get this from the [BotFather](https://t.me/botfather) on Telegram.
  * An **OpenRouter API Key**. You can get this from [OpenRouter.ai](https://openrouter.ai/).

## 🚀 How to Use

1.  **Download:** Save the `AI ChatBot.json` file from this repository.
2.  **Import to n8n:** Open your n8n canvas, click "Import," and select "Import from File." Upload the `AI ChatBot.json` file.
3.  **Configure Credentials:** You must add your own credentials for the following nodes:
      * **Telegram Trigger:**
          * Click the node.
          * In the "Credentials" section, select (or create) your Telegram credentials using your Bot API Token.
      * **OpenRouter Chat Model:**
          * Click the node.
          * In the "Credentials" section, select (or create) your OpenRouter credentials using your API key.
      * **Send a text message:**
          * Click the node.
          * In the "Credentials" section, select the *same* Telegram credentials you used for the trigger.
4.  **Activate:** Save and activate the workflow.
5.  **Test:** Send a message to your Telegram bot. It should now respond using the AI model\!

## ⚠️ Important Configuration Note

**To ensure the bot remembers each user's conversation correctly, you must update the memory node.**

The `Simple Memory` node in the provided JSON has its `Session Key` set to `={{ $json}}`. This will create a new, unique session for *every single message*, meaning the bot will have no memory.

**To fix this:**

1.  Click on the **Simple Memory** node.

2.  Find the **Session Key** parameter.

3.  Change the expression from `={{ $json}}` to:

    ```
    {{ $('Telegram Trigger').first().json.message.chat.id }}
    ```
