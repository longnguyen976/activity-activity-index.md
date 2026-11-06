# UI Conventions: <Product name>

<!--
DBIZ2 Lesson 8 template. File: docs/external-design/ui-conventions.md
One page. It keeps screens consistent with each other when different members, or different AI
tools, draw them. In DBIZ3 the coding agent reads this file before it builds any screen.
-->

## 1. Language and tone

- Interface language: <Vietnamese / English / both, and which is default>
- Address the user as: <"bạn" / "you" / formal>
- Button labels are verbs that say what happens: <"Book seat", not "OK">

## 2. Formats

| Item | Format | Example |
|---|---|---|
| Date | | 16/10/2026 |
| Time | | 09:00 |
| Currency | | 120.000 ₫ |
| Phone | | |
| Names | | |

## 3. Components (use these names in every Screen Spec)

| Component | When to use | Notes |
|---|---|---|
| Primary button | the one main action of a screen | one per screen |
| Secondary button | alternative actions | |
| Text input | | label above, error message below |
| List item | | |
| Banner | non-blocking information | |
| Modal | confirm or cancel a single action | never for navigation |
| Toast | short confirmation after success | disappears by itself |

## 4. Messages

| Situation | Pattern | Example |
|---|---|---|
| Validation error | what is wrong + how to fix | |
| System error | what happened + what the user can do now | |
| Empty state | why it is empty + the next action | |
| Success | what was done | |

## 5. Accessibility minimums

- Body text contrast at least 4.5:1 (WCAG 2.2, success criterion 1.4.3).
- Information is never carried by colour alone (WCAG 2.2, 1.4.1).
- Tap targets at least 24 by 24 CSS pixels (WCAG 2.2, 2.5.8).
- Smallest supported screen width: <e.g. 360px>.
