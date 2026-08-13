import { useTheme } from "./context/ThemeContext.jsx";
import { Routes, Route, NavLink } from "react-router-dom";
import HomePage from "./pages/HomePage";
import AddBookPage from "./pages/AddBookPage";
import BookDetailPage from "./pages/BookDetailPage";

export default function App() {
  const { theme, toggle } = useTheme();

  return (
    <div className="app container" data-theme={theme}>
      <div className="toolbar">
        <nav className="topnav">
          <NavLink to="/">Books</NavLink>
          <NavLink to="/add">Add book</NavLink>
        </nav>
        <button className="theme-btn" onClick={toggle}>
          {theme === "dark" ? " Light" : " Dark"}
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
