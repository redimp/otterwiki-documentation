# Syntax Highlighting

On this page you can find examples on how An Otter Wiki highlights code block with a given syntax.

A code block is fenced with three backticks `` ``` `` on the line before and after the code block. To add syntax highlighting specify the language next to the backticks of the first line, e.g.

A fenced code block with language yaml, like

    ```yaml
    version: '3'
    services:
      otterwiki:
        image: redimp/otterwiki:2
        ports:
        - 8080:80
    ```

will be rendered as

```yaml
version: '3'
services:
  otterwiki:
    image: redimp/otterwiki:2
    ports:
    - 8080:80
```

Examples for multiple other languages can be found below. All the examples implementing the Fibonacci sequence (and more examples in more languages) can be found on [rosettacode.org](https://rosettacode.org/wiki/Fibonacci_sequence).

---

### bash

```bash
fib()
{
  if [ $1 -le 0 ]
  then
    echo 0
    return 0
  fi
  if [ $1 -le 2 ]
  then
    echo 1
  else
    a=$(fib $[$1-1])
    b=$(fib $[$1-2])
    echo $(($a+$b))
  fi
}
```

### C

```c
long long int fibb(int n) {
	int fnow = 0, fnext = 1, tempf;
	while(--n>0){
		tempf = fnow + fnext;
		fnow = fnext;
		fnext = tempf;
		}
		return fnext;
}
```

### C++

```cpp
#include <numeric>
#include <vector>
#include <functional>
#include <iostream>

unsigned int fibonacci(unsigned int n) {
  if (n == 0) return 0;
  std::vector<int> v(n, 1);
  adjacent_difference(v.begin(), v.end()-1, v.begin()+1, std::plus<int>());
  // "array" now contains the Fibonacci sequence from 1 up
  return v[n-1];
}
```

### Dockerfile

```Dockerfile
FROM alpine:3.18.0
MAINTAINER mail@example.com

RUN set -ex \
    && apk update && apk upgrade \
    && apk add --no-cache \
            bash curl dhcping iftop httpie netcat-openbsd py3-pip \
            py3-setuptools vim git

USER root
WORKDIR /root
ENV HOSTNAME toolkit

CMD ["bash"]
```

### Go

```go
import (
	"math/big"
)

func fib(n uint64) *big.Int {
	if n < 2 {
		return big.NewInt(int64(n))
	}
	a, b := big.NewInt(0), big.NewInt(1)
	for n--; n > 0; n-- {
		a.Add(a, b)
		a, b = b, a
	}
	return b
}
```

### Haskell

```haskell
fib x =
  if x < 1
    then 0
    else if x == 1
           then 1
           else fibs !! (x - 1) + fibs !! (x - 2)
  where
    fibs = map fib [0 ..]
```

### html

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <title>The title goes here</title>
  </head>
  <body>
    <!-- a comment -->
    The content goes here.
  </body>
</html>
```

### json

```json
{
  "mixed list": [
    "string",
    42
  ],
  "object": {
    "key": "value",
    "array": [
      {
        "null_value": null
      },
      {
        "boolean": true
      },
      {
        "float": 1.2
      }
    ]
  },
  "content": "Some lines\nwith line breaks"
}
```

### nginx

```nginx
user  www www;
worker_processes  2;
pid /var/run/nginx.pid;
error_log  /var/log/nginx.error_log  debug | info | notice | warn | error | crit;

events {
    connections   2000;
    use kqueue | rtsig | epoll | /dev/poll | select | poll;
}

http {
    log_format main  '$remote_addr - $remote_user [$time_local] '
                     '"$request" $status $bytes_sent '
                     '"$http_referer" "$http_user_agent" '
                     '"$gzip_ratio"';
    send_timeout 3m;
    client_header_buffer_size 1k;
...
```

### python

```python
def fibMemo():
    pad = {0:0, 1:1}
    def func(n):
        if n not in pad:
            pad[n] = func(n-1) + func(n-2)
        return pad[n]
    return func

fm = fibMemo()
for i in range(1,31):
    print fm(i),
```

### php

```php
<?php
function fibIter($n) {
    if ($n < 2) {
        return $n;
    }
    $fibPrev = 0;
    $fib = 1;
    foreach (range(1, $n-1) as $i) {
        list($fibPrev, $fib) = array($fib, $fib + $fibPrev);
    }
    return $fib;
}
?>
```

### rust

```rust
use std::mem;
fn main() {
    fibonacci(0,1);
}

fn fibonacci(mut prev: usize, mut curr: usize) {
    mem::swap(&mut prev, &mut curr);
    if let Some(n) = curr.checked_add(prev) {
        println!("{}", n);
        fibonacci(prev, n);
    }
}
```

### yaml

```yaml
---
mixed list:
- string
- 42
object:
  key: value
  array:
  - null_value:
  - boolean: true
  - float: 1.2
content: |-
  Some lines
  with line breaks
```

### xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<root>
    <mixed_list>string</mixed_list>
    <mixed_list>42</mixed_list>
    <object>
        <key>value</key>
        <array>
            <null_value />
        </array>
        <array>
            <boolean>true</boolean>
        </array>
        <array>
            <float>1.2</float>
        </array>
    </object>
    <content>Some lines
with line breaks</content>
</root>
```
