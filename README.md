:root {
  --bg: #103d1a;  --card: #3a2600;  --line: #262b34;
  --ink: #8888ff; --muted: #9aa0a6; --accent: #c9b81e; --err: #ff6b6b;
}
* { box-sizing: border-box; }
body { margin: 0; background: var(--bg); color: var(--ink);
       font-family: system-ui, -apple-system, sans-serif; }

.container { max-width: 640px; margin: 0 auto; padding: 2rem 1.25rem; }
h1 { font-size: 1.8rem; margin-bottom: 1.5rem; }

.card { background: var(--card); border: 1px solid var(--line);
        border-radius: 14px; padding: 1.5rem; margin-bottom: 1.5rem; }
.card h2 { margin: 0 0 1rem; font-size: 1.2rem; }

label { display: block; font-size: .8rem; color: var(--muted);
        margin: .75rem 0 .35rem; }
input, textarea { width: 100%; padding: .6rem .75rem; background: var(--bg);
        border: 1px solid var(--line); border-radius: 8px; color: var(--ink); font: inherit; }
input:focus, textarea:focus { outline: none; border-color: var(--accent); }
textarea { min-height: 80px; resize: vertical; }

.err { display: block; color: var(--err); font-size: .78rem; margin-top: .3rem; }

button { margin-top: 1.25rem; width: 100%; padding: .7rem; background: var(--accent);
         color: #fff; border: none; border-radius: 8px; font: inherit; font-weight: 600; cursor: pointer; }
button:hover { filter: brightness(1.08); }

.books { list-style: none; padding: 0; display: grid; gap: .75rem; }
.books li { background: var(--card); border: 1px solid var(--line);
            border-radius: 10px; padding: 1rem; }
.books strong { font-size: 1.05rem; }
.books li > div { color: var(--muted); font-size: .85rem; margin: .25rem 0; }
.books p { margin: .5rem 0 0; font-size: .9rem; }

/* Top navigation — segmented pill bar */
.topnav {
  display: flex;
  gap: .35rem;
  padding: .4rem;
  margin-bottom: 1.75rem;
  background: var(--card);
  border: 1px solid var(--line);
  border-radius: 12px;
}
.topnav a {
  padding: .55rem 1.1rem;
  border-radius: 9px;
  color: var(--muted);
  text-decoration: none;
  font-weight: 500;
  font-size: .92rem;
  transition: color .18s ease, background .18s ease;
}
.topnav a:hover {
  color: var(--ink);
  background: color-mix(in srgb, var(--accent) 14%, transparent);
}
.topnav a.active {                  /* current route, set by React Router */
  color: #fff;
  background: var(--accent);
  box-shadow: 0 6px 16px -6px var(--accent);
}

/* light theme: override the variables from Lesson 1 */
.app[data-theme="light"] {
  --bg: #f7f8fa; --card: #ffffff; --line: #e3e6ea;
  --ink: #1b1d21; --muted: #5f6368;
}

.toolbar { display: flex; gap: .75rem; align-items: center; margin-bottom: 1rem; }
.toolbar .topnav { flex: 1; margin-bottom: 0; }   /* nav grows, toggle stays right */

.theme-btn {                                /* reset the full-width button look */
  width: auto; margin: 0; padding: .55rem .9rem;
  background: transparent; color: var(--ink); border: 1px solid var(--line);
}
.theme-btn:hover { border-color: var(--accent); filter: none; }
