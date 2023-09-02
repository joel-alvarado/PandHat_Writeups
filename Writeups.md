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


### Tab, Tab, Attack:
This challenge was pretty simple. We have a ZIP file called `Addadshashanammu.zip` . We can use the `Tab` key to autocomplete, which makes the plethora of sub-folders with long names easy to access through the terminal. Once I got to the deepest sub-directory, I found a file called `fang-of-haynekhtnamet`. I first ran `cat` on it, and the result was some binary code, so I ran the file as an executable and was able to obtain the flag.

### Magikarp Ground Mission:
For this challenge, I had to SSH into a remote server hosted by Pico. After connecting, I ran `ls` and 2 files showed up: `1of3.flag.txt` and `instructions-to-2of3.txt`. The former contained 1/3 of the flag, while the latter had instructions to navigate to another directory using `cd`. I followed the trail and obtained all 3 parts of the flag.

### 2Warm:
This challenge was pretty basic. All I needed to do was convert the number `42` from decimal to binary. I utilized `wcalc -b 42` and the output was the flag's value for this excersise.

### Lets Warm Up:
For this challenge, we need to convert the number `0x70` from hexadecimal to ASCII. To do this, first we go from hexadecimal to decimal, and then using a ASCII character table, we can get the letter. For this case, `0x70 -> 112 -> p`.
The letter `p` was the flag's value.

### Warmed Up: 
This challenge is the same as the first step of `Lets Warm Up`. All we need to do is convert `0x3D` from hexadecimal to decimal. Using `wcalc -d 0x3D`, we get the number `61` as the flag's value.  

## DownUnderCTF

### downunderflow
DUCTF provided a netcat server to connect to, as well as a C source code file that is being run in the server when you nc to it. Analyzing the source code, it is a login, and you can input a number on the terminal and login as a user from 0-6, and anything higher is invalid. While user 7 shows as admin, there is a condition that checks wether the input number is bigger than or equal to the amount of users - 1. Since admin was index 7, it is impossible to input user 7 (admin) without triggering the condition and getting locked out. Upon further inspection, the number is cast into a `unsigned short`, before attempting to log in with said number. What I did was underflow the number enough to where the number became a 7, which in this case was -65529. When entering this number as the login, the unsigned short underflows and becomes a 7, allowing us access to a an admin shell. Then I used ls to see the root directory, and there was a flag.txt file that contained the challenges flag.
