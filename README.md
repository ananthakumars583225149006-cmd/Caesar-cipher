# Caesar-cipher
 <!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Caesar Cipher Tool</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            background: linear-gradient(145deg, #0b0e14, #1a1f2a);
            padding: 20px;
        }

        .container {
            background: #232834;
            padding: 40px 35px;
            border-radius: 24px;
            box-shadow: 0 20px 40px rgba(0, 0, 0, 0.8);
            max-width: 520px;
            width: 100%;
            border: 1px solid #3b4357;
        }

        h1 {
            text-align: center;
            color: #e8edf5;
            font-weight: 600;
            letter-spacing: 1px;
            margin-bottom: 8px;
            font-size: 28px;
        }

        .subtitle {
            text-align: center;
            color: #8794b0;
            font-size: 14px;
            margin-bottom: 30px;
            border-bottom: 1px dashed #2f384a;
            padding-bottom: 15px;
        }

        label {
            color: #bcc9e3;
            font-weight: 500;
            font-size: 14px;
            display: block;
            margin-bottom: 6px;
        }

        textarea,
        input[type="number"] {
            width: 100%;
            padding: 12px 16px;
            border-radius: 12px;
            border: 1px solid #2f384a;
            background: #141a24;
            color: #eef3fc;
            font-size: 16px;
            outline: none;
            transition: 0.2s;
            resize: vertical;
        }

        textarea:focus,
        input:focus {
            border-color: #6c8cff;
            box-shadow: 0 0 0 3px rgba(108, 140, 255, 0.2);
        }

        textarea {
            min-height: 100px;
            font-family: 'Courier New', monospace;
        }

        .input-group {
            margin-top: 18px;
        }

        .row {
            display: flex;
            gap: 15px;
            align-items: center;
            flex-wrap: wrap;
        }

        .shift-box {
            flex: 1;
            min-width: 100px;
        }

        .shift-box input {
            width: 100%;
        }

        .btn-group {
            display: flex;
            gap: 12px;
            margin-top: 22px;
        }

        button {
            flex: 1;
            padding: 13px 0;
            font-weight: 600;
            font-size: 16px;
            border: none;
            border-radius: 14px;
            cursor: pointer;
            transition: 0.25s;
            letter-spacing: 0.5px;
        }

        .btn-encrypt {
            background: #2a6df4;
            color: white;
        }

        .btn-encrypt:hover {
            background: #1e5ad6;
            transform: scale(1.02);
        }

        .btn-decrypt {
            background: #9b6bff;
            color: white;
        }

        .btn-decrypt:hover {
            background: #835be6;
            transform: scale(1.02);
        }

        .output-box {
            margin-top: 25px;
            background: #141a24;
            border-radius: 14px;
            padding: 18px 20px;
            border-left: 4px solid #6c8cff;
        }

        .output-label {
            color: #8794b0;
            font-size: 13px;
            text-transform: uppercase;
            letter-spacing: 1px;
            margin-bottom: 6px;
        }

        #result {
            color: #eef3fc;
            font-size: 18px;
            word-break: break-all;
            font-family: 'Courier New', monospace;
            min-height: 30px;
        }

        .copy-btn {
            margin-top: 10px;
            background: #2f3a52;
            color: #bcc9e3;
            padding: 8px 16px;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            font-size: 13px;
            transition: 0.2s;
        }

        .copy-btn:hover {
            background: #3c4a66;
            color: white;
        }

        .footer-note {
            margin-top: 18px;
            color: #4b5670;
            font-size: 12px;
            text-align: center;
        }
    </style>
</head>
<body>

    <div class="container">
        <h1>🔐 Caesar Cipher</h1>
        <div class="subtitle">Encrypt / Decrypt with a shift</div>

        <label for="message">📝 Message</label>
        <textarea id="message" placeholder="Type your secret message...">Hello World</textarea>

        <div class="input-group">
            <div class="row">
                <div class="shift-box">
                    <label for="shift">🔢 Shift (1–25)</label>
                    <input type="number" id="shift" value="3" min="1" max="25" />
                </div>
            </div>
        </div>

        <div class="btn-group">
            <button class="btn-encrypt" onclick="processCipher('encrypt')">🔒 Encrypt</button>
            <button class="btn-decrypt" onclick="processCipher('decrypt')">🔓 Decrypt</button>
        </div>

        <div class="output-box">
            <div class="output-label">📋 Result</div>
            <div id="result">Ready</div>
            <button class="copy-btn" onclick="copyResult()">📋 Copy Result</button>
        </div>
        <div class="footer-note">⚡ Only letters shift · symbols & numbers stay unchanged</div>
    </div>

    <script>
        // ============================================
        // CAESAR CIPHER - CORE LOGIC
        // ============================================

        function caesarCipher(text, shift, mode) {
            // If decrypting, reverse the shift
            if (mode === 'decrypt') {
                shift = -shift;
            }

            let result = '';

            for (let i = 0; i < text.length; i++) {
                let char = text[i];

                // Check if character is a letter
                if (char >= 'A' && char <= 'Z') {
                    // Uppercase letter
                    let code = char.charCodeAt(0);
                    let newCode = ((code - 65 + shift) % 26 + 26) % 26 + 65;
                    result += String.fromCharCode(newCode);
                } else if (char >= 'a' && char <= 'z') {
                    // Lowercase letter
                    let code = char.charCodeAt(0);
                    let newCode = ((code - 97 + shift) % 26 + 26) % 26 + 97;
                    result += String.fromCharCode(newCode);
                } else {
                    // Non-letter character (keep as is)
                    result += char;
                }
            }

            return result;
        }

        // ============================================
        // UI CONTROLLER
        // ============================================

        function processCipher(mode) {
            // Get values from UI
            var message = document.getElementById('message').value;
            var shiftInput = document.getElementById('shift').value;
            var resultDiv = document.getElementById('result');

            // Validate: message not empty
            if (message.trim() === '') {
                resultDiv.textContent = '⚠️ Please enter a message.';
                resultDiv.style.color = '#ff8a8a';
                return;
            }

            // Validate: shift is a number between 1-25
            var shift = parseInt(shiftInput);
            if (isNaN(shift) || shift < 1 || shift > 25) {
                resultDiv.textContent = '⚠️ Shift must be between 1 and 25.';
                resultDiv.style.color = '#ff8a8a';
                return;
            }

            // Process the cipher
            var output = caesarCipher(message, shift, mode);

            // Display result
            resultDiv.textContent = output;
            resultDiv.style.color = '#eef3fc';

            // Reset copy button
            var copyBtn = document.querySelector('.copy-btn');
            copyBtn.textContent = '📋 Copy Result';
            copyBtn.style.background = '#2f3a52';

            // Log for debugging (check browser console)
            console.log('Mode: ' + mode + ', Shift: ' + shift + ', Input: "' + message + '", Output: "' + output + '"');
        }

        // ============================================
        // COPY TO CLIPBOARD
        // ============================================

        function copyResult() {
            var resultText = document.getElementById('result').textContent;
            var copyBtn = document.querySelector('.copy-btn');

            // Don't copy if it's an error message or placeholder
            if (resultText === 'Ready' || resultText.indexOf('⚠️') !== -1) {
                return;
            }

            // Try modern clipboard API
            if (navigator.clipboard && navigator.clipboard.writeText) {
                navigator.clipboard.writeText(resultText).then(function() {
                    copyBtn.textContent = '✅ Copied!';
                    copyBtn.style.background = '#2a6df4';
                    setTimeout(function() {
                        copyBtn.textContent = '📋 Copy Result';
                        copyBtn.style.background = '#2f3a52';
                    }, 2000);
                }).catch(function() {
                    // Fallback for older browsers
                    fallbackCopy(resultText, copyBtn);
                });
            } else {
                // Fallback for older browsers
                fallbackCopy(resultText, copyBtn);
            }
        }

        function fallbackCopy(text, copyBtn) {
            var textarea = document.createElement('textarea');
            textarea.value = text;
            textarea.style.position = 'fixed';
            textarea.style.opacity = '0';
            document.body.appendChild(textarea);
            textarea.select();
            document.execCommand('copy');
            document.body.removeChild(textarea);

            copyBtn.textContent = '✅ Copied!';
            copyBtn.style.background = '#2a6df4';
            setTimeout(function() {
                copyBtn.textContent = '📋 Copy Result';
                copyBtn.style.background = '#2f3a52';
            }, 2000);
        }

        // ============================================
        // KEYBOARD SHORTCUT: Ctrl+Enter = Encrypt
        // ============================================

        document.getElementById('message').addEventListener('keydown', function(e) {
            if (e.key === 'Enter' && (e.ctrlKey || e.metaKey)) {
                e.preventDefault();
                processCipher('encrypt');
            }
        });

        // ============================================
        // AUTO-RUN ON ENTER IN SHIFT FIELD
        // ============================================

        document.getElementById('shift').addEventListener('keydown', function(e) {
            if (e.key === 'Enter') {
                e.preventDefault();
                var msg = document.getElementById('message').value;
                if (msg.trim() !== '') {
                    processCipher('encrypt');
                }
            }
        });

        console.log('✅ Caesar Cipher Tool Loaded Successfully!');
        console.log('💡 Tip: Type a message, set shift, click Encrypt/Decrypt');
        console.log('⌨️  Shortcut: Ctrl+Enter to Encrypt');
    </script>

</body>
</html>
