from flask import Flask, request, jsonify, render_template_string

import openai

app = Flask(__name__)

# Replace 'YOUR_OPENAI_API_KEY' with your actual OpenAI API key
openai.api_key = '###'

html_template = """
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Jarvis </title>
</head>
<body>
    <h1>Voice AI Assistant</h1>

    <!-- Text input field for user queries -->
    <input type="text" id="user-input" placeholder="Ask me a question...">

    <!-- Button to submit the query -->
    <button id="submit-button">Ask</button>

    <!-- Display AI responses -->
    <div id="ai-response"></div>

    <script>
        document.addEventListener('DOMContentLoaded', function () {
            const userInput = document.getElementById('user-input');
            const submitButton = document.getElementById('submit-button');
            const aiResponse = document.getElementById('ai-response');

            submitButton.addEventListener('click', () => {
                const userQuestion = userInput.value;

                fetch('/ask', {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json'
                    },
                    body: JSON.stringify({ question: userQuestion })
                })
                .then(response => response.json())
                .then(data => {
                    // Display the AI's response
                    aiResponse.innerText = data.answer;

                    // Speak the AI's response
                    const synth = window.speechSynthesis;
                    const aiUtterance = new SpeechSynthesisUtterance(data.answer);
                    synth.speak(aiUtterance);
                })
                .catch(error => {
                    console.error('Error:', error);
                });
            });
        });
    </script>
</body>
</html>
"""

@app.route('/')
def home():
    return render_template_string(html_template)

@app.route('/ask', methods=['POST'])
def ask_ai():
    data = request.get_json()
    user_question = data.get('question').lower()  # Convert the input to lowercase for case-insensitive matching

    if "name" in user_question and "your" in user_question:
        ai_answer = "My name is Jarvis."
    elif "who created you" in user_question:
        ai_answer = "My creator is Sole."
    else:
        try:
            # Make a request to the OpenAI API to get the AI's response
            response = openai.Completion.create(
                engine="text-davinci-002",
                prompt=user_question,
                max_tokens=50,
                n=1,
                stop=None,
                temperature=0.7,
            )

            # Extract the AI's response from the API response
            ai_answer = response.choices[0].text.strip()
        except Exception as e:
            ai_answer = str(e)

    return jsonify({'answer': ai_answer})

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000, debug=True)
