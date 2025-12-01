# Ex03 To-Do List using JavaScript
## NAME : SHRI SAI ARAVIND R
## REG NO : 212223040197

## AIM
To create a To-do Application with all features using JavaScript.

## ALGORITHM
### STEP 1
Build the HTML structure (index.html).

### STEP 2
Style the App (style.css).

### STEP 3
Plan the features the To-Do App should have.

### STEP 4
Create a To-do application using Javascript.

### STEP 5
Add functionalities.

### STEP 6
Test the App.

### STEP 7
Open the HTML file in a browser to check layout and functionality.

### STEP 8
Fix styling issues and refine content placement.

### STEP 9
Deploy the website.

### STEP 10
Upload to GitHub Pages for free hosting.

## PROGRAM

```
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Ex03 – To‑Do List (Vanilla JavaScript)</title>
  <style>
    :root {
      --bg: #0f172a;          /* slate-900 */
      --panel: #111827;       /* gray-900 */
      --muted: #94a3b8;       /* slate-400 */
      --text: #e5e7eb;        /* gray-200 */
      --accent: #22d3ee;      /* cyan-400 */
      --accent-2: #38bdf8;    /* sky-400 */
      --danger: #f87171;      /* red-400 */
      --ok: #34d399;          /* emerald-400 */
      --shadow: 0 8px 30px rgba(0,0,0,.35);
      --radius: 16px;
    }
    * { box-sizing: border-box; }
    body {
      margin: 0; font-family: ui-sans-serif, system-ui, -apple-system, Segoe UI, Roboto, Ubuntu, Cantarell, Noto Sans, "Helvetica Neue", Arial, "Apple Color Emoji", "Segoe UI Emoji";
      background: radial-gradient(1200px 800px at 85% -10%, rgba(34,211,238,.08), transparent),
                  radial-gradient(1000px 700px at -10% 10%, rgba(56,189,248,.08), transparent), var(--bg);
      color: var(--text);
      min-height: 100svh;
      display: grid; place-items: center;
      padding: 32px;
    }
    .app {
      width: 100%; max-width: 720px; background: linear-gradient(180deg, rgba(255,255,255,.03), rgba(255,255,255,.01));
      border: 1px solid rgba(255,255,255,.08); border-radius: var(--radius); box-shadow: var(--shadow);
      overflow: hidden;
    }
    header { padding: 24px 24px 12px; }
    h1 { margin: 0; font-size: clamp(22px, 3.2vw, 32px); letter-spacing: .5px; }
    p.lead { margin: 6px 0 0; color: var(--muted); font-size: 14px; }

    .composer { display: grid; grid-template-columns: 1fr auto; gap: 10px; padding: 16px 24px 20px; }
    .composer input[type="text"] {
      padding: 14px 16px; border-radius: 12px; border: 1px solid rgba(226,232,240,.15);
      background: rgba(17,24,39,.6); color: var(--text); outline: none; font-size: 16px;
    }
    .composer input[type="text"]::placeholder { color: #8191a9; }
    .btn {
      border: none; border-radius: 12px; padding: 12px 16px; cursor: pointer; font-weight: 600;
      background: linear-gradient(180deg, var(--accent), var(--accent-2)); color: #06213a; box-shadow: 0 6px 18px rgba(45,212,191,.25);
    }
    .btn:disabled { filter: grayscale(.8) brightness(.75); cursor: not-allowed; }

    .toolbar { display: flex; flex-wrap: wrap; gap: 10px; align-items: center; padding: 12px 24px; border-top: 1px solid rgba(255,255,255,.06); }
    .filter { display: inline-flex; gap: 8px; background: rgba(255,255,255,.04); border: 1px solid rgba(255,255,255,.08); border-radius: 999px; padding: 4px; }
    .filter button { background: transparent; border: none; color: var(--muted); padding: 8px 12px; border-radius: 999px; cursor: pointer; }
    .filter button.active { background: rgba(34,211,238,.15); color: white; }
    .spacer { flex: 1; }
    .muted { color: var(--muted); font-size: 14px; }
    .secondary { background: rgba(255,255,255,.06); color: var(--text); }

    ul.list { list-style: none; margin: 0; padding: 8px; }
    .item { display: grid; grid-template-columns: 28px 1fr auto; gap: 10px; align-items: center;
            background: rgba(255,255,255,.02); border: 1px solid rgba(255,255,255,.06); padding: 10px 12px; border-radius: 12px; margin: 8px 16px; }
    .item:hover { border-color: rgba(255,255,255,.12); }
    .item input[type="checkbox"] { width: 18px; height: 18px; }
    .title { display: flex; align-items: center; gap: 8px; min-height: 28px; }
    .title span { outline: none; }
    .title .done { text-decoration: line-through; color: #7a8aa7; }
    .actions { display: inline-flex; gap: 8px; }
    .icon-btn { border: none; background: transparent; color: var(--muted); cursor: pointer; padding: 6px 8px; border-radius: 8px; }
    .icon-btn:hover { background: rgba(255,255,255,.06); color: white; }
    .danger { color: var(--danger); }

    footer { padding: 8px 16px 16px; text-align: center; color: var(--muted); font-size: 12px; }
    kbd { padding: 2px 6px; border: 1px solid rgba(255,255,255,.2); border-bottom-width: 2px; border-radius: 6px; font-size: 11px; }

    @media (prefers-reduced-motion: no-preference) {
      .fade-in { animation: fade .25s ease-out; }
      @keyframes fade { from { opacity: 0; transform: translateY(4px);} to { opacity: 1; transform: none; } }
    }
  </style>
</head>
<body>
  <main class="app" aria-labelledby="title">
    <header>
      <h1 id="title">Ex03 – To‑Do List</h1>
      <p class="lead">Add tasks, mark them done, edit inline, filter, and save to your browser automatically.</p>
    </header>

    <section class="composer" aria-label="Add a new task">
      <input id="new-task" type="text" placeholder="What do you need to do? (Press Enter)" autocomplete="off" />
      <button id="add-btn" class="btn" aria-label="Add task">Add</button>
    </section>

    <section class="toolbar" aria-label="Task controls">
      <div class="filter" role="tablist" aria-label="Filter tasks">
        <button data-filter="all" class="active" role="tab" aria-selected="true">All</button>
        <button data-filter="active" role="tab" aria-selected="false">Active</button>
        <button data-filter="completed" role="tab" aria-selected="false">Completed</button>
      </div>
      <div class="spacer"></div>
      <span id="counter" class="muted" aria-live="polite">0 items</span>
      <button id="clear-completed" class="icon-btn" title="Clear completed" aria-label="Clear completed tasks">🧹</button>
    </section>

    <ul id="list" class="list" aria-label="To-do list"></ul>

    <footer>
      Tip: Press <kbd>Enter</kbd> to add, double‑click a task to edit, and press <kbd>Esc</kbd> to cancel editing.
    </footer>
  </main>

  <template id="item-template">
    <li class="item fade-in">
      <input type="checkbox" aria-label="Mark completed" />
      <div class="title">
        <span class="text" role="textbox" aria-label="Task text" tabindex="0"></span>
      </div>
      <div class="actions">
        <button class="icon-btn" data-action="edit" title="Edit" aria-label="Edit">✏️</button>
        <button class="icon-btn danger" data-action="delete" title="Delete" aria-label="Delete">🗑️</button>
      </div>
    </li>
  </template>

  <script>
    // ----- Model -----
    const STORAGE_KEY = 'ex03-todos';
    /** @type {{id:string,text:string,done:boolean}[]} */
    let todos = [];
    let currentFilter = 'all'; // 'all' | 'active' | 'completed'

    const el = {
      input: document.getElementById('new-task'),
      addBtn: document.getElementById('add-btn'),
      list: document.getElementById('list'),
      counter: document.getElementById('counter'),
      clearCompleted: document.getElementById('clear-completed'),
      filterBtns: Array.from(document.querySelectorAll('.filter button')),
      tpl: document.getElementById('item-template')
    };

    // Utils
    const uid = () => (crypto && crypto.randomUUID ? crypto.randomUUID() : Date.now().toString(36) + Math.random().toString(36).slice(2));
    const save = () => localStorage.setItem(STORAGE_KEY, JSON.stringify(todos));
    const load = () => { try { return JSON.parse(localStorage.getItem(STORAGE_KEY)) || []; } catch { return []; } };

    function addTodo(text) {
      const clean = text.trim();
      if (!clean) return;
      todos.push({ id: uid(), text: clean, done: false });
      save();
      render();
      el.input.value = '';
      el.input.focus();
    }

    function toggle(id) {
      const t = todos.find(t => t.id === id);
      if (t) { t.done = !t.done; save(); render(); }
    }

    function updateText(id, newText) {
      const t = todos.find(t => t.id === id);
      if (!t) return;
      const clean = newText.trim();
      if (!clean) { remove(id); return; }
      t.text = clean; save(); render();
    }

    function remove(id) {
      todos = todos.filter(t => t.id !== id);
      save(); render();
    }

    function clearCompleted() {
      todos = todos.filter(t => !t.done);
      save(); render();
    }

    function setFilter(name) {
      currentFilter = name;
      el.filterBtns.forEach(b => b.classList.toggle('active', b.dataset.filter === name));
      render();
    }

    function getVisible() {
      if (currentFilter === 'active') return todos.filter(t => !t.done);
      if (currentFilter === 'completed') return todos.filter(t => t.done);
      return todos;
    }

    // Rendering
    function render() {
      el.list.innerHTML = '';
      const visible = getVisible();
      const frag = document.createDocumentFragment();

      visible.forEach(t => {
        const node = el.tpl.content.firstElementChild.cloneNode(true);
        node.dataset.id = t.id;
        const checkbox = node.querySelector('input[type="checkbox"]');
        const span = node.querySelector('.text');

        checkbox.checked = t.done;
        span.textContent = t.text;
        span.classList.toggle('done', t.done);

        // Toggle done
        checkbox.addEventListener('change', () => toggle(t.id));

        // Edit flow
        node.querySelector('[data-action="edit"]').addEventListener('click', () => beginEdit(span, t.id));
        span.addEventListener('dblclick', () => beginEdit(span, t.id));

        // Delete
        node.querySelector('[data-action="delete"]').addEventListener('click', () => remove(t.id));

        frag.appendChild(node);
      });

      el.list.appendChild(frag);

      const remaining = todos.filter(t => !t.done).length;
      el.counter.textContent = `${remaining} item${remaining === 1 ? '' : 's'} left`;
    }

    function beginEdit(span, id) {
      if (span.isContentEditable) return;
      const original = span.textContent;
      span.contentEditable = 'true';
      span.focus();
      placeCaretAtEnd(span);

      function finish(commit) {
        span.contentEditable = 'false';
        span.removeEventListener('keydown', onKey);
        span.removeEventListener('blur', onBlur);
        if (commit) updateText(id, span.textContent); else { span.textContent = original; }
      }

      function onKey(e) {
        if (e.key === 'Enter') { e.preventDefault(); finish(true); }
        else if (e.key === 'Escape') { e.preventDefault(); finish(false); }
      }
      function onBlur() { finish(true); }

      span.addEventListener('keydown', onKey);
      span.addEventListener('blur', onBlur);
    }

    function placeCaretAtEnd(el) {
      const range = document.createRange();
      const sel = window.getSelection();
      range.selectNodeContents(el);
      range.collapse(false);
      sel.removeAllRanges();
      sel.addRange(range);
    }

    // Events
    el.addBtn.addEventListener('click', () => addTodo(el.input.value));
    el.input.addEventListener('keydown', (e) => {
      if (e.key === 'Enter') addTodo(el.input.value);
    });
    el.clearCompleted.addEventListener('click', clearCompleted);
    el.filterBtns.forEach(b => b.addEventListener('click', () => setFilter(b.dataset.filter)));

    // Init
    todos = load();
    render();
  </script>
</body>
</html>

```



## OUTPUT

<img width="485" height="341" alt="web" src="https://github.com/user-attachments/assets/6d01b600-470a-48c2-bfd1-60ce5db9a89f" />



## RESULT
The program for creating To-do list using JavaScript is executed successfully.
