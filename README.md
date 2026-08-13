export function booksReducer(state, action) {
  switch (action.type) {
    case "load":   return action.books; 
    case "add":    return [action.book, ...state];
    case "delete": return state.filter((b) => b.id !== action.id);
    case "clear":  return [];
    default:       return state;
  }
}
