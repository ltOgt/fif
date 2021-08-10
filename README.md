# Find in File.
Wrapper around `find` `cat` and `grep` to find a file by a String it contains.

```
USAGE:
fif <options> "<search-string>"

<options>:
    -L <num>  Maximum depth of the search.
    -p <pth>  Starting path for the search
    -F <pat>  Pattern to filter files to be searched
    -A <num>  NUM lines after match are printed
    -B <num>  NUM lines before match are printed
    -C        Case sensitive
```

its basically just a fancy version of:
```sh
for file in $(find . -type f); do
	echo $file;
	cat $file | grep -E "search-pattern";
done
```
