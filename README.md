# rgx
### Command line tool for extracting regex capture groups


```
Usage:
./rgx <pattern> <string>
some_command | ./rgx <pattern>
```

This tool takes a pattern and a string (or reads from stdin if a string is not provided), and outputs the first capture group from each line of the input.

Note: This is similar but not identical to `grep -oE`, which prints out the entire matched portion of the input, as opposed to just the captured group.

### Some examples of the tool in use:

##### Extract current battery level (macOS)

```
$ pmset -g batt
Now drawing from 'Battery Power'
 -InternalBattery-0 (id=3866723)	65%; discharging; 2:35 remaining present: true


$ pmset -g batt | rgx "(\d+)%"
65

```
You would need two `grep`s to achieve the same output, because grep returns all of the matched output, not just the capture group:

```
$ pmset -g batt | grep -oE "\d+%" | grep -oE "\d+"
```

##### Show pages accessed on your web server

```
$ cat /var/log/nginx/access.log | rgx "GET (.*) HTTP/1.1"
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
$ cat /var/log/nginx/access.log.1 | rgx "GET (.*) HTTP/1.1" | sort | uniq -c | sort -n
4 /index.html
8 /apple-touch-icon-120x120.png
8 /apple-touch-icon-120x120-precomposed.png
8 /apple-touch-icon.png
8 /apple-touch-icon-precomposed.png
8 /favicon.ico
19 /robots.txt
60 /
```


