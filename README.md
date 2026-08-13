import { createSlice } from "@reduxjs/toolkit";

const booksSlice = createSlice({
  name: "books",
  initialState: [],
  reducers: {
    addBook:    (state, action) => { state.unshift(action.payload); },
    deleteBook: (state, action) => state.filter((b) => b.id !== action.payload),
  },
});

export const { addBook, deleteBook } = booksSlice.actions;
export default booksSlice.reducer;
