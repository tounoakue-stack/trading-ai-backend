import express from "express";
import cors from "cors";
import dotenv from "dotenv";
import OpenAI from "openai";

dotenv.config();

const app = express();
app.use(cors());
app.use(express.json());

const openai = new OpenAI({
  apiKey: process.env.OPENAI_API_KEY
});

app.post("/api/coach", async (req, res) => {
  try {
    const completion = await openai.chat.completions.create({
      model: "gpt-4o-mini",
      messages: [
        {
          role: "system",
          content:
            "Tu es un formateur trading pédagogique. Tu expliques simplement, en français clair, sans jargon, sans promesse d’argent."
        },
        { role: "user", content: req.body.question }
      ]
    });

    res.json({ answer: completion.choices[0].message.content });
  } catch {
    res.status(500).json({ error: "Erreur IA" });
  }
});

app.listen(3000, () => {
  console.log("IA prête");
});
