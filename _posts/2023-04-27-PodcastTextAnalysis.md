---
layout: distill
title: Podcasts Analysis Using NLP and Machine Learning
description: Uncover the power of natural language processing (NLP) and machine learning techniques in analyzing 随机波动 Stochastic Volatility podcasts. Delve into the complete workflow, from data collection and audio transcription to text preprocessing, analysis, and visualization, to gain valuable insights on podcast topics, emotions, and overall content.
giscus_comments: true
date: 2023-04-27
tags: Speech-to-text, Chinese-NLP, ChatGPT4, NLP, Machine Learning, ChatGPT
categories: NLP, Tools, API
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
  - name: Validating the Transcription Results
  - name: Preprocessing the Chinese Text Data
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
    font-size: 10px;
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


<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        <a href="https://www.stovol.club/096" target="_blank">
                {% include figure.html path="/assets/img/2023-04/随机波动_96.png" title="随机波动_96" class="img-fluid rounded z-depth-1" %}
        </a>
    </div>
    <div class="col-sm mt-3 mt-md-0">
        <a href="https://www.stovol.club/094" target="_blank">
                {% include figure.html path="/assets/img/2023-04/随机波动_94.png" title="随机波动_94" class="img-fluid rounded z-depth-1" %}
        </a>
    </div>
    <div class="col-sm mt-3 mt-md-0">
        <a href="https://www.stovol.club/085" target="_blank">
                {% include figure.html path="/assets/img/2023-04/随机波动_85.png" title="随机波动_85" class="img-fluid rounded z-depth-1" %}
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
    english_script(self-added): "Hello everyone, my name is Oscar, and I come from Chongqing, China. Nice to meet you, and I hope to start a learning journey with you here""
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
``` python
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

```
{% enddetails %}

## Validating the Transcription Results via ChatGPT-4 Modwel
After successfully transcribing the audio content of the "Stochastic Volatility" podcast into text using a speech-to-text API, we need to preprocess the text data. This step is crucial because it helps clean up and format the raw text, allowing our machine learning model to understand and process the content more effectively.

However, upon examining the transcribed text, I noticed two major inaccuracies:

1. In this text snippet, the names "服饰演", "张知其", and "冷见过" were incorrectly recognized. The correct names should be "傅适野", "张之琪", and "冷建国".
```text
各位听众朋友大家好，欢迎收听星期的随机波动，我是服饰演我是张知其我是冷见过你怎么还吃
```

2. The Chinese text segmentation is not very accurate, leading to missing periods and commas, which makes the sentence meaning unclear.
```text
...精神上已经就是不是特别稳定了，就是中中间经历了崩。会大哭这样的一个过程
```

3. The text has issues with Chinese punctuation, such as missing periods and commas, which affect the clarity and readability of the content.

To address these issues, we can use ChatGPT, a deep learning model, to correct the recognition errors, add missing punctuation, and improve the overall quality of the text. Although many pre-trained models can perform these tasks, they are mostly designed for English text. In this case, we'll use ChatGPT, which can also handle Chinese text

### Constructing useful Prompt 
In your correct_text function, you are using ChatGPT to correct grammar errors, add punctuation, and fix recognition errors in the given Chinese text. The function takes the transcribed text as input, sends it to the ChatGPT API along with a prompt that explains the task, and then returns the corrected and punctuated text.

#### System message:
```json
{"role": "system", "content": "You are a helpful assistant that corrects grammar errors, adds punctuation, and fixes recognition errors in Chinese text."}
```
This message sets the context for the ChatGPT model, informing it that its role is to be a helpful assistant, responsible for correcting grammar errors, adding punctuation, and fixing recognition errors in the Chinese text.
#### User message (background information):
```json
{"role": "user", "content": "This text is from a cross-cultural podcast initiated by three female media professionals."}

```
This message provides the model with background information about the input text. By mentioning that the text comes from a cross-cultural podcast initiated by three female media professionals, you give the model additional context that might help improve the quality of the generated corrections and adjustments.

#### User message (task instructions):
```json
{"role": "user", "content": f"Please correct any grammar errors, add punctuation, and fix the recognition errors in the following Chinese text:\n{text}\n\nFor example, the host name is '傅适野，张之琪，冷建国', and there might be missing punctuation like commas and periods. Make the text clear and easy to understand."}
```

This message gives specific instructions for the task. It asks the model to correct grammar errors, add punctuation, and fix recognition errors in the input text. It also provides an example of correctly recognized host names and mentions possible issues with missing punctuation like commas and periods. This helps the model understand the desired output and ensures that the text is clear and easy to understand after the corrections are made.
  
#### Corrected Text
If we check the result from ChatGPT4, we can see the sentence:
```json
{
    "before_corrected": {
        "chinese": "我们做这一期节目前就是我们三个因为最近各种新闻确实就是精神状况都觉得有点看特别需问好了",
        "english": "We did this episode just the three of us because recently all kinds of news really made our mental states feel a bit particularly in need of asking if we are okay."
    }
}

```


that has been successfully corrected to:

```json
{
    "after_corrected": {
        "chinese": "我们做这一期节目前，就是我们三个，傅适野、张之琪、冷建国，因为最近各种新闻确实就是精神状况都觉得有点看特别需问好了。",
        "english": "Before we did this episode, it was just the three of us, Fu Shiye, Zhang Zhiqi, and Leng Jianguo, because recently all kinds of news really made our mental states feel a bit particularly in need of asking if we are okay."
    }
}
```

{% details The complete code for ChatGPT  %}  

```python
import json
import string
from pathlib import Path
import openai
import jieba
from dotenv import load_dotenv
import os
import tiktoken
import logging
import time
from tqdm import tqdm
from tenacity import (
    retry,
    stop_after_attempt,
    wait_random_exponential,
)


# Set up logging
logging.basicConfig(level=logging.DEBUG, format='%(asctime)s [%(levelname)s] - %(message)s')
logger = logging.getLogger(__name__)


# Define paths variables
current_path = Path.cwd()
main_path = current_path.parent
data_path = main_path / "data"
env_path = main_path / ".env"

# Load environment variables from .env file
load_dotenv(env_path)

# Get OpenAI API key from environment variables
openai_api_key = os.getenv("OPENAI_API_KEY")

# Use OpenAI API key to authenticate with OpenAI
openai.api_key = openai_api_key

# Define punctuation sets
chinese_punctuation = set("。，！？【】（）<>《》“”‘’；：——…～·")
english_punctuation = set(string.punctuation)

def read_file_contents(file_path):
    """Read a text or JSON file and return its contents as a string or dictionary."""
    file_path = Path(file_path)
    logger.debug(f"Reading file {file_path.name}")
    with open(file_path, "r", encoding="utf-8") as f:
        if file_path.suffix == ".txt":
            contents = f.read().replace("\n", "").replace(" ", "") # read the contents of a text file and remove newlines and spaces
        elif file_path.suffix == ".json":
            contents = json.load(f) # read the contents of a JSON file
        else:
            raise ValueError("File type not supported. Only .txt and .json files are supported.")
    return contents

def save_result(result, file_path, line_length: int=50):
    """Save the result as a plain text file with no more than 20 characters per line."""
    with open(file_path, "w", encoding="utf-8") as f:
        for i in range(0, len(result), line_length):
            line = result[i:i+line_length]
            f.write(line + "\n")




def num_tokens_from_string(string: str, encoding_name: str = 'cl100k_base') -> int:
    """Returns the number of tokens in a text string."""
    encoding = tiktoken.get_encoding(encoding_name)
    num_tokens = len(encoding.encode(string))
    return num_tokens



@retry(wait=wait_random_exponential(min=1, max=60), stop=stop_after_attempt(6))
def completion_with_backoff(**kwargs):
    return openai.ChatCompletion.create(**kwargs)



def correct_text_file(file_path, buffer_tokens=300,  save_corrected_file:bool=True , model_token_limit:int=8192) -> str:
    text_file = Path(file_path)

    logger.debug("Wait for 2 seconds before the next request to OpenAI API")
    time.sleep(2)

    text = read_file_contents(text_file)
    logger.debug(f"Successfully reading text from {text_file}")

    messages = [
        {"role": "system", "content": "You are a helpful assistant that corrects grammar, punctuation, and recognition errors in Chinese text."},
        {"role": "user", "content": "This text is from a podcast by '傅适野，张之琪，冷建国'. There might be missing punctuation, wrong words, and typos."},
        {"role": "user", "content": f"Please correct any errors in the following Chinese text based on context and only return corrected text:\n{text}"},
    ]
    
    ## Calculate the number of tokens based on the messages list

    num_tokens = num_tokens_from_string(' '.join([message['content'] for message in messages]))
    generated_text_token = num_tokens + buffer_tokens
    logger.debug(f"Input text tokens: {num_tokens}")
    logger.debug(f"Max generated tokens set as {generated_text_token}")
    #check token limit
    if (generated_text_token + num_tokens) > model_token_limit:
        logger.debug(f"Token limit exceeded. Please reduce the number of tokens to below {model_token_limit}.")
        return None

    response = completion_with_backoff(
        model="gpt-4-0314",
        messages=messages,
        max_tokens=generated_text_token
    )
    generated_text = response.choices[0]["message"]['content']
    generated_text = generated_text.strip()

    
    if save_corrected_file:
        result_file_path = text_file.with_name(f"{text_file.stem}_restoration.txt")
        save_result(generated_text, result_file_path)
        logger.debug(f"Successfully writing result to {result_file_path.name}")

    logger.debug(f"Successfully processing text from {text_file.name}")
    return generated_text
 
    
def text_restoration(filename ,chunk_size: int = 600):
    """Tokenize Chinese text using jieba, remove punctuation, and write each chunk to a separate file."""
    filename = Path(filename)
    content = read_file_contents(filename)
    #remove all punctuation    
    tokens = [token.strip() for token in jieba.cut(content, cut_all=False) if token not in chinese_punctuation and token not in english_punctuation]
    restoration_text = "".join(tokens)
    tokens_res = jieba.lcut(restoration_text)
    
    #split into chunks
    chunks = [tokens_res[i:i+chunk_size] for i in range(0, len(tokens_res), chunk_size)]
    logger.debug(f"Splitting {filename.name} into {len(chunks)} chunks")
    
    # Create the output directory if it doesn't exist
    output_dir = data_path / filename.stem
    Path(output_dir).mkdir(exist_ok=True)
    file_stem = filename.stem
    
    # Write each chunk to a separate file
    corrected_texts = []
    for i, chunk in enumerate(chunks):
        chunk_text = "".join(chunk)
        chunk_filename = output_dir/f"{file_stem}_chunk_{i+1}.txt"
        
        logger.debug(f"Writing chunk {i+1} to {chunk_filename}")
        
        save_result(chunk_text, chunk_filename)
        
        generated_text = correct_text_file(chunk_filename, save_corrected_file=True)
        corrected_texts.append(generated_text)
    
    combined_corrected_text = "\n".join(corrected_texts)
    combined_corrected_text = combined_corrected_text.replace("\n", "")  # remove all newlines
    # remove all unnecessary spaces
    combined_corrected_text = ' '.join(combined_corrected_text.split())

    combined_file_path = f"output_dir / {file_stem}_restoration.txt"
    save_result(combined_corrected_text, combined_file_path)
    


if __name__ == "__main__":

    filename = ""
    text_restoration(filename, chunk_size=700)
```
{% enddetails %}



## Preprocessing the Chinese Text Data
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
Stop words are common words that do not carry much meaning and can be removed from the text to reduce noise. You can either create a custom list of Chinese stop words or find an existing list online. Load the stop words into a Python set for efficient filtering. Here we will use the stop words list from [hit_stopwords](https://raw.githubusercontent.com/goto456/stopwords/master/hit_stopwords.txt)

```python
import requests

url = 'https://raw.githubusercontent.com/goto456/stopwords/master/hit_stopwords.txt'
response = requests.get(url)
stopwords = response.text.split('\n')

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
import requests

url = 'https://raw.githubusercontent.com/goto456/stopwords/master/hit_stopwords.txt'
response = requests.get(url)
stop_words = response.text.split('\n')

import jieba
def tokenize_chinese_text(text):
    tokens = jieba.lcut(text)
    return tokens
def remove_stop_words(tokens, stop_words):
    cleaned_tokens = [word for word in tokens if word not in stop_words]
    return cleaned_tokens

def remove_special_characters_and_numbers(tokens):
    cleaned_tokens = [word for word in tokens if word.strip() and not word.isdigit()]
    return cleaned_tokens

def preprocess_chinese_text(text, stop_words):
    # Tokenize
    tokens = tokenize_chinese_text(text)

    # Remove stop words
    tokens = remove_stop_words(tokens, stop_words)

    # Remove special characters and numbers
    cleaned_tokens = remove_special_characters_and_numbers(tokens)

    return cleaned_tokens


with open(filename, 'r', encoding='utf-8') as f:
   content = f.read()
   process_content = preprocess_chinese_text(content, stop_words = stop_words)
```


### Analyzing the Text Data

### Visualizing the Results
>add figure here

### Conclusion
>write conclusion here