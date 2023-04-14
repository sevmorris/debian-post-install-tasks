**Create a directory for backups**

```
mkdir /location/for/backups
```
<br>

**Create a script for backing up automatically**<br>
Remember to change BACKUP_DIR path

```
#!/bin/bash
DATE=$(date +%d-%m-%Y)
BACKUP_DIR=”/location/for/backups”

# Delete files older than 1 hour
find $BACKUP_DIR/* -mmin +60 -exec rm {} \;
```
<br>

**Make it executable (changing name as desired)**

```
chmod +x bup-script.sh
```
<br>

**Move script to /bin directory and change permissions**

```
mv bup-script.sh /bin/
```


```
chmod 755 /bin/bup-script.sh
```
<br>

**Create a cron job**

```
crontab -e
```

Add a line to schedule the job
(This example runs every ten minutes)

```
*/10 * * * * /bin/bup-script.sh
```
<br>
***
<br>

**Crontab syntax**

```
1 2 3 4 5 /path/to/script_or_command arg1 arg2
```

**OR**

```
1 2 3 4 5 /bin/bup-script.sh
```
<br>

**Where**

1: Minute (0-59)<br>
2: Hours (0-23)<br>
3: Day (0-31)<br>
4: Month (0-12 [12 == December])<br>
5: Day of the week (0-7 [7 or 0 == sunday])
<br><br>

**Easy to remember format**

```
* * * * * command to be executed
- - - - -
| | | | |
| | | | ----- Day of week (0 - 7) (Sunday=0 or 7)
| | | ------- Month (1 - 12)
| | --------- Day of month (1 - 31)
| ----------- Hour (0 - 23)
------------- Minute (0 - 59)
```
<br><br>
