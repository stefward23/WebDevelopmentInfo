https://gitlab.com/kalilinux/packages/hash-identifier/-/tree/kali/master

python3 hash-id.py

john --format=[format] --wordlist=[path to wordlist] [path to file]

john --format=raw-md5 --wordlist=/usr/share/wordlists/rockyou.txt hash_to_crack.txt

john --list=formats | grep -iF "md5"


# Unshadow
unshadow /etc/shadow /etc/passwd

john --wordlist=/usr/share/wordlists/rockyou.txt --format=sha512crypt unshadowed.txt


# Single crack mode
#### For word mangling
john --single --format=[format] [path to file]

#### https://www.openwall.com/john/doc/RULES.shtml

# Custom rules
[List.Rules:PoloPassword]
--rule=PoloPassword

c: Capitalises the first letter
Az: Appends to the end of the word
[0-9]: A number in the range 0-9
[!£$%@]: The password is followed by one of these symbols

# Zip2John
zip2john zipfile.zip > zip_hash.txt

john --wordlist=/usr/share/wordlists/rockyou.txt zip_hash.txt

unzip filename

# Rar2John
rar2john [rar file] > [output file]

/opt/john/rar2john rarfile.rar > rar_hash.txt

john --wordlist=/usr/share/wordlists/rockyou.txt rar_hash.txt

unrar x filename

# SSH2John

ssh2john [id_rsa private key file] > [output file]

# https://www.openwall.com/john/