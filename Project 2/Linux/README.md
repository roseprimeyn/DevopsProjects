# Linux

## Skills

Requirements to complete the deployment steps:

Linux User Management, Permissions, Directory Structure, File Systems, File Management

## Pre-Requisites

Login to AWS cloud and create Linux based EC2 instance to complete the below assignment.

## Deployment
1. Login to the server as super user and perform below
    
     i. Create users and set passwords – user1, user2, user3
   
    ii. Create Groups – devops, aws
   
   iii. Change primary group of user2, user3 to ‘devops’ group
   
    iv. Add ‘aws’ group as secondary group to the ‘user1’
   
     v. Create the file and directory structure shown in the above diagram.
   
    vi. Change group of /dir1, /dir7/dir10, /f2 to “devops” group
   
   vii.Change ownership of /dir1, /dir7/dir10, /f2 to “user1” user.
   
2. Login as user1 and perform below
 
     i. Create users and set passwords – user4, user5
   
    ii. Create Groups – app, database

3. Login as ‘user4’ and perform below
   
      i. Create directory – /dir6/dir4
   
     ii. Create file – /f3
   
    iii. Move the file from “/dir1/f1” to “/dir2/dir1/dir2”
   
     iv. Rename the file ‘/f2′ to /f4’

4. Login as ‘user1’ and perform below
   
    i. Create directory – “/home/user2/dir1”
   
   ii.Change to “/dir2/dir1/dir2/dir10” directory and create file “/opt/dir14/dir10/f1” using relative path method.
   
   iii.Move the file from “/opt/dir14/dir10/f1” to user1 home directory
  
    iv. Delete the directory recursively “/dir4”
   
   v.Delete all child files and directories under “/opt/dir14” using single command.
    
   vi.Write this text “Linux assessment for a DevOps Engineer!! Rosepetal!!” to the /f3 file and save it.

5. Login as ‘user2’ and perform below
   
     i. Create file “/dir1/f2”
   
    ii.Delete /dir6
   
   iii.Delete /dir8
   
    iv.Replace the “DevOps” text to “devops” in the /f3 file without using editor.
   
     v.Using Vi-Editor copy the line1 and paste 10 times in the file /f3.
   
    vi.Search for the pattern “Engineer” and replace with “engineer” in the file /f3 using single command.
   
   vii.Delete /f3
   
6. Login as ‘root’ user and perform below
   
     i. Search for the file name ‘f3’ in the server and list all absolute paths where f3 file is found.
   
    ii.Show the count of the number of files in the directory ‘/’
   
   iii.Print last line of the file ‘/etc/passwd’

7. Login to AWS and create 5GB EBS volume in the same AZ of the EC2 instance and attach EBS volume to the Instance.

8. Login as ‘root’user and perform below
   
     i.Create File System on the new EBS volume attached in the previous step
   
    ii. Mount the File System on /data directory
   
   iii. Verify File System utilization using ‘df -h’ command – This command must show /data file system

   iv. Create file ‘f1’ in the /data file system.

9. Login as ‘user5’ and perform below
    
     i. Delete /dir1
   
    ii.Delete /dir2
   
   iii.Delete /dir3
   
    iv.Delete /dir5
   
     v.Delete /dir7
   
   vi.Delete /f1 & /f4
   
  vii.Delete /opt/dir14

10. Logins as ‘root’ user and perform below
    
11. Delete users – ‘user1, user2, user3, user4, user5’
    
12. Delete groups – app, aws, database, devops

13. Delete home directories of all users ‘user1, user2, user3, user4, user5’ if any exists still.

14. Unmount /data file system

15. Delete /data directory

16. Login to AWS and detach EBS volume to the EC2 Instance and delete the volume and then terminate EC2 instance.
