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

# $ cd ./pl/

## $ cat ./whoami.cpp

{% code overflow="wrap" fullWidth="false" %}
```cpp
#include <iostream>

std::string whoami = R"(
# $ whoami
~ just a man who lives between zeros and ones.
)";

std::string love = R"(
# $ love
~ cars & codes & offensive security
)";

int main() {
    std::cout << whoami;
    std::cout << love;

    return 0;
}
```
{% endcode %}

```bash
➜ z1ntrx@leet  ~/pl  g++ whoami.cpp  -o whoami
➜ z1ntrx@leet  ~/pl  ./whoami                                       

# $ whoami
~ just a man who lives between zeros and ones.

# $ love
~ cars & codes & offensive security
```

## $ ls ./contacts/

[<img src="https://skillicons.dev/icons?i=twitter&#x26;&#x26;perline=1" alt="twitter" data-size="line">](https://twitter.com/71ntr)  [<img src="https://skillicons.dev/icons?i=linkedin&#x26;&#x26;perline=1" alt="linkedin" data-size="line">](https://www.linkedin.com/in/71ntr/)  [<img src="https://skillicons.dev/icons?i=github&#x26;&#x26;perline=1" alt="linkedin" data-size="line">](https://github.com/Hunt3r0x)
