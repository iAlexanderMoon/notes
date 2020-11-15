dbs -f /working/apps/db --file-per-object --script-drop-create

Do we want to remove all files before doing it? Yes or nothing is exported./

dbs -f /working/apps/db --file-per-object --script-drop-create --exclude-extended-properties 

rm db/*
dbs -f /working/apps/db --file-per-object --script-drop-create --exclude-extended-properties

// Static Lookup table:
dbs -f /working/apps/db --file-per-object --script-drop-create --exclude-extended-properties --data-only --include-objects dbo.Tally

// We could actually backup the entire database this way also instead of a bacpac



