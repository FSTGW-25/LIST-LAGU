<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Music Player</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.4/css/all.min.css">
    <style>
        body {
            font-family: Arial, Helvetica, sans-serif;
            background: linear-gradient(to right,#9a1d86, #1595be);
            font-weight: bold;
            display: flex;
            align-items: center;
            justify-content: center;
            height: 100vh;
            margin: 0;
        }
         /* Music Box */
    .music-box {
      background-color: #333;
      color: white;
      width: 250px; /* Sesuaikan dengan lebar yang diinginkan */
      margin: 20px auto;
      padding: 20px;
      border-radius: 10px;
      box-shadow: 0 4px 12px rgba(255, 255, 255, 0.3);
      text-align: center;
  flex-direction: column;
  align-items: center; /* Semua elemen dalam music box berada di tengah */
  justify-content: center;
    }
    

    .music-box h2 {
  font-size: 1.5em;
  margin: 0 auto;
  text-align: center;
  padding-bottom: 10px; /* Jarak antara teks dan garis bawah */
  border-bottom: 2px solid white; /* Garis bawah */
  display: inline-block;
  width: 100%; /* Agar garis bawah penuh */
}

.cover-circle {
      width: 150px;
      height: 150px;
      margin: 20px auto;
      border-radius: 50%;
      background: url('yellow.jpg') no-repeat center center;
      background-size: cover;
      animation: rotateCover 8s linear infinite;
      box-shadow: 0 0 10px rgba(255, 255, 255, 0.5);
    }


    @keyframes rotateCover {
      0% { transform: rotate(0deg); }
      100% { transform: rotate(360deg); }
    }

    .progress-container {
      width: 100%;
      background-color: #444;
      height: 5px;
      border-radius: 5px;
      position: relative;
      margin-top: 20px;
    }

    .progress-bar {
      height: 100%;
      background-color: #ed7bef;
      width: 0;
      border-radius: 5px;
      transition: width 0.2s ease;
    }

    .time-container {
      display: flex;
      justify-content: space-between;
      font-size: 0.8em;
      color: #bbb;
      margin-top: 5px;
    }

    .controls {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-top: 10px;
    }

    .control-btn {
      width: 50px;
      height: 50px;
      border: none;
      background-color: transparent;
      border-radius: 50%;
      color: white;
      font-size: 1.5em;
      cursor: pointer;
      background: linear-gradient(to right, #FFC371, #FF5F6D);
    }

    .control-btn:hover {
      background-color: 45deg, #d47a14, #FFD700;
    
        }

        .shuffle-btn {
  font-size: 1.5em;
  background:#FFC371, #FF5F6D;
  border: none;
  cursor: pointer;
  color: #FFC371, #FF5F6D;
}

.shuffle-btn.active {
  color: rgb(240, 119, 240); /* Warna hijau saat aktif */
}

        

.music-wave {
    display: flex;
    justify-content: center;
    align-items: flex-end;
    height: 30px;
    gap: 4px;
}

.music-wave span {
    width: 6px;
    height: 10px;
    background: #a95ed2;
    border-radius: 3px;
    animation: wave-animation 1.2s infinite ease-in-out;
    display: inline-block;
}

.music-wave span:nth-child(1) { animation-delay: 0.2s; }
.music-wave span:nth-child(2) { animation-delay: 0.4s; }
.music-wave span:nth-child(3) { animation-delay: 0.6s; }
.music-wave span:nth-child(4) { animation-delay: 0.8s; }
.music-wave span:nth-child(5) { animation-delay: 1s; }

@keyframes wave-animation {
    0%, 100% { height: 10px; }
    50% { height: 25px; }
}



/* Saat lagu tidak diputar, animasi berhenti */
.music-wave.paused span {
    animation: none;
}

    </style>
</head>
<body>
    <div class="music-box">
    <h2>FEARLESS</h2>
    <p>SEVENTEEN</p>
    <div class="cover-circle" id="coverImage"></div>
    <div class="time-container">
      <span id="start-time">00:00</span>
      <span id="end-time">00:00</span>
    </div>
    <div class="progress-container">
      <div class="progress-bar" id="progress-bar"></div>
    </div>
    <div class="controls">
      <button class="control-btn" id="rewind-btn">⏪</button>
      <button class="control-btn" id="play-btn">▶️</button>
      <button class="control-btn" id="forward-btn">⏩</button>
      <button class="control-btn shuffle-btn" onclick="toggleShuffle()">🔀</button>
      <div class="controls">

        <div class="music-wave">
            <span></span>
            <span></span>
            <span></span>
            <span></span>
            <span></span>
        </div>
    </div>
    </div>
  </div>

    <script>
   let track_index = 0;
let isPlaying = false;
let isShuffle = false; // Tambahkan ini
let curr_track = new Audio();

    const track_name = document.querySelector('.music-box h2');
    const track_artist = document.querySelector('.music-box p');
    const track_cover = document.querySelector('.cover-circle');
    const playpause_btn = document.getElementById('play-btn');
    const next_btn = document.getElementById('forward-btn');
    const prev_btn = document.getElementById('rewind-btn');
    const progressBar = document.getElementById("progress-bar");
    const startTime = document.getElementById("start-time");
    const endTime = document.getElementById("end-time");

    
    const music_list = [
    { img: 'foto 1.jpeg', name: 'SWEETEST THING', artist: 'SEVENTEEN', music: 'SWEETEST THING.mp3' },
    { img: 'foto 2.jpeg', name: 'MICROPHONE', artist: 'YOUNGK', music: 'YoungK- Microphone.mp3' },
    { img: 'foto 3.jpeg', name: 'SWEET CHAOS', artist: 'DAY6', music: 'Day6- Sweet Chaos.mp3' },
    { img: 'foto 4.jpeg', name: 'FALLIN FLOWER', artist: 'SEVENTEEN', music: 'FALLIN FLOWER.mp3' },
    { img: 'foto 5.jpeg', name: 'CHEERS TO YOUTH', artist: 'SEVENTEEN', music: 'Seventeen-Cheers To Youth.mp3' },
    { img: 'foto 6.jpeg', name: 'GAME BOY', artist: 'SEVENTEEN', music: 'Seventeen-Game Boy.mp3' },
    { img: 'foto 7.jpeg', name: 'ROSE BLOSSOM', artist: 'H1-KEY', music: 'H1-KEY- Rose Bloosom.mp3' },
    { img: 'foto 8.jpeg', name: 'CIRCLES', artist: 'SEVENTEEN', music: 'Seventeen- Circles.mp3' },
    { img: 'foto 9.jpeg', name: 'BETTER PLACE', artist: 'RACHEL PLATTEN', music: 'Rachel Paltten- Better Place.mp3' },
    { img: 'foto 10.jpeg', name: 'OUR DAWN IS HOTTER THAN DAY', artist: 'SEVENTEEN', music: 'Seventeen-Our Dawn is Hotter Than Day.mp3' },
    { img: 'foto 11.jpeg', name: 'HOT', artist: 'SEVENTEEN', music: 'Seventeen-Hot.mp3' },
    { img: 'foto 12.jpeg', name: 'HOLIDAY', artist: 'SEVENTEEN', music: 'Seventeen- Holiday.mp3' },
    { img: 'foto 13.jpeg', name: 'HOME', artist: 'SEVENTEEN', music: 'Seveneteen-Home.mp3' },
    { img: 'foto 14.jpeg', name: 'KIDULT', artist: 'SEVENTEEN', music: 'Seventeen-Kidult.mp3' },
    { img: 'foto 15.jpeg', name: 'ROCK WITH YOU', artist: 'SEVENTEEN', music: 'Seventeen-Rock WIth You.mp3' },
    { img: 'foto 16.jpeg', name: 'F_CK MY LIFE', artist: 'SEVENTEEN', music: 'Seventeen-F_ck My Life.mp3' },
    { img: 'foto 17.jpeg', name: 'TIME OF OUR LIFE', artist: 'DAY6', music: 'Day6- Time of Our Life.mp3' },
    { img: 'foto 18.jpeg', name: 'FEARLESS', artist: 'SEVENTEEN', music: 'Seventeen-Fearless.mp3' },
    { img: 'foto 19.jpeg', name: 'SNAP SHOOT', artist: 'SEVENTEEN', music: 'Seventeen-Snap Shoot.mp3' },
    { img: 'foto 20.jpeg', name: 'SUNFLOWER', artist: 'PAULINE ZOE PARK', music: 'Pauline Zoe Park- Sunflower.mp3' },
    { img: 'foto 21.jpeg', name: 'FIRE', artist: 'SEVENTEEN', music: 'Seventeen-Fire.mp3' },
    { img: 'foto 22.jpeg', name: 'CHEERS TO YOUTH', artist: 'SEVENTEEN', music: 'Seventeen-Cheers To Youth.mp3' },
    { img: 'foto 23.jpeg', name: 'LOVE SOMEONE', artist: 'LUKAS GRAHAM', music: 'Lukas Graham- Love Someone.mp3' },
    { img: 'foto 24.jpeg', name: 'IMPERFECT LOVE', artist: 'SEVENTEEN', music: 'Seventeen-Imperfect Love.mp3' },
    { img: 'foto 25.jpeg', name: 'OH MY!', artist: 'SEVENTEEN', music: 'Seventeen-Oh My!.mp3' },
    { img: 'foto 26.jpeg', name: 'YAWN', artist: 'SEVENTEEN', music: 'Seventeen-Yawn.mp3' },
    { img: 'foto 27.jpeg', name: 'MY MY', artist: 'SEVENTEEN', music: 'Seventeen-My My.mp3' },
    { img: 'foto 28.jpeg', name: 'YOU AND ME', artist: 'JENNIE', music: 'Jennie- You and Me.mp3' },
    { img: 'foto 29.jpeg', name: 'HUG', artist: 'SEVENTEEN', music: 'Seventeen-Hug.mp3' },
    { img: 'foto 30.jpeg', name: 'CAMPFIRE', artist: 'SEVENTEEN', music: 'Seventeen-Campfire.mp3' }
];

    function loadTrack(track_index) {
    let track = music_list[track_index];  
    curr_track.src = track.music;
    curr_track.load();
    
    // Mengupdate elemen dengan informasi lagu
    document.querySelector("h2").textContent = track.name;
    document.querySelector("p").textContent = track.artist;
    

}


    function loadTrack(track_index) {
        curr_track.src = music_list[track_index].music;
        curr_track.load();

        // Perbarui UI
        track_name.textContent = music_list[track_index].name;
        track_artist.textContent = music_list[track_index].artist;
        track_cover.style.backgroundImage = `url(${music_list[track_index].img})`;

        curr_track.addEventListener("ended", nextTrack);
    }

    function playpauseTrack() {
        if (isPlaying) {
            curr_track.pause();
            isPlaying = false;
            playpause_btn.textContent = "▶️";
        } else {
            curr_track.play();
            isPlaying = true;
            playpause_btn.textContent = "⏸";
        }
    }

    function nextTrack() {
        track_index = (track_index + 1) % music_list.length;
        loadTrack(track_index);
        playTrack();
    }

    function prevTrack() {
        track_index = (track_index - 1 + music_list.length) % music_list.length;
        loadTrack(track_index);
        playTrack();
    }

    function playTrack() {
        curr_track.play();
        isPlaying = true;
        playpause_btn.textContent = "⏸";
    }

    function updateProgress() {
        if (curr_track.duration) {
            let progress = (curr_track.currentTime / curr_track.duration) * 100;
            progressBar.style.width = `${progress}%`;
            startTime.textContent = formatTime(curr_track.currentTime);
            endTime.textContent = formatTime(curr_track.duration);
        }
    }

    function formatTime(time) {
        let minutes = Math.floor(time / 60);
        let seconds = Math.floor(time % 60);
        return `${minutes}:${seconds < 10 ? "0" : ""}${seconds}`;
    }
    function toggleShuffle() {
    isShuffle = !isShuffle;
    document.querySelector('.shuffle-btn').classList.toggle('active', isShuffle);
}

function nextTrack() {
    if (isShuffle) {
        track_index = Math.floor(Math.random() * music_list.length); // Acak lagu
    } else {
        track_index = (track_index + 1) % music_list.length; // Urutan biasa
    }
    loadTrack(track_index);
    playTrack();
}


    curr_track.addEventListener("timeupdate", updateProgress);

    // Event Listeners
    playpause_btn.addEventListener("click", playpauseTrack);
    next_btn.addEventListener("click", nextTrack);
    prev_btn.addEventListener("click", prevTrack);

    // Load first track
    loadTrack(track_index);

   const audio = document.getElementById('audio-player');
const musicWave = document.querySelector('.music-wave');
const playBtn = document.getElementById('play-btn');

audio.addEventListener('play', () => {
    console.log("Lagu diputar"); // Debugging
    musicWave.classList.remove('paused'); // Hapus class paused agar animasi berjalan
});

audio.addEventListener('pause', () => {
    console.log("Lagu dijeda"); // Debugging
    musicWave.classList.add('paused'); // Tambahkan class paused agar animasi berhenti
});


</script>

    </script>
</body>
</html>
