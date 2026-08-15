export const tools = [
  {
    functionDeclarations: [
      {
        name: "add_book",
        description: "Add a book to the user's list.",
        parameters: {
          type: "object",
          properties: {
            author:      { type: "string" },
            title:       { type: "string" },
            description: { type: "string" },
            year:        { type: "integer" },
          },
          required: ["author", "title", "year"],
        },
      },
      {
        name: "list_books",
        description: "Return all books currently in the list.",
        parameters: { type: "object", properties: {} },
      },
    ],
  },
];
