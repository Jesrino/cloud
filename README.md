
export default function HomePage({ books, loading, error, onDelete }) {
  if (loading) return <div className="spinner" />;
  if (error)   return <p className="status error">{error}</p>;
  if (books.length === 0) return <p className="status">No books yet.</p>;

  return (
    <ul className="books">
      {books.map((book) => (
        <li key={book.id} className="book-row">
          <div><strong>{book.title}</strong> ({book.year})</div>
          <div className="row-actions">
            <Link to={`/books/${book.id}/edit`}>Edit</Link>
            <button className="delete" onClick={() => onDelete(book.id)}>Delete</button>
          </div>
        </li>
      ))}
    </ul>
  );
