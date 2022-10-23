## Common `regex` practices

- multiple match conditions at one time with addition restrict

```bash
(^|, )(part1|part2|part3)
# it will match "partX" start with line beginning or ", "
```

## Use `grep` for search

 ```bash
    ssh hname systemctl | grep "Disconnected from" > ssh.log
        ↑sname ↑remote-exec
    # or run it remotely for a faster processing
    ssh hname 'systemctl | grep "Disconnected from"' > ssh.log
 ```

You can directly use regex expression on `grep`

## Use `sed` for regex substitution

>   `sed` is a stream editor and allow you to do basic operations with regex to the context

#### Basics

- Syntax
    - `.` means everything
    - `*` means match zero or more of any matched {pattern}
    - `+` means one or more and the same as below
    - provide the `g` modifier says do this as many as it keeps matching
	- use `-E` parameter makes it use a more modern syntax
	- use `\` for escape
	- `[]` let you match one of many different character

```bash
    # remove everything that comes before 'Disconnected from'
    cat ssh.log | sed 's/.*Disconnected from//' | less

    # [] let you match one of many different character
    echo 'aba' | sed 's/[ab]//'
    $ # ba
	  
    # because regex expressions will just match the pattern once in default mode
    echo 'aba' | sed 's/[ab]//g'
    $ # (nothing output)
```

```bash
    # match a group named 'ab', and match groups as many as it can
    echo 'ababacsabe' | sed -E 's/(ab)*//g'
    # the same as
    echo 'ababacsabe' | sed -E 's/(ab)//g'
    # use \ for escape
    echo 'ababacsabe' | sed 's/\(ab\)*//g'
    $ # acse
    # otherwise it just match a little parenthesis

    # OR operator
    echo 'abcsabe' | sed 's/\(ab\|ac\)*//g'
    echo 'abcsabe' | sed -E 's/(ab|ac)*//g'
    $ # cse
```


#### Example

normally you may want to use `sed -nE '/{pattern}/p'` to print what the pattern match

- there are many **parameter** and **script flags** you can use:
	- `'/1'` or `/num` to output the `n` times result when the `{pattern}` was repeated 
		- discriminate `/1` and `\1`, they are used in different place
	- `-n` do not print lines
	- `/p` print the modified line
	- `/g` replace as many times as it can in one line
	- `/I` ignore cases

- Syntax
	- `?` match zero or once {pattern}
	- anchor `^` match the begin of the line
	- dollar `$` match the end of the line
	- use ^ and $ will let us debug a pattern easier
	- you can write the pattern first, try to exec it, and then edit it, until it works
	- use `()` for capture group in {pattern}, and use `\num` when you need to refer back in {result} substitution, each `()` represents a capture group. capture group start in index `1`
	-  `.*` is non-greed match
	-  what you need to know is that non-greedy mode has never been supported in `sed` 
	- regex is pre-line matching by default

```bash
    # Jan 08 02:34:21 thesquareplanet.com sshd [15262]:Disconnected from invalid user admin 58.120.227.29 port 52978 [preauth]
    # Jan 10 23:09:08 thesquareplanet.com sshd [20215]:Disconnected from user jon 24.61.9.143 port 51098
```

>  we want to list the username that have been try to login by someone

```bash
	cat ssh.log | sed -E 's/^.*Disconnected from (invalid |authenticating )?user (.*) [0-9.]+ port [0-9]+( \[preauth\])?$/\2/' | head -n5
	$ # jon
```

```bash
	's/^.*? invalid (.*) $/\1/'
	$ # username
```

[More Regex Syntax](<https://www.regular-expressions.info/>)

#### Hands-on Practice 

> Make good use of [Regex 101](https://regex101.com/) or [RegexR](https://regexr.com/)

![[output.txt]]

```bash
	cat output.txt | sed -nE 's/^([a-zA-Z]{3} [0-9]{2}) (([0-9]{,2}:?){3}) (.*(Accepted|Disconnected) .* (user|for) ([a-z0-9]{0,}) (from )?(([0-9]{0,3}\.?){4}) port.*)$/\5 conection from \9 for user \"\7\" on \1 \2/p' | sort -k7,7 -nk10,10 -nk11,11 | awk '{count[$7]++; if ($1 ~ /A.*/) {accept[$7]++;} else {deny[$7]++}} END{for (x in count) { printf ("%s: %d", x, count[x]); if (length(accept[x]) > 0) { printf(", %s: %d", "accept", accept[x]); } printf(", %s: %d\n", "deny", deny[x]); }}' | sort | tr '\n' '; '| sed -E 's/;/; /g'
	$ # "admin": 391, deny: 391; "alarm": 5, deny: 5; "baikal": 4, deny: 4; "cirros": 10, deny: 10; "contador": 3, deny: 3; "debian": 10, deny: 10; "duni": 3, deny: 3; "ethos": 5, deny: 5; "ftpuser": 3, deny: 3; "guest": 5, deny: 5; "mine": 4, deny: 4; "miner": 4, deny: 4; "nagios": 5, deny: 5; "oracle": 8, deny: 8; "pi": 13, deny: 13; "postgres": 5, deny: 5; "root": 536, deny: 536; "support": 5, deny: 5; "test": 8, deny: 8; "test1": 3, deny: 3; "test2": 3, deny: 3; "ubnt": 5, deny: 5; "ubuntu": 107, accept: 89, deny: 18; "user": 345, deny: 345; "user2": 333, deny: 333; "usuario": 3, deny: 3; "vagrant": 5, deny: 5; "volumio": 5, deny: 5; "xbmc": 5, deny: 5; %

	# substituted solution
	cat .... | sort | paste -sd\; | ....
```

#### Flexible usage

- use `head -n[-]NUM` to diminish output to the first N line (or print with first N line **excluded**)
	- use `tail -n[-]NUM` to set a boundary limit that outputs only the last N lime

* use `wc` to count words
    - `wc -l` to count lines

* use `sort` to sort input and print them to output
    - `sort | uniq` to unique the result of sort
- `uniq -c` to calculate number of duplicate lines and eliminate them

- `awk` a stream preprocessor to process columns each line of input
	- match with pattern for specific column like `'$1 == 1 && $2 ~ /^c.*e$/ print{ $0 }'`
	- `BEGIN` to specify what should `awk` do before parse the first line of input, and the same like `END`, you can use both of them with `'BEGIN{} {(parse tasks)} END{}'`

- `tr`

- `paste` paste multiple
	- `paste -sd,` line into single with delimiter comma

- `bc` calculator
	- if you have the a bunch of input like `1+2*3/4+5` , you can use them as the input of `bc` , that you will get the result of that formula

- `R` language
	- example of analytics: `R --slave -e 'x <- scan(file="stdin", quiet=TRUE); summary(x)'`

- `gnuplot`

- `xargs` take lines as input and turns them to arguments
	- `xargs apt install` or test it with `xargs echo apt install`

#### Fancy Realm

`ffmpeg -loglevel panic -i /dev/video0 -frames 1 -f image2 - | convert - -colorspace gray - | gzip | ssh tsp 'gzip -d | tee copy.png' | feh -` 
- `-` means the `stdin` file of shell
- `scp tsp:copy.png .`
- `tee` take `stdin` output as a specific `file` and redirect it to `stdout`