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
      ))}
    </ul>
  );
}
