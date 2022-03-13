# LINUX HASH

## 1. Extract the hashes from etc/passwd and etc/shadow
	
The hashes password are located in :
- etc/passwd (old version)
- etc/shadow
		
[optional] To concatenate the two documents : 
	unshadow passwd.txt shadow.txt > unshadowed.txt


## 2. Hash formats :

#### Format of the hash
		
	[username]:$[format]$[password]:[UserID]:[GroupID]:[UserIDInfo]:[HomeDirectory]:[Command/shell]

- **Username:** it should be between 1 and 32 characters in length.

- **Password:** an 'x' character indicates that encrypted password is sotred in /etc/shadow file.

- **User ID (UID):** Each user must be assigned a user ID (UID). UID 0 is reserved for root and UIDs 1-99 arre reserved for other predefined accounts. Further UID 100-999 are reserved by system for admnistrative and system accounts/groups.

- **Group ID (GID):** The primary group ID (stored in /etc/group file)

- **User ID info:** The comment field. it allows you to add extra information about the users such as user's full name, phone number, etc.

- **Home directory:** The aboslute path to the directory the user will be in when they log in.

- **Command/shell:** The absolute path of a command or shell (/bin/bash).



#### Different types of hashes

	$1  = MD5 hashing algorithm. -> format=md5crypt
	$2  = Blowfish Algorithm is in use. -> format=bcrypt
	$2a = eksblowfish Algorithm
	$5  = SHA-256 Algorithm -> format=sha256crypt
	$6  = SHA-512 Algorithm -> format=sha512crypt

	
#### Examples of hash
```
➤ MD5 (passowrd=batman)
sys:$1$fUX6BPOt$Miyc3UpOzQJqz4s5wFD9l0:14742:0:99999:7:::
	
➤ Blowfish (password=bleh)
root:$2y$12$Dwt1BZj6pcyc3Dy1FWZ5ieeUznr71EeNkJkUlypTsgbX1H68wsRom:0:0:root:/root:/bin/bash
	
➤ SHA-256 (password=blink182)
mdavis:$5$i3uY6Gfp$ywzsyCNRs7kbKbN7Ad0SnGR7P6bVmMQ8iJ7008mrGHC:156 52:0:99999:7:::
	
➤ SHA-512 (password=mercedes)
root:$6$riekpK4m$uBdaAyK0j9WfMzvcSKYVfyEHGtBfnfpiVbYbzbVmfbneEbo0wSijW1GQussvJSk8X1M56kzgGj8f7DFN1h4dy1:0:0:root:/root:/bin/bash
```


## 3. John the ripper - Crack the password hashes :


#### General commands and rules

	john [type of attack] --format=[hash of the format] 
			
	--rules:Single
	--rules:Wordlist
	--rules:Extra
	--rules:Jumbo (all the above)
	--rules:KoreLogic
	--rules:All (all the above)


#### Cracking methodology :

1. Verify the type of hash hashes:
```
$1  = MD5 hashing algorithm. -> format=md5crypt
$2  = Blowfish Algorithm is in use. -> format=bcrypt
$2a = eksblowfish Algorithm -> format=bcrypt
$2y = Blowfish Algorithm is in use. -> format=bcrypt-opencl
$5  = SHA-256 Algorithm -> format=sha256crypt
$6  = SHA-512 Algorithm -> format=sha512crypt
```

2. Verify login=password:
```
john --single --format=[md5crypt|bcrypt|sha256crypt|sha512crypt] hashes.txt --fork=4
```

3. Standards dictionnary attacks with[out] rules:
```
john --wordlist=dictionary.txt --format=[md5crypt|bcrypt|sha256crypt|sha512crypt] hashes.txt --fork=4
john --wordlist=dictionary.txt --format=[md5crypt|bcrypt|sha256crypt|sha512crypt] hashes.txt --fork=4 --rules=KoreLogic
```

Be careful, the rules=all parameter is very long. With a lot of hashes, this operation will working during several days.


4. Show result:
	
The cracked hashes are located in the corresponding .pot file

	john --format=[md5crypt|bcrypt|sha256crypt|sha512crypt] hashes.txt --show	


## 4. Hashcat - Crack the password hashes :

#### General commands and rules
```	
hashcat -m <typeOfHash> -a <typeOfAttack> -o <outputFile> <hashesFile.txt> <dictionaryFile.txt>
```

Advances attacks (option -a)
```
0 - Straight : It is the default attack which is also the dictionary attack
1 - Combination : This attack permits to combine several dictionaries. Example: With a wordlist of fruits and a second with color, we obtain a combined wordlist with combinations like : BananaRed, BananaGreen, AppleRed, AppleGreen
3 - Brute-forcing : Traditional Brute-forcing attack which will try each possible combination. Compatible with Masks-Attacks and Rules-Attacks
4 - Permutation : Attack which change the order of the letter for each word of the dictionary. Example with the password 123: 132, 213, 231, 312, 321
````			
			
#### Cracking methodology:

1. Verify the LM hashes:

You can beginning by a crack of the LM password, in the case of the some hash alues are different to :

	$1  = MD5 hashing algorithm. -> -m 500
	$2  = Blowfish Algorithm is in use. -> -m 3200
	$2a = eksblowfish Algorithm -> -m 3200
	$2y = Blowfish Algorithm is in use. -> -m 3200
	$5  = SHA-256 Algorithm -> -m 7400
	$6  = SHA-512 Algorithm -> -m 1800

2. Verify login=password

	Username should be added in a new list in order to be tested


3. Dictionnary attack
```
➤ For MD5 ($1) hash :
hashcat -m 500 -a 0 [Dictionnary].txt [Hash].txt --force
		
➤ For Blowfish ($2,$2a,$2y,...) hash:
hashcat -m 3200 -a 0 [Dictionnary].txt [Hash].txt --force
			
➤ For SHA-256 ($5) hash :
hashcat -m 7400 -a 0 [Dictionnary].txt [Hash].txt --force

➤ For SHA-512 ($6) hash :
hashcat -m 1800 -a 0 [Dictionnary].txt [Hash].txt --force
```

4. Dictionnary attack with rules

To see the existing rules attacks
	ls -l /usr/share/hashcat/rules/

	➤ For MD5 ($1) hash :
	hashcat -m 500 -a 0 [Dictionnary].txt [Hash].txt  -r [Rules_path] --force

	➤ For Blowfish ($2,$2a,$2y,...) hash:
	hashcat -m 3200 -a 0 [Dictionnary].txt [Hash].txt -r [Rules_path] --force

	➤ For SHA-256 ($5) hash :
	hashcat -m 7400 -a 0 [Dictionnary].txt [Hash].txt -r [Rules_path] --force
	
	➤ For SHA-512 ($6) hash :			
	hashcat -m 1800 -a 0 [Dictionnary].txt [Hash].txt -r [Rules_path] --force


5. Show cracked/not cracked passwords
```
➤ For MD5 ($1) hash :
hashcat -m 500 -a 0 [Dictionnary].txt [Hash].txt -show

➤ For Blowfish ($2,$2a,$2y,...) hash:
hashcat -m 3200 -a 0 [Dictionnary].txt [Hash].txt -show

➤ For SHA-256 ($5) hash :
hashcat -m 7400 -a 0 [Dictionnary].txt [Hash].txt -show

➤ For SHA-512 ($6) hash :	
hashcat -m 1800 -a 0 [Dictionnary].txt [Hash].txt -show
```
