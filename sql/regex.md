!SLIDE subsection
# Regexes
    @@@sql
    SELECT 'foo bar baz' ~ '\s+baz';

.notes Like is for sissies. Sometimes you just really need to do more than wildcard matching will let you. Postgres has great support for proper POSIX regular expressions, but doesn't have a good index type to support them yet, so when that's a concern, stick to LIKE.

!SLIDE
# matching
    @@@sql
    SELECT 'foo bar baz' ~ '\s+baz';

!SLIDE
# lazy data generation
    @@@sql
    SELECT regexp_split_to_table(
      'the brown fox jumped the lazy dog',
      '\s+') as words;

!SLIDE
# search and replace
    @@@sql
    UPDATE gear_names SET name = 
    regexp_replace(name, '\w+', 'chicken');

!SLIDE
## Further reading
.notes https://dataclips.heroku.com/dxtuxfabmwomtssljyqxwrifzhhl

http://www.postgresql.org/docs/9.2/static/functions-matching.html
