# Speech-to-Text n8n Workflow

A simple and efficient n8n workflow that converts audio/video files to text using various Speech-to-Text APIs. This workflow receives audio/video files via webhook, transcribes them, and returns the text transcript.

## 🎯 Features

- **Webhook-based**: Easy integration with any application
- **Multiple provider support**: Choose from various Speech-to-Text services
- **Simple setup**: Just 4 nodes to get started
- **Instant response**: Returns transcript immediately after processing

## 🏗️ Workflow Architecture

```
Webhook → Speech-to-Text API → Extract Transcript → Respond to Webhook
```

## 📋 Prerequisites

- n8n instance (self-hosted or cloud)
- API credentials for your chosen Speech-to-Text provider

## 🔧 Setup Instructions

### 1. Import the Workflow

1. Copy the workflow JSON from `workflow.json`
2. In n8n, go to **Workflows** → **Import from File** or **Import from URL**
3. Paste the JSON and import

### 2. Configure Credentials

Choose one of the supported providers and set up credentials:

#### **ElevenLabs** (Default in this workflow)
- Sign up at [ElevenLabs](https://elevenlabs.io/)
- Get your API key from the dashboard
- In n8n: **Credentials** → **ElevenLabs account** → Add API key

#### **OpenAI Whisper**
- Sign up at [OpenAI](https://platform.openai.com/)
- Generate API key from API settings
- In n8n: **Credentials** → **OpenAI account** → Add API key
- Replace the ElevenLabs node with **OpenAI** node
- Select **Whisper** model for transcription

#### **Google Cloud Speech-to-Text**
- Enable Speech-to-Text API in [Google Cloud Console](https://console.cloud.google.com/)
- Create service account and download JSON key
- In n8n: **Credentials** → **Google Cloud** → Upload service account key
- Replace with **Google Cloud Speech-to-Text** node

#### **AssemblyAI**
- Sign up at [AssemblyAI](https://www.assemblyai.com/)
- Get API key from dashboard
- In n8n: **Credentials** → **AssemblyAI** → Add API key
- Use HTTP Request node with AssemblyAI endpoints

#### **Azure Speech Services**
- Create Speech resource in [Azure Portal](https://portal.azure.com/)
- Get subscription key and region
- In n8n: **Credentials** → **Microsoft Azure** → Add credentials
- Use HTTP Request node with Azure endpoints

#### **AWS Transcribe**
- Set up AWS account and create IAM user
- Enable Amazon Transcribe service
- In n8n: **Credentials** → **AWS** → Add access keys
- Use AWS node configured for Transcribe

### 3. Activate the Workflow

1. Click **Active** toggle in the top-right corner
2. Note the webhook URL from the Webhook node
3. Use this URL to send audio/video files

## 🚀 Usage

### Making a Request

Send a POST request to your webhook URL with the audio/video file:

#### Using cURL
```bash
curl -X POST \
  https://your-n8n-instance.com/webhook/767c4afe-1c60-488a-bfe2-047d5fe0b8a8 \
  -H "Content-Type: multipart/form-data" \
  -F "file=@/path/to/your/audio.mp3"
```

#### Using Python
```python
import requests

url = "https://your-n8n-instance.com/webhook/767c4afe-1c60-488a-bfe2-047d5fe0b8a8"
files = {"file": open("audio.mp3", "rb")}

response = requests.post(url, files=files)
print(response.json())
```

#### Using JavaScript/Node.js
```javascript
const FormData = require('form-data');
const fs = require('fs');
const axios = require('axios');

const form = new FormData();
form.append('file', fs.createReadStream('audio.mp3'));

axios.post('https://your-n8n-instance.com/webhook/767c4afe-1c60-488a-bfe2-047d5fe0b8a8', form, {
  headers: form.getHeaders()
})
.then(response => console.log(response.data))
.catch(error => console.error(error));
```

### Response Format

```json
{
  "Transcript": "Your transcribed text will appear here..."
}
```

## 🔄 Supported File Formats

Depending on your chosen provider, common supported formats include:
- MP3
- WAV
- M4A
- FLAC
- OGG
- WebM
- MP4 (audio track)

## 📊 Provider Comparison

| Provider | Accuracy | Speed | Pricing | Languages | Special Features |
|----------|----------|-------|---------|-----------|------------------|
| **ElevenLabs** | ⭐⭐⭐⭐ | Fast | Competitive | 20+ | High accuracy, multiple accents |
| **OpenAI Whisper** | ⭐⭐⭐⭐⭐ | Fast | $0.006/min | 50+ | Best accuracy, multilingual |
| **Google Cloud** | ⭐⭐⭐⭐⭐ | Very Fast | Pay-as-you-go | 125+ | Real-time, word timestamps |
| **AssemblyAI** | ⭐⭐⭐⭐ | Fast | $0.00025/sec | English+ | Speaker detection, sentiment |
| **Azure Speech** | ⭐⭐⭐⭐ | Very Fast | Pay-as-you-go | 100+ | Custom models, real-time |
| **AWS Transcribe** | ⭐⭐⭐⭐ | Fast | $0.024/min | 35+ | Medical/legal vocabulary |

## 🛠️ Customization

### Adding Language Detection
Add a **Set** node after transcription to detect language:
```javascript
// In Set node
{{ $json.language || 'auto-detected' }}
```

### Adding Error Handling
1. Add an **Error Trigger** node
2. Connect it to send notifications or log errors
3. Configure retry logic if needed

### Saving Transcripts to Database
Add a database node (PostgreSQL, MongoDB, MySQL) after the Extract Transcript node to store results.

### Adding Authentication
In the Webhook node:
1. Go to **Authentication** section
2. Choose **Header Auth** or **Basic Auth**
3. Set credentials for secure access

## 🐛 Troubleshooting

### Issue: "No credentials found"
**Solution**: Make sure you've configured credentials for your chosen Speech-to-Text provider in n8n.

### Issue: "File too large"
**Solution**: Most providers have file size limits (usually 25MB). Consider splitting large files or using streaming APIs.

### Issue: "Invalid audio format"
**Solution**: Convert your audio to a supported format (MP3, WAV) before uploading.

### Issue: "Timeout errors"
**Solution**: Increase the timeout in the Webhook node settings for large files.

## 📝 License

MIT License - feel free to use and modify as needed.

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## 📧 Support

For issues or questions:
- Open an issue in this repository
- Check [n8n community forum](https://community.n8n.io/)
- Review provider-specific documentation

## 🔗 Useful Links

- [n8n Documentation](https://docs.n8n.io/)
- [ElevenLabs API Docs](https://elevenlabs.io/docs)
- [OpenAI Whisper API](https://platform.openai.com/docs/guides/speech-to-text)
- [Google Cloud Speech-to-Text](https://cloud.google.com/speech-to-text/docs)
- [AssemblyAI Docs](https://www.assemblyai.com/docs)
- [Azure Speech Services](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/)
- [AWS Transcribe](https://docs.aws.amazon.com/transcribe/)

---

⭐ If you find this workflow helpful, please star this repository!