import { Link } from "react-router-dom";

// books arrive as a prop from App (Lesson 3 will switch this to Redux)
export default function HomePage({ books }) {
  return (
    <ul className="books">
      {books.map((b) => (
        <li key={b.id}>
          <Link to={`/books/${b.id}`}>
            <strong>{b.title}</strong> ({b.year})
          </Link>
        </li>
      ))}import { useParams, Link } from "react-router-dom";

export default function BookDetailPage({ books }) {
  const { id } = useParams();                 // reads :id from the URL
  const book = books.find((b) => b.id === id);

  if (!book) return <p>Book not found. <Link to="/">Go back</Link></p>;

  return (
    <article className="card">
      <h1>{book.title}</h1>
      <p>by {book.author} · {book.year}</p>
      <p>{book.description}</p>
      <Link to="/">← Back to list</Link>
    </article>
  );
}
    </ul>
  );
}
