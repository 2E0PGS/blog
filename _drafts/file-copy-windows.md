

# Moving lots of files between two Windows servers

lots of small files 100k+

I have done this many times now. This is the best method besides using SFTP.

## Destination server

RPD into the destination server. Use it's RDP client to connect to the source server but make sure to specify device attachment of local HDD.

This is more reliable than copy paste shared clipboard.

## Now on the source RDP

Don't use windows right click paste file copy. This has some odd memory hogging behavour. It also doesn't preseve "Date modified".

Robocopy also allows us to update those files incrementally again. Useful if you are migrating bulk of the historical data now. Then when you are ready to migrate server before final move you can refresh the new server with the latest files.

Use: `robocopy "D:\myfiles" "\\tsclient\D\myfiles" /MIR`

Warning it will remove files in the destination dir where they dont exsist on the source.
