# 🎯 Колесо Баланса — Инструкция по настройке

## Шаг 1: Создать проект в Firebase

1. Открой [console.firebase.google.com](https://console.firebase.google.com)
2. Нажми **"Создать проект"** (или "Add project")
3. Введи название, например: `koleso-balansa`
4. Google Analytics — можно отключить (не нужно)
5. Нажми **"Создать проект"**

---

## Шаг 2: Добавить веб-приложение

1. На главной странице проекта нажми иконку **`</>`** (веб)
2. Введи название приложения: `Колесо Баланса`
3. ✅ Поставь галку **"Firebase Hosting"** (необязательно)
4. Нажми **"Зарегистрировать приложение"**
5. Скопируй блок с конфигурацией — он выглядит так:

```js
const firebaseConfig = {
  apiKey: "AIzaSy...",
  authDomain: "koleso-balansa.firebaseapp.com",
  projectId: "koleso-balansa",
  storageBucket: "koleso-balansa.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abc123"
};
```

---

## Шаг 3: Настроить Google Authentication

1. В боковом меню → **Authentication** → **Sign-in method**
2. Нажми **Google** → включи (Enable)
3. Введи email поддержки
4. Нажми **Сохранить**
5. Перейди на вкладку **"Authorized domains"**
6. Добавь свой домен: `forevernatali.github.io`

---

## Шаг 4: Создать базу данных Firestore

1. В боковом меню → **Firestore Database**
2. Нажми **"Создать базу данных"**
3. Выбери **"Начать в продакшн режиме"**
4. Выбери регион (например `europe-west3` — Франкфурт)
5. Нажми **"Создать"**

### Правила безопасности Firestore:

1. В Firestore → вкладка **"Правила"**
2. Замени всё содержимое на:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{userId}/{document=**} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
  }
}
```

3. Нажми **"Опубликовать"**

---

## Шаг 5: Вставить конфигурацию в приложение

1. Открой файл `index.html`
2. Найди блок (около строки 70):

```js
const firebaseConfig = {
  apiKey:            "REPLACE_WITH_YOUR_API_KEY",
  authDomain:        "REPLACE_WITH_YOUR_AUTH_DOMAIN",
  ...
};
```

3. Замени все `REPLACE_WITH_...` значения на данные из вашего Firebase

---

## Шаг 6: Загрузить на GitHub

Загрузи все файлы в репозиторий `my_wheel`:
- `index.html`
- `manifest.json`
- `sw.js`
- `icon-192.png`
- `icon-512.png`
- `apple-touch-icon.png`

---

## Установка на телефон (как приложение)

### iPhone:
1. Открой сайт в Safari
2. Нажми кнопку **"Поделиться"** (квадрат со стрелкой вверх)
3. **"На экран «Домой»"**

### Android:
1. Открой сайт в Chrome
2. Нажми меню **⋮** → **"Добавить на главный экран"**

---

## Структура данных в Firestore

```
users/
  {userId}/
    wheel/
      data:
        values: { health: 7, family: 8, love: 9, ... }
        notes:  { health: "...", family: "...", ... }
        updatedAt: timestamp
```

Каждый пользователь видит только свои данные ✅
