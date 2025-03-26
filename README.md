<!DOCTYPE html>
<html lang="ar">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>تطبيق مشاهدة الأفلام</title>
  <script src="https://cdn.jsdelivr.net/npm/hls.js@latest"></script>
  <style>
    body {
      font-family: Arial, sans-serif;
      background-color: #16072E;
      margin: 0;
      padding: 0;
      display: flex;
      flex-direction: column;
      align-items: center;
      color: #fff;
    }

    .header {
      height: 50px;
      background-color: #281047;
      color: #ffa345;
      text-align: center;
      width: 100%;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 1.8em;
      font-weight: bold;
    }

    /* تنسيق القالب المتحرك */
    .slider-container {
      width: 90%;
      max-width: 600px;
      height: 230px;
      overflow: hidden;
      position: relative;
      border-radius: 13px;
      box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.5);
      margin-top: 10px;
    }

    .slider {
      display: flex;
      width: 300%;
      transition: transform 0.5s ease-in-out;
    }

    .slider img {
      width: 100%;
      height: 250px;
      object-fit: cover;
      border-radius: 15px;
    }

    /* أزرار التنقل */
    .prev, .next {
      position: absolute;
      top: 50%;
      transform: translateY(-50%);
      background: rgba(0, 0, 0, 0.5);
      color: white;
      border: none;
      padding: 10px;
      cursor: pointer;
      border-radius: 50%;
      width: 35px;
      height: 35px;
      display: flex;
      align-items: center;
      justify-content: center;
    }

    .prev { left: 10px; }
    .next { right: 10px; }

    /* قائمة الأفلام */
    .movie-list {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
      gap: 10px;
      width: 90%;
      max-width: 800px;
      margin-top: 20px;
      justify-content: center;
    }

    .movie {
      text-align: center;
    }

    .movie img {
      width: 150px;
      height: 90px;
      cursor: pointer;
      border-radius: 10px;
      transition: transform 0.3s;
    }

    .movie img:hover {
      transform: scale(1.05);
    }

    /* مشغل الفيديو */
    .video-container {
      margin-top: 20px;
      text-align: center;
      display: none;
      position: relative;
      width: 100%;
    }

    video {
      width: 90%;
      max-width: 600px;
      border-radius: 8px;
    }

    #movieTitle {
      margin-top: 10px;
      font-size: 1em;
    }

    /* زر الإغلاق */
    #closeButton {
      position: absolute;
      top: 10px;
      right: calc(5% + 10px);
      background-color: #f00;
      color: #fff;
      border: none;
      font-size: 1.2em;
      cursor: pointer;
      padding: 5px 10px;
      border-radius: 5px;
    }
  </style>
</head>
<body>
  <div class="header">JoKaR🍒</div>

  <!-- القالب المتحرك -->
  <div class="slider-container">
    <div class="slider" id="slider">
      <img src="https://www2.0zz0.com/2025/03/26/05/850528629.jpeg" alt="إعلان 1">
      <img src="اضف" alt="إعلان 2">
      <img src="https://www2.0zz0.com/2025/03/26/05/850528629.jpeg" alt="إعلان 3">
    </div>
    <button class="prev" onclick="prevSlide()">‹</button>
    <button class="next" onclick="nextSlide()">›</button>
  </div>

  <div class="movie-list">
    <div class="movie">
      <img src="https://i.postimg.cc/KvQJJPDz/knghit1-X.jpg" alt="فيلم 1" onclick="playVideo('https://video.twimg.com/ext_tw_video/1904197478624428032/pu/vid/avc1/720x1280/BbgMUxY2URj_k16D.mp4?tag=12', 'فيلم 1')">
      <p>فيلم 1</p>
    </div>
    <div class="movie">
      <img src="https://i.postimg.cc/q7C0Cptv/HD-7m.jpg" alt="فيلم 2" onclick="playVideo('https://vdownload-45.sb-cd.com/1/6/16215584-1080p.mp4', 'فيلم 2')">
      <p>فيلم 2</p>
    </div>
    <div class="movie">
      <img src="https://i.postimg.cc/fWHh09Sc/X-x-https-t-co-FCY871-Eqn-F-X.jpg" alt="فيلم 3" onclick="playVideo('https://video.twimg.com/amplify_video/1904335291625676800/pl/avc1/1920x1080/vVmIPe_WBs4LrT0p.m3u8', 'فيلم 3')">
      <p>فيلم 3</p>
    </div>
  </div>

  <div id="videoContainer" class="video-container">
    <button id="closeButton" onclick="closeVideo()">X</button>
    <h2>مشاهدة الفيديو</h2>
    <video id="videoPlayer" controls>
      متصفحك لا يدعم فيديو HTML5.
    </video>
    <p id="movieTitle"></p>
  </div>

  <script>
    let currentIndex = 0;
    const slides = document.querySelectorAll(".slider img");
    const slider = document.getElementById("slider");

    function nextSlide() {
      currentIndex = (currentIndex + 1) % slides.length;
      updateSlider();
    }

    function prevSlide() {
      currentIndex = (currentIndex - 1 + slides.length) % slides.length;
      updateSlider();
    }

    function updateSlider() {
      slider.style.transform = `translateX(-${currentIndex * 100}%)`;
    }

    setInterval(nextSlide, 9000);

    function playVideo(videoSrc, title) {
      document.querySelector('.movie-list').style.display = 'none';
      document.getElementById('videoContainer').style.display = 'block';
      const video = document.getElementById('videoPlayer');
      const movieTitle = document.getElementById('movieTitle');

      movieTitle.textContent = title;

      if (Hls.isSupported() && videoSrc.endsWith(".m3u8")) {
          const hls = new Hls();
          hls.loadSource(videoSrc);
          hls.attachMedia(video);
          hls.on(Hls.Events.MANIFEST_PARSED, function () {
              video.play();
          });
      } else {
          video.src = videoSrc;
          video.play();
      }
  }

    function closeVideo() {
      document.getElementById('videoContainer').style.display = 'none';
      document.querySelector('.movie-list').style.display = 'grid';
    }
  </script>
</body>
</html>
