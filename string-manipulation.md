String Manipulation
===================

## `sed`, `awk`

### Prepend all lines with a string

Prepend `hello ` to all lines:

	sed 's/^/hello /'

Example:

	$ echo -e 'one\ntwo\nthree' | sed 's/^/hello /'
	hello one
	hello two
	hello three

### Output only N-th line

Output only tenth line:

	sed -n 10p

### Output only lines M through N

Output only lines 10, 11, …, 20:

	sed -n 10,20p

### Remove N-th line from output

Output all lines but the tenth:

	sed 10d

### Output even/odd/N-th lines only

Even lines only:

	sed -n '2~2p'

Odd lines only:

	sed -n '1~2p'

Generic form:

	sed -n 'A~Bp'

This starts with the A-th line and then outputs every B-th line.

### Output all lines between two matches

Output all lines from the line containing `A` through the line containing `B`. The two boundary lines are included:

	awk '/A/,/B/'

Example:

	$ echo -e 'one\ntwo\nthree\nfour\nfive\nsix' | awk /two/,/five/
	two
	three
	four
	five

### Output unique lines without sorting

	awk '!x[$0]++'

(not suitable for large files)

Example:

	$ echo -e 'three\ntwo\ntwo\none\nthree\nthree\none' | awk '!x[$0]++'
	three
	two
	one

### Repeat a string N times

	echo -e $_{1..N}'\bSTRING'

(where `N` is the count and `STRING` is the word to repeat)

Without `\b` the words will be separated by spaces:

	echo $_{1..N}'STRING'

Example: Repeat the word `Hello` 6 times:

	$ echo -e $_{1..6}'\bHello'
	HelloHelloHelloHelloHelloHello

## Bash variable manipulation

### Search and replace

Replace the first dot with an underscore:

	$ var=a.b.c.d; echo ${var/./_}
	a_b.c.d

Replace all dots with underscores:

	$ var=a.b.c.d; echo ${var//./_}
	a_b_c_d

### Search and remove

Remove the first dot:

	$ var=a.b.c.d; echo ${var/.}
	ab.c.d

Remove all dots:

	$ var=a.b.c.d; echo ${var//.}
	abcd

### Conditional replace

Replace the first character with an `X` if it is an `a`/`b`:

	$ var=a.b.c.d; echo ${var/#a/X}
	X.b.c.d

	$ var=a.b.c.d; echo ${var/#b/X}
	a.b.c.d

Replace the last character with an `X` if it is an `d`/`e`:

	$ var=a.b.c.d; echo ${var/%d/X}
	a.b.c.X

	$ var=a.b.c.d; echo ${var/%e/X}
	a.b.c.d

### Strip the beginning/end according to a pattern

Remove everything after the last dot (ungreedy pattern):

	$ var=a.b.c.d; echo ${var%.*}
	a.b.c

Remove everything after the first dot (greedy pattern):

	$ var=a.b.c.d; echo ${var%%.*}
	a

Remove everything before the first dot (ungreedy pattern):

	$ var=a.b.c.d; echo ${var#*.}
	b.c.d

Remove everything before the last dot (greedy pattern):

	$ var=a.b.c.d; echo ${var##*.}
	d

### Convert to lowercase/uppercase

Convert the first character to uppercase/lowercase:

	$ var=abcd; echo ${var^}
	Abcd

	$ var=ABCD; echo ${var,}
	aBCD

Convert all characters to uppercase/lowercase:

	$ var=abcd; echo ${var^^}
	ABCD

	$ var=ABCD; echo ${var,,}
	abcd

### Substring

All but the first 2 characters:

	$ var=abcd; echo ${var:2}
	cd

3 characters, starting from the 2nd:

	$ var=abcdef; echo ${var:2:3}
	cde

The last 2 characters:

	$ var=abcdef; echo ${var: -2}
	ef

(The space before the minus/dash is necessary since `:-` means: use a default value if the variable is not set.)

### String length

	$ var=abcdef; echo ${#var}
	6

## Other utilities

### Pick a random line

	shuf -n 1

`shuf` shuffles all lines, `-n 1` outputs only the first line.

### Output lines in reverse order

Type `cat` in reverse:

	tac

Example:

	$ echo -e 'one\ntwo\nthree' | tac
	three
	two
	one

### Diff two unsorted files without creating temporary files

	diff <(sort file1.txt) <(sort file2.txt)
