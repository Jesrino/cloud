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
