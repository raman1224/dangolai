<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Dangol Chatbot</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/twitter-bootstrap/4.3.1/css/bootstrap.min.css" />
    <script src="https://cdn.jsdelivr.net/npm/marked/marked.min.js"></script>
    <style>
        .chatbot-container {
            text-align: left;
            background-color: rgb(232, 232, 236);
            height: 100%;
            width: 25%;
            color: black;
            margin-left: 6px;
            border-radius: 7px;
            margin-top: 15px;
            padding: 20px;
        }
        .chatbot-container button {
            height: 50px;
            border-radius: 6px;
            width: 180px;
            text-align: left;
            margin-top: 10px;
            background-color: rgb(209, 209, 222);
            color: black;
        }
.chatbot-container input {
    height: auto;
     width: auto; 
    text-align: left;
    border-radius: 5px;
}


        #response h2 {
            color: aqua;
            font-size: 20px;
        }
        #response ul {
            padding-right: 20px;
        }
        #response li {
            margin-bottom: 10px;
        }
    </style>
</head>
<body>
    <div class="chatbot-container">
        <h2>DANGOL CHATBOT</h2>
        <div class="chat">
            <input type="text" class="chatbot" id="userInput" placeholder="Enter your question" />
        </div>
        <button class="btn btn-success" onclick="sendMessage()">Ask!</button>
        <div id="response"></div>
    </div>

    <script>
        async function sendMessage() {
            const input = document.getElementById('userInput').value;
            const responseDiv = document.getElementById('response');

            if (!input) {
                responseDiv.innerHTML = 'Please enter a message.';
                return;
            }

            responseDiv.innerHTML = 'Loading...';

            try {
                const response = await fetch("https://openrouter.ai/api/v1/chat/completions", {
                    method: "POST",
                    headers: {
                        "Authorization": "Bearer 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",
                        "HTTP-Referer": "https://globalgkwww.blogspot.com/",
                        "X-Title": "Global GK",
                        "Content-Type": "application/json"
                    },
                    body: JSON.stringify({
                        "model": "deepseek/deepseek-r1:free",
                        "messages": [{ role: "user", content: input }],
                    })
                });

                const data = await response.json();
                console.log(data);

                const markdownText = data.choices?.[0]?.message?.content || 'No response received.';
                responseDiv.innerHTML = marked.parse(markdownText); // Use marked to parse Markdown
            } catch (error) {
                responseDiv.innerHTML = 'Error: ' + error.message;
            }
        }
    </script>
</body>
</html>
