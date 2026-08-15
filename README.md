import { useState } from "react";
import { runAgent } from "../ai/agent";

export default function Assistant({ books, onAddBook }) {
  const [input, setInput] = useState("");
  const [reply, setReply] = useState("");
  const [busy, setBusy] = useState(false);

  async function send() {
    setBusy(true);
    const actions = {
      addBook:   (book) => onAddBook(book),   // POST via useBooks → list updates
      listBooks: ()     => books,             // current list from the hook
    };
    setReply(await runAgent(input, actions));
    setInput("");
    setBusy(false);
  }

  return (
    <div className="card assistant">
      <h2>AI assistant (Gemini)</h2>
      <div className="row">
        <input
          value={input}
          placeholder="e.g. Add Dune by Frank Herbert, 1965"
          onChange={(e) => setInput(e.target.value)}
        />
        <button onClick={send} disabled={busy}>
          {busy ? "Thinking…" : "Send"}
        </button>
      </div>
      {reply && <p className="reply">{reply}</p>}
    </div>
  );
}
