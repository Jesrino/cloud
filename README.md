import { useState, useEffect } from "react";
import * as api from "../api/books";

export function useBooks() {
  const [books, setBooks]     = useState([]);
  const [loading, setLoading] = useState(true);
  const [error, setError]     = useState(null);

  // GET the whole list once when the hook is first used
  useEffect(() => {
    api.getBooks()
      .then(setBooks)
      .catch(() => setError("Could not reach the server"))
      .finally(() => setLoading(false));
  }, []);

  // POST — add, then prepend the saved book to state
  const addBook = async (form) => {
    const saved = await api.createBook(form);
    setBooks((prev) => [saved, ...prev]);
  };

  // DELETE — remove, then drop it from state
  const removeBook = async (id) => {
    await api.deleteBook(id);
    setBooks((prev) => prev.filter((book) => book.id !== id));
  };

  // PUT — update, then swap the changed book in state
  const updateBook = async (id, changes) => {
    const updated = await api.updateBook(id, changes);
    setBooks((prev) => prev.map((book) => (book.id === id ? updated : book)));
  };

  return { books, loading, error, addBook, removeBook, updateBook };
}
