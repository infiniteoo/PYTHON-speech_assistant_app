# PYTHON - Speech Assistant Application

### About

In this project we build a speech assistant app using the speech recognition library and Google's text-to-speech API. Inspired by Amazon Alexa, we will be able to create a voice-activated application that will respond to the user's speech. The application will be able to respond to the user's speech and will be able to perform the following tasks:

    * Search the web
    * Get the time
    * Show a location on a map
    * Tell us your name

### Code

```
import speech_recognition as sr
from playsound import playsound
import webbrowser
import time
import os
import random
from gtts import gTTS
from time import ctime


r = sr.Recognizer()

def record_audio(ask = False):
    with sr.Microphone() as source:
        if ask:
            alexis_speak(ask)
        audio = r.listen(source)
        voice_data = ''
        try:
            voice_data = r.recognize_google(audio)
        except sr.UnknownValueError:
            alexis_speak('Sorry, I did not get that')
        except sr.RequestError:
            alexis_speak('Sorry, my speech service is down')
        return voice_data

def alexis_speak(audio_string):
    tts = gTTS(text=audio_string, lang="en")
    r = random.randint(1,10000000)
    audio_file = 'audio-' + str(r) + '.mp3'
    tts.save(audio_file)
    playsound(audio_file)
    print(audio_string)
    os.remove(audio_file)

def respond(voice_data):
    if 'what is your name' in voice_data:
        alexis_speak('my name is alexis')
    if 'what time is it' in voice_data:
        alexis_speak(ctime())
    if 'search' in voice_data:
        search = record_audio('What do you want to search for?')
        url = 'https://google.com/search?q=' + search
        webbrowser.get().open(url)
        alexis_speak('Here is what I found for ' + search)
    if 'find location' in voice_data:
        location = record_audio('What is the location?')
        url = 'https://google.nl/maps/place/' + location + '/&amp;'
        webbrowser.get().open(url)
        alexis_speak('Here is the location of  ' + location)
    if 'exit' in voice_data:
        exit()

time.sleep(1)
print('How can I help you?')
while 1:
  voice_data = record_audio()
  respond(voice_data)
```
