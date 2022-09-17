---
id: zommz1xs1ornq2o8g0f1avp
title: Mongodb
desc: ''
updated: 1663409164559
created: 1663409164559
isDir: false
title_imported: mongoDB
updated_imported: '2020-05-25T10:50:14.000Z'
created_imported: '2019-10-30T16:03:21.000Z'
---

### Location

If you installed via the package manager, the data directory `/var/lib/mongodb` and the log directory `/var/log/mongodb` are created during the installation.

### Start the service

```bash
sudo service mongod start
sudo service mongod status
sudo service mongod restart
sudo service mongod stop
```


### MongoDB command

```bash
show dbs 	#show database
use [database-name] #switch to database-name
show collections #Show collections 
db.[collection name].find() #show all data
```
