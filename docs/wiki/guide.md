
# FortranFormat usage

## Basic usage

```
>>> from FortranFormat import FortranRecordWriter
>>> format = FortranRecordWriter('(3I10)')
>>> output = format.write([0.0, 1.0, None])
>>> output
'         0         1'
```


```
>>> from FortranFormat import FortranRecordReader
>>> format = FortranRecordReader('(3I10)')
>>> output = format.read('         0         1')
>>> output
[0.0, 1.0, None]
```

It supports all the edit descriptors found in F77 as well as repeat formats.

## Configuration

### `RET_WRITTEN_VARS_ONLY`

Default: `False`

In FORTRAN you must first declare a bunch of variables to be filled when you read in a file. So if you read a text file that has 5 values but you allocate an array of length 10 then the array will be half filled. In Python we don't need to pre-allocate so we can just return the values that we read, by default though we read to the end of the record and pad the results with the `None` value. You can set this value to `True` to return just the written values.

To return FORTRAN default values rather than `None` see `RET_UNWRITTEN_VARS_NONE`.

#### Example

```
>>> format = FortranRecordReader('(3I10)')
>>> output = format.read('         0         1')
>>> output
[0.0, 1.0, None]
>>> config.RET_WRITTEN_VARS_ONLY = True
>>> output = format.read('         0         1')
>>> output
[0.0, 1.0]
```


### `RET_UNWRITTEN_VARS_NONE`

Default: `True`

In FORTRAN you must first declare a bunch of variables to be filled when you read in a file. If there are not enough values read in to fill all the variables then FORTRAN will preserve their default values (e.g. 0 for a float). Rather than returning an array of zero's (which is misleading) the library by default returns `None` instead. If you need the more precise FORTRAN behaviour then set this to `False` to return the default FORTRAN values (e.g. 0).

To only return written values see `RET_WRITTEN_VARS_ONLY`.

#### Example

```
>>> format = FortranRecordReader('(3I10)')
>>> output = format.read('         0         1')
>>> output
[0.0, 1.0, None]
>>> config.RET_UNWRITTEN_VARS_NONE = False
>>> output = format.read('         0         1')
>>> output
[0.0, 1.0, 0.0]
```

### `G_INPUT_TRIAL_EDS`

Default: `['F', 'L', 'A']`

There are cases where using the `G` descriptor on input can be ambiguous (e.g. is it a string or a boolean?). FORTRAN is okay with this because the variable used to capture the value has a type which Python doesn't really do. We specify here a preferential ordering of edit descriptors to try until we find something that fits. By default it tries reading it as a number, then a boolean, then a string. If you don't want to interpret the input, for example, as a string then you can set this to `['F', 'L']` and it will raise an error otherwise.


### `RECORD_SEPARATOR`

Default: `'\n'`

When wrapping the records, this string is used to delimit the lines.

#### Example

```
>>> config.RECORD_SEPARATOR = '|'
>>> format = FortranRecordWriter('(2I10)')
>>> output = format.write([0, 0, 0, 0])
>>> output
'         0         0|         0         0'
```

## `reset()`

Call this to reset the configuration to its defaults

```
>>> config.RECORD_SEPARATOR = '|'
>>> format = FortranRecordWriter('(2I10)')
>>> output = format.write([0, 0, 0, 0])
>>> output
'         0         0|         0         0'
>>> config.reset()
>>> output = format.write([0, 0, 0, 0])
>>> output
'         0         0\n         0         0'

