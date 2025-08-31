https://gitlab.com/kalilinux/packages/hash-identifier/-/tree/kali/master

python3 hash-id.py

john --format=[format] --wordlist=[path to wordlist] [path to file]

john --format=raw-md5 --wordlist=/usr/share/wordlists/rockyou.txt hash_to_crack.txt

john --list=formats | grep -iF "md5"


#Unshadow

john --wordlist=/usr/share/wordlists/rockyou.txt --format=sha512crypt unshadowed.txt

