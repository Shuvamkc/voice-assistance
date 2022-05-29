
#code

from email.mime import audio
from hashlib import new
from logging import exception
from re import S
from winreg import QueryInfoKey, QueryValue
from numpy import True_
import pyttsx3 #text to voice module/package
import speech_recognition as sr # speech recognizing module/package
import datetime #date and time module/package
import wikipedia #wikipedia module/package
import webbrowser #webbrowser module/package
import os   #operating system module/package
import smtplib

print("Hello alpha")

engine=pyttsx3.init('sapi5')    #making object  engine from pyttsx3 module
voices=engine.getProperty('voices')   # calling getproperty function 
engine.setProperty('voice',voices[0].id)  #calling setpropert function to set the voice from pyttsx3 module

def speak(text):    #making a speak function to pronounce for alpha
    engine.say(text)   #calling say function from engine object
    engine.runAndWait()  #saying to wait after pronounciation

speak("hello i am alpha...")    #calling speak function
speak("how can i help you...")
#speak(datetime.datetime.now())

def command():    #this function takes command from microphone
  
    r=sr.Recognizer()      #creating an object r from recognizer class of speech recognizer module
    with sr.Microphone() as source:       #sr.microphone is  a context manager
        print('listning')
        audio=r.listen(source)   #listen is a function which accepts voice from microphone

    try :              
        print("recognizing ")
        Query=r.recognize_google(audio, language='en-in')   #we are using google to recognise the input
        print(f"user said : {Query}\n")

    except Exception as e:
        speak("say that again please")
        Query=None
        return Query
            
    if 'wikipedia' in Query.lower():        #we are matching our input with wikipedia over a query
         speak('searching wikipedia')
         Query=Query.replace("wikipedia","")
         result=wikipedia.summary(Query,sentences=2)
         speak(result)

    elif 'open youtube' in Query.lower():
        webbrowser.open("youtube.com")
                    
    elif 'open google' in Query.lower():
        webbrowser.open("google.com")

    elif 'time right now' in Query.lower():
        speak(datetime.datetime.now())

    elif 'music' in Query.lower():
        songs_dir="C:\\Users\\user\\Desktop\\music"         #path of songs folder
        songs=os.listdir(songs_dir)                         #making object of os.listdir class
        os.startfile(os.path.join(songs_dir,songs[0]))       #satrting a file in the given directory

    elif 'thankyou alpha' in Query.lower():
        speak("thankyou and have a nice day")

    else :
        speak(" extremely sorry i couldnt understand")
        
n=5
while n>0:
    command()
    n=n-1
