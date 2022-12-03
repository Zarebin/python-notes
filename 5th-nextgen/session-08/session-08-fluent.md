# Pattern Matching Sequences in an Interpreter  

Peter Norvig of Stanford University wrote lis.py: an interpreter for a subset of the
Scheme dialect of the Lisp programming language.  
In this section, we’ll compare a key part of Norvig’s
code—which uses *if/elif*  and unpacking—with a rewrite using *match/case.*  
The two main functions of lis.py are parse and evaluate.5 The parser takes Scheme
parenthesized expressions and returns Python lists. Here are two *examples:*

```python
>>> parse('(gcd 18 45)')
['gcd', 18, 45]
>>> parse('''
... (define double
...   (lambda (n)
...     (* n 2)))
...  ''')
['define', 'double', ['lambda', ['n'], ['*', 'n', 2]]]
```

### Note:  
The evaluator takes lists like these and executes them. The first example is calling a
gcd function with 18 and 45 as arguments. When evaluated, it computes the greatest
common divisor of the arguments: 9. The second example is defining a function
named double with a parameter n. The body of the function is the expression (* n
2). The result of calling a function in Scheme is the value of the last expression in its
body.

## Matching patterns without match/case:  

```python
def evaluate(exp: Expression, env: Environment) -> Any:
    "Evaluate an expression in an environment."
    if isinstance(exp, Symbol):
    # variable reference
        return env[exp]
    # ... lines omitted
    elif exp[0] == 'quote':
    # (quote exp)
        (_, x) = exp
        return x
    elif exp[0] == 'if':
    # (if test conseq alt)
        (_, test, consequence, alternative) = exp
        if evaluate(test, env):
           return evaluate(consequence, env)
        else:
            return evaluate(alternative, env)
    elif exp[0] == 'lambda':
    # (lambda (parm…) body…)
        (_, parms, *body) = exp
        return Procedure(parms, body, env)
    elif exp[0] == 'define':
        (_, name, value_exp) = exp
        env[name] = evaluate(value_exp, env)
        # ... more lines omitted
```


### Note:  
Note how each elif clause checks the first item of the list, and then unpacks the list,
ignoring the first item. The extensive use of unpacking suggests that Norvig is a fan of
pattern matching, but he wrote that code originally for Python 2 (though it now
works with any Python 3).

**Pattern matching with match/case—requires Python ≥ 3.10**  

```python
def evaluate(exp: Expression, env: Environment) -> Any:
"Evaluate an expression in an environment."
    match exp:
# ... lines omitted
        case ['quote', x]:
            return x
        case ['if', test, consequence, alternative]:
            if evaluate(test, env):
                return evaluate(consequence, env)
            else:
                return evaluate(alternative, env)
        case ['lambda', [*parms], *body] if body:
            return Procedure(parms, body, env)
        case ['define', Symbol() as name, value_exp]:
            env[name] = evaluate(value_exp, env)
# ... more lines omitted
        case _:
            raise SyntaxError(lispstr(exp))
```

* Match if subject is a two-item sequence starting with 'quote'.
* Match if subject is a four-item sequence starting with 'if'.
* Match if subject is a sequence of three or more items starting with 'lambda'. The
guard ensures that body is not empty.
* Match if subject is a three-item sequence starting with 'define', followed by an
instance of Symbol.
* It is good practice to have a catch-all case. In this example, if exp doesn’t match
any of the patterns, the expression is malformed, and I raise *SyntaxError.*

## Alternative patterns for lambda  
This is the syntax of lambda in Scheme, using the syntactic convention that the suffix
… means the element may appear zero or more times:  

(lambda (parms…) body1 body2…)  

**A simple pattern for the lambda case 'lambda' would be this:**

```python
case ['lambda', parms, *body] if body:
```

**I made the 'lambda' pattern safer using a nested sequence pattern:**

```python
case ['lambda', [*parms], *body] if body:
    return Procedure(parms, body, env)
```
In a sequence pattern, * can appear only once per sequence. Here we have two
sequences: the outer and the inner.

