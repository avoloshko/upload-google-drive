# upload-google-drive
A shell script to upload a file to google drive.

Overwriting creates a revision.

[gdrive](https://github.com/prasmussen/gdrive) is a command line utility for interacting with Google Drive.

```bash
#!/bin/sh

set -e

PARENT_ID=$1
FILE_PATH=$2
FILE_NAME=$(basename ${FILE_PATH})

if [ ! -f $FILE_PATH ]
then
  echo "$FILE_PATH: No such file"
  exit 1
fi

FILE_ID=$(gdrive list --absolute  --no-header --query "'${PARENT_ID}' in parents and name = '${FILE_NAME}'" | awk '{print $1}')

if [ -z "$FILE_ID" ]
then
  gdrive upload -p $PARENT_ID $FILE_PATH
else
  gdrive update $FILE_ID $FILE_PATH
```

Example:
```bash
sh gdrive_publish.sh 0A1Eg6nOIQNNKeUJ1TGxPZHl1aE0 app/build/outputs/apk/LitecoinWallet.apk
```

PARENT_ID can be found on Google Drive:
![alt text](https://raw.githubusercontent.com/avoloshko/upload-google-drive/master/parent_id.png)
