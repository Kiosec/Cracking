# MSSQL 

## 1. Dump the Hash password file from the output of the audit script

## 2. Hash formats :

#### MSSQL05 hash (MSSQL 2005)
```	
0x01004086CEB6BF932BC4151A1AF1F13CD17301D70816A8886908
username:0x01004086CEB6BF932BC4151A1AF1F13CD17301D70816A8886908
username:0x01004086CEB6BF932BC4151A1AF1F13CD17301D70816A8886908:::::::
```

#### MSSQL hash (MSSQL 2000)
```
0x0100A607BA7C54A24D17B565C59F1743776A10250F581D482DA8B6D6261460D3F53B279CC6913CE747006A2E3254
username:0x0100A607BA7C54A24D17B565C59F1743776A10250F581D482DA8B6D6261460D3F53B279CC6913CE747006A2E3254
username:0x0100A607BA7C54A24D17B565C59F1743776A10250F581D482DA8B6D6261460D3F53B279CC6913CE747006A2E3254:::::::
```

#### MSSQL05 hash (MSSQL 2012 = SHA512)
```
0x0200F8AD6746B48DC390E37F07597844806A9488D286E65E901CB1AA33AE425B8335E7C5858840105DA0AF14BD26AA3662EAF33E40ADABD0FECFCC740B5338497584697AA69F
username:0x0200F8AD6746B48DC390E37F07597844806A9488D286E65E901CB1AA33AE425B8335E7C5858840105DA0AF14BD26AA3662EAF33E40ADABD0FECFCC740B5338497584697AA69F
username:0x0200F8AD6746B48DC390E37F07597844806A9488D286E65E901CB1AA33AE425B8335E7C5858840105DA0AF14BD26AA3662EAF33E40ADABD0FECFCC740B5338497584697AA69F:::::::
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

#### "MSSQL05" Cracking methodology :
```
➤ 1. Verify login=password
john --single --format=mssql05 hashes.txt --fork=4

➤ 2. Standards dictionnary attacks with[out] rules
john --wordlist=dictionary.txt --format=mssql05 hashes.txt --fork=4
john --wordlist=dictionary.txt --format=mssql05 hashes.txt --fork=4 --rules=KoreLogic
	
Be careful, the rules=all parameter is very long. With a lot of hashes, this operation will working during several days.
```

#### "MSSQL" Cracking methodology : 
```	
➤ 1. Verify login=password
john --single --format=raw-md5 hashes.txt --fork=4

➤ 2. Standards dictionnary attacks with[out] rules
john --wordlist=dictionary.txt --format=mssql hashes.txt --fork=4
john --wordlist=dictionary.txt --format=mssql hashes.txt --fork=4 --rules=KoreLogic

Be careful, the rules=all parameter is very long. With a lot of hashes, this operation will working during several days.
```

#### "MSSQL12" Cracking methodology : 
```	
➤ 1. Verify login=password
john --single --format=mssql12 hashes.txt --fork=4
	
➤ 2. Standards dictionnary attacks with[out] rules
john --wordlist=dictionary.txt --format=mssql12 hashes.txt --fork=4
john --wordlist=dictionary.txt --format=mssql12 hashes.txt --fork=4 --rules=KoreLogic
	
Be careful, the rules=all parameter is very long. With a lot of hashes, this operation will working during several days.
```

#### Show result:
```
➤ The cracked hashes are located in the corresponding .pot file

MSSQL05 -> john --format=mssql05 hashes.txt --show
MSSQL -> john --format=mssql hashes.txt --show
MSSQL12 -> john --format=mssql12 hashes.txt --show		
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

#### "MSSQL05" Cracking methodology :
```
➤ 1. Hash format
0x01004086CEB6BF932BC4151A1AF1F13CD17301D70816A8886908

➤ 2. Verify login=password
Username should be added in a new list in order to be tested

➤ 3. Standards dictionnary attacks
hashcat -m 132 -a 0 [Dictionnary].txt [Hash].txt --force

➤ 4. Standards dictionnary attack with rules
To see the existing rules attacks: ls -l /usr/share/hashcat/rules/

hashcat -m 132 -a 0 [Dictionnary].txt [Hash].txt  -r [Rules_path] --force
```

#### "MSSQL" Cracking methodology :
```
➤ 1. Hash format
0x0100A607BA7C54A24D17B565C59F1743776A10250F581D482DA8B6D6261460D3F53B279CC6913CE747006A2E3254

➤ 2. Verify login=password
Username should be added in a new list in order to be tested

➤ 3. Standards dictionnary attacks
hashcat -m 131 -a 0 [Dictionnary].txt [Hash].txt --force

➤ 4. Standards dictionnary attack with rules
To see the existing rules attacks: ls -l /usr/share/hashcat/rules/

hashcat -m 131 -a 0 [Dictionnary].txt [Hash].txt  -r [Rules_path] --force
```

#### "MSSQL12" Cracking methodology :
```
➤ 1. Hash format
0x0200F8AD6746B48DC390E37F07597844806A9488D286E65E901CB1AA33AE425B8335E7C5858840105DA0AF14BD26AA3662EAF33E40ADABD0FECFCC740B5338497584697AA69F
	
➤ 2. Verify login=password
Username should be added in a new list in order to be tested
			
➤ 3. Standards dictionnary attacks
hashcat -m 1731 -a 0 [Dictionnary].txt [Hash].txt --force

➤ 4. Standards dictionnary attack with rules
To see the existing rules attacks: ls -l /usr/share/hashcat/rules/

hashcat -m 1731 -a 0 [Dictionnary].txt [Hash].txt  -r [Rules_path] --force
```

#### Show result:
```
➤ For "MSSQL05" hash :
hashcat -m 132 -a 0 [Dictionnary].txt [Hash].txt --show

➤ For "MSSQL" hash :
hashcat -m 131 -a 0 [Dictionnary].txt [Hash].txt --show

➤ For "MSSQL12" hash :
hashcat -m 1731 -a 0 [Dictionnary].txt [Hash].txt --show
```
