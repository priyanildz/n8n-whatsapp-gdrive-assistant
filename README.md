####WHATSAPP-DRIVEN GOOGLE DRIVE ASSISTANT (n8n)
This project is a fully functional WhatsApp-based assistant for managing Google Drive files, built entirely as a workflow in n8n. It allows a user to send simple chat commands via WhatsApp to list, move, delete, and generate AI-powered summaries of their documents.

✨ Features
List Files: Get information about files inside a specified Google Drive folder.

Move Files: Move a file from one folder to another.

Safe Deletion: Deletes a file only after a specific confirmation command.

AI Summarization: Generate concise, bullet-point summaries of text-based documents (.pdf, .txt, .docx, etc.) using the OpenAI API.

Audit Logging: Every command sent to the assistant is automatically logged in a Google Sheet with a timestamp.

🛠️ Tech Stack
Automation: n8n (self-hosted via Docker)

Messaging: Twilio Sandbox for WhatsApp

Cloud Storage: Google Drive API

AI Summarization: OpenAI API

Public Webhook: ngrok

🚀 Setup and Installation
Follow these steps to set up and run the project locally.

Prerequisites
Docker Desktop: Must be installed and running.

n8n: This project is designed to run on a local n8n instance.

Twilio Account: A free account is sufficient.

Google Cloud Account: To create API credentials.

OpenAI Account: To get an API key.

Step 1: Set up n8n with Docker
Create a new project folder and save a docker-compose.yml file with the following content:

version: '3.8'

services:
  n8n:
    image: n8n:latest
    container_name: n8n
    restart: always
    ports:
      - "5678:5678"
    volumes:
      - n8n_data:/home/node/.n8n
    environment:
      - N8N_BASIC_AUTH_ACTIVE=true
      - N8N_BASIC_AUTH_USER=admin
      - N8N_BASIC_AUTH_PASSWORD=your_secure_password
      - N8N_HOST=localhost
      - N8N_PORT=5678
      - N8N_PROTOCOL=http
      - N8N_EDITOR_BASE_URL=http://localhost:5678/
      - GENERIC_TIMEZONE=Asia/Kolkata
      - TZ=Asia/Kolkata
    command: start

volumes:
  n8n_data:

In your terminal, navigate to the project folder and run:

docker-compose up -d

Access n8n at http://localhost:5678 and complete the owner setup.

Step 2: Configure Google Cloud & OpenAI Credentials
Google Drive API: In the Google Cloud Console, enable the Google Drive API, create an OAuth 2.0 client ID (Web application), and add http://localhost:5678/rest/oauth2-credential/callback as an Authorized Redirect URI.

OpenAI API: Go to the OpenAI API keys page and create a new secret key.

Step 3: Import and Configure the n8n Workflow
In your n8n instance, go to the Workflows page and click "Import from File".

Import the whatsapp-drive-assistant.json file from your project folder.

Click on the nodes with a red triangle and add your credentials for Google Drive, Twilio, and OpenAI.

Manually connect the nodes following the instructions in the project's documentation.

In every Twilio node, replace the placeholder phone number with your actual Twilio WhatsApp number.

Step 4: Set up the Webhook
Open a new terminal and run: ngrok http 5678

Copy the https forwarding URL from ngrok.

In your n8n workflow, click the Webhook node and copy its path (e.g., webhook/whatsapp-inbound).

Combine the ngrok URL and the webhook path to get your full public URL.

In the Twilio Console's WhatsApp Sandbox settings, paste this full URL into the "When a message comes in" field and click Save.

💬 Command Reference
Send the following commands from your WhatsApp number to the Twilio number.

Command

Example

Description

LIST

LIST MyFolder

Lists all files inside MyFolder.

DELETE

DELETE MyFile.pdf

Deletes the file named MyFile.pdf.

MOVE

MOVE MyFile.pdf Archive

Moves MyFile.pdf to the Archive folder.

SUMMARY

SUMMARY MyDocument.txt

Generates an AI summary of MyDocument.txt.

📜 License
This project is licensed under the MIT License
