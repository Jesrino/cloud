import { useReducer, useEffect } from "react"
import BookForm from "./components/BookForm";
import BookList from "./components/BookList";
import "./App.css";
import { Routes, Route, NavLink } from "react-router-dom";
import HomePage from "./pages/HomePage";
import AddBookPage from "./pages/AddBookPage";
import BookDetailPage from "./pages/BookDetailPage";
import { booksReducer } from "./reducers/booksReducer";
import { useTheme } from "./context/ThemeContext";

export default function App() {
  const addBook    = (book) => dispatch({ type: "add", book });
  const deleteBook = (id)   => dispatch({ type: "delete", id });
  const [books, dispatch] = useReducer(booksReducer, []);
  const { theme, toggle } = useTheme();

  // load saved books once on mount → dispatch "load" (was setBooks before)
  useEffect(() => {
    const saved = localStorage.getItem("books");
    if (saved) dispatch({ type: "load", books: JSON.parse(saved) });
  }, []);

  // save whenever the list changes (unchanged from the useEffect step)
  useEffect(() => {
    localStorage.setItem("books", JSON.stringify(books));
  }, [books]);

  return (
    <div className="app container" data-theme={theme}>
      <div className="toolbar">
        <nav className="topnav">
          <NavLink to="/">Books</NavLink>
          <NavLink to="/add">Add book</NavLink>
        </nav>
        <button className="theme-btn" onClick={toggle}>
          {theme === "dark" ? "☀ Light" : "🌙 Dark"}
        </button>
      </div>

      <Routes>
        <Route path="/" element={<HomePage />} />
        <Route path="/add" element={<AddBookPage />} />
        <Route path="/books/:id" element={<BookDetailPage />} />
        <Route path="*" element={<p>Page not found</p>} />
      </Routes>
    </div>
  );
}
