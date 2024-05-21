---
description: OBJECT
layout:
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: false
  outline:
    visible: true
  pagination:
    visible: false
---

# whoami

{% code overflow="wrap" fullWidth="false" %}
```wasm
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
{% endcode %}

[<img src="https://img.shields.io/badge/-71ntr-blue?style=flat-square&#x26;logo=Linkedin&#x26;logoColor=white" alt="" data-size="line">](https://www.linkedin.com/in/71ntr/) [<img src="https://img.shields.io/badge/-71ntr-blue?style=flat-square&#x26;logo=twitter&#x26;logoColor=white" alt="" data-size="line">](https://www.twitter.com/71ntr/) [<img src="https://img.shields.io/badge/-h1nt3r-blue?style=flat-square&#x26;logo=medium&#x26;logoColor=white" alt="" data-size="line">](https://h1nt3r.medium.com/)
