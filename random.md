# Linux - Random commands
Some random things I learned on different Linux systems that I have to do now and again. .

## Disclaimer:
These commands might impact the system. Although I am trying to ensure that everything works, there could be changes/differences in systems that impact how the command is implemented. 
Using these commands are at the users own risk and care should be taken.

## 1. SCP - Secure Copy Protocol via SSH
To copy filed between 2 linux systems, it is best to have SSH enabled and login details configured between the systems. 

the `scp` command works in the following way:

`scp <from> <to>`

Let's say, you have a workstation and a server. You are logged into the workstations. 

### Scenario 1: Copy a file to the server:
If you want to copy from the workstation to the server, your basic syntax looks like this:

'scp /path/to/file username@server:/path/to/remote/destination'

In the below example, we copy a file from our home directory to the home directory of the user on the server:

`scp /home/local-user/test.file user@server:/home/user`

This will result in a test.file in the user@server home directory

### Scenario 2: Copy a file from the server to the workstation:

If you want to copy from the server to the workstation, your basic syntax looks like this:

'scp username@server:/path/to/remote/destination /path/to/file'

In the below example, we copy a file from our home directory to the home directory of the user on the server:

`scp user@server:/home/user/test2.file /home/test-user`

This will result in a test2.file in the home directory of the test user.

> The destination can be a folder or a file. To be 100% sure, always put a file name.

### Scenario 3: Multiple files?
What if we want to copy multiple files? Like test1.file, test2.file, test3.file?
This works best from local to remote:
`scp test1.file test2.file test3.file user@serber:~/`

alternatively, you can use wildcards:
`scp *.file user@server:~/`

and finally, if you want to copy all files in a directory, use the `-r` flag:
`scp -r /home/test-user/directory/ user@server:~/`

the above copies the folder 'directory' to the home directory of user@server.

---
*Sources:*
 - https://docs.csc.fi/data/moving/scp/
 - https://docs.oracle.com/cd/E26502_01/html/E29001/remotehowtoaccess-55154.html
 - https://stackoverflow.com/questions/16639483/how-do-i-use-scp-to-copy-a-file-from-the-server-to-the-client-side
