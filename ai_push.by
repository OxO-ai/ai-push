import requests
import os
import random
from datetime import datetime

DEEPSEEK_API_KEY = os.environ.get("DEEPSEEK_KEY")
BARK_URL = os.environ.get("BARK_URL")

PROMPTS = [
    "以朋友口吻随便发条消息，简短自然，30字内。",
    "随便说点日常小事，像朋友随口聊天，20字左右。",
    "随口问一句在干嘛，自然点别太热情，20字内。"
]

def send_ai_message():
    headers = {
        "Authorization": f"Bearer {DEEPSEEK_API_KEY}",
        "Content-Type": "application/json"
    }
    
    payload = {
        "model": "deepseek-chat",
        "messages": [
            {"role": "system", "content": "你是一个真实朋友，说话非常简短自然，口语化，不用任何格式表情。"},
            {"role": "user", "content": random.choice(PROMPTS)}
        ],
        "max_tokens": 60,
        "temperature": 0.9
    }
    
    try:
        resp = requests.post(
            "https://api.deepseek.com/chat/completions",
            headers=headers,
            json=payload,
            timeout=15
        )
        resp.raise_for_status()
        text = resp.json()["choices"][0]["message"]["content"].strip()
    except Exception as e:
        text = f"(出错啦: {e})"
    
    try:
        requests.post(
            BARK_URL,
            json={"title": "💬", "body": text, "group": "AI主动聊天"},
            timeout=10
        )
        print(f"[{datetime.now()}] 已推送: {text}")
    except Exception as e:
        print(f"推送失败: {e}")

send_ai_message()
