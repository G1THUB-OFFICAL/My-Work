# Antigravity 2.0 → Antigravity IDE Conversation Migration

A Windows utility for migrating Antigravity 2.0 conversation history into Antigravity IDE.

## What this script does

The migration script:

1. Checks that the expected Antigravity 2.0 and Antigravity IDE folders exist.
2. Closes running Antigravity processes.
3. Copies all `.pb` conversation files from:
   `%USERPROFILE%\.gemini\antigravity\conversations`

   to:
   `%USERPROFILE%\.gemini\antigravity-ide\conversations`
4. Merges:
   `antigravityUnifiedStateSync.trajectorySummaries`

   from the old Antigravity database into the IDE database.
5. Migrates relevant Antigravity preference/state entries that are missing in the IDE.
6. Adjusts old Antigravity paths to the Antigravity IDE paths where applicable.
7. Leaves the original Antigravity 2.0 conversation files untouched.

## Important: No Backup

**This version intentionally does NOT create a backup.**

This keeps additional disk-space usage as low as possible.

However, because the destination's `.pb` files can be overwritten, you should only use this version if you are comfortable proceeding without a backup.

The source Antigravity 2.0 files are not deleted.

## Requirements

- Windows
- Python 3.x installed and available as `python`
- Antigravity 2.0 installed
- Antigravity IDE installed
- Both applications should use the normal Windows storage locations

Expected source:

```text
%USERPROFILE%\.gemini\antigravity
```

Expected destination:

```text
%USERPROFILE%\.gemini\antigravity-ide
```

Expected databases:

```text
%APPDATA%\Antigravity\User\globalStorage\state.vscdb
%APPDATA%\Antigravity IDE\User\globalStorage\state.vscdb
```

## Before running

### 1. Close Antigravity

Close:

- Antigravity 2.0
- Antigravity IDE

The script also attempts to close Antigravity processes automatically.

### 2. Check that Python is installed

Open PowerShell and run:

```powershell
python --version
```

You should get a Python version such as:

```text
Python 3.x.x
```

## Running the migration

Place these files together, for example on the Desktop:

```text
migrate_antigravity.py
README.md
```

Open PowerShell and run:

```powershell
python "$env:USERPROFILE\Desktop\migrate_antigravity.py"
```

If the script is stored somewhere else, change the path accordingly.

The script will display:

```text
Continue? Type YES to continue:
```

Enter:

```text
YES
```

The script will then perform the migration.

## Expected output

A successful migration should look approximately like:

```text
=================================================================
 Antigravity 2.0 -> Antigravity IDE
 Conversation Migration
=================================================================

NO BACKUP WILL BE CREATED.

[INFO] Closing Antigravity processes...
[OK]   Antigravity processes stopped/checked.

[INFO] Found 93 conversation files.
[OK]   Copied 93 conversation files.

[INFO] Opening databases...
[INFO] Old Antigravity keys: ...
[INFO] New IDE keys: ...
[INFO] Merging trajectory summaries...
[OK]   Database migration completed.

=================================================================
 MIGRATION COMPLETED
=================================================================
```

The exact number of database entries can vary between installations and versions.

## After migration

Open **Antigravity IDE** and check the conversation history.

Do not open both Antigravity applications simultaneously while testing the migrated history.

If the conversations appear, the migration is complete.

## What the script does NOT do

The script does not:

- Delete the original Antigravity 2.0 conversation files.
- Replace the entire `state.vscdb` database.
- Delete your Antigravity 2.0 installation.
- Upload your conversations anywhere.
- Send your conversation data to an external server.
- Create an additional backup directory.
- Modify files outside the Antigravity storage locations involved in the migration, except for the script itself.

## Important warning

This is an unofficial migration utility based on the storage layout used by Antigravity installations.

Antigravity updates may change its internal storage format. A future version may therefore require changes to this script.

If the script reports an error, do not repeatedly run it blindly. Check the error first.

## Troubleshooting

### Error: Antigravity data directory not found

Make sure Antigravity 2.0 is installed and has been used on the Windows account running the script.

The script expects:

```text
%USERPROFILE%\.gemini\antigravity
```

### Error: Antigravity IDE data directory not found

Make sure Antigravity IDE has been installed and opened at least once.

The script expects:

```text
%USERPROFILE%\.gemini\antigravity-ide
```

### Error: database not found

Open the corresponding application once, close it, and run the script again.

The expected database is:

```text
%APPDATA%\Antigravity\User\globalStorage\state.vscdb
```

and:

```text
%APPDATA%\Antigravity IDE\User\globalStorage\state.vscdb
```

### Conversations still do not appear

First confirm that the `.pb` files were copied:

```powershell
Get-ChildItem "$env:USERPROFILE\.gemini\antigravity-ide\conversations" -Filter *.pb | Measure-Object
```

Then compare the number with the source:

```powershell
Get-ChildItem "$env:USERPROFILE\.gemini\antigravity\conversations" -Filter *.pb | Measure-Object
```

The two counts should normally match after migration.

Do not delete the source conversations.

## Disk-space note

The migration does not create a separate backup.

There will normally be a copy of the conversation files in both:

```text
.antigravity\conversations
```

and:

```text
.antigravity-ide\conversations
```

because the two applications use separate conversation directories.

The script does not delete the source copy because that would make recovery impossible if something goes wrong.

## License / Disclaimer

Provided as an unofficial community migration utility.

Use at your own risk.

The script is not an official Google/Antigravity migration tool.

Always review the script before running it on important data.
