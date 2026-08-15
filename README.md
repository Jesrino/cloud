.assistant .row { display: flex; gap: .5rem; }
.assistant .row input { flex: 1; }
.assistant .row button { width: auto; margin: 0; padding: .6rem 1.1rem; white-space: nowrap; }
.assistant button:disabled { opacity: .55; cursor: default; }

.assistant .reply {
  margin-top: 1rem; padding: .85rem 1rem;
  background: var(--bg); border: 1px solid var(--line);
  border-radius: 10px; font-size: .9rem; line-height: 1.5;
}
.assistant .reply::before { content: "🤖 "; }
