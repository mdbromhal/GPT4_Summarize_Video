# GPT4_Summarize_Video
ABSTRACT: Summarizing Videos with OpenAI's GPT4

Source: https://www.oreilly.com/library/view/developing-apps-with/9781098152475/

1. Use OpenCV to capture the video (either from a link or a mp4 file)
```
video = cv2.VideoCapture("PATH_TO_VIDEO") # Add the path to your mp4 file here :)
```
2. Extract the frames from the video as base64 strings
```
# Extract the frames from the video
base64Frames = []
i=0

while video.isOpened():
    success, frame = video.read()
    i+=1
    if not success or i>5:
        break
    _, buffer = cv2.imencode(".jpg", frame)
    base64Frames.append(base64.b64encode(buffer).decode("utf-8"))
```
3. Create an object comprising of the first 10 frames to send to OpenAI
```
images = [{"image": frame, "resize":768} for frame in base64Frames[2::3]]
```
4. Send the frames to GPT-4 vision model and ask it to summarize the video
```
response = client.chat.completions.create(
    model= "gpt-4-vision-preview",
    max_tokens= 200,
    messages=[{"role": "user", "content": ["These are the frames from a video. Generate a two sentence summary.", *images]}
    ])
```
5. Retreive the response
```
print(response.choices[0].message.content)
```

Note: no example run here, since when I ran the code, I remembered I don't have GPT-4...
