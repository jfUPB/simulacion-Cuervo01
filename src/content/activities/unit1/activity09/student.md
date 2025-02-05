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

            function draw() {
                requestAnimationFrame(draw);
                analyser.getByteFrequencyData(dataArray);

                const maxVolume = Math.max(...dataArray);
                ctx.clearRect(0, 0, canvas.width, canvas.height);

                // Calcula el color según el volumen
                const volumeRatio = maxVolume / 256; // Normaliza el volumen (0 a 1)
                const hue = 120; // Verde
                const lightness = 20 + (50 - 20) * volumeRatio; // Degradé desde verde oscuro profundo a verde brillante

                for (let i = 0; i < 100; i++) {
                    const angle = Math.random() * 2 * Math.PI;
                    const radius = ((Math.random() * maxVolume) / 4) * (maxVolume > 20 ? 2 : 1);
                    const x = canvas.width / 2 + radius * Math.cos(angle);
                    const y = canvas.height / 2 + radius * Math.sin(angle);
                    const size = (Math.random() * 10 + 5) * (maxVolume / 50); // Ajusta el tamaño según el volumen

                    ctx.beginPath();
                    ctx.arc(x, y, size, 0, 2 * Math.PI);
                    ctx.fillStyle = `hsl(${hue}, 100%, ${lightness}%)`;
                    ctx.fill();
                }
            }

            draw();
        })
        .catch(err => {
            console.error('Error al acceder al micrófono', err);
        });
});
```


