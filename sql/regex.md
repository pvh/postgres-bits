# Regexes

Like is for sissies. Sometimes you just really need to do more than wildcard matching will let you. Postgres has great support for proper POSIX regular expressions, but doesn't have a good index type to support them yet, so when that's a concern, stick to LIKE.

## Great for
digging out more complicated text expressions
making lazy arrays and tables
search and replace

## Examples

SELECT 'foo bar baz' ~ E'\\s+baz';
SELECT regexp_split_to_table('the quick brown fox jumped over the lazy dog', E'\\s+') as words;
https://dataclips.heroku.com/dxtuxfabmwomtssljyqxwrifzhhl

## Further reading

http://www.postgresql.org/docs/9.2/static/functions-matching.html
