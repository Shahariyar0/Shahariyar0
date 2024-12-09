from flask import Flask, request, jsonify
import openai

app = Flask(__name__)
openai.api_key = 'your-api-key-here'

@app.route('/generate-image', methods=['POST'])
def generate_image():
    prompt = request.json.get('prompt')
    response = openai.Image.create(prompt=prompt, n=1, size="1024x1024")
    return jsonify({'imageUrl': response['data'][0]['url']})

if __name__ == '__main__':
    app.run(debug=True)
import React, { useState } from 'react';

function App() {
  const [prompt, setPrompt] = useState('');
  const [imageUrl, setImageUrl] = useState('');

  const generateImage = async () => {
    const response = await fetch('/generate-image', { 
      method: 'POST', 
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ prompt })
    });
    const data = await response.json();
    setImageUrl(data.imageUrl);
  };

  return (
    <div>
      <input onChange={(e) => setPrompt(e.target.value)} />
      <button onClick={generateImage}>Generate</button>
      {imageUrl && <img src={imageUrl} alt="Generated" />}
    </div>
  );
}

export default App;

