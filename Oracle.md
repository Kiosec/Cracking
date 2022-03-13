# Oracle

## 1. Dump the Hash password file from the output of the audit script
 
 
## 2. Hash formats :

Oracle database could contains several type of password hashes : H, S and T. The cracking method is not the same for each type of hash.

Example of hash for a user:
```
S:8F2D65FB5547B71C8DA3760F10960428CD307B1C6271691FC55C1F56554A;
H:DC9894A01797D91D92ECA1DA66242209;
T:23D1F8CAC9001F69630ED2DD8DF67DD3BE5C470B5EA97B622F757FE102D8BF14BEDC94A3CC046D10858D885DB656DC0CBF899A79CD8C76B788744844CADE54EEEB4FDEC478FB7C7CBFBBAC57BA3EF22C
```

#### How to formalized the hash in order to crack them?
```	
➤ 1. 'S' hash

Keep only the username and the S hash without "S:". [ADD format]

Example : 
S:8F2D65FB5547B71C8DA3760F10960428CD307B1C6271691FC55C1F56554A; 
->
username:8F2D65FB5547B71C8DA3760F10960428CD307B1C6271691FC55C1F56554A

➤ 2. 'H' hash

keep only the hash. In fact, the hash is an MD5 of USERNAME:XDB:password

Example : 
H:DC9894A01797D91D92ECA1DA66242209;
->
DC9894A01797D91D92ECA1DA66242209

Be careful, the dico used to crack the H pass have to be the following form (not only include password) and respect uppercase (USERNAME:XDB):
USERNAME:XDB:passwordtested

➤ 3. T hash
Work in progress
More information : https://www.trustwave.com/en-us/resources/blogs/spiderlabs-blog/changes-in-oracle-database-12c-password-hashes/
```

## 3. John the ripper - Crack the password hashes :

#### General commands and rules
```
john [type of attack] --format=[hash of the format] 

--rules:Single
--rules:Wordlist
--rules:Extra
--rules:Jumbo (all the above)
--rules:KoreLogic
--rules:All (all the above)	
```

#### S" Cracking methodology :
```
➤ 1. Verify login=password
john --single --format=oracle11 hashes.txt --fork=4

➤ 2. Standards dictionnary attacks with[out] rules
john --wordlist=dictionary.txt --format=oracle11 hashes.txt --fork=4
john --wordlist=dictionary.txt --format=oracle11 hashes.txt --fork=4 --rules=KoreLogic

Be careful, the rules=all parameter is very long. With a lot of hashes, this operation will working during several days.
```

#### "H" Cracking methodology : 
```	
➤ 1. Read the hash format before to launch the H crack.

➤ 2. Verify login=password
john --single --format=raw-md5 hashes.txt --fork=4

➤ 3. Standards dictionnary attacks with[out] rules
john --wordlist=dictionary.txt --format=raw-md5 hashes.txt --fork=4
john --wordlist=dictionary.txt --format=raw-md5 hashes.txt --fork=4 --rules=KoreLogic

Be careful, the rules=all parameter is very long. With a lot of hashes, this operation will working during several days.
```

#### "T" Cracking methodology :
```
Work in progress
More information : https://www.trustwave.com/en-us/resources/blogs/spiderlabs-blog/changes-in-oracle-database-12c-password-hashes/
```

#### Show result:
```	
The cracked hashes are located in the corresponding .pot file
		
S -> john --format=oracle11 hashes.txt --show
H -> john --format=raw-md5 hashes.txt --show
T -> Work in progress
```

## 4. Hashcat - Crack the password hashes :

John the ripper is a better industrial tools for S and H hashes.

#### General commands and rules
```		
hashcat -m <typeOfHash> -a <typeOfAttack> -o <outputFile> <hashesFile.txt> <dictionaryFile.txt>
			
Advances attacks (option -a)
- 0 - Straight : It is the default attack which is also the dictionary attack
- 1 - Combination : This attack permits to combine several dictionaries. Example: With a wordlist of fruits and a second with color, we obtain a combined wordlist with combinations like : BananaRed, BananaGreen, AppleRed, AppleGreen
- 3 - Brute-forcing : Traditional Brute-forcing attack which will try each possible combination. Compatible with Masks-Attacks and Rules-Attacks
- 4 - Permutation : Attack which change the order of the letter for each word of the dictionary. Example with the password 123: 132, 213, 231, 312, 321
```

#### "S" Cracking methodology :
```	
➤ 1. Hash format
The hash format required is splitted (":") between hash and salt. The first 40 caracters are the hash and the end (20 caracters) the salf.
			
Example:
S:8F2D65FB5547B71C8DA3760F10960428CD307B1C6271691FC55C1F56554A;
->
8F2D65FB5547B71C8DA3760F10960428CD307B1C:6271691FC55C1F56554A
				
➤ 2. Verify login=password
Username should be added in a new list in order to be tested
			
➤ 3. Standards dictionnary attacks
hashcat -m 112 -a 0 [Dictionnary].txt [Hash].txt --force
		
➤ 4. Standards dictionnary attack with rules
To see the existing rules attacks: ls -l /usr/share/hashcat/rules/
					
hashcat -m 112 -a 0 [Dictionnary].txt [Hash].txt  -r [Rules_path] --force
```
	
#### "H" Cracking methodology :
```
➤ 1. Verify login=password
Username should be added in a new list in order to be tested
			
➤ 2. Standards dictionnary attacks
hashcat -m 3100 -a 0 [Dictionnary].txt [Hash].txt --force
		
➤ 3. Standards dictionnary attack with rules
To see the existing rules attacks : ls -l /usr/share/hashcat/rules/
		
hashcat -m 3100 -a 0 [Dictionnary].txt [Hash].txt  -r [Rules_path] --force
```

#### "T" Cracking methodology :
```
➤ 1. Verify login=password
Username should be added in a new list in order to be tested
			
➤ 2. Standards dictionnary attacks
hashcat -m 12300 -a 0 [Dictionnary].txt [Hash].txt --force
	
➤ 3. Standards dictionnary attack with rules
To see the existing rules attacks:  ls -l /usr/share/hashcat/rules/
				
hashcat -m 12300 -a 0 [Dictionnary].txt [Hash].txt  -r [Rules_path] --force
```

#### Show result:
```				
➤ For "S" hash :
hashcat -m 112 -a 0 [Dictionnary].txt [Hash].txt --show

➤ For "H" hash :
hashcat -m 0 -a 0 [Dictionnary].txt [Hash].txt --show

Be careful, the cracked passwords are in hex format

➤ For "T" hash :
hashcat -m 12300 -a 0 [Dictionnary].txt [Hash].txt --show
```
