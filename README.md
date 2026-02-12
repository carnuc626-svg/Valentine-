# Valentine-<!DOCTYPE html><html lang="ru">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Валентинка</title>  <style>
    body {
      margin: 0;
      font-family: "Segoe UI", sans-serif;
      background: linear-gradient(180deg, #fff0f5, #ffe4ec);
      display: flex;
      align-items: center;
      justify-content: center;
      height: 100vh;
      overflow: hidden;
    }

    .card {
      background: white;
      border-radius: 24px;
      box-shadow: 0 10px 30px rgba(255, 105, 180, 0.25);
      padding: 28px 22px;
      text-align: center;
      max-width: 380px;
      width: 92%;
      position: relative;
      animation: fadeIn 0.5s ease;
    }

    h1 { font-size: 22px; margin-bottom: 14px; }

    .preview-img {
      width: 100%;
      max-height: 200px;
      object-fit: cover;
      border-radius: 16px;
      margin-bottom: 12px;
    }

    .btn {
      margin-top: 10px;
      background: linear-gradient(135deg, #ff6fa5, #ff3d8b);
      border: none;
      color: white;
      padding: 12px 18px;
      font-size: 15px;
      border-radius: 999px;
      cursor: pointer;
      transition: 0.15s;
      box-shadow: 0 6px 18px rgba(255, 61, 139, 0.35);
    }

    .btn:hover { transform: translateY(-2px); }

    .btn-secondary {
      background: #eee;
      color: #555;
      box-shadow: none;
    }

    .buttons { display: flex; justify-content: center; gap: 10px; flex-wrap: wrap; }

    .hidden { display: none; }

    input, textarea {
      width: 100%;
      margin-top: 8px;
      padding: 10px;
      border-radius: 10px;
      border: 1px solid #ffd1e0;
      font-size: 14px;
      outline: none;
    }

    textarea { resize: none; height: 70px; }

    .heart {
      position: absolute;
      color: #ff5c9c;
      animation: floatUp linear infinite;
      opacity: 0.6;
      pointer-events: none;
    }

    @keyframes floatUp {
      from { transform: translateY(0); opacity: 0.7; }
      to { transform: translateY(-120vh); opacity: 0; }
    }

    @keyframes fadeIn {
      from { opacity: 0; transform: scale(0.95); }
      to { opacity: 1; transform: scale(1); }
    }

    .open-anim {
      animation: openCard 0.6s ease forwards;
    }

    @keyframes openCard {
      from { transform: scale(0.8) rotateX(30deg); opacity: 0; }
      to { transform: scale(1) rotateX(0); opacity: 1; }
    }
  </style></head>
<body>  <!-- Музыка -->  <audio id="music" loop>
    <source src="https://cdn.pixabay.com/download/audio/2022/03/15/audio_115b9b3c0c.mp3" type="audio/mpeg">
  </audio>  <!-- Шаг 1 -->  <div class="card" id="step1">
    <h1>Создай валентинку 💖</h1><input type="file" id="startImage" accept="image/*" />
<img id="imgPreview" class="preview-img hidden" />

<input id="name" placeholder="Имя получателя" />
<textarea id="message" placeholder="Твоё сообщение"></textarea>

<button class="btn" onclick="goPreview()">Далее</button>

  </div>  <!-- Шаг 2 -->  <div class="card hidden" id="step2">
    <img id="questionImg" class="preview-img hidden" />
    <h1>Откроешь валентинку? (≧◡≦)</h1><div class="buttons">
  <button class="btn" onclick="openValentine()">Да</button>
  <button class="btn btn-secondary" onmouseover="runAway(this)">Нет</button>
</div>

  </div>  <!-- Шаг 3: слайдшоу -->  <div class="card hidden" id="step3">
    <img id="slideImg" class="preview-img hidden" />
    <h1 id="finalText"></h1><input type="file" id="moreImages" accept="image/*" multiple />
<textarea id="moreText" placeholder="Дополнительное пожелание"></textarea>

<div class="buttons">
  <button class="btn" onclick="nextSlide()">Следующий слайд</button>
  <button class="btn btn-secondary" onclick="shareLink()">Скопировать ссылку</button>
  <button class="btn btn-secondary" onclick="restart()">Заново</button>
</div>

  </div>  <script>
    let slides = [];
    let currentSlide = 0;

    const music = document.getElementById('music');

    const startInput = document.getElementById('startImage');
    const imgPreview = document.getElementById('imgPreview');
    const questionImg = document.getElementById('questionImg');

    startInput.onchange = e => {
      const file = e.target.files[0];
      if (!file) return;
      const url = URL.createObjectURL(file);
      imgPreview.src = url;
      imgPreview.classList.remove('hidden');
      questionImg.src = url;
      questionImg.classList.remove('hidden');
    };

    function goPreview() {
      step1.classList.add('hidden');
      step2.classList.remove('hidden');
      music.play().catch(()=>{});
    }

    function openValentine() {
      const name = document.getElementById('name').value || 'тебя';
      const msg = document.getElementById('message').value || 'С Днём Святого Валентина!';

      const text = `${name}, ${msg} 💗`;
      document.getElementById('finalText').innerText = text;

      step2.classList.add('hidden');
      step3.classList.remove('hidden');
      step3.classList.add('open-anim');
    }

    const moreImages = document.getElementById('moreImages');
    const slideImg = document.getElementById('slideImg');

    moreImages.onchange = e => {
      slides = [...e.target.files].map(f => URL.createObjectURL(f));
      currentSlide = 0;
      showSlide();
    };

    function showSlide() {
      if (!slides.length) return;
      slideImg.src = slides[currentSlide];
      slideImg.classList.remove('hidden');
    }

    function nextSlide() {
      const extra = document.getElementById('moreText').value;
      if (extra) alert(extra);

      if (!slides.length) return;
      currentSlide = (currentSlide + 1) % slides.length;
      showSlide();
    }

    function shareLink() {
      const name = document.getElementById('name').value || '';
      const msg = document.getElementById('message').value || '';

      const data = btoa(encodeURIComponent(name + '|' + msg));
      const url = location.origin + location.pathname + '?v=' + data;

      navigator.clipboard.writeText(url);
      alert('Ссылка скопирована 💌');
    }

    function loadFromLink() {
      const params = new URLSearchParams(location.search);
      const v = params.get('v');
      if (!v) return;

      const [name, msg] = decodeURIComponent(atob(v)).split('|');

      document.getElementById('name').value = name;
      document.getElementById('message').value = msg;

      goPreview();
      openValentine();
    }

    function restart() { location.href = location.pathname; }

    function runAway(btn) {
      btn.style.position = 'absolute';
      btn.style.left = Math.random() * 80 + '%';
      btn.style.top = Math.random() * 80 + '%';
    }

    // сердечки
    setInterval(() => {
      const heart = document.createElement('div');
      heart.className = 'heart';
      heart.innerText = '❤';
      heart.style.left = Math.random() * 100 + 'vw';
      heart.style.animationDuration = 4 + Math.random() * 4 + 's';
      heart.style.fontSize = 14 + Math.random() * 10 + 'px';
      document.body.appendChild(heart);
      setTimeout(() => heart.remove(), 8000);
    }, 400);

    loadFromLink();
  </script></body>
</html>
