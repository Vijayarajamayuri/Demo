#Linux Lab
##Prerequisites:
- Make sure you have access to Pluralsight.com 

##Reference documentation
https://www.digitalocean.com/community/tutorials/linux-commands#the-pwd-command-in-linux 

**More information can be found about mounts:** https://www.geeksforgeeks.org/linux-directory-structure/

##Set up Sandbox environment 
Go to https://app.pluralsight.com/hands-on/playground/cloud-sandboxes and open Azure Sandbox in Incognito mode 

1. Sign in using the credentials provided by Pluralsight 
2. Create a virtual machine name it ltlf
3. Sign in using `ssh username@publicip`

##Gather information about the Linux VM 

Helpful commands that will help you troubleshoot

Man <Command> - Access manual pages for all Linux commands

<Command> --help | less

Whatis <command> - Find what a command is used for

clear - Clear the terminal display

##Lab
<details>
<summary>1. Navigate to the root directory. List at all the directories available in root. Print working directory. Then navigate back to your home directory</summary>

```
cd /
ls
pwd
cd
pwd
```
</details>

<details>
<summary>2. Use a linux command to find your username, ip address, and the OS?
</summary>

```

whoami - Get the active username
ip addr or ifconfig to also get network configs
uname - Linux command to get basic information about the OS
```
</details>

<details>
<summary>3. What is the top process running on your system with their filesystem and system usage on your VM? </summary>

```

top - View active processes live with their system usage
```
</details>

<details> 
<summary>3. What filesystem is using up the most space?</summary>  

`df  - Display disk filesystem information`

</details>

<details>
<summary>4. Check to see if port 22 is open</summary> 

```
sudo apt install net-tools
netstat -tulpn
```
```
--tcp|-t
--udp|-u
-l, --listening
   Show only listening sockets.  (These are omitted by default.)
-p, --program
   Show the PID and name of the program to which each socket belongs.
--numeric, -n
   Show numerical addresses instead of trying to determine symbolic host, port or user names.
   ```
</details>

<details>
<summary>5.Install Romeo and Juliet from https://gutenberg.org/cache/epub/1513/pg1513.txt</summary>

```

wget https://gutenberg.org/cache/epub/1513/pg1513.txt
```
</details>

<details>
<summary>6. Find the quote "Do you bite your thumb at us, sir?"</summary>

```
grep "Do you bite your thumb at us, sir?" pg1513.txt
```
</details>

<details><summary>7. Output the first 10 lines of Romeo and Juliet</summary>

```
head pg1513.txt
```
</details>

<details><summary>8. Output the last 10 lines of Romeo and Juliet </summary>

```
tail pg1513.txt
```
</details>

<details>
<summary>9. Create 3 directory in /tmp called EHDS and DCE</summary>

```
Mkdir /tmp/ehds
Mkdir /tmp/dce
mkdir /tmp/cltransition
```
</details>

<details>
<summary>10. Remove cltransion directory</summary>

`rmdir /tmp/cltransition`
</details>

<details>
<summary>11. Create Files in each of the directories
In EHDS create 2 files called file1.txt, file2.txt, file3.txt
In DCE create hello.sh</summary>

```
touch /tmp/EHDS/file1.txt
touch /tmp/EHDS/file2.txt /tmp/EHDS/filee.txt /tmp/DCE/hello.sh 
```
</details>

<details>
<summary>12. Copy file /tmp/ehds/file3.txt to /tmp/dce. Then move /tmp/dce to /tmpe</summary>

cp /tmp/ehds/file3.txt /tmp/DCE/file3.txt 
mv /tmp/dce/file3.txt /tmp/
</details>

<details>
<summary>13. Install wordpress from the internet</summary>

curl https://wordpress.org/latest.zip > latest.zip
</details>

<details>
<summary>14. Install zip and unzip. Use these commands to unzip wordpress file</summary>

```

sudo apt install unzip
unzip latest.zip
```
</details>

<details>
<summary>15. What is the permission for this example?
rwxr-xr-x</summary>

755
</details>

<details>
<summary>16. Create a hello world shell script in the file hello.sh </summary>

```
#!/bin/bash
echo "Hello, World!"
```
</details>

<details>
<summary>17. Check the permissions on hello.sh </summary>

```
cd /tmp/dce
ls -al
```
</details>

<details>
<summary>18. Change the permission of //tmp/dce/hello.sh to be executable </summary>

`chmod +x hello.sh`
</details>

<details>
<summary>19. Install nginc</summary>

```
- make sure apt is up to date 
sudo apt update
- install nginx
sudo apt intall nginx
```
</details>

<details>
<summary>20. check if nginx is running</summary>

```

ps -aux
```
</details>

<details>
<summary>21. Stop nginx service</summary>

```

sudo systemctl stop nginx
ps -aux 
```
</details>
