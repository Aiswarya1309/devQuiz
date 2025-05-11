devquiz/
├── client/                # React Frontend
├── server/                # Flask Backend
├── README.md              # Documentation


---

Backend (Flask) Highlights

# server/app.py
from flask import Flask, request, jsonify
import openai
from flask_cors import CORS

app = Flask(_name_)
CORS(app)

openai.api_key = "YOUR_OPENAI_API_KEY"

@app.route("/generate-questions", methods=["POST"])
def generate_questions():
    tech_stack = request.json.get("techStack")
    prompt = f"Generate 5 coding and 5 HR interview questions for a developer skilled in {tech_stack}."
    response = openai.ChatCompletion.create(
        model="gpt-4",
        messages=[{"role": "user", "content": prompt}]
    )
    return jsonify({"questions": response['choices'][0]['message']['content']})

if _name_ == "_main_":
    app.run(debug=True)


---

Frontend (React) Highlights

// client/src/App.jsx
import React, { useState } from "react";
import axios from "axios";

function App() {
  const [techStack, setTechStack] = useState("");
  const [questions, setQuestions] = useState("");

  const handleGenerate = async () => {
    const res = await axios.post("http://localhost:5000/generate-questions", {
      techStack,
    });
    setQuestions(res.data.questions);
  };

  return (
    <div className="App">
      <h1>DevQuiz - AI Interview Prep</h1>
      <input
        type="text"
        value={techStack}
        onChange={(e) => setTechStack(e.target.value)}
        placeholder="Enter your tech stack..."
      />
      <button onClick={handleGenerate}>Generate Questions</button>
      <pre>{questions}</pre>
    </div>
  );
}

