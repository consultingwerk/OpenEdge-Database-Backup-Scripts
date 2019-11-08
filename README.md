# OpenEdge-Database-Backup-Scripts
Ant scripts for backup of OpenEdge databases.

This is a very simple Ant script to backup any number of databases, and handle the After Image Archive files for that database. This can very easily be incorporated into Jenkins to run on a schedule. 

## Execution
To run, install Ant. Install Ant Contrib. Customise backup.properties. 
Command line: ant -f DBBackup.xml

## Ant Targets
### BOM
Not necessary but prints a basic list of property values to the terminal. Useful for problem determination when running automated under Jenkins

### BackupDatabases
Backs up the actual databases. Runs offline or online depending on property value. Does not use probkup, but rather replicates the logic of this script, as Jenkins has a hard time getting return values from batch files. 

### VerifyBackup
Runs a partial verify of the backup file. Useful for checking there are no bad blocks. 

### CopyOffsite
Copies the backups offsite once we're happy they verify ok

### BackupConfig
Backs up any config you define

### CopyAIOffsite
Copies After Image Archive files offsite. You may wish to have this in a separate script that runs more frequently. 

### CleanupAI
Removes local aged After Image Archive files based on the retention policy set in the properties file

### RemoveOldBackups
Removes local aged backup files based on the retention policy set in the properties file

## backup.properties
### progress.DLC
Progress DLC directory to use
### backup.online
'online' will run an online backup. Anything else will run offline.
### Private.Buffers
The number of Private Buffer (-Bp) to allocate for an online backup
### AI.Retention.Number
The number of units of time for AI retention
### AI.Retention.Unit
The units of time for AI retention
### Backup.Retention.Number
The number of units of time for Backup retention
### Backup.Retention.Unit
The units of time for Backup retention

### Temp.Directory
The temp directory to use
### parallel
Run scripts in parallel or not
### threadCount
How many pararallel threads to spawn

### Database.List
A comma delimited list of Database Logical Names to back up

### Database specific parameters
There must be one of each for each database to be backed up
#### Database.List.ldname.Location
The physical folder the database .db file lives in
#### Database.List.ldname.FileName
The filename (without extension) of the database
#### Database.List.ldname.LocalBackup
The local backup folder
#### Database.List.ldname.BackupFile
The base name of the backup file. It will be datestamped for uniqueness.
#### Database.List.ldname.AIArchive
The AI Archive Location
#### Database.List.ldname.OffsiteBackup
The offsite backup location. Currently only supports physical volumes, but you could wrap other backup solutions in here.
#### Database.List.ldname.OffsiteAIBackup
The offsite backup location for AI Archive files
#### Database.List.ldname.ConfigFiles
A comma delimited list of any config files (absolute path) you wish to also keep a backup of. 
