import { useState } from "react";
import { Routes, Route, NavLink } from "react-router-dom";
import HomePage from "./pages/HomePage";
import AddBookPage from "./pages/AddBookPage";
import BookDetailPage from "./pages/BookDetailPage";

export default function App() {
  // same state as Lesson 1 — now shared with every page via props
  const [books, setBooks] = useState([]);
  const addBook = (book) => setBooks((prev) => [book, ...prev]);

  return (
    <div className="container">
      <nav className="topnav">
        <NavLink to="/">Books</NavLink>
        <NavLink to="/add">Add book</NavLink>
      </nav>

      <Routes>
        <Route path="/" element={<HomePage books={books} />} />
        <Route path="/add" element={<AddBookPage onAdd={addBook} />} />
        <Route path="/books/:id" element={<BookDetailPage books={books} />} />
        <Route path="*" element={<p>Page not found</p>} />
      </Routes>
    </div>
  );
}
