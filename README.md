<!DOCTYPE html>
<html lang="kk">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Алмаз Генератор</title>
    <style>
        body {
            background-color: black;
            color: lime;
            text-align: center;
            font-family: Arial, sans-serif;
        }
        .container {
            margin-top: 50px;
        }
        input {
            background: black;
            color: lime;
            border: 1px solid lime;
            padding: 10px;
            font-size: 18px;
            text-align: center;
        }
        button {
            background: lime;
            color: black;
            border: none;
            padding: 10px 20px;
            font-size: 18px;
            cursor: pointer;
            margin: 5px;
        }
        #randomNumbers {
            font-size: 20px;
            margin-top: 20px;
        }
        .hacker {
            width: 150px;
            height: auto;
        }
        #diamondMessage, #errorMessage {
            display: none;
            font-size: 18px;
            margin-top: 20px;
        }
        #errorMessage {
            color: red;
            font-size: 24px;
            font-weight: bold;
        }
        .loading::after {
            content: "⏳ В обработке";
            animation: loadingDots 1.5s infinite steps(4);
        }
        @keyframes loadingDots {
            0% { content: "⏳ В обработке"; }
            33% { content: "⏳ В обработке."; }
            66% { content: "⏳ В обработке.."; }
            100% { content: "⏳ В обработке..."; }
        }
        .button-container {
            display: flex;
            justify-content: center;
            gap: 10px;
            margin-top: 10px;
        }
    </style>
</head>
<body>

<div class="container">
    <h1>Алмаз Генератор</h1>
    <p>ID енгізіңіз:</p>
    <input type="text" id="idInput" maxlength="12" placeholder="ID">
    <p>Алмаздың саны:</p>
    <input type="text" id="extraInput" maxlength="5" placeholder="5 сан">
    
    <p><img src="https://yt3.googleusercontent.com/rt855NZ6C3MYfNsekzonK483XJyZBS_3LIFzbCXpwKx0AD8HUJqmCcjN8nN3eMa6-UE6QNPJZg=s900-c-k-c0x00ffffff-no-rj" class="hacker"></p>
    
    <p id="randomNumbers">Генерация күтілуде...</p>

    <p>
        <a href="https://www.tiktok.com/@ff_sns22" target="_blank" id="registerLink" onclick="setRegistered()">Осы сілтемеге тіркеліңіз</a>
    </p>

    <div class="button-container">
        <button onclick="confirmRegistration()">Тіркелдім</button>
        <button id="completeButton" style="display: none;" onclick="completeOperation()">Операцияны аяқтау</button>
    </div>

    <p id="diamondMessage">⚠️ Алмаз 12 сағат ішінде түседі!</p>
    <p id="errorMessage">❌ Вы ещё не подписывались!</p>

    <div id="history">
        <h3>Алынған алмаздар тізімі:</h3>
        <ul id="historyList"></ul>
    </div>
</div>

<script>
    let isRegistered = false;
    let registerClicked = false;
    let isGenerationComplete = false;
    let historyData = {};
    let randomInterval;

    function setRegistered() {
        isRegistered = true;
    }

    function confirmRegistration() {
        if (registerClicked) {
            let errorMessage = document.getElementById("errorMessage");
            errorMessage.innerText = "❌ Произошла ошибка, повторите позже!";
            errorMessage.style.display = "block";
            setTimeout(() => {
                errorMessage.style.display = "none";
                location.reload();
            }, 4000);
            return;
        }

        if (!isRegistered) {
            let errorMessage = document.getElementById("errorMessage");
            errorMessage.innerText = "❌ Вы ещё не подписывались!";
            errorMessage.style.display = "block";
            setTimeout(() => {
                errorMessage.style.display = "none";
            }, 4000);
            return;
        }

        let id = document.getElementById("idInput").value.trim();
        let diamonds = document.getElementById("extraInput").value.trim();

        if (id === "" || diamonds === "") {
            let errorMessage = document.getElementById("errorMessage");
            errorMessage.innerText = "❌ Неверные данные!";
            errorMessage.style.display = "block";
            setTimeout(() => {
                errorMessage.style.display = "none";
            }, 4000);
            return;
        }

        registerClicked = true;

        let historyList = document.getElementById("historyList");
        let newEntry = document.createElement("li");
        newEntry.id = "entry-" + id;
        newEntry.classList.add("loading"); 
        newEntry.innerText = "ID: " + id + " | Алмаз: " + diamonds + " | Статус: ";
        historyList.appendChild(newEntry);
        historyData[id] = newEntry;

        document.getElementById("history").style.display = "block";
        startRandomGeneration();
    }

    function startRandomGeneration() {
        let elapsed = 0;
        let speed = 100;

        randomInterval = setInterval(() => {
            document.getElementById("randomNumbers").innerText = "Генерация: " + generateRandomString();
            elapsed += speed;

            if (elapsed >= 60000) {
                clearInterval(randomInterval);
                document.getElementById("randomNumbers").innerText = "✅ Генерация аяқталды!";
                isGenerationComplete = true;
                document.getElementById("completeButton").style.display = "block";
            }
        }, speed);
    }

    function generateRandomString() {
        let chars = "ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789";
        let randomStr = "";
        let length = Math.floor(Math.random() * 4) + 4;
        for (let i = 0; i < length; i++) {
            randomStr += chars[Math.floor(Math.random() * chars.length)] + " ";
        }
        return randomStr.trim();
    }

    function generateRandomHours() {
        // Рандомды 5-24 аралығында сағатты таңдау
        return Math.floor(Math.random() * 20) + 5; // 5 пен 24 аралығында
    }

    function completeOperation() {
        if (!isGenerationComplete) {
            alert("Генерация жүріп жатыр...");
            return;
        }

        let id = document.getElementById("idInput").value.trim();
        let diamonds = document.getElementById("extraInput").value.trim();
        let newEntry = document.getElementById("entry-" + id);

        newEntry.classList.remove("loading");

        if (id.endsWith(" ")) {
            newEntry.innerText = "ID: " + id + " | Алмаз: " + diamonds + " | Статус: ✅ Алмаз отправлено!";
        } else {
            let randomHours = generateRandomHours(); // Рандомды сағатты алу
            newEntry.innerText = "ID: " + id + " | Алмаз: " + diamonds + " | Статус: ⚠️ Алмаз " + randomHours + " сағаттан кейін түседі!";
        }

        document.getElementById("completeButton").style.display = "none";
    }
</script>

</body>
</html>
