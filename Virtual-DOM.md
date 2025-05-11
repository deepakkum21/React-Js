# Virtual DOM

1. React checks for necessary DOM updates via a `“Virtual DOM”`
2. It `creates & compares virtual DOM snapshots to find out which parts of the rendered UI` need to be updated
3. Steps
   - Step 1: `Creating a Component Tree`
   - Step 2: `Creating a Virtual Snapshot of the Target HTML Code`
   - Step 3: `Compare New Virtual DOM Snapshot to Previous (Old) Virtual DOM Snapshot`
   - Step 4: `Identify & Apply Changes to the “Real DOM”`
