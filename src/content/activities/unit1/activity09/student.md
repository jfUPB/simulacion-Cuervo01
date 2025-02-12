## Aplicación

### Código:

```javascript
window.addEventListener('load', () => {
    const canvas = document.createElement('canvas');
    document.body.appendChild(canvas);
    const ctx = canvas.getContext('2d');
    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;

    // Configuración del micrófono
    navigator.mediaDevices.getUserMedia({ audio: true })
        .then(stream => {
            const AudioContext = window.AudioContext || window.webkitAudioContext;
            const audioContext = new AudioContext();
            const source = audioContext.createMediaStreamSource(stream);
            const analyser = audioContext.createAnalyser();
            analyser.fftSize = 256;
            source.connect(analyser);

            const bufferLength = analyser.frequencyBinCount;
            const dataArray = new Uint8Array(bufferLength);

            let isTriangle = false;
            let noiseTime = 0;
            let noiseX = Math.random() * 1000;
            let noiseY = Math.random() * 1000;

            function perlinNoise(x) {
                return Math.sin(x) * 0.5 + 0.5;
            }

            function drawCircle(x, y, size) {
                ctx.beginPath();
                ctx.arc(x, y, size, 0, 2 * Math.PI);
                ctx.fill();
            }

            function drawTriangle(x, y, size) {
                ctx.beginPath();
                ctx.moveTo(x, y - size);
                ctx.lineTo(x - size, y + size);
                ctx.lineTo(x + size, y + size);
                ctx.closePath();
                ctx.fill();
            }

            function draw() {
                requestAnimationFrame(draw);
                analyser.getByteFrequencyData(dataArray);
                const maxVolume = Math.max(...dataArray);
                ctx.clearRect(0, 0, canvas.width, canvas.height);

                // Configuración de color
                const volumeRatio = maxVolume / 256;
                const hue = 120;
                const lightness = 20 + (50 - 20) * volumeRatio;
                ctx.fillStyle = `hsl(${hue}, 100%, ${lightness}%)`;

                // Movimiento de todo el grupo con ruido Perlin
                noiseTime += 0.01;
                let groupX = canvas.width * perlinNoise(noiseX + noiseTime);
                let groupY = canvas.height * perlinNoise(noiseY + noiseTime);

                // Ocasionalmente cambiar de círculo a triángulo y viceversa
                if (Math.random() < 0.02) {
                    isTriangle = !isTriangle;
                }

                for (let i = 0; i < 100; i++) {
                    const angle = Math.random() * 2 * Math.PI;
                    const radius = ((Math.random() * maxVolume) / 4) * (maxVolume > 20 ? 2 : 1);
                    const x = groupX + radius * Math.cos(angle);
                    const y = groupY + radius * Math.sin(angle);
                    const size = (Math.random() * 10 + 5) * (maxVolume / 50);

                    if (isTriangle) {
                        drawTriangle(x, y, size);
                    } else {
                        drawCircle(x, y, size);
                    }
                }
            }

            draw();
        })
        .catch(err => {
            console.error('Error al acceder al micrófono', err);
        });
});

```


https://github.com/user-attachments/assets/bc6c75f4-6b74-464b-9bab-73189dce5df8

