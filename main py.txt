from flask import Flask, request, jsonify
from youtube_transcript_api import YouTubeTranscriptApi

app = Flask(__name__)

@app.route('/transcribe', methods=['GET'])
def transcribe():
    video_id = request.args.get('video_id')
    try:
        transcript = YouTubeTranscriptApi.get_transcript(video_id)
        full_text = " ".join([item['text'] for item in transcript])
        return jsonify({"transcript": full_text})
    except Exception as e:
        return jsonify({"error": str(e)})

app.run(host='0.0.0.0', port=8080)
