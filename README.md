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

My first thought was that each line on chart represent some byte value
To my my analysis easier i first rotated cut the image, then using GIMP saved it
as a .data file using two-color palette so the image looks like this:




![ScreenShot](https://raw.githubusercontent.com/Deuterion/GynvaelEN-Mission-15/master/chartgray.bmp)


Now looking at the portion of binary data we see that black pixel is represented as single
0x00 byte and white as 0x01

```
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 01 01 01 01 01 01 01 01 01 01 01 01 01 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 01 01 01 01 01 01 01 01 01 01 01 01 01 00 00 00 00 00

```

It was to to write a simple python script 
I deliberately rotated the image so every "line" (which was in fact 125 wide string of bytes)
represented a single byte value
``` python
import os

f = open('mission_data','rb')
data = f.read()
f.close()
width = 125 

bytes = []

for i in range(int(len(data)/width)):
	for j in range(width):
		if(data[i * width + j] != 0x00):
			# We reached the white pixel
			# so the length is our byte value 
			bytes.append(j)
			break


for byte in bytes:
	# Now simply cast it to text
	print("%c" % (byte),end='')
```


###Output:
```
<?php

if (!isset($_GET['password']) || !is_string($_GET['password'])) {
  die("bad password");


$p = $_GET['password'];

if (strlen($p) !== 25) {
  die("bad password");


if (md5($p) !== 'e66c97b8837d0328f3e5522ebb058f85') {
  die("bad password");


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
  


die("GW!");

```

Bingo!
