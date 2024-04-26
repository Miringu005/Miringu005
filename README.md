#import io
from google.cloud import speech_v1p1beta1 as speech

def transcribe_audio(audio_file, language_code):
    client = speech.SpeechClient()

    with io.open(audio_file, "rb") as f:
        content = f.read()

    audio = speech.RecognitionAudio(content=content)
    config = speech.RecognitionConfig(
        encoding=speech.RecognitionConfig.AudioEncoding.LINEAR16,
        sample_rate_hertz=16000,
        language_code=language_code,
    )

    response = client.recognize(config=config, audio=audio)

    transcript = ""
    for result in response.results:
        transcript += result.alternatives[0].transcript.strip() + " "

    return transcript

# Example usage:
english_transcript = transcribe_audio("english_audio.wav", "en-US")
print("English Transcript:", english_transcript)

swahili_transcript = transcribe_audio("swahili_audio.wav", "sw-TZ")
print("Swahili Transcript:", swahili_transcript)


-->
