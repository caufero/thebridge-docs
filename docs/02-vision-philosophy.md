# 2. Vision & Philosophy

## 2.1 Everything is One. Everything is a Process
We will not be shipping many separate modules. We ship one **engine** that can express many modules. The user defines a process; the system renders forms, runs steps, logs actions, and stores results.

---

## 2.2 Materials → Action → Result
Every operation binds three things:

    - **Material:** the object we work on
    - **Action:** the task we perform
    - **Result:** the outcome we record

We store this consistently so we can slice history from any angle.

**1P = 1E = 1C.** Each process instance carries its own entity and composition context. Practically, it means every run has identity, data, and relationships packaged together through JSON. The code reads one definition and knows what to render, who to involve, and what to persist.

---

## 2.3 Keep Schema Simple
We will keep the schema simple and small. We avoid heavy calculated fields and table relationships.
