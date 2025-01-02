## 1. Dump the Windows files which contain the local password hashes (Windows registry files: SAM, SECURITY and SYSTEM):

Dump les fichiers de registre à l'aide de "REG SAVE" (nécessite les droits admin):
```
reg save HKLM\SAM C:\temp\sam
reg save HKLM\SYSTEM C:\temp\system 
reg save HKLM\SECURITY C:\temp\security
```



## 2. Extract the windows local password hashes:

***Using secretsdump***
```
secretsdump.py -sam sam -security security -system system LOCAL
```

***Using creddump7***
```
git clone https://github.com/Tib3rius/creddump7
pip3 install pycrypto
python3 creddump7/pwdump.py SYSTEM SAM
OR
python3 creddump7/pwdump.py SYSTEM SAM > hashes.txt
```

## 3. Hash formats :

Two types of hashes are present in SAM file
	- LM (old storage method of Windows)
	- NTLM


Examples of hash

	[username]::[LM]:[NTLM]:::

```
Administrator:500:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
User:1000:aad3b435b51404eeaad3b435b51404ee:0b82e1dace77e29dd1de00896ba1c5bc:::
```

- If the LM hash has the value "aad3b435b51404eeaad3b435b51404ee" so the password is not storage in the LM hash
- If the NTLM hash has the value "31d6cfe0d16ae931b73c59d7e0c089c0" so the password is empty (potentially due to a desactivated account)
- If the NTLM hash has the value "b1b1d2ebf0bb5707135a9032caea49d3" so we have the same password



## 4. John the ripper - Crack the password hashes :

#### General commands and rules

	john [type of attack] --format=[hash of the format] 

	--rules:Single
	--rules:Wordlist
	--rules:Extra
	--rules:Jumbo (all the above)
	--rules:KoreLogic
	--rules:All (all the above)


#### Cracking methodology :

1. Verify the LM hashes:

You can beginning by a crack of the LM password, in the case of the some hash alues are different to :
```
"aad3b435b51404eeaad3b435b51404ee" -> not storage in LM hash
"31d6cfe0d16ae931b73c59d7e0c089c0" -> empty password
"b1b1d2ebf0bb5707135a9032caea49d3" -> we have the same password
````

2. Verify login=password
```
john --single --format=[LM|NTLM] hashes.txt --fork=4
```

3. Standards dictionnary attacks with[out] rules
```
john --wordlist=dictionary.txt --format=LM hashes.txt --fork=4
john --wordlist=dictionary.txt --format=LM hashes.txt --fork=4 --rules=KoreLogic
```

Be careful, the rules=all parameter is very long. With a lot of hashes, this operation will working during several days.

4. Show result:

The cracked hashes are located in the corresponding .pot file

	john --format=LM hashes.txt --show
	john --format=NTLM hashes.txt --show



## 5. Hashcat - Crack the password hashes :

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
```

#### Cracking methodology :

1. Verify the LM hashes:

You can beginning by a crack of the LM password, in the case of the some hash alues are different to :
```
"aad3b435b51404eeaad3b435b51404ee" -> not storage in LM hash
"31d6cfe0d16ae931b73c59d7e0c089c0" -> empty password
"b1b1d2ebf0bb5707135a9032caea49d3" -> we have the same password
```

2. Verify login=password
```
➤ For LM hash :

• Generate the list of the login
cat [LIST_OF_HASH].txt | cut -d':' -f1 > [OUTPUT_LOGIN].txt

• Try login=password 
hashcat -m 3000 hash.txt [LOGIN].txt
```

```
➤ For NTLM hash :

• Generate the list of the login
cat [LIST_OF_HASH].txt | cut -d':' -f1 > [OUTPUT_LOGIN].txt

• Try login=password 
hashcat -m  1000 hash.txt [LOGIN].txt
```

3. Dictionnary attack
```
➤ For LM hash :
hashcat -m 3000 -a 0 [Dictionnary].txt [Hash].txt --force

➤ For NTLM hash:
hashcat -m 1000 -a 0 [Dictionnary].txt [Hash].txt --force
```

4. Dictionnary attack with rules

To see the existing rules attacks ls -l /usr/share/hashcat/rules/

```
➤ For LM hash :
hashcat -m 3000 -a 0 [Dictionnary].txt [Hash].txt  -r [Rules_path] --force

➤ For NTLM hash:
hashcat -m 1000 -a 0 [Dictionnary].txt [Hash].txt -r [Rules_path] --force
```

5. Show cracked/not cracked passwords
```
➤ For LM hash :
hashcat -m 3000 -a 0 [Dictionnary].txt [Hash].txt -show

➤ For NTLM hash:
hashcat -m 1000 -a 0 [Dictionnary].txt [Hash].txt -show
```
