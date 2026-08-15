import { tools } from "./tools";

// map a function name to a real action passed in from the component
async function runTool(name, args, actions) {
  if (name === "add_book")   return actions.addBook(args);
  if (name === "list_books") return actions.listBooks();
  return { error: "unknown tool" };
}

export async function runAgent(userText, actions) {
  const contents = [{ role: "user", parts: [{ text: userText }] }];

  for (let step = 0; step < 5; step++) {            // safety cap on loops
    const data = await fetch("/ai", {               // → Vite proxy → your server
      method: "POST",
      headers: { "content-type": "application/json" },
      body: JSON.stringify({ contents, tools }),
    }).then((r) => r.json());

    const parts = data.candidates?.[0]?.content?.parts ?? [];
    contents.push({ role: "model", parts });        // remember Gemini's turn

    const calls = parts.filter((p) => p.functionCall);
    if (calls.length === 0) {                        // no calls → final answer
      const textPart = parts.find((p) => p.text);
      return textPart ? textPart.text : "Done.";
    }

    const responseParts = [];
    for (const { functionCall } of calls) {
      const result = await runTool(functionCall.name, functionCall.args, actions);
      responseParts.push({
        functionResponse: { name: functionCall.name, response: { result } },
      });
    }
    contents.push({ role: "user", parts: responseParts });   // feed results back
  }
  return "Stopped after too many steps.";
}
