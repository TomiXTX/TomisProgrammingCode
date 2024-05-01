# TomisProgrammingCode
Für meine Hausübung
<!DOCTYPE html>
<html>

<head>
    <meta charset="utf-8">
    <title>Bouncing Ball mit Paddle</title>
</head>

<body>
    <canvas id="MeinCanvas" width="600" height="400"></canvas>
    <script>
        var canvas = document.getElementById('MeinCanvas');
        var context = canvas.getContext('2d');
        var ballRadius = 10;
        var x = canvas.width / 2;
        var y = canvas.height - 30;
        var dx = 2;
        var dy = -2;
        var paddleHeight = 10;
        var paddleWidth = 75;
        var paddleX = (canvas.width - paddleWidth) / 2;
        var rightPressed = false;
        var leftPressed = false;

        // Funktion zum Zeichnen des Balls
        function drawBall() {
            context.beginPath();
            context.arc(x, y, ballRadius, 0, Math.PI * 2);
            context.fillStyle = "#0095DD";
            context.fill();
            context.closePath();
        }

        // Funktion zum Zeichnen des Paddles
        function drawPaddle() {
            context.beginPath();
            context.rect(paddleX, canvas.height - paddleHeight, paddleWidth, paddleHeight);
            context.fillStyle = "#0095DD";
            context.fill();
            context.closePath();
        }

        // Funktion zum Erkennen von Tastaturereignissen
        document.addEventListener("keydown", keyDownHandler, false);
        document.addEventListener("keyup", keyUpHandler, false);

        // Funktion zur Handhabung des Tastaturereignisses beim Drücken einer Taste
        function keyDownHandler(e) {
            if (e.key == "Right" || e.key == "ArrowRight") {
                rightPressed = true;
            } else if (e.key == "Left" || e.key == "ArrowLeft") {
                leftPressed = true;
            }
        }

        // Funktion zur Handhabung des Tastaturereignisses beim Loslassen einer Taste
        function keyUpHandler(e) {
            if (e.key == "Right" || e.key == "ArrowRight") {
                rightPressed = false;
            } else if (e.key == "Left" || e.key == "ArrowLeft") {
                leftPressed = false;
            }
        }

        // Funktion zur Aktualisierung und Zeichnung des Spiels
        function draw() {
            context.clearRect(0, 0, canvas.width, canvas.height);
            drawBall();
            drawPaddle();
            // Kollisionserkennung und -reaktion
            if (x + dx > canvas.width - ballRadius || x + dx < ballRadius) {
                dx = -dx;
            }
            if (y + dy < ballRadius) {
                dy = -dy;
            } else if (y + dy > canvas.height - ballRadius) {
                if (x > paddleX && x < paddleX + paddleWidth) {
                    dy = -dy;
                } else {
                    document.location.reload(); // Neustart, wenn Ball am unteren Rand außerhalb des Paddles ist
                    alert("Game Over");
                }
            }
            // Paddle-Bewegung
            if (rightPressed && paddleX < canvas.width - paddleWidth) {
                paddleX += 7;
            } else if (leftPressed && paddleX > 0) {
                paddleX -= 7;
            }
            x += dx;
            y += dy;
        }

        // Funktion zum Starten des Spiels bei Betätigung der Enter-Taste
        function startGame(e) {
            if (e.key === 'Enter' || e.keyCode === 13) {
                setInterval(draw, 10);
            }
        }

        // Starten des Spiels beim Laden der Seite
        document.addEventListener('keypress', startGame);

    </script>
</body>

</html>
