import { useState, useEffect } from "react";
import { useParams, useNavigate } from "react-router-dom";
import { getBook } from "../api/books";

export default function EditBookPage({ onUpdate }) {
  const { id } = useParams();
  const navigate = useNavigate();
  const [form, setForm] = useState(null);

  // useEffect that GETs the single book, then fills the form
  useEffect(() => {
    getBook(id).then(setForm);          // GET /api/books/:id
  }, [id]);                             // re-run if the id changes

  if (!form) return <div className="spinner" />;   // still loading the book

  const change = (e) => setForm({ ...form, [e.target.name]: e.target.value });

  async function save() {
    await onUpdate(id, form);           // PUT via the useBooks hook
    navigate("/");                      // back to the list
  }

  return (
    <div className="card">
      <h2>Edit book</h2>
      <label>Title</label>
      <input name="title" value={form.title} onChange={change} />
      <label>Author</label>
      <input name="author" value={form.author} onChange={change} />
      <label>Year</label>
      <input name="year" type="number" value={form.year} onChange={change} />
      <button onClick={save}>Save changes</button>
    </div>
  );
}
