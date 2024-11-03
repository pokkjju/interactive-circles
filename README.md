<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Interactive Circles</title>
    <style>
        @font-face {
            font-family: 'Unscii';
            src: url('./assets/fonts/unscii-8-alt.ttf') format('truetype');
        }

        body {
            font-family: 'Unscii', sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            background-color: #f0f0f0;
            overflow: hidden;
        }

        .container {
            display: flex;
            justify-content: center;
        }

        .circle {
            width: 30px;
            height: 30px;
            border-radius: 50%;
            background-color: blue;
            color: white;
            display: flex;
            justify-content: center;
            align-items: center;
            cursor: pointer;
            font-size: 14px;
            margin: 10px;
        }

        .overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100vw;
            height: 100vh;
            background-color: rgba(0, 0, 0, 0.8);
            color: white;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            z-index: 10;
            display: none;
        }

        .mosaic {
            position: absolute;
            top: 0;
            left: 0;
            width: 100vw;
            height: 100vh;
            display: grid;
            grid-template-columns: repeat(auto-fill, 5%);
            grid-template-rows: repeat(auto-fill, 5%);
            z-index: 9;
        }

        .mosaic-item {
            width: 100%;
            height: 100%;
            animation: blink-animation 1s forwards;
        }

        @keyframes blink-animation {
            0% { opacity: 1; }
            25% { opacity: 1; transform: scale(1.2); }
            40% { opacity: 0; }
            50% { opacity: 1; transform: scale(1.2); }
            60% { opacity: 0; }
            100% { opacity: 0; }
        }

        .back-button {
            font-family: 'Unscii', sans-serif;
            background-color: transparent;
            border: none;
            color: white;
            cursor: pointer;
            font-size: 14px;
            margin-top: 20px;
            padding: 5px 10px;
        }

        #dimension-text {
            font-size: 20px;
            margin-bottom: 20px;
        }

        video, img {
            max-width: 60%;
            height: auto;
            margin: 20px 0;
            display: none;
        }

        #video1 {
            max-width: 40%;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="circle" id="circle1">1</div>
        <div class="circle" id="circle2">2</div>
        <div class="circle" id="circle3">3</div>
    </div>

    <div id="overlay" class="overlay">
        <div id="mosaic" class="mosaic"></div>
        <h1 id="dimension-text"></h1>
        <button id="back-button" class="back-button">back</button>
        <video id="video1" autoplay muted loop controls>
            <source src="/Users/gyurisim/Desktop/personal/web/커2 중간/해골.mp4" type="video/mp4">
        </video>
        <video id="video2" autoplay muted loop controls>
            <source src="/Users/gyurisim/Desktop/personal/web/커2 중간/main_1.mp4" type="video/mp4">
        </video>
        <img id="image" src="/Users/gyurisim/Desktop/personal/web/커2 중간/심규리_C335211_2주차_리서치아카이브-과제 복사본.jpg" alt="Your Image">
    </div>

    <script>
        document.querySelectorAll('.circle').forEach(circle => {
            circle.addEventListener('click', () => showMosaic(circle.id));
        });

        function showMosaic(circleId) {
            const overlay = document.getElementById('overlay');
            const mosaic = document.getElementById('mosaic');
            const dimensionText = document.getElementById('dimension-text');
            const backButton = document.getElementById('back-button');

            overlay.style.display = 'flex';
            mosaic.style.display = 'grid';
            mosaic.innerHTML = ''; 

            const colors = ['#00ff0c', 'black']; // 모자이크 색상 설정

            for (let i = 0; i < 500; i++) {
                const mosaicItem = document.createElement('div');
                mosaicItem.className = 'mosaic-item';
                mosaicItem.style.backgroundColor = colors[Math.floor(Math.random() * colors.length)];
                mosaicItem.style.animationDelay = `${Math.random() * 0.5}s`;
                mosaic.appendChild(mosaicItem);
            }

            dimensionText.textContent = `dimension ${circleId.slice(-1)}`;
            backButton.style.display = 'block'; // 뒤로가기 버튼 보이게 설정

            // 모자이크 애니메이션 끝나고 영상 표시
            setTimeout(() => {
                mosaic.style.display = 'none';
                const video1 = document.getElementById('video1');
                const video2 = document.getElementById('video2');
                const image = document.getElementById('image');

                video1.style.display = circleId === 'circle1' ? 'block' : 'none';
                video2.style.display = circleId === 'circle2' ? 'block' : 'none';
                image.style.display = circleId === 'circle3' ? 'block' : 'none';

                // 자동재생 확인
                if (circleId === 'circle1' || circleId === 'circle2') {
                    video1.play();
                    video2.play();
                }
            }, 1000); // 모자이크 애니메이션 시간과 동일하게 설정

        }

        document.getElementById('back-button').addEventListener('click', () => {
            document.getElementById('overlay').style.display = 'none';
            document.querySelectorAll('video, img').forEach(media => {
                media.style.display = 'none';
                media.pause();
            });
        });
    </script>
</body>
</html>

