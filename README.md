## GynvaelEN-Mission-15


>MISSION 015               goo.gl/JKN1Zq             DIFFICULTY: █████░░░░░ [5╱10]
>┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅
>
>One of our operatives managed to find an information leak vulnerability on an
>internal website of a hostile syndicate. The vulnerability itself is quite
>amusing, as it allows to leak any file on the FS in form of a bar chart. Our
>operative leaked a script responsible for authenticating a system operator.
>
>Your task is to analyze the chart and then the script, and attempt to recover
>system operator's password.
>
>  Chart: goo.gl/YbYYcS
>
>Good luck!
>
>---------------------------------------------------------------------------------
>
>If you find the answer, put it in the comments under this video! If you write a
>blogpost / post your solution / code online, please add a link as well!
>
>P.S. I'll show/explain the solution on the stream in ~one week.

My first thought was that each line on chart represents some byte value.
To make my analysis easier i first rotated and cut the image, then using GIMP saved it
as a .data file using two-color palette so the image looks like this:




![ScreenShot](https://raw.githubusercontent.com/Deuterion/GynvaelEN-Mission-15/master/chartgray.bmp)


Now looking at the portion of binary data we see that black pixel is represented as single
0x00 byte and white as 0x01.

```
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 01 01 01 01 01 01 01 01 01 01 01 01 01 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 01 01 01 01 01 01 01 01 01 01 01 01 01 00 00 00 00 00

```

It was time to write a simple python script. 
I deliberately rotated the image so every "line" (which was in fact 125 wide string of bytes)
represented a single byte value.
``` python
import os

# First read .data file
f = open('mission_data','rb')
data = f.read()
f.close()
width = 125 # The width of modified image

bytes = []

for i in range(int(len(data)/width)):
	byte_set = False
	for j in range(width):
		if(data[i * width + j] != 0x00):
			# We reached the white pixel
			# so the length is our byte value 
			bytes.append(j)
			byte_set = True
			break
	# If whole line is black
	if not byte_set:
		bytes.append(width)


for byte in bytes:
	# Now simply cast it to text
	print("%c" % (byte),end='')
```


### Output:
``` php
<?php

if (!isset($_GET['password']) || !is_string($_GET['password'])) {
  die("bad password");
}

$p = $_GET['password'];

if (strlen($p) !== 25) {
  die("bad password");
}

if (md5($p) !== 'e66c97b8837d0328f3e5522ebb058f85') {
  die("bad password");
}

// Split the password in five and check the pieces.
// We need to be sure!
$values = array(
  0 => 'e6d9fe6df8fd2a07ca6636729d4a615a',
  5 => '273e97dc41693b152c71715d099a1049',
  10 => 'bd014fafb6f235929c73a6e9d5f1e458',
  15 => 'ab892a96d92d434432d23429483c0a39',
  20 => 'b56a807858d5948a4e4604c117a62c2d'
);

for ($i = 0; $i < 25; $i += 5) {
  if (md5(substr($p, $i, 5)) !== $values[$i]) {
    die("bad password");
  }
}

die("GW!");

```

Bingo!

So the password is 25 characters long. I figured that bruteforcing entire password may be futile
so i looked further.

Ok, it seems that every 5-char chunk of password is being md5-ed.
This is far easier to do so let's write another python script.
I first took into consideration ASCII letters + space.

``` python
import hashlib
import string
import itertools

# string containing characters to use in bruteforcing
chars = string.ascii_lowercase + ' ' + string.ascii_uppercase

def md5(str):
	return hashlib.md5(str).hexdigest()

hashes = ['e6d9fe6df8fd2a07ca6636729d4a615a',
		'273e97dc41693b152c71715d099a1049',
		'bd014fafb6f235929c73a6e9d5f1e458',
		'ab892a96d92d434432d23429483c0a39',
		'b56a807858d5948a4e4604c117a62c2d']

for i in itertools.product(chars, repeat = 5):
	key = (''.join(i)).encode("ascii")
	hash = md5(key)
	if hash in hashes:
		print("Password [%i] is %s!!!" % (hashes.index(hash),key))
```

### Output:
```
Password [3] is b'delic'!!!
Password [1] is b'harts'!!!
Password [2] is b' are '!!!
Password [0] is b'Pie c'!!!
```

So 5th chunk couldn't be found, are we missing characters?

Password: Pie charts are delic?????


The last chunk has to be 'ious', but that's just 4 letters.

A number wouldn't fit there so it's a special character. Maybe "!"?

Let's change 
```python
chars = string.ascii_lowercase + ' ' + string.ascii_uppercase
```
to
```python
chars = '!' + string.ascii_lowercase
```
A few seconds later:
```
Password [4] is b'ious!'!!!
```

### Password: Pie charts are delicious!


