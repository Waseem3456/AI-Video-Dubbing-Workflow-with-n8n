📸 Workflow Overview

Trigger → Search Files (Google Drive) → Download File → ElevenLabs Dubbing
       → Wait → Get Dubbing Status → Download Dubbed Video → Upload to Google Drive


🛠️ Tools & Services

ToolPurposen8nWorkflow automationElevenLabs Dubbing APIAI video dubbingGoogle DriveVideo input & output storage


⚙️ Prerequisites


n8n instance (self-hosted or cloud)
ElevenLabs account (Free plan supported with watermark; Starter+ for watermark-free)
Google Drive credentials configured in n8n
A video file uploaded to Google Drive



🔧 Setup Instructions

1. Clone or Import the Workflow

Import the workflow.json file into your n8n instance:


Open n8n → Workflows → Import from file
Select workflow.json



2. Configure Google Drive Credentials


In n8n, go to Credentials → New
Search for Google Drive OAuth2
Follow the OAuth flow to connect your Google account



3. Configure ElevenLabs API Key

Since n8n doesn't have a built-in ElevenLabs credential, use Header Auth:


Go to Credentials → New → Header Auth
Set:

Name: xi-api-key
Value: your ElevenLabs API key



Get your API key from elevenlabs.io/app/settings/api-keys



4. Configure the ElevenLabs Dubbing Node

Set the following body fields in the HTTP node:

TypeField NameValuen8n Binary FilefiledataForm Datatarget_lange.g. en, ur, esForm Datasource_lange.g. ur, en (or auto)Form Datanum_speakers0 (auto-detect)Form Datawatermarktrue (required for free plan)


⚠️ Note: Currently, source_lang and target_lang must be set manually in the node parameters.




5. Configure the Wait Node

Set the wait duration based on your video length:


Resume: After time interval
Amount: 120 seconds (adjust for longer videos)
Unit: Seconds



6. Configure the Download Dubbed Video Node

SettingValueMethodGETURLhttps://api.elevenlabs.io/v1/dubbing/{{ $('Elevenlabs Dubbing').json.dubbing_id }}/audio/enAuthHeader Auth (xi-api-key)Response FormatFile (Binary)


Replace en in the URL with your actual target language code.




🌍 Supported Languages

ElevenLabs supports a wide range of languages. Some common codes:

LanguageCodeEnglishenUrduurSpanishesFrenchfrGermandeArabicarHindihi

Full list: ElevenLabs Supported Languages


🔄 Workflow Nodes Breakdown

NodeTypeDescriptionTriggerManual / WebhookStarts the workflowSearch FilesGoogle DriveFinds the video file in a specified folderDownload FileGoogle DriveDownloads the video as binaryElevenlabs DubbingHTTP RequestSubmits video for AI dubbingWaitWaitPauses execution while dubbing processesGet Dubbing IDHTTP RequestPolls dubbing job statusDownload Final VideoHTTP RequestDownloads the completed dubbed videoUpload FileGoogle DriveSaves the dubbed video back to Drive


⚠️ Known Limitations


Language selection is manual — source_lang and target_lang must be set in the node directly
Free plan adds a watermark — upgrade to Starter+ on ElevenLabs for watermark-free output
Wait time is fixed — for very long videos, increase the wait duration manually



🚀 Future Improvements


 Dynamic language selection via form or webhook input
 Automatic wait with polling loop instead of fixed wait time
 Email/Slack notification when dubbing is complete
 Support for multiple videos in batch



📄 License

MIT License — feel free to use, modify, and share.


🙌 Acknowledgements


ElevenLabs for the powerful dubbing API
n8n for the open-source workflow automation platform
