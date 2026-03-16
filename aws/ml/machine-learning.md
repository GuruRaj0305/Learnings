## Machine Learning


### Amazon Rekognition

- Find objects, people, text, scenes in images and videos using ML
- Facial analysis and facial search to do user verification, people counting
- Create a database of “familiar faces” or compare against celebrities
- **Use cases:**
  - Labeling
  - Content Moderation
  - Text Detection
  - Face Detection and Analysis (gender, age range, emotions…)
  - Face Search and Verification
  - Celebrity Recognition
  - Pathing (e.g., for sports game analysis)

### Amazon Rekognition – Content Moderation

- Detect content that is inappropriate, unwanted, or offensive (images and videos)
- Used in social media, broadcast media, advertising, and e-commerce to create a safer user experience
- Set a Minimum Confidence Threshold for items that will be flagged
- Flag sensitive content for manual review in Amazon Augmented AI (A2I)
- Helps comply with regulations

### Amazon Transcribe

- Automatically convert speech to text
- Uses a deep learning process called Automatic Speech Recognition (ASR) to convert speech to text quickly and accurately
- Automatically remove Personally Identifiable Information (PII) using Redaction
- Supports Automatic Language Identification for multi-lingual audio
- **Use cases:**
  - Transcribe customer service calls
  - Automate closed captioning and subtitling
  - Generate metadata for media assets to create a fully searchable archive

### Amazon Polly

- Turn text into lifelike speech using deep learning
- Allows you to create applications that talk

### Amazon Polly – Lexicon & SSML

- Customize the pronunciation of words with Pronunciation lexicons:
  - Stylized words: `St3ph4ne` → “Stephane”
  - Acronyms: `AWS` → “Amazon Web Services”
- Upload the lexicons and use them in the `SynthesizeSpeech` operation
- Generate speech from plain text or from documents marked up with **Speech Synthesis Markup Language (SSML)** – enables more customization:
  - Emphasizing specific words or phrases
  - Using phonetic pronunciation
  - Including breathing sounds, whispering
  - Using the Newscaster speaking style

### Amazon Translate

- Natural and accurate language translation
- Allows you to localize content (such as websites and applications) for international users
- Easily translate large volumes of text efficiently

### Amazon Lex & Connect

- **Amazon Lex** (same technology that powers Alexa):
  - Automatic Speech Recognition (ASR) to convert speech to text
  - Natural Language Understanding to recognize the intent of text/callers
  - Helps build chatbots, call center bots
- **Amazon Connect:**
  - Receive calls, create contact flows, cloud-based virtual contact center
  - Can integrate with other CRM systems or AWS
  - No upfront payments; 80% cheaper than traditional contact center solutions
- **Typical flow:** Phone Call → Amazon Connect → Amazon Lex (intent recognized) → Lambda (invoke) → e.g., Schedule an Appointment

### Amazon Comprehend

- For Natural Language Processing (NLP)
- Fully managed and serverless service
- Uses machine learning to find insights and relationships in text:
  - Language of the text
  - Extracts key phrases, places, people, brands, or events
  - Understands how positive or negative the text is
  - Analyzes text using tokenization and parts of speech
  - Automatically organizes a collection of text files by topic
- **Sample use cases:**
  - Analyze customer interactions (emails) to find what leads to positive or negative experience
  - Create and group articles by topics that Comprehend will uncover

### Amazon Comprehend Medical

- Detects and returns useful information in unstructured clinical text:
  - Physician’s notes
  - Discharge summaries
  - Test results
  - Case notes
- Uses NLP to detect Protected Health Information (PHI) – `DetectPHI` API
- Store documents in Amazon S3, analyze real-time data with Kinesis Data Firehose, or use Amazon Transcribe to transcribe patient narratives into text for analysis by Comprehend Medical

### Amazon SageMaker AI

- Fully managed service for developers / data scientists to build ML models
- Typically, difficult to do all the processes in one place + provision servers
- Machine Learning process:
  1. Collect historical data (e.g., years of experience in IT, time spent on course, exam scores)
  2. Label & build the training dataset
  3. Train and tune the ML model
  4. Apply model to new data for predictions

### Amazon Kendra

- Fully managed document search service powered by Machine Learning
- Extract answers from within a document (text, PDF, HTML, PowerPoint, MS Word, FAQs…)
- Natural language search capabilities
- Learns from user interactions/feedback to promote preferred results (Incremental Learning)
- Ability to manually fine-tune search results (importance of data, freshness, custom…)
- Data Sources: Amazon S3, Amazon RDS, Google Drive, MS SharePoint, MS OneDrive, 3rd party APNs, and more

### Amazon Personalize

- Fully managed ML service to build apps with real-time personalized recommendations
- Example: personalized product recommendations/re-ranking, customized direct marketing (e.g., User bought gardening tools → provide next gardening recommendation)
- Same technology used by Amazon.com
- Integrates into existing websites, applications, SMS, email marketing systems…
- Implement in days, not months (no need to build, train, and deploy ML solutions from scratch)
- **Use cases:** retail stores, media and entertainment

### Amazon Textract

- Automatically extracts text, handwriting, and data from any scanned documents using AI and ML
- Extract data from forms and tables
- Read and process any type of document (PDFs, images…)
- **Use cases:**
  - Financial Services (e.g., invoices, financial reports)
  - Healthcare (e.g., medical records, insurance claims)
  - Public Sector (e.g., tax forms, ID documents, passports)

### AWS Machine Learning – Summary

| Service | Purpose |
| --- | --- |
| Rekognition | Face detection, labeling, celebrity recognition |
| Transcribe | Audio to text (e.g., subtitles) |
| Polly | Text to audio |
| Translate | Language translations |
| Lex | Build conversational bots – chatbots |
| Connect | Cloud contact center |
| Comprehend | Natural Language Processing |
| SageMaker | Machine learning for every developer and data scientist |
| Kendra | ML-powered search engine |
| Personalize | Real-time personalized recommendations |
| Textract | Detect text and data in documents |
