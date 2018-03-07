# a script to be run in jenkins job to backup to S3

## inject env vars
```ini
JENKINS_HOME=/var/lib/jenkins
BUCKET=name-jenkins-backup
FILE_PREFIX=JenkinsBackup
```
## inject password vars to build
```ini
GPG_CRYPTO='Jslfn23U#*O#FAMF'
```

```sh
DATE=`date +%Y-%m-%d`
BACKUP_FILE=$FILE_PREFIX-$BUILD_ID-$DATE.tar.gz
ENCRYPTED_FILE=$BACKUP_FILE.gpg
TARGET_FILE="target_file_name"

tar czf $BACKUP_FILE $TARGET_FILE

gpg --yes --batch --passphrase=$GPG_CRYPTO -c $BACKUP_FILE

sudo /bin/aws --profile idps-ops-02 s3 cp $ENCRYPTED_FILE s3://$BUCKET

sudo rm -r $WORKSPACE/*
```