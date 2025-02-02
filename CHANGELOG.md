# Changelog

## V 1.0.0
  * **BREAKING** No longer uses `eval` for importing modules - `eval` was originally used to support very early versions of Python (2.3+) but it now means that modern packaging systems cannot understand `fortranformat` module structure. Since the latter is a more likely use-case the `eval` statements have now been dropped. It has been tested with Python 2.7+ and likely still works with earlier versions
  * Migrated and recreated docs on Git/Github from Mercurial/Bitbucket
  * Fixed issue where decimal values of G and E edit descriptors were causing exceptions on output
  * The `config.RECORD_SEPARATOR` is not reset properly when calling `config.reset()`
  * Tests for more of the edge cases
  * Edit descriptors now output quoted strings even when there are no more values to output


## V 0.2.3
  * Fixed issue 10 - Edit descriptor reversion now starts a new record
