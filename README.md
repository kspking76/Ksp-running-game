here<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>KSP Mini Royale</title>

<style>
    body {
        margin: 0;
        overflow: hidden;
        background: #111;
    }

    #player {
        width: 40px;
        height: 40px;
        background: cyan;
        position: absolute;
        left: 20px;
        top: 50%;
        transform: translateY(-50%);
        border-radius: 5px;
    }

    .enemy {
        width: 40px;
        height: 40px;
        background: red;
        position: absolute;
        right: 0;
        top: 50%;
        border-radius: 5px;
        animation: moveEnemy 4s linear infinite;
    }

    @keyframes moveEnemy {
        from { right: -50px; }
        to { right: 100%; }
    }

    .bullet {
        width: 10px;
        height: 5px;
        background: yellow;
        position: absolute;
        border-radius: 2px;
    }

    #score {
        position: fixed;
        color: white;
        font-size: 22px;
        left: 10px;
        top: 10px;
    }
</style>

</head>
<body>

<div id="player"></div>
<div class="enemy"></div>
<div id="score">Score: 0</div>

<script>
    const player = document.getElementById("player");
    const enemy = document.querySelector(".enemy");
    let scoreVal = 0;
    const scoreBox = document.getElementById("score");

    document.addEventListener("click", () => shoot());

    function shoot() {
        const bullet = document.createElement("div");
        bullet.classList.add("bullet");

        bullet.style.left = player.offsetLeft + 30 + "px";
        bullet.style.top = player.offsetTop + 18 + "px";

        document.body.appendChild(bullet);

        const interval = setInterval(() => {
            bullet.style.left = parseInt(bullet.style.left) + 15 + "px";

            if (collision(bullet, enemy)) {
                scoreVal++;
                scoreBox.innerText = "Score: " + scoreVal;
                enemy.style.top = Math.random() * (window.innerHeight - 50) + "px";
                clearInterval(interval);
                bullet.remove();
            }

            if (parseInt(bullet.style.left) > window.innerWidth) {
                clearInterval(interval);
                bullet.remove();
            }
        }, 20);
    }

    function collision(obj1, obj2) {
        const r1 = obj1.getBoundingClientRect();
        const r2 = obj2.getBoundingClientRect();
        return !(
            r1.top > r2.bottom ||
            r1.bottom < r2.top ||
            r1.left > r2.right ||
            r1.right < r2.left
        );
    }
</script>

</body>
</html>
