import express from "express";
const app = express();
app.use(express.json());

const MODEL = "gemini-2.5-flash";   // newer models may exist — see note below
const URL = `https://generativelanguage.googleapis.com/v1beta/models/${MODEL}:generateContent`;

app.post("/ai", async (req, res) => {
  const r = await fetch(URL, {
    method: "POST",
    headers: {
      "content-type": "application/json",
      "x-goog-api-key": process.env.GEMINI_API_KEY,   // stays on the server
    },
    body: JSON.stringify(req.body),                    // { contents, tools }
  });
  res.json(await r.json());
});

app.listen(8787, () => console.log("Gemini proxy on http://localhost:8787"));
