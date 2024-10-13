<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ポーカースタック管理</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 1000px;
            margin: 0 auto;
            padding: 20px;
        }
        table {
            width: 100%;
            border-collapse: collapse;
            margin-bottom: 20px;
        }
        th, td {
            border: 1px solid #ddd;
            padding: 8px;
            text-align: left;
        }
        th {
            background-color: #f2f2f2;
        }
        input, textarea {
            width: 100%;
            padding: 5px;
            box-sizing: border-box;
        }
        button {
            background-color: #4CAF50;
            color: white;
            padding: 10px 15px;
            border: none;
            cursor: pointer;
            margin-top: 10px;
        }
        #timer {
            font-size: 24px;
            margin-top: 20px;
            text-align: center;
        }
        #timerControls {
            text-align: center;
            margin-top: 10px;
        }
        .disabled {
            opacity: 0.5;
            pointer-events: none;
        }
        #resetMessage {
            color: red;
            font-weight: bold;
            text-align: center;
            margin-top: 10px;
            padding: 10px;
            background-color: #ffe6e6;
            border: 1px solid red;
            display: none;
        }
    </style>
</head>
<body>
    <h1>ポーカースタック管理</h1>
    <div>
        <label for="bb">BB:</label>
        <input type="number" id="bb" placeholder="BBを入力">
    </div>
    <table>
        <thead>
            <tr>
                <th>シート</th>
                <th>スタック</th>
                <th>BB数</th>
                <th>メモ</th>
            </tr>
        </thead>
        <tbody id="stackTable">
            <!-- JavaScriptで動的に行を生成 -->
        </tbody>
    </table>
    <button onclick="calculateBB()">計算</button>

    <div id="timerSettings">
        <label for="timerMinutes">タイマー設定 (分):</label>
        <input type="number" id="timerMinutes" value="20" min="1">
        <button onclick="setTimer()">タイマーセット</button>
    </div>
    <div id="timer">20:00</div>
    <div id="timerControls">
        <button onclick="startTimer()">開始</button>
        <button onclick="pauseTimer()">一時停止</button>
        <button onclick="resetTimer()">リセット</button>
    </div>
    <div id="resetMessage"></div>

    <script>
        let stackInputs = [];
        
        // テーブルの行を動的に生成
        const tableBody = document.getElementById('stackTable');
        for (let i = 1; i <= 9; i++) {
            const row = tableBody.insertRow();
            row.innerHTML = `
                <td>シート${i}</td>
                <td><input type="number" id="stack${i}" placeholder="スタック額を入力"></td>
                <td id="bbCount${i}"></td>
                <td><textarea id="memo${i}" placeholder="メモを入力"></textarea></td>
            `;
            stackInputs.push(document.getElementById(`stack${i}`));
        }

        function calculateBB() {
            const bb = parseFloat(document.getElementById('bb').value);
            
            if (isNaN(bb) || bb === 0) {
                alert('有効なBB値を入力してください。');
                return;
            }
            
            for (let i = 1; i <= 9; i++) {
                const stack = parseFloat(document.getElementById(`stack${i}`).value);
                const bbCountElement = document.getElementById(`bbCount${i}`);
                
                if (isNaN(stack)) {
                    bbCountElement.textContent = '-';
                } else {
                    const bbCount = stack / bb;
                    bbCountElement.textContent = bbCount.toFixed(2);
                }
            }
        }

        // タイマー機能
        let timerInterval;
        let timeLeft = 20 * 60; // デフォルトは20分

        function updateTimerDisplay() {
            const minutes = Math.floor(timeLeft / 60);
            const seconds = timeLeft % 60;
            document.getElementById('timer').textContent = 
                `${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}`;
        }

        function setTimer() {
            const minutes = parseInt(document.getElementById('timerMinutes').value);
            if (isNaN(minutes) || minutes < 1) {
                alert('有効な分数を入力してください。');
                return;
            }
            timeLeft = minutes * 60;
            updateTimerDisplay();
        }

        function startTimer() {
            if (!timerInterval) {
                timerInterval = setInterval(() => {
                    if (timeLeft > 0) {
                        timeLeft--;
                        updateTimerDisplay();
                    } else {
                        clearInterval(timerInterval);
                        alert('時間切れです。BBを更新してください。');
                        disableStackInputs();
                    }
                }, 1000);
            }
        }

        function pauseTimer() {
            clearInterval(timerInterval);
            timerInterval = null;
        }

        function resetTimer() {
            clearInterval(timerInterval);
            timerInterval = null;
            setTimer(); // 現在設定されている時間でリセット
            enableStackInputs();
            showResetMessage();
        }

        function disableStackInputs() {
            stackInputs.forEach(input => {
                input.disabled = true;
                input.classList.add('disabled');
            });
        }

        function enableStackInputs() {
            stackInputs.forEach(input => {
                input.disabled = false;
                input.classList.remove('disabled');
            });
        }

        function showResetMessage() {
            const resetMessage = document.getElementById('resetMessage');
            resetMessage.textContent = 'タイマーがリセットされました。スタック入力が再び有効になりました。';
            resetMessage.style.display = 'block';
            setTimeout(() => {
                resetMessage.style.display = 'none';
            }, 5000); // 5秒後にメッセージを非表示
        }

        updateTimerDisplay();
    </script>
</body>
</html>
