import { useState, useRef, useEffect } from "react";

export default function BookForm({ onAdd }) {
  const [title, setTitle] = useState("");
  const [author, setAuthor] = useState("");
  const authorRef = useRef(null); 

  useEffect(() => {
    authorRef.current?.focus(); 
  }, []);

  const handleSubmit = (e) => {
    e.preventDefault();
    if (!title || !author) return;
    onAdd({ id: Date.now().toString(), title, author });
    setTitle("");
    setAuthor("");
  };

  return (
    <form onSubmit={handleSubmit} className="card">
      <div>
        <label>Author</label>
        <input
          ref={authorRef} // 2. attach the ref
          type="text"
          name="author"
          value={author}
          onChange={(e) => setAuthor(e.target.value)}
        />
      </div>

      <div>
        <label>Title</label>
        <input
          type="text"
          name="title"
          value={title}
          onChange={(e) => setTitle(e.target.value)}
        />
      </div>

      <button type="submit">Add Book</button>
    </form>
  );
}
