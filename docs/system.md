# System

Utils for handling system, like to track file, make directory, link and so on.


## system.FileTracker
```python
FileTracker()
```

Track current opening files, started with `FileTracker.track()`.
When opening several files, `FileTracker` tracks them and you can locate them by calling
`FileTraker.get_openfiles()`.

```python
FiltTracker.track()
```

Start tracking opening files.

```python
FiltTracker.untrack()
```

Stop tracking opening files.

```python
FiltTracker.get_openfiles()
```

Get current opening files.

```python
>>> from pydu.system import FileTracker
>>> FileTracker.track()
>>> f = open('test', 'w')
>>> FileTracker.get_openfiles()
{<_io.TextIOWrapper name='test' mode='w' encoding='UTF-8'>}
>>> f.close()
>>> FileTracker.get_openfiles()
set()
>>> FileTracker.untrack()
>>> f = open('test', 'w')
>>> FileTracker.get_openfiles()
set()
```


## system.makedirs
```python
makedirs(path, mode=0o755, ignore_errors=False, exist_ok=False)
```

Based on `os.makedirs`,create a leaf directory and all intermediate ones.
`mode` default is `0o755`. When make an exists path, if exist_ok is false,
`makedirs` will raise an `Exception`. If `ignore_errors` which will ignore
all errors raised by `os.makedirs`.

```python
>>> from pydu.system import makedirs
>>> makedirs('test1/test2')
>>> makedirs('test1',exist_ok=True)
>>> makedirs('test1')
Traceback (most recent call last):
...    OSError: Create dir: test1 error.
```

## system.remove
```python
remove(path, mode=0o755, ignore_errors=False, onerror)
```

Remove a file or directory.

If `ignore_errors` is set, errors are ignored; otherwise, if `onerror`
is set, it is called to handle the error with arguments (`func` ,
`path` , `exc_info` ) where func is platform and implementation dependent;
`path` is the argument to that function that caused it to fail; and
`exc_info` is a tuple returned by `sys.exc_info()`.  If `ignore_errors`
is `False` and `onerror` is None, it attempts to set `path` as writeable and
then proceed with deletion if `path` is read-only, or raise an exception
if `path` is not read-only.

```python
>>> from pydu.system import makedirs
>>> from pydu.system import remove
>>> from pydu.system import touch
>>> makedirs('test')
>>> remove('test')
>>> touch('test.txt')
>>> remove('test.txt')
>>> remove('test.txt', ignore_errors=True)
>>> remove('test.txt')
Traceback (most recent call last):
...    OSError: Remove path: test error. Reason: [Errno 2] No such file or directory: 'test.txt'
```

## system.removes
```python
removes(paths, mode=0o755, ignore_errors=False, onerror)
```

Remove a list of file and/or directory.Other parameters same as `remove`.

```python
>>> from pydu.system import makedirs
>>> from pydu.system import remove
>>> from pydu.system import open_file
>>> makedirs('test1')
>>> makedirs('test2')
>>> open_file('test.txt')
>>> removes(['test.txt','test1','test2'])
```

## system.open_file
```python
open_file(path, mode='wb+', buffer_size=-1, ignore_errors=False):
```

Open a file, defualt mode `wb+`. If path not exists, it will be created
automatically. If `ignore_errors` is set, errors are ignored.

```python
>>> from pydu.system import open_file
>>> open_file('test.txt')
>>> ls
test.txt
>>> open_file('test1.txt',mode='r')
Traceback (most recent call last):
...    OSError: Open file: test1.txt error
```

## system.copy
```python
copy(src, dst, ignore_errors=False, follow_symlinks=True):
```

Copy data and mode bits (`cp src dst`).Both the source and destination
may be a directory.When `copy` a directory,which contains a symlink,if
the optional symlinks flag is true, symbolic  links in the source tree
result in symbolic links in the  destination tree; if it is false, the
contents of the files pointed to by symbolic links are copied.When copy
a file,if follow_symlinks is false and src is a symbolic link, a new
symlink will be created instead of copying the file it points to,else
the contents of the file pointed to by symbolic links is copied.

```python
>>> from pydu.system import copy,symlink
>>> from pydu.system import makedirs,open_fle
>>> open_fle('test/test.txt')
>>> symlink('test/test.txt','test/test.link')
>>> copy('test/test.link','test/test_copy1.link')
>>> copy('test/test.link','test/test_copy2.link',follow_symlink=False)
```

## system.touch
```python
touch(path):
```

Make a new file.

```python
>>> from pydu.system import touch
>>> touch('test.txt')
```

## system.symlink
```python
symlink(src, dst, overwrite=False, ignore_errors=False)
```

It create a symbolic link pointing
to source named link_name.If dist is exist and overwrite is true,a new
symlink will be created.

```python
>>> from pydu.system import symlink
>>> symlink('test.txt','test.link')
```

!> `symlink` can only be used on `unix-like` system.

## system.link
```python
link(src, dst, overwrite=False, ignore_errors=False):
```

It create a hard link pointing to
source named link_name.If dist is exist and overwrite is true, a
new link will be created.

```python
>>> from pydu.system import link
>>> link('test.txt','test.link')
```

!> `link` can only be used on `unix-like` system.


## system.which
```python
which(cmd, mode=os.F_OK | os.X_OK, path=None):
```

Given a command, mode, and a PATH string, return the path which
conforms to the given mode on the PATH, or None if there is no such
file.

`mode` defaults to os.F_OK | os.X_OK. `path` defaults to the result
of os.environ.get("PATH"), or can be overridden with a custom search
path.

`which` is `shutil.which` in Python 3.

```python
>>> from pydu.system import which
>>> which('echo')
/bin/echo
```


## system.chmod
```python
chmod(path, mode, recursive=False)
```

Change permissions to the given mode.
If `recursive` is True perform recursively.

```python
>>> from pydu.system import chmod
>>> chmod('/opt/sometest', 0o744)
>>> oct(os.stat('/opt/sometest').st_mode)[-3:]
'744'
```

!> Although Windows supports `chmod`, you can only set the file’s
read-only flag with it (via the stat.S_IWRITE and stat.S_IREAD constants
or a corresponding integer value). All other bits are ignored.


## system.chcp
```python
chcp(code)
```

Context manager which sets the active code page number.
It could also be used as function.

```python
>>> from pydu.system import chcp
>>> chcp(437)
<active code page number: 437>
>>> with chcp(437):
...     pass
>>>
```

!> `chcp` can only be used on `Windows` system.


## system.preferredencoding
```python
preferredencoding(code)
```

Get best encoding for the system.
