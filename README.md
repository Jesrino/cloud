import { useState, useMemo, useCallback } from "react";
import { useSelector, useDispatch } from "react-redux";
import { deleteBook } from "../store/booksSlice";

export default function HomePage() {
  const [query, setQuery] = useState("");
  const books = useSelector((state) => state.books);
  const dispatch = useDispatch();
  const visibleBooks = useMemo(() => {
    return books
      .filter((b) => b.title.toLowerCase().includes(query.toLowerCase()))
      .sort((a, b) => (a.year || 0) - (b.year || 0));
  }, [books, query]);

  const handleDelete = useCallback(
    (id) => dispatch(deleteBook(id)),
    [dispatch]
  );

  return (
    <>
      <input
        className="search"
        placeholder="Search title..."
        value={query}
        onChange={(e) => setQuery(e.target.value)}
      />
      <ul className="books">
        {visibleBooks.map((b) => (
          <li key={b.id} onClick={() => handleDelete(b.id)}>
            {b.title} {b.year ? `(${b.year})` : ""}
          </li>
        ))}
      </ul>
    </>
  );
}
