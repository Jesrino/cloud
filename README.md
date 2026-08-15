.spinner {
  width: 22px; height: 22px; margin: 1.5rem auto;
  border: 3px solid var(--line); border-top-color: var(--accent);
  border-radius: 50%; animation: spin .8s linear infinite;
}
@keyframes spin { to { transform: rotate(360deg); } }

.book-row { display: flex; justify-content: space-between; align-items: center; gap: 1rem; }
.row-actions { display: flex; gap: .6rem; align-items: center; }
.row-actions a { color: var(--accent); text-decoration: none; font-size: .85rem; }
.delete {
  width: auto; margin: 0; padding: .4rem .7rem; font-size: .8rem;
  background: transparent; color: var(--err); border: 1px solid var(--line);
}
.delete:hover { background: var(--err); color: #fff; filter: none; }
