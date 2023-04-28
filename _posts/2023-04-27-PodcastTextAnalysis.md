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
      name: 
  

toc:
  - name: Introduction
  - name: Collecting Podcast Data
  - name: Transcribing Podcast Audio to Text
  - name: Preprocessing the Text Data
  - name: Analyzing the Text Data
  - name: Visualizing the Results
  - name: Conclusion


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
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/2023-04/随机波动_96.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/2023-04/随机波动_94.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/2023-04/随机波动_85.png" class="img-fluid rounded z-depth-1" zoomable=true %}
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


Before transcribing entire episodes, we can test the Google Cloud Speech-to-Text API on a short audio clip. To do this, we can use the `30sec_example.mp3` audio file to get start.
`speech.RecognitionAudio` and `speech.RecognitionConfig` are required to use the Google Cloud Speech-to-Text API. `speech.RecognitionAudio` is used to specify the audio file to be transcribed, and `speech.RecognitionConfig` is used to specify the language code and audio encoding of the audio file. The language code for Chinese is `zh-CN`, and the audio encoding for MP3 is `MP3`. For more information on the language codes and audio encodings supported by the Google Cloud Speech-to-Text API, please refer to the [Google Cloud Speech-to-Text API documentation](https://cloud.google.com/speech-to-text/docs/languages).


{% highlight json %}
//https://cloud.google.com/speech-to-text/docs/reference/rest/v1/RecognitionAudio
"RecognitionAudio" :{

  // Union field audio_source can be only one of the following:
  "content": string,
  "uri": string
  // End of list of possible types for union field audio_source.
}

//https://cloud.google.com/speech-to-text/docs/reference/rest/v1/RecognitionConfig
"RecognitionConfig":{
  "encoding": enum (AudioEncoding),
  "sampleRateHertz": integer,
  "audioChannelCount": integer,
  "enableSeparateRecognitionPerChannel": boolean,
  "languageCode": string,
  "alternativeLanguageCodes": [
    string
  ],
  "maxAlternatives": integer,
  "profanityFilter": boolean,
  "adaptation": {
    object (SpeechAdaptation)
  },
  "speechContexts": [
    {
      object (SpeechContext)
    }
  ],
  "enableWordTimeOffsets": boolean,
  "enableWordConfidence": boolean,
  "enableAutomaticPunctuation": boolean,
  "enableSpokenPunctuation": boolean,
  "enableSpokenEmojis": boolean,
  "diarizationConfig": {
    object (SpeakerDiarizationConfig)
  },
  "metadata": {
    object (RecognitionMetadata)
  },
  "model": string,
  "useEnhanced": boolean
}
{% endhighlight%}

{% highlight python %}
pip install --upgrade google-cloud-speech
pip install --upgrade google-cloud-storage

import os
from google.cloud import speech_v1p1beta1 as speech 
os.environ['GOOGLE_APPLICATION_CREDENTIALS'] = 'path/to/your/credentials.json'
#initialize speech client
speech_client = speech.SpeechClient()

#example audio file in different format
media_file_name_mp3 = '/Users/wuyoscar/Downloads/exmaple.mp3'
media_file_name_flac = '/Users/wuyoscar/Downloads/exmaple.flac' 
{% endhighlight%}






