# HabitTrail

<!-- plain text
Home
    -> Progress
    -> Support
    -> Privacy
Progress
    -> Home
    -> Support
    -> Privacy
Support
    -> Home
    -> Progress
    -> Privacy
Privacy
    -> Home
    -> Progress
    -> Support -->

Every page shares the same navigation (Home, Progress, Support, Privacy), so each page links to all of the others.

```mermaid
    flowchart TD
        Home <--> Progress
        Home <--> Support
        Home <--> Privacy
        Progress <--> Support
        Progress <--> Privacy
        Support <--> Privacy
```
