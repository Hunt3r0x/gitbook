---
description: OBJECT
cover: >-
  https://images.unsplash.com/photo-1597449031666-21da12583121?crop=entropy&cs=srgb&fm=jpg&ixid=M3wxOTcwMjR8MHwxfHNlYXJjaHwxfHxibGFjayUyMGhvbGV8ZW58MHx8fHwxNzE2MjYzNTM4fDA&ixlib=rb-4.0.3&q=85
coverY: -98.46520874751492
---

# whoami

```armasm
section .data
    bio db "## cars & codes & offensive security", 0
    len equ $ - bio

section .text
    global _start

_start:
    ; Print the bio
    mov edx, len     ; length of our string
    mov ecx, bio     ; pointer to our string
    mov ebx, 1       ; file descriptor (stdout)
    mov eax, 4       ; system call number (sys_write)
    int 0x80         ; call kernel

    ; Exit program
    mov eax, 1       ; system call number (sys_exit)
    xor ebx, ebx     ; exit code 0
    int 0x80         ; call kernel

```

[<img src="https://img.shields.io/badge/-71ntr-blue?style=flat-square&#x26;logo=Linkedin&#x26;logoColor=white" alt="" data-size="line">](https://www.linkedin.com/in/71ntr/) [<img src="https://img.shields.io/badge/-71ntr-blue?style=flat-square&#x26;logo=twitter&#x26;logoColor=white" alt="" data-size="line">](https://www.twitter.com/71ntr/) [<img src="https://img.shields.io/badge/-h1nt3r-blue?style=flat-square&#x26;logo=medium&#x26;logoColor=white" alt="" data-size="line">](https://h1nt3r.medium.com/)
