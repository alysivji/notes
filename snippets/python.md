# Python Snippets

## Testing

* Running coverage with pytest

```console
$ py.test --cov=backend tests/
```

## Debugging

### Q

[Github](https://github.com/zestyping/q)

```python
# print
import q; q(foo)

# trace function with decorator
@q.t

# interactive shell
import q; q.d()
```


Output goes to `$TEMPDIR\q` or `\tmp\q`

```console
tail -f $TMPDIR\q
```


Created an Bash Function
```console
# connect to q log file for python debugging
q ()
{
    if [ -z $TMPDIR ]; then tail -f \tmp\q; else tail -f $TMPDIR\q; fi
}
```


### IPython

```python
from IPython import embed; embed() # interactive shell with IPython
```

### Pdb

```python
import ipdb
ipdb.set_trace()
```
