# HabitTrail

<!-- plain text
Home
    -> Support
    -> Privacy
Support
    -> Home
    -> Privacy
Privacy
    -> Home
    -> Support -->

Every page shares the same navigation (Home, Support, Privacy), so each page links to both of the others.

```mermaid
    flowchart TD
        Home <--> Support
        Home <--> Privacy
        Support <--> Privacy
```
