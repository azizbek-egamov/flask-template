# Flask To'liq Dokumentatsiyasi

## 1. Flask haqida umumiy tushuncha
Flask — bu Python asosida yaratilgan engil veb-framework bo'lib, u minimalizm va kengaytirish imkoniyatlari bilan ajralib turadi. Flask micro-framework bo'lib, Django kabi tayyor funksiyalarni o'z ichiga olmaydi, ammo u modullar va kutubxonalar orqali kengaytirilishi mumkin.

## 2. Flask o'rnatish
Flask'ni o'rnatish uchun quyidagi buyruqdan foydalaning:
```bash
pip install flask
```

## 3. Flask asosiy tushunchalari

### 3.1 Flask ilovasini yaratish
Flask yordamida minimal veb-ilovani yaratish uchun quyidagi koddan foydalanish mumkin:

```python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def home():
    return "Salom, Flask!"

if __name__ == '__main__':
    app.run(debug=True)
```

### 3.2 Flask konfiguratsiyasi
Flask turli usullar orqali konfiguratsiyani qo'llab-quvvatlaydi:

```python
app.config['DEBUG'] = True
app.config['SECRET_KEY'] = 'maxfiy_kalit'
```

Konfiguratsiyani tashqi fayldan yuklash:
```python
app.config.from_pyfile('config.py')
```

## 4. URL marshrutlash
Flask'da marshrutlashni yaratish uchun `@app.route()` dekoratori ishlatiladi:
```python
@app.route('/hello/<name>')
def hello(name):
    return f"Salom, {name}!"
```

Qo'shimcha metodlarni qo'llash:
```python
@app.route('/submit', methods=['GET', 'POST'])
def submit():
    if request.method == 'POST':
        return "Post so'rovi yuborildi."
    return "Get so'rovi yuborildi."
```

## 5. HTTP so'rovlar bilan ishlash
Flask `GET`, `POST`, `PUT`, `DELETE` so'rovlarini qo'llab-quvvatlaydi:

### 5.1 GET so'rovi
```python
@app.route('/get-example', methods=['GET'])
def get_example():
    return "Bu GET so'rovi."
```

### 5.2 POST so'rovi
```python
@app.route('/post-example', methods=['POST'])
def post_example():
    return "Bu POST so'rovi."
```

### 5.3 PUT so'rovi
```python
@app.route('/put-example', methods=['PUT'])
def put_example():
    return "Bu PUT so'rovi."
```

### 5.4 DELETE so'rovi
```python
@app.route('/delete-example', methods=['DELETE'])
def delete_example():
    return "Bu DELETE so'rovi."
```

## 6. JSON bilan ishlash
Flask JSON formatidagi ma'lumotlarni qayta ishlashi mumkin:
```python
from flask import jsonify

@app.route('/api/data')
def get_data():
    data = {"ism": "Ali", "yosh": 25}
    return jsonify(data)
```

## 7. Jinja2 templating
Flask Jinja2 templating tizimini qo'llab-quvvatlaydi:

### `templates/index.html`
```html
<!DOCTYPE html>
<html>
<head>
    <title>Flask Sahifasi</title>
</head>
<body>
    <h1>Salom, {{ name }}!</h1>
</body>
</html>
```

### Flask kodida sahifani render qilish
```python
from flask import render_template

@app.route('/welcome/<name>')
def welcome(name):
    return render_template('index.html', name=name)
```

## 8. Flask va SQLite bilan ishlash
Flask SQLite ma'lumotlar bazasi bilan integratsiyalash mumkin:

```python
import sqlite3

def get_db_connection():
    conn = sqlite3.connect('database.db')
    conn.row_factory = sqlite3.Row
    return conn

@app.route('/users')
def users():
    conn = get_db_connection()
    users = conn.execute('SELECT * FROM users').fetchall()
    conn.close()
    return jsonify([dict(row) for row in users])
```

## 9. Flask'da autentifikatsiya
Flask login tizimini qo'shish uchun `Flask-Login` dan foydalanish mumkin:
```bash
pip install flask-login
```

Kod namunasi:
```python
from flask_login import LoginManager, UserMixin

app.config['SECRET_KEY'] = 'maxfiy_kalit'
login_manager = LoginManager(app)

class User(UserMixin):
    pass

@login_manager.user_loader
def load_user(user_id):
    return User()
```

## 10. Flask'ni Docker bilan ishlatish
`Dockerfile` yaratish:
```dockerfile
FROM python:3.9
WORKDIR /app
COPY . .
RUN pip install -r requirements.txt
CMD ["python", "app.py"]
```

Docker konteyner yaratish va ishga tushirish:
```bash
docker build -t flask_app .
docker run -p 5000:5000 flask_app
```

## 11. Flask'ni ishlab chiqarish rejimida ishlatish
Gunicorn bilan Flask'ni ishlab chiqarish rejimida ishga tushirish:
```bash
pip install gunicorn
```

```bash
gunicorn -w 4 -b 0.0.0.0:8000 app:app
```

## 12. Xulosa
Flask oddiy va kuchli veb-framework bo'lib, u turli loyihalar uchun moslashuvchanlik va kengaytirish imkoniyatlarini taqdim etadi. Ushbu hujjat Flask'ning asosiy jihatlarini qamrab oldi, lekin ko'proq funksiyalar va kutubxonalar orqali uni kengaytirish mumkin.

