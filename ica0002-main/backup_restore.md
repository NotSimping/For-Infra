### Did we loose all data? never fear your backups are here i hope.
### hera are the steps we should do in ordet to get the system backup and running:

### *ATTENTION*
### first and formost we should check Grafna dashboard and take a look at `Dashboard > MYQSL` and see which host is set to `Read/Write`
### OR we could go to `group_vars > all.yaml` and look for the `mysql_host` variable. the host on that file should be the primary mysql host.

### Now we shall begin backup steps for mysql:
1. SSH to the primary MYSQL Server.
2. Run `sudo su backup` to change to user `backup`
3. Get backups from the backup server by running `duplicity --no-encryption full /home/backup/influxdb/ rsync://AlecSimp@backup.bindchilling.rice/influxdb`
4. Veriry if the restored file is in the appropriet directory by `ls -l /home/backup/restore/mysql` *ignore the error that prints out*
5. We should then have a backup file called `agama.sql`
6. Exit out of backup user using `exit` and change to user `root` by using `sudo su`
7. To restore contents of agama database with : `mysql agama < /home/backup/restore/mysql/agama.sql`
8. We should verify mysql is ruuning with `sudo service mysql status` and you should see active `(running)`.
9. Next we verify the server if it contains the data from backup with the following.
    - `AGAMA webpage` shoudl show the items
    - `mysql -e "SELECT * FROM agama.item"` should show the data of the item table.
10. If the database was gone or empty we shoud run `sudo mysql -e "SELECT * FROM agama.item LIMIT 3"` to check if there is data in the database now.
11. Now we run `exit` to log out of the root user.

### Now we begin the steps to backup for influxdb:
### first make sure that you have `ssh` to `vm3` if you are in vm3 then you may proceed with the steps.

0. `ssh to machine 3` if you haven't if you're not sure which machine `influxdb` runs in you could check with `host` file.
1. first step we need to switch to user `backup` by running the `sudo su backup`.
2. we grab the backup with the command `duplicity --no-encryption restore rsync://AlecSimp@backup/influxdb /home/backup/restore/influxdb` 
3. to check if the backup worked we should now use the command `ls -l /home/backup/restore/influxdb` and we would see the backup files there. once done use the command `exit` to get out of user backup
4. no we switch to user `root` with `sudo su` or `sudo su root`
5. the we run the following command `service telegraf stop` to stop telegraf from sending logs.
6. now we would need to remove the telegraf database with the command `influx -execute 'DROP DATABASE telegraf'`
7. and now we would begin to restore the database with this command `influxd restore -portable -database telegraf /home/backup/restore/influxdb`
8. to begin verificatioon of telegraf database is present we would now use the following commands in succesion.
 - `influx`
 - `show databases`
 - `use telegraf`
 - `show measurements`
### you should see `cpu`,`disk` etc..
9. if done correctly you should be able to the data in telegraf measurements now you could use `exit` to get out of influx
10. now we would start talegraf to start sending logs by using the command `service telegraf start`
11. then we could check that telegraf is running by `service telegraf status` and we should see that its active and runnig. 
12. now everything is done we could use `exit` to get out of user root. then we could.
13. now you could check your `grafana > dashboard > syslog` and see the measurements there.

### if done correctly all data should be back. "done and dusted out of this world" -chinese dude