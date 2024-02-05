This files describes the backup measures done incase of unfoseen probwems UwU.

Install and configure infrastructure with Ansible:

    ansible-playbook infra.yaml

Restore MySQL data from the backup:

    sudo -u backup duplicity restore <args>
    <another-command>
    <yet-another-command>

<add a few words here how the result of backup restore can be checked>


Main services to be backed up.

    - MySQL
    - InfluxDB
    - group_vars all.yaml

Reasons: this service contain cruicial data that we could never recreat hence we have to properly have a backup of these services. side note group_vars would also be backed up as it would be helpful to still have access to all the variables used for the program and it would be eaiser to rebuild the rest of the ansible services.

I chose to make this backup to a bear minimum as i beleive that keeping a backup of the databases is eneough as this is the most important part of the overall system. futher more keeping it in the bare minimum makes it a lot easier to back up and would required far less resources as from a companies stand point maximizing efficiency and lowcost is crucial and and recreating most of the services/play book is recreatable.

we would not out right not backup everything i would probably keep one backup of the "/etc/roles/"services"/tasks/main.yaml" as this would make setting up the whole service from scratch faster than totally zero.

as for the rest of the ansible files it could be left out as it would easily be recreated.

So how do?? or what do??

    Backup coverage
        - MySQL
        - InfluxDB
        this files would be backed up to lessen the blow of data lost.
    RPO (recovery point objective)
        The point of the backup in this case is that we could make recreate the system in an in litle time as possible as i could not afford to loose all files or else i would fail the class. hence making a back up that at least sets me back by a day would be helpfull instead of loosing a file that would set me back by a month.
    Versioning and retention
        I would have 3 backups of the datas. 2 on site and one off site. having two on site would be helfull as it would allow for quick recovery as ease of access to the data. in case of all this data is utter crap i would then depend on the file that is off site as that file would probably be not tamperd with.
        off site backup would be updated monthly and on site would be updated weekly with one backup per week and changing in between drives in case of problems.
    Usability checks
        before backups are made all files would be verified if they are working by runnig the playbook and cheking if any errors occured and reference the changes if there are any suspicios lines in the file.
    Restoration criteria
        backups would be restored when systems have been whiped clean of any data that would mess with the backup this is done to prevent any futher problems from occuring cause if the back ups are also not good then everything would be gone. sort off... but better safe than sorry.
    RTO (recovery time objective)
        realistically knowing that i am slow i should atleast be able to set everything back up and running by atleast under 12 hours and the latest be 24 hours to recover the system and get it back up and runnig.-- how many backup versions are stored and for how long
 everything i would probably keep one backup of the "/etc/roles/"services"/tasks/main.yaml" as this would make setting up the whole service from scratch faster than totally zero.

as for the rest of the ansible files i