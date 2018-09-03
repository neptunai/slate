---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell


toc_footers:
  - <a href='https://neptunai.com/studio/register'>Sign Up for an API Key</a>
  - <a href='https://neptunai.com/contact'>Have a question? Contact us</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the NeptunAI API documentation! You can use NeptunAI API to access any service in our portfolio.

We prepared this documentation to serve as a reference as you start to integrate the NeptunAI API into your code.

You need a valid API KEY to use our services. If this is your first time trying NeptunAI API, please refer to [NeptunAI Signup](https://github.com/lord/slate) to get your API KEY. 

# Authentication

> To authorize, use this code:

```shell
# With shell, you can just pass the correct header with each request
curl -X POST -H 'X-API-Key: YOUR_API_KEY'
  "api_end_point_here"
```

> Make sure to replace `YOUR_API_KEY` with your API key.

API requests are authenticated by a key which can be found and managed from your user profile's My API section. 

NeptunAI expects for the API key to be included in all API requests to the server in a header that looks like the following:

`X-API-Key: YOUR_API_KEY`

<aside class="notice">
You must replace <code>YOUR_API_KEY</code> with your personal API key.
</aside>

# API Endpoints

We provide a variety of Image Colorization, Text Extraction, Image-to-Analyzed Text, Nudity Detection, Emotion Detection and Image Classification features that allow you to manipulate, analyze and understand the images in an automated fashion.

<aside class="info">
<strong>Requests to the API must be formatted in JSON.</strong>
</aside>


## Image Colorization


```shell
curl -X POST -H 'X-API-Key: YOUR_API_KEY' 
-H 'Content-Type: application/json' 
-d '{ "url" : "https://example.com/image.jpg" }' 
https://neptunai.com/api/v1/colorize
```


> The above command returns JSON structured like this:

```json
{
    "result": {
        "colorized": "https://neptunai.com/content/colorized/0902060620-7033-example.jpeg"
    }
}
```

Takes URL of black & white photo and uses deep learning to classify objects/regions within the image and color them accordingly. It returns an url of colorized version of your photo.

Colorized photo is being hosted on our servers for 30 days. For a longer period of hosting, you might consider to download the image to your own servers.

### HTTP Request

`POST https://neptunai.com/api/v1/colorize`

### Query Parameters

Parameter | Required | Description
--------- | ------- | -----------
url | yes | A valid URL of black & white photo. Supported file types: .jpg, .jpeg and .png

<aside class="info">
Response of this call might take up to a minute according to size of the image that sent to our servers.
</aside>

<aside class="warning">
Maximum allowed file size is <strong>10MB</strong>.
</aside>

## Text Recognation



```shell
curl -X POST -H 'X-API-Key: YOUR_API_KEY' 
-H 'Content-Type: application/json' 
-d '{ 
      "url" : "https://example.com/image.jpg", 
      "lang" : "eng" 
    }' 
https://neptunai.com/api/v1/ocr
```


> The above command returns JSON structured like this:

```json
{
    "result": {
        "url": "https://example.com/image.jpg",
        "text": "Extracted text will appear here."
    }
}
```

This endpoint accurately decyphers and extracts text from images with deep learning. It supports over 100 other languages as well. API works best when the image only contains text and is on a single line such as business letters.


### HTTP Request

`POST https://neptunai.com/api/v1/ocr`

### Query Parameters

Parameter | Required | Description
--------- | ------- | -----------
url | yes | A valid URL of an image that contains text.
lang | no | Language code of the text to be extracted from. Default languge is English.

[Here is the full list of supported languages and codes](?#language-codes)


## Image-to-Analyzed Text


```shell
curl -X POST -H 'X-API-Key: YOUR_API_KEY' 
-H 'Content-Type: application/json' 
-d '{ "url" : "https://example.com/image.jpg" }' 
https://neptunai.com/api/v1/analyzed-ocr
```


> The above command returns JSON structured like this:

```json
{
    "result": {
        "url": "https://www.example.com/cover-letter-template.png",
        "text": {
            "full_text": "Full version of the extracted text will appear here",
            "summary": "Summary of text will appear here"
        },
        "sentiment": {
            "result": "positive",
            "confidence": 0.87191122770309
        },
        "tags": "Marketing, Business, Cover Letter"
    }
}
```

This endpoint accurately decyphers and extracts text from images with deep learning. 

In addition, it analyzes the text, classifies, summarizes and runs document-level sentiment analysis. 

<aside class="warning">
This service is <strong>available only in English</strong> at this moment. We are planning to add new languages very soon.
</aside>

### HTTP Request

`POST https://neptunai.com/api/v1/ocr`

### Query Parameters

Parameter | Required | Description
--------- | ------- | -----------
url | yes | A valid URL of an image that contains text.


## Emotion Detection


```shell
curl -X POST -H 'X-API-Key: YOUR_API_KEY' 
-H 'Content-Type: application/json' 
-d '{ 
      "url" : "https://example.com/image.jpg" 
      "limit" : 5
    }' 
https://neptunai.com/api/v1/emotions
```


> The above command returns JSON structured like this:

```json
{
    "result": [
        {
            "faceCoordinates": [
                {
                    "top": 43,
                    "bottom": 266,
                    "left": 68,
                    "right": 291
                }
            ],
            "emotions": [
                {
                    "emotion": "Neutral",
                    "confidence": 0.3957436
                },
                {
                    "emotion": "Happy",
                    "confidence": 0.2977428
                },
                {
                    "emotion": "Fear",
                    "confidence": 0.1349366
                },
                {
                    "emotion": "Disgust",
                    "confidence": 0.1311134
                },
                {
                    "emotion": "Sad",
                    "confidence": 0.0243831
                }
            ]
        }
    ]
}
```

Autodetects emotions and gives you the list of emotions for each person in the given photo with its corresponding confidence rate as well as bounding-box information for each detected person.

<aside class="info">
Response time can vary depending upon <strong>how many people detected in the given photo</strong>, but cost of the call remains the same.
</aside>

### HTTP Request

`POST https://neptunai.com/api/v1/emotions`

### Query Parameters

Parameter | Required | Description
--------- | ------- | -----------
url | yes | A valid URL of an image that contains text.
limit | no |  Number of results. Default is 7 if not specified.


## Image Classification


```shell
curl -X POST -H 'X-API-Key: YOUR_API_KEY' 
-H 'Content-Type: application/json' 
-d '{ "url" : "https://example.com/image.jpg" }' 
https://neptunai.com/api/v1/classify
```


> The above command returns JSON structured like this:

```json
{
    "result": [
        {
            "tag": "baseball",
            "confidence": 0.98731774091721
        },
        {
            "tag": "ballplayer, baseball player",
            "confidence": 0.012508379295468
        },
        {
            "tag": "partridge",
            "confidence": 0.000022209002054296
        },
        {
            "tag": "ruffed grouse, partridge, Bonasa umbellus",
            "confidence": 0.0000041196412894351
        },
        {
            "tag": "scoreboard",
            "confidence": 0.0000029124785214663
        }
    ]
}
```

This endpoint generates relevant tags for the given photo with confidence rate. It uses deep learning to generate most relevant tags.


### HTTP Request

`POST https://neptunai.com/api/v1/classify`

### Query Parameters

Parameter | Required | Description
--------- | ------- | -----------
url | yes | A valid URL of an image that contains text.

## Nudity Detection


```shell
curl -X POST -H 'X-API-Key: YOUR_API_KEY' 
-H 'Content-Type: application/json' 
-d '{ "url" : "https://example.com/nude-image.jpg" }' 
https://neptunai.com/api/v1/nudity
```


> The above command returns JSON structured like this:

```json
{
    "result": {
        "url": "https://example.com/nude-image.jpg",
        "isNude": true,
        "confidence": 0.99999999
    }
}
```

This endpoint determines whether or not an image contains nudity.


### HTTP Request

`POST https://neptunai.com/api/v1/nudity`

### Query Parameters

Parameter | Required | Description
--------- | ------- | -----------
url | yes | A valid URL of an image that contains text.

# Language Codes

The list of languages and language codes that is supported by <strong>Text Recognation API</strong>

Lang Code | Language
--------- | ---------
afr | Afrikaans
amh | Amharic
ara | Arabic
asm | Assamese
aze | Azerbaijani
aze_cyrl  | Azerbaijani - Cyrillic
bel | Belarusian
ben | Bengali
bod | Tibetan
bos | Bosnian
bul | Bulgarian
cat | Catalan; Valencian
ceb | Cebuano
ces | Czech
chi_sim | Chinese - Simplified
chi_tra | Chinese - Traditional
chr | Cherokee
cym | Welsh
dan | Danish
deu | German
dzo | Dzongkha
ell | Greek, Modern (1453-)
eng | English
enm | English, Middle (1100-1500)
epo | Esperanto
est | Estonian
eus | Basque
fas | Persian
fin | Finnish
fra | French
frk | Frankish
frm | French, Middle (ca. 1400-1600)
gle | Irish
glg | Galician
grc | Greek, Ancient (-1453)
guj | Gujarati
hat | Haitian; Haitian Creole
heb | Hebrew
hin | Hindi
hrv | Croatian
hun | Hungarian
iku | Inuktitut
ind | Indonesian
isl | Icelandic
ita | Italian
ita_old | Italian - Old
jav | Javanese
jpn | Japanese
kan | Kannada
kat | Georgian
kat_old | Georgian - Old
kaz | Kazakh
khm | Central Khmer
kir | Kirghiz; Kyrgyz
kor | Korean
kur | Kurdish
lao | Lao
lat | Latin
lav | Latvian
lit | Lithuanian
mal | Malayalam
mar | Marathi
mkd | Macedonian
mlt | Maltese
msa | Malay
mya | Burmese
nep | Nepali
nld | Dutch; Flemish
nor | Norwegian
ori | Oriya
pan | Panjabi; Punjabi
pol | Polish
por | Portuguese
pus | Pushto; Pashto
ron | Romanian; Moldavian; Moldovan
rus | Russian
san | Sanskrit
sin | Sinhala; Sinhalese
slk | Slovak
slv | Slovenian
spa | Spanish; Castilian
spa_old | Spanish; Castilian - Old
sqi | Albanian
srp | Serbian
srp_latn  | Serbian - Latin
swa | Swahili
swe | Swedish
syr | Syriac
tam | Tamil
tel | Telugu
tgk | Tajik
tgl | Tagalog
tha | Thai
tir | Tigrinya
tur | Turkish
uig | Uighur; Uyghur
ukr | Ukrainian
urd | Urdu
uzb | Uzbek
uzb_cyrl  | Uzbek - Cyrillic
vie | Vietnamese
yid | Yiddish