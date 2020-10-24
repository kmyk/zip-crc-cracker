# Zip CRC cracker

Zip files have CRC values, the checksum of the plaintext of contents (even if they are encrypted).
If the file size is short enough (around 4 bytes because CRC32 is 32 bits), you can see the content of the encrypted zip file without the password.

This script is for trivial CTF tasks. Due to the restriction about file sizes, in most cases, you cannot use this for real-world tasks.

## usage

``` sh
$ python3 crack.py a.zip
```

## example

``` sh
$ echo crc > 1.txt
$ echo 32 > 2.txt
$ echo foo > 3.txt
$ echo decy > 4.txt
$ echo pt > 5.txt
$ echo able > 6.txt
$ zip 1.zip 1.txt 2.txt
  adding: 1.txt (stored 0%)
  adding: 2.txt (stored 0%)
$ zip 2.zip 3.txt
  adding: 3.txt (stored 0%)
$ zip 3.zip 4.txt 5.txt 6.txt
  adding: 4.txt (stored 0%)
  adding: 5.txt (stored 0%)
  adding: 6.txt (stored 0%)
```

and

``` python
$ time python3 crack.py 1.zip 2.zip 3.zip
reading zip files...
file found: 1.zip / 1.txt: crc = 0xf1a7eab5, size = 4
file found: 1.zip / 2.txt: crc = 0xd4c93fb4, size = 3
file found: 2.zip / 3.txt: crc = 0x7e3265a8, size = 4
file found: 3.zip / 4.txt: crc = 0xf2d17f79, size = 5
file found: 3.zip / 5.txt: crc = 0x36e4ae, size = 3
file found: 3.zip / 6.txt: crc = 0x4acdd2d0, size = 5
compiling...
searching...
crc found: d4c93fb4: "32
"
crc found: 4acdd2d0: "able
"
crc found: f1a7eab5: "crc
"
crc found: f2d17f79: "decy
"
crc found: 7e3265a8: "foo
"
crc found: 0036e4ae: "pt
"
crc found: 4acdd2d0: "@<|
                         F"
R"c found: 4acdd2d0: "\s 
done

1.zip / 1.txt : 'crc\n'
1.zip / 2.txt : '32\n'
2.zip / 3.txt : 'foo\n'
3.zip / 4.txt : 'decy\n'
3.zip / 5.txt : 'pt\n'
3.zip / 6.txt : 'able\n'
3.zip / 6.txt : '@<|\x0cF'
3.zip / 6.txt : '\\s \rR'
python3 crack.py 1.zip 2.zip 3.zip  133.58s user 0.04s system 99% cpu 2:13.63 total
```
