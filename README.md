---
description: OBJECT
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: false
---

# $ cat ./whoami

{% code overflow="wrap" fullWidth="false" %}
```wasm
section .data
    whoami db "# $ whoami", 10, "~ just a man who lives between zeros and ones.", 10
    love db "# $ love", 10, "~ cars & codes & offensive security", 10, 0

section .text
    global _start

_start:
    ; Print the bio
    mov eax, 4        ; system call number (sys_write)
    mov ebx, 1        ; file descriptor (stdout)
    mov ecx, whoami   ; pointer to the first part of the bio
    mov edx, len1     ; length of the first part of the bio
    int 0x80          ; call kernel

    mov ecx, love     ; pointer to the second part of the bio
    mov edx, len2     ; length of the second part of the bio
    int 0x80          ; call kernel

    ; Exit program
    mov eax, 1        ; system call number (sys_exit)
    xor ebx, ebx      ; exit code 0
    int 0x80          ; call kernel

section .data
    len1 equ $ - whoami
    len2 equ $ - love
```
{% endcode %}

[<img src="https://skillicons.dev/icons?i=twitter&#x26;&#x26;perline=1" alt="twitter" data-size="line">](https://twitter.com/71ntr) [<img src="https://skillicons.dev/icons?i=linkedin&#x26;&#x26;perline=1" alt="linkedin" data-size="line">](https://www.linkedin.com/in/71ntr/)[<img src="https://skillicons.dev/icons?i=github&#x26;&#x26;perline=1" alt="linkedin" data-size="line">](https://github.com/Hunt3r0x)
