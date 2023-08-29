# CTF Writeups
All of these CTFs are available at [PicoCTF](https://play.picoctf.org/).
 
## Assignment Set #1 

### Obedient Cat:
Since the description states that the flag is in plain sight, my first instict was to run `cat flag`, which immeditely gave the flag.

### Python Wrangling:
Ran `python3 ende.py`, and it indicated that it had `-d/-e` usage. I tried `-d` and it prompted for a password. I tried leaving it empty, which caused an error and a traceback, which had the following statement: `Fernet key must be 32 url-safe base64-encoded bytes`. I then did the same but used the password provided in `pw.txt`, which then gave another traceback:

	Traceback (most recent call last):
	File "ende.py", line 46, in <module>
	    with open(sys.argv[2], "r") as f:
    IndexError: list index out of range

Looking specifically at `with open(sys.argv[2], "r"):` indicates that we are missing an argument when running the script, and seeing `open` means it tries to open the file. Assuming that `flag.txt.en` is the file that `ende.py` is trying to open, I finally ran `python3 ende.py -d flag.txt.en`, which prompted the password. Upon inputting the password, I was able to obtain the flag.

### Wave a flag:
Utilizing the hints that Pico provided, I ran `chmod +x warm` to convert the `warm` file into an executable, which when executed displayed the following: 
`Hello user! Pass me a -h to learn what I can do!`
Running `./warm -h`then outputted the flag.

### Nice netcat...:
Running the command provided by Pico (`nc mercury.picoctf.net 22902`), I was able to establish a netcat connection with the server. This then outputted a series of numbers. Utilizing the hint that mentioned ASCII characters, I converted each number to it's corresponding ASCII character and obtained the flag.

### Static ain't always noise:
Running the script `ltdis.sh`,  I got the following output:

	Attempting disassembly of  ...
	objdump: 'a.out': No such file
	objdump: section '.text' mentioned in a -j option, but not found in any input file
	Disassembly failed!
	Usage: ltdis.sh <program-file>
	Bye!

Reading into this we can see that the usage of this file requires a binary program in its arguments. I then utilized the binary `static` along with `ltdis.sh` by running `./ltdis.sh static`, it output a text file with the strings it found in the binary. I then ran `cat` on this file, and looking through the strings, I found the flag.
