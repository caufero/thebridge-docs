# 7. User Interface Design

## 7.1 UI Rules
- **Keep layouts light** — avoid heavy portals and unnecessary objects.
- **Use scripted finds / ExecuteSQL** for lists and filtered views (fast, targeted reads).
- **Cache Template JSON** for the session when safe (invalidate on template/version change).
- **Defer expensive renders** — load detail panes lazily after list selection.
- **Minimize redraws** — prefer one WebViewer render fed by JSON over many native widgets.
- **Consistent components** — reuse buttons, menus, and patterns across screens.
- **Keyboard-first** where possible — tab order, quick actions, Enter-to-commit.
- **Clear feedback** — toasts/spinners for long ops; disable buttons during actions.
- **Error surfaces** — inline, human-readable, with remediation hints.
