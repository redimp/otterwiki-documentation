# MathJax

For people that want to put mathematics in their wiki, [MathJax](https://www.mathjax.org/)
comes to the rescue. Since there is no markdown standard for LaTeX, multiple syntax
styles have been established. An Otter Wiki supports two of them:

## TeX Style

The TeX style notation, where inline math is encapsulated with `$` and
equations with `$$`, e.g.

	When $a \ne 0$, there are two solutions
    to $ax^2 + bx + c = 0$ and they are
    
    $$x = {-b \pm \sqrt{b^2-4ac} \over 2a}.$$

When $a \ne 0$, there are two solutions to $ax^2 + bx + c = 0$ and they are

$$x = {-b \pm \sqrt{b^2-4ac} \over 2a}.$$

## Code Style

The Code style, where inline math is encapsulated with
`` `$` `` and equations with in code blocks with language `math`, e.g.

	When `$a \ne 0$`, there are two solutions
    to `$ax^2 + bx + c = 0$` and they are
    
	```math
	x = {-b \pm \sqrt{b^2-4ac} \over 2a}.    
    ```

When `$a \ne 0$`, there are two solutions to `$ax^2 + bx + c = 0$` and they are
```math
x = {-b \pm \sqrt{b^2-4ac} \over 2a}.
```
