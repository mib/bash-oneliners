File Manipulation
=================

### Emptying a file

	> file.txt

It is shorter than the commonly used:

	echo '' > file.txt

### Filesize in bytes

Instead of trying to parse `ls -l` output, it's easier to use `stat`:

	stat -c %s file.txt
