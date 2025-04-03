<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Speech-to-Text & Text-to-Speech</title>
</head>
<body>
    <h1>Speech-to-Text & Text-to-Speech Demo</h1>

    <textarea id="text-area" rows="4" cols="50" placeholder="Enter text or speak to convert speech to text..."></textarea><br><br>

    <button id="start-listening">Start Listening</button>
    <button id="stop-listening">Stop Listening</button>
    <button id="speak-text">Speak Text</button>

    <script>
        // Speech-to-Text (Speech Recognition)
        window.SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
        
        if ('SpeechRecognition' in window) {
            const recognition = new SpeechRecognition();
            recognition.continuous = true; // Keep listening until stopped
            recognition.interimResults = false; // Get final results only
            recognition.lang = 'en-US'; // Set language

            // Start speech recognition
            document.getElementById('start-listening').addEventListener('click', () => {
                recognition.start();
                console.log("Listening...");
            });

            // Stop speech recognition
            document.getElementById('stop-listening').addEventListener('click', () => {
                recognition.stop();
                console.log("Stopped Listening.");
            });

            // Capture speech and update the textarea
            recognition.onresult = (event) => {
                let transcript = '';
                for (let i = event.resultIndex; i < event.results.length; i++) {
                    transcript += event.results[i][0].transcript;
                }
                document.getElementById('text-area').value = transcript;
            };

            recognition.onerror = (event) => {
                console.error("Speech Recognition Error:", event.error);
            };
        } else {
            alert("Sorry, your browser does not support Speech-to-Text.");
        }

        // Text-to-Speech (Speech Synthesis)
        function speakText() {
            let text = document.getElementById('text-area').value;
            if (text === '') {
                alert('Please enter some text to speak.');
                return;
            }
            let speech = new SpeechSynthesisUtterance(text);
            speech.lang = 'en-US'; // Set language
            speech.pitch = 1; // Adjust pitch
            speech.rate = 1;  // Adjust speed
            window.speechSynthesis.speak(speech);
        }

        document.getElementById('speak-text').addEventListener('click', speakText);
    </script>
</body>
</html>
