# regex
### A command line tool for extracting regex capture groups


```
Usage:
./regex <pattern> <string>
some_command | ./regex <pattern>
```

It isn't trivial to extract text matching a certain regex from a string or stdout of some command. `grep` is a fantastic tool but it always prints the entire matched line.  This tool takes a pattern and a string (or reads from stdin if a string is not provided), and outputs only the first capture group from the input.

### Some examples of the tool in use:

##### Extract current battery level (macOS)

```
$ pmset -g batt
Now drawing from 'Battery Power'
 -InternalBattery-0 (id=3866723)	65%; discharging; 2:35 remaining present: true


$ pmset -g batt | ./regex "(\d+%)"
65%
```

##### Show pages accessed on your web server

```
$ cat /var/log/nginx/access.log | ~/regex "GET (.*) HTTP/1.1"
/
/robots.txt
/
/
/favicon.ico
/index.html
/apple-touch-icon.png
/apple-touch-icon.png
...(truncated)...
```

Then sort pages by most visited

```
$ cat /var/log/nginx/access.log.1 | ~/regex "GET (.*) HTTP/1.1" | sort | uniq -c | sort -n
4 /index.html
8 /apple-touch-icon-120x120.png
8 /apple-touch-icon-120x120-precomposed.png
8 /apple-touch-icon.png
8 /apple-touch-icon-precomposed.png
8 /favicon.ico
19 /robots.txt
60 /
```


