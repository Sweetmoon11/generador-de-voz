import streamlit as st
import os 
from dotenv import load_dotenv, find_dotenv
#variables de entorno
from openai import OpenAI 
#Modelo de lenguaje natural

#cargar archivo env
load_dotenv(find_dotenv(), override=True)

apiKey = os.environ.get("OPENAI_API_KEY")
#se crea un cliente de openai
client = OpenAI(api_key=apiKey)

#opciones de voz 
voices = ["alloy", "antoni", "bella", "camila", "dario", "eric", "gabriela", "jose", "lucia", "maria", "nicolas", "sara"]

#vamos a hacer un titulo para nuestra aplicacion 

st.title("Generador de voz con IA")

#ahora generamos un cuadro de texto para que el usuario pueda ingresar el texto que desea convertir a voz
text = st.text_area("Ingrese el texto que desea convertir a voz", height=200)

#selector de voz 

voice = st.selectbox("Seleccione la voz", voices)

#boton para generar la voz
if st.button("Generar voz"):
    if text:
        #generamos la voz
        response = client.audio.speech.create(
            model="tts-1",
            voice=voice,
            input=text
        )
        audio_path = "audio.mp3"
        with open(audio_path, "wb") as output_file:
            for chunk in response.iter_bytes():
                output_file.write(chunk)
        st.success("Voz generada con exito en {audio_path}")

        audio_file = open(audio_path, "rb")
        audio_bytes = audio_file.read()
        st.audio(audio_bytes, format="audio/mp3")
    else:
        st.error("por favor ingrese un texto") 
- Crea un entorno virtual (opcional pero recomendado):
        python -m venv venv
        source venv/bin/activate  # En Windows: venv\Scripts\activate
  
 - Instala las dependencias:
pip install -r requirements.txt

- Configura tu archivo .env con tu clave de OpenAI
OPENAI_API_KEY=sk-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx


🧪 Ejecución
Lanza la app con Streamlit:

streamlit run app.py

🎧 Voces disponible
alloy, antoni, bella, camila, dario, eric, gabriela, jose, lucia, maria, nicolas, sara
