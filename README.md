# -.-
from googleapiclient.discovery import build

def get_youtube_comments(api_key, video_id):
    youtube = build('youtube', 'v3', developerKey=api_key)
    comments = []
    
    request = youtube.commentThreads().list(
        part="snippet",
        videoId=video_id,
        maxResults=100
    )
    response = request.execute()

    for item in response.get('items', []):
        comment = item['snippet']['topLevelComment']['snippet']['textDisplay']
        comments.append(comment)
        
    return comments
    from kiwipiepy import Kiwi
import re

kiwi = Kiwi()

def extract_nouns(text_list):
    nouns = []
    for text in text_list:
        clean_text = re.sub(r'[^가-힣\s]', '', text)
        tokens = kiwi.tokenize(clean_text)
        for token in tokens:
            if token.tag.startswith('NN'):  # 명사 태그 추출
                nouns.append(token.form)
    return " ".join(nouns)
