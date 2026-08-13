import { Link } from "react-router-dom";
import { useState, useMemo, useCallback } from "react";
import { useSelector, useDispatch } from "react-redux";
import { deleteBook } from "../store/booksSlice";

export default function HomePage() {
  const [query, setQuery] = useState("");

  // 1. Move Hooks INSIDE the component body
  const books = useSelector((state) => state.books);
  const dispatch = useDispatch();

  // 2. Filter & sort books safely
  const visibleBooks = useMemo(() => {
    if (!books) return [];
    return books
      .filter((b) => b.title.toLowerCase().includes(query.toLowerCase()))
      .sort((a, b) => a.year - b.year);
  }, [books, query]);

  // 3. Dispatch deletion inside an event handler
  const handleDelete = useCallback(
    (id) => {
      dispatch(deleteBook(id));
    },
    [dispatch]
  );

  return (
    <div>
      <input
        className="search"
        value={query}
        onChange={(e) => setQuery(e.target.value)}
        placeholder="Search books..."
      />

      <ul className="books">
        {visibleBooks.map((b) => (
          <li key={b.id} onClick={() => handleDelete(b.id)}>
            {b.title} ({b.year})
          </li>
        ))}
      </ul>
    </div>
  );
}
