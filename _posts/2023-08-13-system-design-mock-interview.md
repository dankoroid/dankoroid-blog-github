---
title: "System design mock interview"
date: 2023-08-13
---

I've watched quite good [System Design Mock interview](https://www.youtube.com/watch?v=JWkyxKHItx4) (unfortunately, it is not longer available), here are some points from it:

1. Use good visualization tool (here [Excalidraw](https://excalidraw.com/) is used) - it helps to visualize & separate/group your thoughts, write/illustrate your suggestions, make color codings, show interactions
2. Good questions for start:
    - How this application is expected to be run (cloud-based, on-premise, etc)
    - What is expected average usage of application (peaks?)
    - What are main use cases that we're going to cover (functional requirements)
    - Availability/consistency requirements 
3. Make your starting points:
    - Assume daily transactions count
    - Assume data storage volume (and separation of those storages based on data relations)
    - Assume average/peak bandwidth
4. Make high level structure:
    - Make start points clear (mobile app; web app; desktop app; message queue; etc)
    - Show main user interactions with system (API)
    - Describe system components & interactions between them (what specific DBs, what specific services, HTTP/queue,etc communication protocols; what caches are used; should LB be used)
    - Make a good & clear picture of those components by grouping them in some groups, adding color, naming arrows, writing down quantity of services or other assumptions
    - Write down potential optimizations in structure that you think about and could come later to them if needed
5. Make points about ensuring that system will be stable
    - Describe caches
    - Describe atomic operations & retries
    - Think about master/slave or replications, etc 
    - Describe potential bottlenecks

Good communication phrases:
- Really great to have you with us
- It was a really fascinating answer, thank you for sharing it with us
- (interviewer) I've got a really interesting question, I think you're going to like it
