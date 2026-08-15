export default function App() {
  const { theme, toggle } = useTheme();
  const { books, loading, error, addBook, removeBook, updateBook } = useBooks();

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
        <Route path="/" element={
          <HomePage books={books} loading={loading} error={error} onDelete={removeBook} />
        } />
        <Route path="/add" element={<AddBookPage onAdd={addBook} />} />
        <Route path="/books/:id" element={<BookDetailPage books={books} />} />
        <Route path="/books/:id/edit" element={<EditBookPage onUpdate={updateBook} />} />
      </Routes>
    </div>
  );
}
