# docs/DESIGN.md template (UI projects)

```markdown
# <Project>: design system

The look and feel every screen follows. The owner's standard: <e.g. "professional and
beautiful">, in every task, not as a final polish pass.

## Principles
- <Personality in one line, e.g. "calm, paper-like, content first">.
- One accent colour, used sparingly; red only for destructive actions.
- Clear type hierarchy: <title > h1 > h2 > body sizes>.
- Light and dark from the OS setting, equal care for both.
- Works at phone width: <breakpoint and what collapses>.
- Accessible by default: visible focus, AA contrast, real buttons/links, reduced motion.

## Implementation
- <Approach, e.g. plain CSS with design tokens; no framework>. Tokens live in
  `<path>`; never hard-code a colour or size in a component.
- Icons: <inline SVG set / library>; font: <font>.

## Tokens (summary)
| Group | Tokens |
|---|---|
| Surfaces | … |
| Text | … |
| Accent / status | … |
| Shape, spacing, type, layout | … |

## Components
| Component | Classes / file | Use |
|---|---|---|

## Checking a UI change (part of every UI task's Verify)
1. `<shots>` captures every screen in light and dark, desktop and phone. Add new screens
   to the capture script when a task adds UI.
2. Look at the images: alignment, hierarchy, spacing, contrast, both themes, phone
   layout, long text. Fix what they show before calling the task done.
3. Tests select elements by role and accessible name, not by class or position, so a
   redesign never breaks them.
```

## Things screenshots caught in practice (check for these)

- A heading inside content larger than the page title (hierarchy upside down).
- A global focus ring boxing a whole editor in dark mode.
- A warning banner taking a third of a sidebar.
- A phone toolbar overflowing; and a phone-only rule that leaked into a dialog rendered
  inside the toolbar (scope styles with a dedicated class).
- A label captured mid-animation in a still image.
