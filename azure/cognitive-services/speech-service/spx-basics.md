https://learn.microsoft.com/en-us/azure/cognitive-services/speech-service/spx-basics#speech-to-text-speech-recognition

# Linux
## Installation
```
dotnet tool install --global Microsoft.CognitiveServices.Speech.CLI
sudo apt update
sudo apt install ffmpeg
```
## Format
```

spx config @key --set <>
spx config @region --set japaneast

ffmpeg -i originalfile.otherformat -acodec pcm_s16le -ac 1 -ar 16000 original.wav
ffmpeg -ss 132 -t 139 -i original.wav short.wav
```
## Recognize
```
spx recognize --language ja-jp --file short.wav
```