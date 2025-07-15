# AWS-Comprehend

Objective:
Extract summarized insights, keywords, sentiment, and topics from a video file

✅ Step 1: Enable Transcribe & Create Job
Option A: Use AWS Console
Go to Amazon Transcribe Console

Click Create transcription job

Fill in:

Name: databricks-workshop-transcription

Language: Auto-detect or manually set (e.g., English)

Input file location: s3://dbrickstraining735/Data Intelligence with Databricks on AWS Workshop.mp4

Format: MP4

Output location: Choose default or specify an S3 bucket

Optionally enable:

Speaker identification

Custom vocabulary (if technical terms like “Databricks”, “Delta Lake”, etc.)

Click Create Job

Option B: Use AWS CLI
bash
Copy
Edit
aws transcribe start-transcription-job \
  --transcription-job-name "databricks-workshop-transcription" \
  --language-code "en-US" \
  --media MediaFileUri="s3://dbrickstraining735/Data Intelligence with Databricks on AWS Workshop.mp4" \
  --output-bucket-name "your-output-bucket-name"
✅ Step 2: Wait for Transcription Completion
Once complete, it will generate a JSON file with:

json
Copy
Edit
{
  "results": {
    "transcripts": [{ "transcript": "full text here..." }],
    "items": [...]
  }
}
You can also download the transcript text from the provided output S3 location.

✅ Step 3: Clean and Extract Text (optional)
You can write a small Python script or AWS Lambda to parse just the text:

python
Copy
Edit
import json

with open("transcript.json") as f:
    data = json.load(f)
    transcript_text = data['results']['transcripts'][0]['transcript']
✅ Step 4: Analyze with Amazon Comprehend
Now that you have clean text, pass it to Amazon Comprehend to extract:

📌 Example: Entity Detection (Python Boto3)
python
Copy
Edit
import boto3

comprehend = boto3.client('comprehend')

response = comprehend.detect_entities(
    Text=transcript_text,
    LanguageCode='en'
)

for entity in response['Entities']:
    print(f"{entity['Text']} ({entity['Type']}) - Confidence: {entity['Score']}")
Other Comprehend APIs:
detect_sentiment

detect_key_phrases

detect_syntax

detect_dominant_language

batch_detect_entities (for larger texts split into chunks)
