---
layout: distill
title: 随机波动 (StochasticVolatility) Analysis Using NLP and Machine Learning
description: Uncover the power of natural language processing (NLP) and machine learning techniques in analyzing 随机波动 Stochastic Volatility podcasts. Delve into the complete workflow, from data collection and audio transcription to text preprocessing, analysis, and visualization, to gain valuable insights on podcast topics, emotions, and overall content.
giscus_comments: true
date: 2023-04-27
tags: 
categories: NLP, Machine Learning,Podcasts
toc: true


authors:
  - name: Oscar Wu 
    url: wuyoscar.github.io
    affiliations:
      name: Personal Blog
  - name: Yika Gu 
    url: 
    affiliations:
      name: Deakin University 

toc:
  - name: Introduction
  - name: Collecting Podcast Data
  - name: Transcribing Podcast Audio to Text
  - name: Data Preprocessing 
  - name: Analyzing the Text Data
  - name: Visualizing the Results
  - name: Conclusion

_styles: >
  .fake-img {
    background: #bbb;
    border: 1px solid rgba(0, 0, 0, 0.1);
    box-shadow: 0 0px 4px rgba(0, 0, 0, 0.1);
    margin-bottom: 12px;
  }
  .fake-img p {
    font-family: monospace;
    color: white;
    text-align: left;
    margin: 12px 0;
    text-align: center;
    font-size: 16px;
  }
---


## Introduction
Podcasts are a popular form of media, offering listeners the opportunity to learn, be entertained, and stay informed. However, analyzing a podcast can be a time-consuming and challenging task, especially if you have a lot of episodes to review. In this blog post, we'll explore how to use natural language processing (NLP) and machine learning to analyze a podcast. By applying these techniques, we can gain insights into the topics discussed, the emotions expressed, and the overall content of the podcast. We'll cover the entire process, from collecting the podcast data to visualizing the results. So, let's get started! 

[Stochastic Volatility](https://www.stovol.club/) is a cross-cultural podcast initiated by three female media professionals. Today, I will use the Stochastic Volatility as an example to show you how to analyze a podcast using NLP and machine learning techniques.


  <div class="l-body">
    <figure>
    <a href = "https://www.stovol.club/">
      <img src="/assets/img/2023-04/随机波动.png" alt="Image description">
      <figcaption>This is a caption for the image.</figcaption>
      </a>
    </figure>
  </div>


## Collecting Podcast Data


At Stochastic Volatility, we have selected three episodes with themes related to ``Feminism`` for analysis. These episodes cover a range of topics related to women, such as gender equality, feminism, and women's roles in society. By using NLP and machine learning techniques to analyze the text data of these episodes, we hope to gain deeper insights into the discussions and perspectives presented on these important topics.


<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0 mb-4">
        <a href="https://www.stovol.club/096" target="_blank">
            <img src="/assets/img/2023-04/随机波动_96.png" class="img-fluid rounded z-depth-1" data-action="zoom" />
        </a>
    </div>
    <div class="col-sm mt-3 mt-md-0 mb-4">
        <a href="https://www.stovol.club/094" target="_blank">
            <img src="/assets/img/2023-04/随机波动_94.png" class="img-fluid rounded z-depth-1" data-action="zoom" />
        </a>
    </div>
    <div class="col-sm mt-3 mt-md-0 mb-4">
        <a href="https://www.stovol.club/085" target="_blank">
            <img src="/assets/img/2023-04/随机波动_85.png" class="img-fluid rounded z-depth-1" data-action="zoom" />
        </a>
    </div>
</div>



As you can see, the average duration of each episode is **1 hrs 30mins**, which means that we have to spend roughly **4 hrs 30mins** to listen to these three episodes. However, we can use NLP and machine learning techniques to analyze the text data of these episodes, which will save us a lot of time. However, before we can analyze the text data, we need to transcribe the audio to text. Next, we'll explore how to do this.

## Transcribing Podcast Audio to Text
To transcribe Chinese audio to text, there are several options available. Here are three popular choices:

- Baidu Speech Recognition API: This is a speech recognition service provided by Baidu, a Chinese tech giant. It supports Chinese and can transcribe audio to text in real-time. It is relatively accurate and has a free trial with a limited amount of transcription time per month.

- iFlytek Speech Recognition API: This is a speech recognition service provided by iFlytek, a leading Chinese AI company. It supports Chinese and can transcribe audio to text with high accuracy. It has a free trial with a limited amount of transcription time per day.

- Google Cloud Speech-to-Text API: This is a speech recognition service provided by Google Cloud, which supports several languages including Chinese. It has high accuracy and can transcribe long-form audio. However, it is a paid service and can be expensive for large amounts of transcription.

To use these services, you will need to register for an account, obtain API keys, and then integrate them into your transcription workflow. Once you have integrated the service of your choice, you can transcribe your Chinese audio to text by uploading or streaming the audio to the API, which will return the text transcription. Here, we will use the Google Cloud Speech-to-Text API to transcribe our podcast audio to text.

### Setting up Google Cloud Speech-to-Text API
To use the Google Cloud Speech-to-Text API, you will need to set up a Google Cloud account and create a project. Then, you will need to enable the Speech-to-Text API and create a service account. Finally, you will need to download the service account key file and set the environment variable `GOOGLE_APPLICATION_CREDENTIALS` to the path of the key file. For more detailed instructions, please refer to the [Google Cloud Speech-to-Text API documentation](https://cloud.google.com/speech-to-text/docs/quickstart-client-libraries):
1. Login in to your Google Cloud account and create a project, then click `Dashboard` to go to the project dashboard:

   <div class="l-body">
   <figure>
      <img src="/assets/img/2023-04/gcc_step1.png" alt="This is a caption for the image.">
   </figure>
   </div>

2. In the left navigation panel, click on "APIs & Services" > "Dashboard". Click on + `ENABLE APIS AND SERVICES` at the top of the page.

3. Search `Cloud Speech-to-Text API & Cloud Storage API` and click on it. Click on `ENABLE` to enable the API.

   <div class="l-body">
   <figure>
      <img src="/assets/img/2023-04/gcc_step2.png" alt="This is a caption for the image.">
      </figure>
   </div>

4. In the left navigation panel, click `Credentials`, and create a select `Service account`, you can choose download your credentials by `json` and store it in your local folder.

   <div class="l-body">
   <figure>
      <img src="/assets/img/2023-04/gcc_step3.png" alt="This is a caption for the image.">
      </figure>
   </div>




### Test the Google Cloud Speech-to-Text API
Before transcribing entire episodes, we can test the Google Cloud Speech-to-Text API on a short audio clip. To do this, we can use the `30sec_example.mp3` audio file to get start.
`speech.RecognitionAudio` and `speech.RecognitionConfig` are required to use the Google Cloud Speech-to-Text API. `speech.RecognitionAudio` is used to specify the audio file to be transcribed, and `speech.RecognitionConfig` is used to specify the language code and audio encoding of the audio file. The language code for Chinese is `zh-CN`, and the audio encoding for MP3 is `MP3`. 
{% details  Test example code %}
<d-code language="python">
pip install --upgrade google-cloud-speech
pip install --upgrade google-cloud-storage

# Import required libraries
import os
from google.cloud import speech_v1p1beta1 as speech
import soundfile as sf

# Set environment variable for Google Cloud authentication
os.environ['GOOGLE_APPLICATION_CREDENTIALS'] = 'path/to/your/credentials.json'

# Initialize the Google Cloud Speech-to-Text client
speech_client = speech.SpeechClient()

# Define the paths for the MP3 and FLAC audio files
media_file_name_mp3 = '/Users/wuyoscar/Downloads/exmaple.mp3'
media_file_name_flac = '/Users/wuyoscar/Downloads/exmaple.flac'

# Read audio data and sample rate from the FLAC file using the soundfile library
audio_flac, sample_rate_flac = sf.read(media_file_name_flac)

# Get the number of audio channels from the FLAC file using the soundfile library
flac_info = sf.info(media_file_name_flac)
num_channels_flac = flac_info.channels

# Read the contents of the FLAC file into a RecognitionAudio object
with open(media_file_name_flac, 'rb') as f:
    audio_flac = speech.RecognitionAudio(content=f.read())

# Read the contents of the MP3 file into a RecognitionAudio object
with open(media_file_name_mp3, 'rb') as f:
    audio_mp3 = speech.RecognitionAudio(content=f.read())

# Set up the recognition config object for the MP3 file with the required parameters
config_mp3 = speech.RecognitionConfig(
    sample_rate_hertz=44100,
    language_code="zh-CN"
)

# Set up the recognition config object for the FLAC file with the correct number of channels and other parameters
config_flac = speech.RecognitionConfig(
    encoding=speech.RecognitionConfig.AudioEncoding.FLAC,
    sample_rate_hertz=sample_rate_flac,
    audio_channel_count=num_channels_flac,
    language_code="cmn-Hans-CN"
)

speech_client.recognize(
    config=config_mp3, 
    audio=audio_mp3
)

</d-code>
{% enddetails %}



```python
results {
  alternatives {
    transcript: "Hello大家好我叫Oscar,我来自中国重庆很高兴认识大家,在这里希望跟大家开始一段学习旅程"
    confidence: 0.950523674
  }
  result_end_time {
    seconds: 13
    nanos: 920000000
  }
  language_code: "cmn-hans-cn"
}
total_billed_time {
  seconds: 14
}
request_id: 5898031208######
```




### Transcribe the entire episode

Now that we have tested the Google Cloud Speech-to-Text API on a short audio clip, we can transcribe the entire episode. To do this, we can use the `transcribe_audio` function to transcribe the audio file and save the transcription to a text file. The `transcribe_audio` function is as following:

Table below is Reference for selected [parameters](https://cloud.google.com/speech-to-text/docs/reference/rpc/google.cloud.speech.v1p1beta1#recognitionconfig) :

| Name       | Desc           | Variable  |
| --------------- |:---------------|:-------------:|
| `load_custom_phrases`     |  You can create phrases and store list in yaml file | Function |
| `create_adaptation_resources` (**skip**)     |By utilizing the model adaptation function, you can specify the words and/or phrases that Speech-to-Text must recognize more frequently in the audio data, without suggesting other potentially suitable alternative expressions.     |   Function |
| `upload_blob` | If audio file is more than 1 mins, you need upload to file into *Cloud Storage*    |  Function  |
|`language_code`  | [speech-to-text-supported-languages](https://cloud.google.com/speech-to-text/docs/speech-to-text-supported-languages) | Fcuntion Parameter|
|`bucket_name`  | Name of the Google Cloud Storage bucket | Google Cloud Variable|
|`file_path`  |Path to the audio file to be transcribed | Audio File|
|`credentials_path`| path to service account credential | Google Cloud Variable|

{% details Wrap everything together and custmoize configuration based on content%}  
<d-code language='python'>
from google.cloud import speech_v1p1beta1 as speech
from google.cloud import storage
import os
import yaml

credentials_path = "path/to/your/credentials.json"
os.environ['GOOGLE_APPLICATION_CREDENTIALS'] = credentials_path

load_custom_phrases_path = 'path/to/your/custom_phrases.yaml'
project_id = "your-project-id"
location = "global"
custom_class_id = "mycustomclass01"
phrase_set_id = "myphraseset01"


def load_custom_phrases(file_path=load_custom_phrases_path):
    with open(file_path, 'r', encoding='utf-8') as file:
        return yaml.safe_load(file)



def upload_blob(bucket_name, source_file_name, destination_blob_name):
    """Uploads a file to the bucket."""
    storage_client = storage.Client.from_service_account_json(credentials_path)
    bucket = storage_client.get_bucket(bucket_name)
    blob = bucket.blob(destination_blob_name)

    blob.upload_from_filename(source_file_name)
    print(f"File {source_file_name} uploaded to {destination_blob_name}.")


def transcribe_audio_file(bucket_name, file_path, phrases):
    destination_blob_name = os.path.basename(file_path)
    storage_client = storage.Client.from_service_account_json(credentials_path)
    bucket = storage_client.get_bucket(bucket_name)
    blob = bucket.blob(destination_blob_name)

    if not blob.exists():
        upload_blob(bucket_name, file_path, destination_blob_name)
    else:
        print(f"File {destination_blob_name} already exists in the bucket.")

    client = speech.SpeechClient.from_service_account_json(credentials_path)

    config = speech.RecognitionConfig(
        encoding=speech.RecognitionConfig.AudioEncoding.MP3,
        sample_rate_hertz=44100,
        audio_channel_count=3,
        language_code="zh-CN",
        enable_automatic_punctuation=True,
        enable_separate_recognition_per_channel=True,
        use_enhanced=True,
        enable_speaker_diarization=True,
        diarization_speaker_count=3,
        speech_contexts=[{"phrases": phrases}],
    )

    storage_uri = f"gs://{bucket_name}/{destination_blob_name}"
    audio = speech.RecognitionAudio(uri=storage_uri)

    operation = client.long_running_recognize(config=config, audio=audio)
    print("Waiting for operation to complete...")

    timeout_seconds = 7200
    response = operation.result(timeout=timeout_seconds)
    return response

def save_response_to_file(response, output_file):
    with open(output_file, 'w', encoding='utf-8') as f:
        for index, result in enumerate(response.results):
            # Choose the first alternative (most likely) from the list of alternatives
            alternative = result.alternatives[0]
            transcript = alternative.transcript
             #skip empty string and only save the frist transcript
            if transcript.strip() and index % 2 != 0 :
                print(transcript.strip())
                f.write(transcript + '\n')
    print(f"Transcript saved to {output_file}")

</d-code>
{% enddetails %}

## Data Preprocessing 
### Improving Punctuation in Transcribed Text
Before diving into the preprocessing of the text data, it's important to address an issue with the speech-to-text output. The transcription process might not have recognized all punctuation correctly, leading to a suboptimal final text file. To improve the quality of the text data, we will perform an additional preprocessing step to correct the punctuation
<div class="fake-img l-gutter">
  <p>to be continue</p>
</div>

### Preprocessing the Text Data
After successfully transcribing the audio content of the 随机波动 (Stochastic Volatility) podcasts, the next step in our NLP and machine learning analysis is to preprocess the text data. This step is crucial as it helps to clean and format the raw text, making it easier for our machine learning models to understand and process the content effectively.

In this section, we will cover the following preprocessing techniques:
1. Tokenization
2. Removing stop words
3. Removing special characters and numbers
4. Lowercasing
5. Lemmatization

### Tokenization
Tokenization is the process of breaking down the text into individual words or tokens. For Chinese text, we will use the Jieba library for tokenization.
```python
import jieba
def tokenize_chinese_text(text):
    tokens = jieba.lcut(text)
    return tokens

```

### Removing stop words
Stop words are common words that do not carry much meaning and can be removed from the text to reduce noise. You can either create a custom list of Chinese stop words or find an existing list online. Load the stop words into a Python set for efficient filtering.

```python
def load_stop_words(file_path):
    with open(file_path, 'r', encoding='utf-8') as file:
        stop_words = set(line.strip() for line in file)
    return stop_words

stop_words_file_path = 'path/to/chinese_stop_words.txt'
stop_words = load_stop_words(stop_words_file_path)

def remove_stop_words(tokens, stop_words):
    cleaned_tokens = [word for word in tokens if word not in stop_words]
    return cleaned_tokens

```

### Removing special characters and numbers
We will remove any special characters, numbers, and whitespace from the tokens to further clean the text.
```python
def remove_special_characters_and_numbers(tokens):
    cleaned_tokens = [word for word in tokens if word.strip() and not word.isdigit()]
    return cleaned_tokens
```

### Preprocessing Pipeline
Now, we will create a preprocessing pipeline that combines all the above techniques. The pipeline takes raw Chinese text as input and returns cleaned tokens.
```python
def preprocess_chinese_text(text, stop_words):
    # Tokenize
    tokens = tokenize_chinese_text(text)

    # Remove stop words
    tokens = remove_stop_words(tokens, stop_words)

    # Remove special characters and numbers
    cleaned_tokens = remove_special_characters_and_numbers(tokens)

    return cleaned_tokens

file_path = "path/to/your/txt_file.txt"
text = read_text_file(file_path)
preprocessed_tokens = preprocess_chinese_text(text, stop_words)
```
