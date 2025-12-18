Create a Jenkins job named database-backup.


Configure it to take a database dump of the kodekloud_db01 database present on the Database server in Stratos Datacenter, the database user is kodekloud_roy and password is asdfgdsd.


The dump should be named in db_$(date +%F).sql format, where date +%F is the current date.

Copy the db_$(date +%F).sql dump to the Backup Server under location /home/clint/db_backups.


Further, schedule this job to run periodically at */10 * * * * (please use this exact schedule format)




```bash

sshpass -p "$db_server_pass" ssh -o StrictHostKeyChecking=no $db_server_user \
"echo "$db_server_pass" | sudo -S mysqldump -u $db_user -p$db_pass $db_name > /tmp/$file_name" 
sshpass -p "$db_server_pass" ssh -o StrictHostKeyChecking=no $db_server_user \
"sshpass -p "$backup_pass" scp -o StrictHostKeyChecking=no /tmp/$file_name $backup_user:/tmp" 
sshpass -p "$backup_pass" ssh -o StrictHostKeyChecking=no $backup_user \
"echo "$backup_pass" | sudo -S cp /tmp/$file_name /home/clint/db_backups"
```