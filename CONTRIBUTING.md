## Contributing

Here's what to do whenever adding new rules:

1. Add your regex to `rules.ini`
2. Add a corresponding test to `tests/types/` with a matching name
3. Add corresponding lines to `tests/types/_NonMatchingTests.txt` that your test should NOT pick up on
4. Add a corresponding file in `descriptions` with a matching name describing the technology in 1-2 sentence

**Example:**
Let's say we want to add a rule to detect the FNA game engine. This one is very convenient because it can be matched by simply finding a file named `fna.dll`. However, we also want to be sure to match `some/directory/fna.dll`, but we *don't* want to return a match if we find `some_file_that_just_ends_with_fna.dll` or `fna.dllsomethingelse`.

The regex we want to add is this:
`FNA = (?:^|/)fna\.dll$`

That makes sure to match the file by itself or in any set of subdirectories, and only with the exact extension `.dll` and no more characters after that. To test this, we add the file `Engine.FNA.txt` to `tests/types/` and it has this content:

```
fna.dll
Sub/Folder/fna.dll
```

If the rule is written correctly, it should match both of these filenames.

Then add some lines to `tests/types/_NonmatchingTests.txt` with this content:

```
fna_dll
notactuallyfna.dll
fna.dllwhoops
sub/dir/notactuallyfna.dll
sub/dir/fna.dllwhoops
```

If the rule is written correctly, it should NOT match any of these filenames.

New contributions should make sure they also provide tests and have run those tests themselves, and should be careful about introducing lots of false positives or negatives. Ideally, you want to look for the most unique looking file that is common to most or all games of a particular engine/technology, that is very unlikely to occur for other apps.

Also note that we are not particularly interested in maintaining rules for engines that only like 3 people have ever used.

## Tests

If you have PHP installed locally, you can run the tests from the root directory by typing `php tests/Test.php` and `php tests/Test2Pass.php`
