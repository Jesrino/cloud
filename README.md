import { useNavigate } from "react-router-dom";
import BookForm from "../components/BookForm";   // reused from Lesson 1

export default function AddBookPage({ onAdd }) {
  const navigate = useNavigate();

  function handleAdd(book) {
    onAdd(book);              // lift the new book up to App
    navigate("/");            // then go to the list
  }

  return <BookForm onAdd={handleAdd} />;
}
