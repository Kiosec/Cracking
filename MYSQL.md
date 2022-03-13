# MYSQL

## 1. Dump the Hash password file from the output of the audit script

	
## 2. Hash formats :

#### MD5 hashes
```
pass1:cac36191e75ca4ddcbe5800c5cd25f92
pass2:33c5d4954da881814420f3ba39772644
pass3:c25846bdbf27696caa5541a474e9521b
```

#### SHA1 hashes
```
pass4:d9d71ab718931a89de1e986bc62f6c988ddc1813
pass5:348162101fc6f7e624681b7400b085eeac6df7bd
pass6:36e618512a68721f032470bb0891adef3362cfa9
```

#### Double SHA1 hashes (MySQL specific algorithm - MYSQL 4.1 - MySQL 5)
```
pass7:*1DD2A6D6B8D512EBF62D49B75BB598E1E8661A93
pass8:*EA107160A003D070FE3EFE409813807BBA5ECE03
pass9:*6C8989366EAF75BB670AD8EA7A7FC1176A95CEF4
```

#### SHA256 hashes
```
pass10:8b68c46294a9ccb2449324c24fe774f95b7c14e4b56fc51c7f8e6c5b01c7020f
pass11:9cc80741546051ae3de7d31246327968c98af3c65125376acb7d49a0760d42a3
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

#### "MD5" Cracking methodology :
```
➤ 1. Example of john format:
8b70bf2ffce34ced3223dfc9e4fa9cc7
username:8b70bf2ffce34ced3223dfc9e4fa9cc7
username:8b70bf2ffce34ced3223dfc9e4fa9cc7:::::::

➤ 2. Verify login=password
john --single --format=raw-md5 hashes.txt --fork=4
	
➤ 3. Standards dictionnary attacks with[out] rules
john --wordlist=dictionary.txt --format=raw-md5 hashes.txt --fork=4
john --wordlist=dictionary.txt --format=raw-md5 hashes.txt --fork=4 --rules=KoreLogic

Be careful, the rules=all parameter is very long. With a lot of hashes, this operation will working during several days.
```

#### "SHA1" Cracking methodology : 
```
➤ 1. Example of john format:
d10c988ca61b785f5a7756b5852683d798fe4d92
username:d10c988ca61b785f5a7756b5852683d798fe4d92
username:d10c988ca61b785f5a7756b5852683d798fe4d92:::::::
		
➤ 2. Verify login=password
john --single --format=raw-sha1 hashes.txt --fork=4
	
➤ 3. Standards dictionnary attacks with[out] rules
john --wordlist=dictionary.txt --format=raw-sha1 hashes.txt --fork=4
john --wordlist=dictionary.txt --format=raw-sha1 hashes.txt --fork=4 --rules=KoreLogic
	
Be careful, the rules=all parameter is very long. With a lot of hashes, this operation will working during several days.
```

#### "Double SHA1" Cracking methodology : 
```
➤ 1. Example of john format:	
*2470C0C06DEE42FD1618BB99005ADCA2EC9D1E19
username:*2470C0C06DEE42FD1618BB99005ADCA2EC9D1E19
username:*2470C0C06DEE42FD1618BB99005ADCA2EC9D1E19:::::::
	
➤ 2. Verify login=password
john --single --format=msql-sha1 hashes.txt --fork=4

➤ 3. Standards dictionnary attacks with[out] rules
john --wordlist=dictionary.txt --format=msql-sha1 hashes.txt --fork=4
john --wordlist=dictionary.txt --format=msql-sha1 hashes.txt --fork=4 --rules=KoreLogic

Be careful, the rules=all parameter is very long. With a lot of hashes, this operation will working during several days.	
```

#### - "SHA256" Cracking methodology : 
```
➤ 1. Example of john format:
8b68c46294a9ccb2449324c24fe774f95b7c14e4b56fc51c7f8e6c5b01c7020f
username:8b68c46294a9ccb2449324c24fe774f95b7c14e4b56fc51c7f8e6c5b01c7020f
username:8b68c46294a9ccb2449324c24fe774f95b7c14e4b56fc51c7f8e6c5b01c7020f:::::::

➤ 2. Verify login=password
john --single --format=raw-sha256 hashes.txt --fork=4

➤ 3. Standards dictionnary attacks with[out] rules
john --wordlist=dictionary.txt --format=raw-sha256 hashes.txt --fork=4
john --wordlist=dictionary.txt --format=raw-sha256 hashes.txt --fork=4 --rules=KoreLogic

Be careful, the rules=all parameter is very long. With a lot of hashes, this operation will working during several days.
```

#### Show result:
```
The cracked hashes are located in the corresponding .pot file
		
➤ For "MD5" hash :
john --format=raw-md5 hashes.txt --show

➤ For "SHA1" hash :
john --format=raw-sha1 hashes.txt --show

➤ For "DOUBLE SHA1" hash :
john --format=msql-sha1 hashes.txt --show

➤ For "SHA256" hash :
john --format=msql-sha1 hashes.txt --show
```

## 4. Hashcat - Crack the password hashes :

#### General commands and rules
```			
hashcat -m <typeOfHash> -a <typeOfAttack> -o <outputFile> <hashesFile.txt> <dictionaryFile.txt>

Advances attacks (option -a)
0 - Straight : It is the default attack which is also the dictionary attack
1 - Combination : This attack permits to combine several dictionaries. Example: With a wordlist of fruits and a second with color, we obtain a combined wordlist with combinations like : BananaRed, BananaGreen, AppleRed, AppleGreen
3 - Brute-forcing : Traditional Brute-forcing attack which will try each possible combination. Compatible with Masks-Attacks and Rules-Attacks
4 - Permutation : Attack which change the order of the letter for each word of the dictionary. Example with the password 123: 132, 213, 231, 312, 321
```

#### MD5" Cracking methodology :
```
➤ 1. Hash format
cac36191e75ca4ddcbe5800c5cd25f92

➤ 2. Verify login=password
Username should be added in a new list in order to be tested

➤ 3. Standards dictionnary attacks
hashcat -m 0 -a 0 [Dictionnary].txt [Hash].txt --force

➤ 4. Standards dictionnary attack with rules
To see the existing rules attacks: ls -l /usr/share/hashcat/rules/

hashcat -m 0 -a 0 [Dictionnary].txt [Hash].txt  -r [Rules_path] --force
```

#### "SHA1" Cracking methodology :
```
➤ 1. Hash format
d9d71ab718931a89de1e986bc62f6c988ddc1813

➤ 2. Verify login=password
Username should be added in a new list in order to be tested

➤ 3. Standards dictionnary attacks
hashcat -m 100 -a 0 [Dictionnary].txt [Hash].txt --force

➤ 4. Standards dictionnary attack with rules
To see the existing rules attacks: ls -l /usr/share/hashcat/rules/

hashcat -m 100 -a 0 [Dictionnary].txt [Hash].txt  -r [Rules_path] --force
```

#### "DOUBLE SHA1" Cracking methodology :
```	
➤ 1. Hash format
Delete the symbol * at the beginning of each hash. Example :
*1DD2A6D6B8D512EBF62D49B75BB598E1E8661A93 -> 1DD2A6D6B8D512EBF62D49B75BB598E1E8661A93

➤ 2. Verify login=password
Username should be added in a new list in order to be tested

➤ 3. Standards dictionnary attacks
hashcat -m 4500 -a 0 [Dictionnary].txt [Hash].txt --force

➤ 4. Standards dictionnary attack with rules
To see the existing rules attacks: ls -l /usr/share/hashcat/rules/

hashcat -m 4500 -a 0 [Dictionnary].txt [Hash].txt  -r [Rules_path] --force
```

#### "SHA256" Cracking methodology :
```	
➤ 1. Hash format
8b68c46294a9ccb2449324c24fe774f95b7c14e4b56fc51c7f8e6c5b01c7020f

➤ 2. Verify login=password
Username should be added in a new list in order to be tested

➤ 3. Standards dictionnary attacks
hashcat -m 1400 -a 0 [Dictionnary].txt [Hash].txt --force

➤ 4. Standards dictionnary attack with rules
To see the existing rules attacks: ls -l /usr/share/hashcat/rules/

hashcat -m 1400 -a 0 [Dictionnary].txt [Hash].txt  -r [Rules_path] --force
```

#### Show result:
```				
➤ For "MD5" hash :
hashcat -m 0 -a 0 [Dictionnary].txt [Hash].txt --show

➤ For "SHA1" hash :
hashcat -m 100 -a 0 [Dictionnary].txt [Hash].txt --show

➤ For "DOUBLE SHA1" hash :
hashcat -m 4500 -a 0 [Dictionnary].txt [Hash].txt --show

➤ For "SHA256" hash :
hashcat -m 1400 -a 0 [Dictionnary].txt [Hash].txt --show
```
