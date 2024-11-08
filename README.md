# [yt-dlp-host](https://github.com/Vasysik/yt-dlp-host) API Client

This is a Python library for interacting with the [yt-dlp-host](https://github.com/Vasysik/yt-dlp-host) API.

## Installation

You can install the library using pip:

```
pip install yt-dlp-host-api
```

## Usage

Here's a basic example of how to use the library:

```python
import yt_dlp_host_api

# Initialize the API client
api = yt_dlp_host_api.api('http://your-api-url.com')
client = api.get_client('YOUR_API_KEY')

# Download a complete video
client.get_video(url='https://youtu.be/1FPdtR_5KFo').save_file("test_video.mp4")
print("Video saved to test_video.mp4")

# Download a video segment with precise cutting
client.get_video(
    url='https://youtu.be/1FPdtR_5KFo',
    start_time="00:05:00",
    end_time="00:10:00",
    force_keyframes=True
).save_file("precise_cut.mp4")
print("Precisely cut segment saved to precise_cut.mp4")

# Download a video segment with faster cutting at keyframes
client.get_video(
    url='https://youtu.be/1FPdtR_5KFo',
    start_time="00:05:00",
    end_time="00:10:00",
    force_keyframes=False
).save_file("keyframe_cut.mp4")
print("Keyframe-cut segment saved to keyframe_cut.mp4")

# Download a complete audio
client.get_audio(url='https://youtu.be/1FPdtR_5KFo').save_file("test_audio.mp3")
print("Audio saved to test_audio.mp3")

# Get info
info_json = client.get_info(url='https://youtu.be/1FPdtR_5KFo').get_json(['qualities', 'title'])
print("Video info:", info_json)

# Admin operations (requires admin API key)
new_key = client.create_key("user_key", ["get_video", "get_audio", "get_info"])
keys = client.get_keys()
key = client.get_key("user_key")
client.delete_key("user_key")
```

## Features

- Download YouTube videos
  - Download complete videos
  - Download specific time segments
    - Precise cutting with frame re-encoding
    - Fast cutting at keyframes
  - Choose video and audio quality
- Download YouTube audio
  - Download complete audio
  - Download specific time segments
  - Choose audio quality
- Extract live stream segments
- Retrieve video information
- Checking client permissions
- Admin operations:
  - Create new API keys
  - List existing API keys
  - Get API key by key name
  - Delete API keys

## API Reference

### Client

- `client.get_video(url, video_format="bestvideo", audio_format="bestaudio", start_time=None, end_time=None, force_keyframes=False)`: Get video with optional time segment selection
- `client.get_audio(url, audio_format="bestaudio", start_time=None, end_time=None, force_keyframes=False)`: Get audio with optional time segment selection
- `client.get_live_video(url, duration, start=0, video_format="bestvideo", audio_format="bestaudio")`: Get live video segment
- `client.get_live_audio(url, duration, start=0, audio_format="bestaudio")`: Get live audio segment
- `client.get_info(url)`: Get video information
- `client.send_task.get_video(url, video_format="bestvideo", audio_format="bestaudio", start_time=None, end_time=None, force_keyframes=False)`: Initiate a video download task
- `client.send_task.get_audio(url, audio_format="bestaudio", start_time=None, end_time=None, force_keyframes=False)`: Initiate an audio download task
- `client.send_task.get_live_video(url, duration, start=0, video_format="bestvideo", audio_format="bestaudio")`: Initiate a live video download task
- `client.send_task.get_live_audio(url, duration, start=0, audio_format="bestaudio")`: Initiate a live audio download task
- `client.send_task.get_info(url)`: Initiate an info retrieval task
- `client.check_permissions(permissions)`: Check for all permissions in the list

### Time Format

Time parameters (`start_time` and `end_time`) should be provided in the following format:
- "HH:MM:SS" (hours:minutes:seconds)
Examples:
- "00:05:00" - 5 minutes
- "01:30:45" - 1 hour, 30 minutes, and 45 seconds

### Cutting Modes

The `force_keyframes` parameter determines how video/audio segments are cut:
- `force_keyframes=False` (default): Faster cutting that aligns to nearest keyframes. May not be exactly at specified timestamps but is much faster as it avoids re-encoding.
- `force_keyframes=True`: Precise cutting at exact timestamps. This requires re-encoding which takes longer but provides exact cuts.

### Task

- `task.get_status()`: Get the current status of a task
- `task.get_result()`: Wait for and return the result of a task

### TaskResult

- `result.get_file()`: Get the file
- `result.get_file_url()`: Get the URL of the downloaded file
- `result.save_file(path)`: Save the downloaded file to the specified path
- `result.get_json(fields=None)`: Get the JSON data for info tasks (optionally filtered by fields)

### Admin

- `client.create_key(name, permissions)`: Create a new API key
- `client.get_keys()`: List all existing API keys
- `client.get_key(name)`: Get API key by key name
- `client.delete_key(name)`: Delete an API key

## Error Handling

The library uses exceptions to handle errors. Catch `yt_dlp_host_api.exceptions.APIError` to handle API-related errors.

## Contributing

Contributions to yt-dlp-host-api are welcome! If you have any suggestions, bug reports, or feature requests, please open an issue on the GitHub repository. Pull requests are also encouraged.
