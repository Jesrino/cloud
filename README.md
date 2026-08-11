<nav className="flex gap-1 p-1.5 mb-7 rounded-xl border border-zinc-800 bg-zinc-900">
  <NavLink
    to="/"
    className={({ isActive }) =>
      "px-4 py-2 rounded-lg text-sm font-medium transition " +
      (isActive
        ? "bg-indigo-500 text-white shadow-lg shadow-indigo-500/30"
        : "text-zinc-400 hover:text-zinc-100 hover:bg-white/5")
    }
  >
    Books
  </NavLink>
  {/* repeat for the "Add book" link */}
</nav>
